---
name: fix-gitlab-pipeline
description: Investigates GitLab CI/CD pipeline failures. Use when asked to investigate GitLlab CI/CD pipeline failures. 
compatibility: Requires access to the internet, the GitLab CLI (glab), a GitLab API host (saved as an environment variable called GITLAB_API_HOST), and a GitLab Personal Access Token (saved as an environment variable called GITLAB_TOKEN) with at least the read_api permission in the repositories where the Work Items exist.
---

## Prerequisites

Check if the tools below are installed. If any of them are not installed, stop what you are doing, report which tools are missing, and do not continue.
  * GitLab CLI (`glab`)

## How to Fix a GitLab CI/CD Pipeline

Keep this workflow read-only with respect to GitLab Work Items and repositories. A local clone or fetch for inspection is allowed, but do not push commits, edit or close Work Items, add comments, change labels, or otherwise update remote state.

1. Check the current working directory and the user's home directory for a local clone of the repository. Be cognizant of the URL provided. For example, if the URL includes `gitlab.com/organization/repo`, look for `~/organization/repo`. If you can't find the repository, clone it using `glab`. 
2. Change your working directory to the repository.
3. Use `glab` to investigate the pipeline failure.
4. Generate a summary.

## Error Handling

If you encounter any GitLab authentication errors while following these steps, stop what you are doing, report the error, and do not continue.
