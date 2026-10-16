# Async HTTP Integration

Use these examples as generic references for the style. They intentionally avoid project-specific names, identifiers, endpoints, and domain details.


```python
# Third party imports.
from httpx import AsyncClient


async def get_coordinates(location: str) -> dict[str, float]:
    """Get coordinates for a location.

    Args:
        location: Human-readable location.

    Returns:
        Latitude and longitude values.
    """
    async with AsyncClient(timeout=10.0) as client:
        response = await client.get(
            "https://api.example.test/coordinates",
            params={"q": location},
        )
        response.raise_for_status()
        data = response.json()

    return {
        "latitude": float(data["latitude"]),
        "longitude": float(data["longitude"]),
    }
```

Keep request construction explicit, check the response status, and return a structured result.

