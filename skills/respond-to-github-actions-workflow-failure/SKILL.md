---
name: respond-to-github-actions-workflow-failure
description: Analyze and respond to a GitHub Actions workflow failure. Use when asked to analyze and/or respond to a GitHub Actions workflow failure.
compatibility: Requires access to the internet, the GitHub CLI (gh), and a GitHub Personal Access Token (saved as an environment variable called GH_TOKEN) with at least Contents (Read-only), Pull requests (Read and write), and Workflows (Read and write) permissions in the repository where the workflow ran.
---

## Setup

Check if tools below are installed. If any of them are not installed, stop your analysis/response and report which tools are missing.
  * GitHub CLI (`gh`)
  * Make (`make`)

## How to Triage a GitHub Actions Workflow Failure

1. Locate the repository locally. Check your current working directory and the the user's home directory. Be cognizant of the URL provided. For example, if the URL includes `github.com/organization/repo`, look for `~/organization/repo`. If you can't find the repository, clone it using `gh`. 
2. Change your working directory to the repository.
3. Use `gh` to identify the most recent failed workflow run and inspect its logs.
4. Determine the root cause.
5. Check for an existing branch or PR that addresses it:
  * **Branch without a PR:** Check out the branch, then go to "Verifying Changes."
  * **PR with maintainer feedback:** Go to "Responding to Feedback on a Pull Request."
  * **PR without maintainer feedback:** Comment requesting feedback, then stop.
  * **Neither:** Create a branch using `gh`, implement a fix, then go to "Verifying Changes."

### Responding to Feedback on a Pull Request

1. Check out the PR’s branch and review the maintainer feedback.
2. If the feedback is unclear, comment on the PR requesting clarification, then stop.
3. Implement the requested changes, then go to "Verifying Changes."

### Verifying Changes

1. Run the relevant Makefile targets to confirm the changes address the root cause and any requested feedback.
2. If verification fails or cannot be completed, do not commit the changes. Report the verification failure and then stop your analysis/response.
3. If verification succeeds, go to "Publishing Changes."

### Publishing Changes

1. Commit and push the changes, if any.
2. If a PR already exists, comment with a summary of the changes and verification results. Otherwise, use `gh` to create a PR using the [PR Template](assets/template-pull-request.md) for its body.

## Errors

If you encounter any GitHub authentication errors while following these steps, stop your analysis/response and report them.
