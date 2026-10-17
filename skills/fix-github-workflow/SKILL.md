---
name: fix-github-workflow
description: Reads GitHub Actions workflow evidence and updates the affected repository with an approved fix. Use when asked to read or fix a failed GitHub Actions workflow run.
compatibility: Requires access to the internet, the GitHub CLI (gh), and a GitHub Personal Access Token (saved as an environment variable called GH_TOKEN) with at least Contents (Read and write), Pull requests (Read and write), and Workflows (Read and write) permissions in the repository where the workflow ran.
---

# How to Fix a GitHub Actions Workflow

## Prerequisites

Check if the tools below are installed. If any of them are not installed, stop what you are doing, report which tools are missing, and do not continue.
  * GitHub CLI (`gh`)
  * Make (`make`)

## Investigate the Failure

1. Locate the repository locally. Check your current working directory and the the user's home directory. Be cognizant of the URL provided. For example, if the URL includes `github.com/organization/repo`, look for `~/organization/repo`. If you can't find the repository, clone it using `gh`. 
2. Change your working directory to the repository.
3. Use `gh` to identify the most recent failed workflow run and inspect its logs.
4. Determine the root cause.
5. Check for an existing branch or pull request that addresses it. Inspect maintainer feedback when a pull request exists, but do not comment or change its branch.
6. Keep investigation read-only with respect to the repository and GitHub. Do not implement changes, create or switch branches, commit, push, comment, dispatch workflows, or create a pull request during this phase.

## Propose Remediation Options

Present concrete remediation options supported by the evidence. For each option, explain the files or workflow behavior it would change, expected benefit, risks, and verification approach. Recommend the smallest option that addresses the root cause and account for any maintainer feedback.

Ask the user to select an option and explicitly approve execution. Stop before any mutation until that approval is received. Analysis or a general request to resolve the failure does not authorize implementation, commits, pushes, comments, workflow dispatches, or pull request creation.

## Execute the Approved Option

After explicit approval, implement only the selected option. Use an existing branch or pull request when the user approved that route, otherwise create the approved branch. If maintainer feedback is unclear, report the ambiguity and request user direction before posting a GitHub comment or changing code.

**Verifying Changes**

1. Run the relevant Makefile targets to confirm the changes address the root cause and any requested feedback. If there isn't a Makefile, stop your analysis/response and report that the repository does not have a Makefile. 
2. If verification fails or cannot be completed, do not commit the changes. Stop your analysis/response and report the verification failure.
3. If verification succeeds, go to "Publishing Changes."

**Publishing Changes**

Publishing is a separate mutation and requires explicit user approval unless the selected option already included the exact publishing actions. After approval, commit and push the changes if requested. If an approved pull request already exists, comment with a summary of the changes and verification results only when approved. Otherwise, create a pull request only when approved, using the [PR Template](assets/template-pull-request.md) for its body.

## Error Handling

If you encounter any GitHub authentication errors while following these steps, stop your analysis/response and report them.
