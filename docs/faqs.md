## What is MCP?

* MCP (Model Context Protocol) 
  * == 💡STANDARD way / AI applications & agents can
    * connect -- to -- your data sources & tools
    * work -- with -- your data sources & tools💡
  * == adapter -- for -- AI applications
    * _Example:_ == USB-C -- for -- physical devices (use case) 
      * USB-C can connect devices -- to -- various peripherals and accessoriesSimilarly 
  * allows
    * 👀easily add connections -- to -- AI applications & agents👀
      * | BEFORE MCP,
        * you needed to build CUSTOM adapters / EACH data source OR tool 
      * | NOW (with MCP),
        * 1! adapter / ALL data source or tools
      * _Example:_ == USB-C -- for -- physical devices (use case)
        * BEFORE USB-C,
          * you needed DIFFERENT cables / EACH connection

* _Examples of 
  * data sources:_ local files, databases, or content repositories
  * tools:_ GitHub, Google Maps, or Puppeteer

## How does MCP work?

* MCP
  * == modular system /
    * you can add NEW capabilities -- WITHOUT -- changing AI applications 

![](images/mcp-simple-diagram.png)

- [**MCP servers**](docs/learn/server-concepts.md)
- **MCP clients**
  - run by AI applications (_Examples:_ Claude Desktop, ...)
  - allows
    - connect AI applications -- to -- MCP servers

## Why does MCP matter?

### | AI application users

* allows
  * AI can extend their knowledge area
    * ❌!= being limited to what it ALREADY knows❌

* MCP servers
  * 👀can provide MORE personalized & contextually relevant assistance👀
    * Reason: 🧠applications can access your 
      * Google Drive's personal documents
      * GitHub codebase🧠  

* _Example:_ AI assistant can
  - read -- connecting, through an MCP server, to your Google Drive -- meeting notes
  - understand who needs -- , based on the notes, -- follow-ups 
  - schedule AUTOMATICALLY -- connecting, through an MCP server, to your calendar -- meetings 

### | developers

* | build AI applications / need to access various data sources,
  * reduces development time & complexity 
  * developers can focus | building great AI experiences
    * ❌NOT focus | repeatedly creating custom connectors❌

## Who creates and maintains MCP servers?

* creators & maintainers
  - Anthropic's Developers
    - use cases
      - common tools & data sources
  - Open source contributors
    - use cases
      - tools / they use
  - Enterprise development teams
    - use cases
      - their internal systems
  - Software providers
    - make their applications AI-ready

* if the MCP Server is open source -> it can be used by any MCP-compatible AI application
  * == growing ecosystem of connections
* see
  * [_Example of MCP servers:_](examples.md)
  * [how to build your OWN MCP server](docs/develop/build-server.md)
