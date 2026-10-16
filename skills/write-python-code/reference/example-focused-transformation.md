# Focused Transformation

Use these examples as generic references for the style. They intentionally avoid project-specific names, identifiers, endpoints, and domain details.


```python
def normalize_names(values: list[str]) -> list[str]:
    """Normalize and sort non-empty names."""
    return sorted(
        value.strip()
        for value in values
        if value.strip()
    )
```

A short comprehension is appropriate when the transformation remains obvious.

