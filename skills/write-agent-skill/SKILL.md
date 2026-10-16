---
name: write-agent-skill
description: Create or update agent skills. Use when asked to create or update an agent skill.
---

# How to Write an Agent Skill

## Frontmatter

* Use either `read`, `resolve`, or `write` as the skill name prefix.
* Use `read` for skills that involves reading and summarizing. 
* Use `resolve` for skills involve investigating and/or fixing.
* Use `write` for skills involve creating and/or updating. 
* Use create, read, and/or update in the skill description.
* Use the `compatibility` field to identify the tools, network access, credentials, and/or permissions required to execute the skill. 

## Body Content

* Start the body of the skill with a header that matches the skill name prefixed with "How to." For example, use `# How to Write Text in Markdown` for the `write-text-in-markdown` skill.
* If there is a specification relevant to the skill, use it to inform how the skill's body content is organized. For example, the sections in the `write-text-in-markdown` skill were chosen based on the sections in [the original specification for Markdown](https://daringfireball.net/projects/markdown/syntax). The sections in the `write-code-in-python` skill were chosen based on the sections in [the Python standard library reference manual](https://docs.python.org/3/library/index.html).

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

**Scripts**  

The `scripts` directory is for providing scripts agents will run to execute the skill. 

* Avoid including a `scripts` directory unless told otherwise. 

## Validation

* If installed, use the `skills-ref` tool to validate the skill's frontmatter. 
