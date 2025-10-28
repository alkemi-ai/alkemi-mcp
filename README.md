# Alkemi MCP Server

Integrate your Alkemi Data, connected to Snowflake, Google BigQuery, DataBricks and other sources, with your MCP Client.

This is a STDIO wrapper for the Streamable HTTP MCP Endpoint:

```
https://api.alkemi.cloud/mcp
```

## Alkemi.ai

Querying databases requires knowledge about the schema of the tables and may require examples of the kinds of queries that can answer specific questions. Otherwise, you may be getting the wrong answers. Maintaining all that information in every agent or MCP Client that queries your database is a challenge and doesn't scale to teams looking to share data.

The Alkemi MCP Server uses Alkemi to store the database metadata, generate proper queries and actually query the database so you can share your MCP Server with teammates and everyone will have the same ability to query with quality.


## Installation

To add OpenAI to Claude Desktop, add the server config:

On MacOS: `~/Library/Application Support/Claude/claude_desktop_config.json`

On Windows: `%APPDATA%/Claude/claude_desktop_config.json`

### Env Vars

- `MCP_NAME`: The name of the MCP Server. This is optional. If you configure multiple, this is required so they do not have the same names in your MCP Client..
- `BEARER_TOKEN`: The Bearer token for the Streamable HTTP MCP Server. This is required for the STDIO MCP Integration.
- `PRODUCT_ID`: The ID of the Product if you want to narrow scope to just a single product. This is optional.


### Configuration

You can use it via `npx` in your Claude Desktop configuration like this:

```json
{
  "mcpServers": {
    "alkemi": {
      "command": "npx",
      "args": [
        "@alkemiai/alkemi-mcp"
      ],
      "env": {
        "BEARER_TOKEN": "sk-12345"
      }
    }
  }
}
```


Or, if you clone the repo, you can build and use in your Claude Desktop configuration like this:


```json

{
  "mcpServers": {
    "alkemi-data": {
      "command": "node",
      "args": [
        "/path/to/alkemi-mcp/build/index.js"
      ],
      "env": {
        "BEARER_TOKEN": "sk-12345"
      }
    }
  }
}
```

If you want to specify a specific product that the MCP Server should use, you can specify the `PRODUCT_ID` environment variable. And with setting the `MCP_NAME`, you can configure multiple.


```json

{
  "mcpServers": {
    "alkemi-customer-data": {
      "command": "node",
      "args": [
        "/path/to/alkemi-mcp/build/index.js"
      ],
      "env": {
        "MCP_NAME": "customer-data",
        "PRODUCT_ID": "123",
        "BEARER_TOKEN": "sk-12345"
      }
    },
    "alkemi-web-traffic-data": {
      "command": "node",
      "args": [
        "/path/to/alkemi-mcp/build/index.js"
      ],
      "env": {
        "MCP_NAME": "web-traffic-data",
        "PRODUCT_ID": "234",
        "BEARER_TOKEN": "sk-12345"
      }
    }
  }
}
```

## Development

Install dependencies:
```bash
npm install
```

Build the server:
```bash
npm run build
```

For development with auto-rebuild:
```bash
npm run watch
```

### Debugging

Since MCP servers communicate over stdio, debugging can be challenging. We recommend using the [MCP Inspector](https://github.com/modelcontextprotocol/inspector), which is available as a package script:

```bash
npm run inspector
```

The Inspector will provide a URL to access debugging tools in your browser.

### Acknowledgements

- Obviously the modelcontextprotocol and Anthropic teams for the MCP Specification and integration into Claude Desktop. [https://modelcontextprotocol.io/introduction](https://modelcontextprotocol.io/introduction)
