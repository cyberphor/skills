---
name: fix-uds-core-k8s-workload
description: Troubleshoots Kubernetes workloads that integrate with UDS Core. Use when asked assess or troubleshoot Kubernetes workloads that integrate with UDS Core.
compatibility: Requires network access to the  
---

## Prerequisites

Check if the tools below are installed. If any of them are not installed, stop what you are doing, report which tools are missing, and do not continue.
  * `uds`

## Troubleshoot a Kubernetes Workload Integrated with UDS Core

Do these checks before you investigate other possible causes. If you cannot complete a check, record the reason. Include only results relevant to the problem or the next action in your response.

  * Read the Zarf package definition.
  * Review the Zarf component order for deployment dependencies.
  * Confirm the Helm charts belong to the intended application or its dependencies.
  * Read the UDS Package definition.
  * Check the status of the application's pods.
  * Examine recent events for these pods.
  * Read recent logs from each container, including init containers.
  * If a container has restarted, read the previous container logs when available.
  * Confirm the presence of the required Zarf container images in the deployment registry.
  * Review the UDS Package for the required network access.
  * Compare the UDS Package service names, pod selectors, and ports with the deployed resources.
  * Test DNS resolution for each exposed application hostname from the affected client.
  * Compare the resolved IP addresses with the intended ingress IP addresses.
