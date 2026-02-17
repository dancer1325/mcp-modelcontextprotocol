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

### [Current reference servers](https://github.com/modelcontextprotocol/servers?tab=readme-ov-file#-reference-servers)

### [servers / archived](https://github.com/modelcontextprotocol/servers-archived)

## [Official platform integrations](https://github.com/modelcontextprotocol/servers?tab=readme-ov-file#%EF%B8%8F-official-integrations)

## [Community implementations](https://github.com/modelcontextprotocol/servers?tab=readme-ov-file#-community-servers)

## Getting started

### how to use reference servers?

* if you are going to build
  * TypeScript-based servers -> `npx -y @modelcontextprotocol/server-memory`
  * Python-based servers -> use

    ```bash
    # 1. -- via -- uvx
    uvx mcp-server-git
    
    # 2. -- via -- pip
    pip install mcp-server-git
    python -m mcp_server_git
    ```

### how to configure -- with -- Claude?

* | your configuration,
  * add

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
