# FastAPI Lifespan Orchestration

Use these examples as generic references for the style. They intentionally avoid project-specific names, identifiers, endpoints, and domain details.


```python
# Standard library imports.
from contextlib import asynccontextmanager

# Third party imports.
from fastapi import FastAPI


api = FastAPI()


@asynccontextmanager
async def lifespan(app: FastAPI):
    client = await create_client()
    register_routes(app=app, client=client)
    yield
    await client.aclose()


api.router.lifespan_context = lifespan
```

Use lifespan boundaries for resources that should be constructed and cleaned up with the application.

