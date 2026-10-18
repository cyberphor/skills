---
name: fix-uds-core-k3d-deployment
description: Troubleshoots k3d-based Unified Defense Stack (UDS) Core deployments within Windows Subsystem for Linux (WSL). Use when asked to troubleshoot k3d-based UDS Core deployments within WSL.
compatibility: Requires access to WSL and sudo privileges as well as kubectl and uds to be installed. 
---

## Prerequisites

Check if the tools below are installed. If any of them are not installed, stop what you are doing, report which tools are missing, and do not continue.
  * `kubectl`
  * `uds`

## How to Troubleshoot a k3d-Based UDS Core Deployment within WSL

Do the following before investigating or considering other root causes for the problem reported by the user. If any of these checks did not help your investigation, leave them out of your response.
  * Read their `zarf.yaml` file. 
  * Check the order of components listed in their `zarf.yaml` file. 
  * Check if the container images mentioned in their `zarf.yaml` file exist in the cluster's Zarf registry. 
  * Read the Helm charts listed in their `zarf.yaml` file.
  * Check if the Helm charts listed in their `zarf.yaml` file belong to the app they are trying to deploy.
  * Check the status of the pods declared in their Helm charts.
  * Check the logs of the pods declared in their Helm charts. 
  * Read their UDS package manifest.
  * Check if their UDS package facilitates the network communication their app requires. 
  * Check if their DNS configuration resolves the IP address of the host identified in their UDS package. 
  * Check if they exhausted the number inotify instances they can open concurrently. 
  * Check if the `NodeHosts` data in the `coredns` ConfigMap includes `host.k3d.internal`.
  * Compare the version of WSL they are running against the latest available version. 
