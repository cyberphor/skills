# Explicit Provider Dispatch

Use these examples as generic references for the style. They intentionally avoid project-specific names, identifiers, endpoints, and domain details.


```python
class ClientError(Exception):
    """Raised when a client cannot be created."""


def get_client(provider: str):
    """Create a client for the requested provider."""
    match provider:
        case "provider_a":
            return get_provider_a_client()
        case "provider_b":
            return get_provider_b_client()
        case _:
            raise ClientError(
                "Invalid provider. Expected one of: provider_a, provider_b."
            )
```

Use explicit dispatch when supported modes are few and well defined.

