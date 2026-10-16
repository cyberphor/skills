# Explicit Loop for Branching Logic

Use these examples as generic references for the style. They intentionally avoid project-specific names, identifiers, endpoints, and domain details.


```python
def validate_records(records: list[dict[str, str]]) -> list[str]:
    """Return validation errors for the provided records."""
    errors: list[str] = []

    for index, record in enumerate(records):
        if not record.get("name"):
            errors.append(f"Record {index} is missing a name.")
            continue

        if not record.get("value"):
            errors.append(f"Record '{record['name']}' is missing a value.")

    return errors
```

Prefer an explicit loop once branching would make a comprehension difficult to scan.

