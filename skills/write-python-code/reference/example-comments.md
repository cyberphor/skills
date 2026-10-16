# Comments

Use these examples as generic references for the style. They intentionally avoid project-specific names, identifiers, endpoints, and domain details.


Prefer:

```python
# Load the policy once so every registered tool uses the same rules.
policy = get_policy()
```

Avoid:

```python
# Get the policy.
policy = get_policy()
```

Comments should explain intent, lifecycle, or a non-obvious constraint.

