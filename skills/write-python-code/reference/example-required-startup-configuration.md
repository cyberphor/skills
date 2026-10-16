# Required Startup Configuration

Use these examples as generic references for the style. They intentionally avoid project-specific names, identifiers, endpoints, and domain details.


```python
# Standard library imports.
from os import environ


def get_port() -> int:
    """Get the service port.

    Returns:
        The configured service port.

    Raises:
        RuntimeError: If the service port is not configured.
        ValueError: If the configured value is not an integer.
    """
    if "SERVICE_PORT" not in environ:
        raise RuntimeError("The SERVICE_PORT environment variable is not set.")

    return int(environ["SERVICE_PORT"])
```

Validate required startup values at the boundary instead of allowing a later, less obvious failure.

