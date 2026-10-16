# Avoid Unnecessary Abstraction

Use these examples as generic references for the style. They intentionally avoid project-specific names, identifiers, endpoints, and domain details.


Prefer:

```python
def build_message(name: str) -> str:
    return f"Hello, {name}."
```

Over introducing a class, factory, interface, or dependency injection layer when there is only one behavior and no current requirement for extension.

