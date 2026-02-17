# Example MCP Servers

* goal
  * Model Context Protocol (MCP) servers /
    * demonstrate the protocol's capabilities & versatility
    * enable Large Language Models (LLMs) can securely access tools & data sources

## Reference implementations

* goal
  * demonstrate
    * core MCP features
    * SDK usage

### Current reference servers

- **[Everything](https://github.com/modelcontextprotocol/servers/tree/main/src/everything)** 
  - Reference / test server with prompts, resources, and tools
- **[Fetch](https://github.com/modelcontextprotocol/servers/tree/main/src/fetch)**
  - Web content fetching and conversion for efficient LLM usage
- **[Filesystem](https://github.com/modelcontextprotocol/servers/tree/main/src/filesystem)**
  - Secure file operations with configurable access controls
- **[Git](https://github.com/modelcontextprotocol/servers/tree/main/src/git)** 
  - Tools to read, search, and manipulate Git repositories
- **[Memory](https://github.com/modelcontextprotocol/servers/tree/main/src/memory)**
  - Knowledge graph-based persistent memory system
- **[Sequential Thinking](https://github.com/modelcontextprotocol/servers/tree/main/src/sequentialthinking)**
  - Dynamic and reflective problem-solving through thought sequences
- **[Time](https://github.com/modelcontextprotocol/servers/tree/main/src/time)**
  - Time and timezone conversion capabilities

### Additional example servers (archived)

Visit the [servers-archived repository](https://github.com/modelcontextprotocol/servers-archived) to get access to archived example servers that are no longer actively maintained.

They are provided for historical reference only.

## Official platform integrations

* [here](https://github.com/modelcontextprotocol/servers?tab=readme-ov-file#%EF%B8%8F-official-integrations)

## Community implementations

Visit the [MCP Servers Repository (Community section)](https://github.com/modelcontextprotocol/servers?tab=readme-ov-file#-community-servers) for a list of MCP servers maintained by community members.

## Getting started

### Using reference servers

TypeScript-based servers can be used directly with `npx`:

```bash
npx -y @modelcontextprotocol/server-memory
```

Python-based servers can be used with `uvx` (recommended) or `pip`:

```bash
# Using uvx
uvx mcp-server-git

# Using pip
pip install mcp-server-git
python -m mcp_server_git
```

### Configuring with Claude

To use an MCP server with Claude, add it to your configuration:

```json
{
  "mcpServers": {
    "memory": {
      "command": "npx",
      "args": ["-y", "@modelcontextprotocol/server-memory"]
    },
    "filesystem": {
      "command": "npx",
      "args": [
        "-y",
        "@modelcontextprotocol/server-filesystem",
        "/path/to/allowed/files"
      ]
    },
    "github": {
      "command": "npx",
      "args": ["-y", "@modelcontextprotocol/server-github"],
      "env": {
        "GITHUB_PERSONAL_ACCESS_TOKEN": "<YOUR_TOKEN>"
      }
    }
  }
}
```

## Additional resources

Visit the [MCP Servers Repository (Resources section)](https://github.com/modelcontextprotocol/servers?tab=readme-ov-file#-resources) for a collection of other resources and projects related to MCP.

Visit our [GitHub Discussions](https://github.com/orgs/modelcontextprotocol/discussions) to engage with the MCP community.
