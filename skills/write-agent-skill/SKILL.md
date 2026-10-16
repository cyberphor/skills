---
name: write-agent-skill
description: Creates or updates agent skills. Use when asked to create or update an agent skill.
---

# How to Write an Agent Skill

## Frontmatter

* Use either `fix`, `read`, or `write` as the skill name prefix.
* Use the `fix` prefix for skills that involve resolving a problem.
* Use the `read` prefix for skills that involves reading and summarizing text.
* Use the `write` prefix for skills involve creating and/or updating text.
* Segment skills names by putting the action first followed by increasingly unique elements. For example, use `write-python-django` instead of `write-django-python`.
* Avoid using words like `in`, `from`, `to`, `the`, `app`, and `failure` in the file name and frontmatter of skills (this does not apply when writing out the full name of the skill as a title).
* For the description field, use words that either describe the lifecycle of the object within a given skill (e.g., `create`, `update`, and `delete`) or actions that can be performed against it (e.g., `read`). These are two examples: `Use when asked to create Python code` and `Use when asked to read or summarize group chats`.
* Split descriptions into two sentences. Use the first sentence to describe, in present tense, what the skill does (e.g., `Writes Python code.`). Use the second sentence to describe when to invoke the skill (e.g., `Use when asked to read, create, or update Python code.`).
* Use the `compatibility` field to identify the tools, network access, credentials, and/or permissions required to execute the skill. 

## Body Content

* Start the body of the skill with a header that matches the skill name prefixed with "How to." For example, use `# How to Write Markdown Text` for the `write-markdown-text` skill.
* If there is a specification relevant to the skill, use it to inform how the skill's body content is organized. For example, the sections in the `write-markdown-text` skill were chosen based on the sections in [the original specification for Markdown](https://daringfireball.net/projects/markdown/syntax). The sections in the `write-python-code` skill were chosen based on the sections in [the Python standard library reference manual](https://docs.python.org/3/library/index.html).

## Progressive Disclosure

* Keep the `SKILL.md` file below 500 lines. 

## Optional Directories

**References**  

The `reference` directory is for providing examples or additional details. 

* When relevant, include a `reference` directory with at least one example of a topic mentioned in the skill.  
* Prefix files that contain an example with `example-`. 
* Use one file per example instead of one file for all examples. For example, if the skill involves writing code in Go, include one file for loops, one file for channels, etc. 

**Assets**  

The `assets` directory is for providing images, templates, configuration files, etc. that should be used to execute the skill. 

* When relevant, include an `assets` directory for templates mentioned in the skill. 
* Prefix files that contain a template with `template-`. 

**Scripts**  

The `scripts` directory is for providing scripts agents will run to execute the skill. 

* Avoid including a `scripts` directory unless told otherwise. 

## Validation

* If installed, use the `skills-ref` tool to validate the skill's frontmatter. 
