# Pytest Example

Use these examples as generic references for the style. They intentionally avoid project-specific names, identifiers, endpoints, and domain details.


```python
# Third party imports.
import pytest

# Local imports.
from example.parser import RecordParseError, read_records


def test_read_records_rejects_missing_file(tmp_path) -> None:
    missing_path = tmp_path / "missing.xml"

    with pytest.raises(RecordParseError, match="No such file"):
        read_records(str(missing_path))
```

Test the public contract and expected failure mode rather than internal implementation details.

