---
name: write-markdown-text
description: Writes or updates text in Markdown format. Use when asked to create or update Markdown content.
---

# How to Write Markdown Text

## Headers

Read the [complete Markdown document example](reference/example-complete-markdown-document.md) when a full document example would help.

* Use atx-style headers only. 
* Do not "close" headers with a trailing `#`. For example, use `## Header` instead of `## Header #`.
* Do not use header levels greater than 3. 
* Use header 1 (`#`) for the titles, header 2 (`##`) for sections, and header 3 (`###`) for subsections. For sections and subsections that are shorter than 3 lines, use `**` instead of header 2 (`###`). For example, use `**Section**` instead of `### Section`. 
* Capitalize the first letter of each word in headers, except for articles, conjunctions, and prepositions. For example, use `## How to Write Markdown` instead of `## How to write markdown`.

## Lists

* Use `*` for unordered lists (do not use `-` or `+`). 
* Use incrementing numbers for ordered lists. For example, use `1.`, `2.`, `3.` instead of `1.`, `1.`, `1.`. 
* For key-value pairs, use a colon (`:`) to separate the key and value. For example, use `Key: Value` instead of `Key - Value` or `Key = Value`.

## Links

* Use inline links instead of reference links. For example, use `[Link Text](https://example.com)` instead of `[Link Text][1]` or `[1]: https://example.com`.

## Tables

* Add spaces to column headers to make the width of a column match the width of the longest cell in that column. 

This is an example of a table with properly spaced headers. 
```markdown
| Header 1 | Header 2 |
|----------|----------|
| Cell 1   | Cell 2   |
```

This is an example of a table with improperly spaced headers. 
```markdown
| Header 1 | Header 2 |
|----------|----------|
| Cell 1 | Cell 2 | 
```
