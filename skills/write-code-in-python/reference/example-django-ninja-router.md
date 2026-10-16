# Django Ninja Router

Use these examples as generic references for the style. They intentionally avoid project-specific names, identifiers, endpoints, and domain details.


```python
# Third party imports.
from ninja import Router

# Local imports.
from records.models import Record
from records.schemas import NotFoundSchema, RecordSchema


record_router = Router(tags=["records"])


@record_router.get("/{record_id}", response={200: RecordSchema, 404: NotFoundSchema})
def get_record(request, record_id: int):
    try:
        return 200, Record.objects.get(id=record_id)
    except Record.DoesNotExist:
        return 404, {"message": "record not found"}
```

Use the framework's response conventions and keep endpoint behavior narrow.

