---
name: write-rust
description: Writes or updates Rust code in the user's preferred style. Use when asked to read, make, or update Rust code.
metadata:
  author: Victor Fernandez III
---

# How to Write Rust Code

* Init new Rust projects using Cargo. If Rust is not installed, stop what you are doing, report the issue, and do not continue.
* Error messages must describe what was expected.
* Put logic in lib.rs so it’s importable.
* Keep main.rs a thin wrapper.
* Don’t write tests unless the user asks for them.
* Always run `cargo fmt` and `cargo clippy` to validate the code-related changes you make.
* For variables that hold structs, use the full name of the struct name as the name of the variable. For example, for a struct named Request, the variable name would be `request` (not `req`).
* Start Boolean-returning functions with either `is_` or `has_`.
* Annotate the type on the let binding, right after the variable name (`let request: Request = serde_json::from_str(&input)`), rather than on the function call with the turbofish (`serde_json::from_str::<Request>(&input)`), so a reader can see what the variable holds without reading to the end of the line.
* Use descriptive names for functions and variables.
