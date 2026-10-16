# Domain Parser

Use these examples as generic references for the style. They intentionally avoid project-specific names, identifiers, endpoints, and domain details.


```python
# Standard library imports.
from pathlib import Path
from typing import TypedDict

# Third party imports.
from defusedxml import ElementTree


class Record(TypedDict):
    identifier: str
    title: str


class RecordParseError(Exception):
    """Raised when a record file cannot be parsed."""


def read_records(path: str) -> list[Record]:
    """Read records from an XML file.

    Args:
        path: Path to the XML file.

    Returns:
        Records extracted from the document.

    Raises:
        RecordParseError: If the file is missing or malformed.
    """
    input_path = Path(path)

    if not input_path.is_file():
        raise RecordParseError(f"No such file: {path}")

    try:
        tree = ElementTree.parse(input_path)
    except ElementTree.ParseError as exc:
        raise RecordParseError(f"Malformed XML in {path}: {exc}") from exc

    records = [
        {
            "identifier": element.get("id", ""),
            "title": element.findtext("title", default="").strip(),
        }
        for element in tree.findall(".//record")
    ]

    if not records:
        raise RecordParseError(f"No records found in {path}.")

    return records
```

Use domain-specific exceptions when callers need to distinguish parsing failures from unrelated errors.

