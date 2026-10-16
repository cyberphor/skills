# Small Module with Configuration

Use these examples as generic references for the style. They intentionally avoid project-specific names, identifiers, endpoints, and domain details.


```python
# Standard library imports.
from os import environ
from pathlib import Path

# Constants.
DEFAULT_OUTPUT_DIRECTORY = Path("output")


def get_output_directory() -> Path:
    """Get the configured output directory."""
    return Path(environ.get("OUTPUT_DIRECTORY", DEFAULT_OUTPUT_DIRECTORY))
```

Use module-level constants for stable defaults. Keep configuration conversion explicit.

