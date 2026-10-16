# FastMCP Service Entrypoint

Use these examples as generic references for the style. They intentionally avoid project-specific names, identifiers, endpoints, and domain details.


```python
# Standard library imports.
from os import environ
from pathlib import Path

# Third party imports.
from fastmcp import FastMCP
from fastmcp.server.providers.skills import SkillsDirectoryProvider
from starlette.requests import Request
from starlette.responses import JSONResponse


def main() -> None:
    """Start the MCP server."""
    if "FASTMCP_PORT" not in environ:
        raise RuntimeError("The FASTMCP_PORT environment variable is not set.")

    fastmcp_port = int(environ["FASTMCP_PORT"])

    # Init an MCP server.
    mcp = FastMCP(name="example")

    # Register skills with the MCP server.
    mcp.add_provider(
        SkillsDirectoryProvider(
            roots=Path(__file__).parent / "skills",
        )
    )

    @mcp.custom_route("/api/v1/health", methods=["GET"])
    async def health(request: Request) -> JSONResponse:
        """Respond to health checks.

        Returns:
            A JSON response that indicates the service is running.
        """
        return JSONResponse(content={"status": "ok"})

    # Start the MCP server.
    mcp.run(
        transport="streamable-http",
        host="0.0.0.0",
        port=fastmcp_port,
    )


if __name__ == "__main__":
    main()
```

Keep service startup linear and visible.

