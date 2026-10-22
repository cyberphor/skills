---
name: fix-uds-core-k3d-deployment
description: Troubleshoots k3d-based deployments of Unified Defense Stack (UDS) Core within Windows Subsystem for Linux (WSL). Use when asked to troubleshoot k3d-based deployments of UDS Core within WSL.
compatibility: Requires access to WSL and sudo privileges as well as kubectl and uds to be installed. 
---

## Prerequisites

Check if the tools below are installed. If any of them are not installed, stop what you are doing, report which tools are missing, and do not continue.
  * `kubectl`
  * `uds`

## How to Troubleshoot a k3d-Based UDS Core Deployment within WSL

Do the following before investigating or considering other root causes for the problem reported by the user. If any of these checks did not help your investigation, leave them out of your response.
  * Check if they exhausted the number inotify instances they can open concurrently. 
  * Check if the `NodeHosts` data in the `coredns` ConfigMap includes `host.k3d.internal`.
  * Check if Istio's workload certificates have expired.
  * Check if there are any known issues for the the UDS Core component versions currently deployed. 
  * Check if the kernel driver Falco is configured to use is compatible with the WSL host's kernel.
  * Check if there are any known issues for the WSL version they are running. 
