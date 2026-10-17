---
name: fix-uds-core-deployment
description: Reads local k3s and k3d UDS Core deployment evidence and updates affected resources with an approved fix. Use when asked to read or fix namespace termination, admission, Pepr, Istio, host limit, package reconciliation, or Keycloak problems.
compatibility: Requires kubectl access and permission to read the affected local k3s or k3d cluster resources, plus permission to update them for an approved recovery.
---

# How to Fix a UDS Core Deployment

## Prerequisites

Before collecting evidence, require the user to identify:

* The exact kubeconfig context. Never rely on the current context implicitly.
* The application namespace and, when relevant, the UDS Package name.
* The failed command and its complete error message.

Check that `kubectl` is installed and that the named context exists. Stop and report what is missing if either check fails.

Treat diagnostic access as read-only. After identifying the cause, present concrete remediation options with their expected effect, risks, blast radius, rollback path, and verification approach. Ask the user to select an option and explicitly approve execution. Stop before restarting workloads, deleting resources, removing packages, changing host settings, editing finalizers, or making any other recovery mutation. A request to investigate or resolve the failure does not itself approve a recovery mutation.

## How to Troubleshoot k3s UDS Core

1. **Confirm the target and capture cluster state.** Use the explicit context and namespace in every command. Record versions and the immediate state without changing resources.

```sh
kubectl --context <context> version
kubectl --context <context> get namespace <namespace> -o yaml
kubectl --context <context> get pods -n <namespace> -o wide
kubectl --context <context> get events -n <namespace> --sort-by=.lastTimestamp
```

Keep output scoped. Prefer named resources, label selectors, and namespaces over cluster-wide dumps.

2. **Trace the failure through its dependencies.** Start with the object named in the deployment error, then follow each dependency until the first unhealthy component is found. A typical path is application object → admission webhook → Pepr Service and endpoints → Pepr Pod → Istio ambient networking → node or host limits. Do not assume that the last error message names the root cause.

Inspect Services, EndpointSlices, Pods, owner references, readiness conditions, and recent events. Use bounded logs:

```sh
kubectl --context <context> logs -n <namespace> <pod> --all-containers --since=15m --tail=200
kubectl --context <context> logs -n <namespace> <pod> --all-containers --previous --tail=200
```

Use `--previous` only for restarted containers. Redact tokens, passwords, registry credentials, and private certificate material from findings.

3. **Diagnose a namespace stuck in `Terminating`.** Inspect `.status.conditions`, `.spec.finalizers`, remaining namespaced resources, and API discovery failures. If `NamespaceDeletionDiscoveryFailure` is true, inspect unavailable APIService registrations before touching finalizers.

```sh
kubectl --context <context> get namespace <namespace> -o yaml
kubectl --context <context> get apiservice
kubectl --context <context> describe apiservice <apiservice>
```

Repair or remove the stale API registration only after identifying who owns it and receiving authorization. Wait for normal namespace finalization afterward. Force-removing namespace finalizers is a last resort for a disposable cluster after remaining content and discovery failures have been investigated. Warn that it can orphan resources.

4. **Diagnose an unavailable admission webhook.** Identify the exact webhook configuration, Service, namespace, and port from the error. Confirm that the Service selector matches ready Pods and that EndpointSlices contain ready addresses.

```sh
kubectl --context <context> get mutatingwebhookconfiguration,validatingwebhookconfiguration
kubectl --context <context> get service,endpointslice -n <webhook-namespace>
kubectl --context <context> get pods -n <webhook-namespace> -o wide
```

For a Pepr webhook, inspect the Pepr controller and watcher health, readiness, events, and bounded logs. An empty endpoint set is usually a consequence of unready Pods. Diagnose those Pods before retrying the application deployment or changing webhook failure policy.

5. **Diagnose Pepr `CrashLoopBackOff` or readiness failure.** Describe the affected Pod and inspect current and previous logs. Separate configuration errors, Kubernetes API connectivity, certificate problems, and mesh interception failures. Check the Pepr Service endpoints again after addressing the dependency. Do not delete the webhook configuration merely to bypass admission unless the user explicitly accepts the cluster-wide policy impact.

6. **Inspect Istio ambient dependencies.** When Pepr cannot reach the Kubernetes API, its Service has no ready endpoints, or traffic fails unexpectedly, check Istio CNI and ztunnel before treating Pepr as the root cause.

```sh
kubectl --context <context> get pods -n istio-system -o wide
kubectl --context <context> get daemonset -n istio-system
kubectl --context <context> get events -n istio-system --sort-by=.lastTimestamp
```

Inspect bounded current and previous logs for the unhealthy Istio CNI or ztunnel Pods. Confirm DaemonSet desired, current, available, and ready counts. Trace node-specific failures to the affected node instead of restarting every component.

7. **Check inotify exhaustion when logs contain `too many open files` or inotify initialization errors.** Distinguish process file-descriptor limits from Linux inotify limits. Read the host values and, for containerized k3d nodes, confirm the effective values visible to the node.

```sh
sysctl fs.inotify.max_user_instances fs.inotify.max_user_watches
```

Raising a sysctl is a host mutation. Before doing so, show the current value, propose a bounded value appropriate for the local development host, explain whether it survives reboot, and request authorization. After a change, restart only components that remain unhealthy and verify their rollout.

8. **Diagnose UDS Package or Keycloak reconciliation failure.** Inspect the Package custom resource conditions, generation, events, and controller logs. Confirm the Keycloak Pods, Service, EndpointSlices, and relevant network path are healthy. Compare failures across other UDS packages to distinguish an application-specific client request from a cluster-wide Keycloak or mesh problem.

Do not interpret a running application Pod as proof that package reconciliation succeeded. Record whether workloads, admission, identity configuration, and the Package readiness condition are each healthy.

9. **Propose recovery options and wait for approval.** Present concrete options for the earliest failed dependency, including the expected effect, risks, blast radius, rollback path, and verification approach. Recommend the smallest recovery. Examples include restoring an APIService backend, correcting a Service selector, raising an exhausted host limit, or restarting one unhealthy DaemonSet or Deployment after its root cause is addressed. Ask the user to select an option and explicitly approve it. Execute only the selected option after approval.

A cluster restart is not a substitute for root-cause analysis. Recommend it only for a disposable local cluster when evidence indicates stale runtime or networking state, the underlying cause has been corrected, and the user authorizes the interruption. Explain what state persists across the restart and what must be rechecked.

10. **Retry deployment only after prerequisites are healthy.** If package cleanup or namespace deletion is required, request authorization, use the deployment tool appropriate to the package, and wait with a finite timeout before redeploying. Do not begin a new deployment into a namespace that is still terminating.

**Verification**

1. Confirm the repaired UDS Core Pods and controllers are ready.
2. Confirm the affected Service has ready EndpointSlice addresses.
3. Confirm admission succeeds with a scoped server-side dry run or the least invasive equivalent when available.
4. Confirm the application namespace is `Active` before deployment.
5. Confirm application workloads are ready and the UDS Package condition reports successful reconciliation.
6. Review new events and bounded logs for recurrence of the original error.

Use finite timeouts for waits and rollouts. Report partial health explicitly. Workloads can be running while identity reconciliation or Package readiness is still failed.

## Error Handling

* If the context or namespace is ambiguous, stop and request the exact value. Do not guess.
* If credentials or role-based access control prevent a read-only check, report the denied operation and the minimum access needed. Do not work around access controls.
* If a mutation fails, stop repeating it. Re-read the affected resource, report its current state, and reassess the dependency chain.
* If logs or events are no longer available, state the evidence gap and collect current conditions. Do not present an inferred cause as confirmed.
* If recovery would weaken admission, delete persistent data, remove finalizers, or affect unrelated workloads, explain the blast radius and require explicit authorization.
* If verification does not pass, do not report recovery as complete. Identify the first failing verification step and the evidence needed next.
