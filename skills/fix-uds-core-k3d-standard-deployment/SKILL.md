---
name: fix-uds-core-k3d-standard-deployment
description: Troubleshoots k3d-based Unified Defense Stack (UDS) Core deployments within the Windows Subsystem for Linux (WSL). Use when asked to troubleshoot k3d-based UDS Core deployments within WSL.
compatibility: Requires access to WSL, sudo privileges, and the following tools to be installed: kubectl and uds. 
---

## Prerequisites

Check if the tools below are installed. If any of them are not installed, stop what you are doing, report which tools are missing, and do not continue.
  * `kubectl`
  * `uds`

## How to Troubleshoot a k3d-Based UDS Core Deployment within WSL

Do the following first before investigating or considering other root causes for the problem reported by the user. 
  * Check if they exhausted the number inotify instances they can open concurrently. 
  * Check if the `NodeHosts` data in the `coredns` ConfigMap includes `host.k3d.internal`.
  * Compare the version of WSL they are running against the latest available version. 
