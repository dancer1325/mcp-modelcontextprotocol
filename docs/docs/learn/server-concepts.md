* MCP servers
  * == programs /
    * expose (data sources & tools)' specific capabilities -- , through standardized protocol interfaces, to -- AI applications 
      * _Example of data sources & tools:_ Google Drive, Slack, Git, ...
  * _Examples:_ 
    * file system servers -- for -- document access
    * database servers -- for -- data queries
    * GitHub servers -- for -- code management
    * Slack servers -- for -- team communication
    * calendar servers -- for -- scheduling

## Core Server Features

| Feature       | Explanation                                                                                                                                                                                                                   | Use cases                                                            | Who controls it |
| ------------- |-------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|----------------------------------------------------------------------| --------------- |
| **Tools**     | Functions / your LLM <br/> &nbsp;&nbsp; can actively call <br/> &nbsp;&nbsp; decides when to use them -- based on -- user requests <br> _Examples:_ write \| databases, call external APIs, modify files, trigger other logic | Search flights <br/> Send messages <br/> Create calendar events      | Model           |
| **Resources** | Passive data sources / provide read-only access -- to -- information -- for -- context <br/> _Examples:_ file contents, database schemas, or API documentation                                                                | Retrieve documents <br/> Access knowledge bases <br/> Read calendars | Application     |
| **Prompts**   | Pre-built instruction templates / tell the model to work -- with -- specific tools and resources                                                                                                                              | Plan a vacation <br/> Summarize my meetings <br/> Draft an email     | User            |

### Tools

* enable 
  * AI models can perform actions
* specific operation / typed inputs and outputs
  * / EACH tool
* AI models
  * requests tool execution -- based on -- context

#### How Tools Work

Tools are schema-defined interfaces that LLMs can invoke
* MCP uses JSON Schema for validation
* Each tool performs a single operation with clearly defined inputs and outputs
* Tools may require user consent prior to execution, helping to ensure users maintain control over actions taken by a model.

**Protocol operations:**

| Method       | Purpose                  | Returns                                |
| ------------ | ------------------------ | -------------------------------------- |
| `tools/list` | Discover available tools | Array of tool definitions with schemas |
| `tools/call` | Execute a specific tool  | Tool execution result                  |

**Example tool definition:**

```typescript
{
  name: "searchFlights",
  description: "Search for available flights",
  inputSchema: {
    type: "object",
    properties: {
      origin: { type: "string", description: "Departure city" },
      destination: { type: "string", description: "Arrival city" },
      date: { type: "string", format: "date", description: "Travel date" }
    },
    required: ["origin", "destination", "date"]
  }
}
```

#### User Interaction Model

Tools are model-controlled, meaning AI models can discover and invoke them automatically
* However, MCP emphasizes human oversight through several mechanisms.

For trust and safety, applications can implement user control through various mechanisms, such as:

- Displaying available tools in the UI, enabling users to define whether a tool should be made available in specific interactions
- Approval dialogs for individual tool executions
- Permission settings for pre-approving certain safe operations
- Activity logs that show all tool executions with their results

### Resources

Resources provide structured access to information that the AI application can retrieve and provide to models as context.

#### How Resources Work

Resources expose data from files, APIs, databases, or any other source that an AI needs to understand context
* Applications can access this information directly and decide how to use it - whether that's selecting relevant portions, searching with embeddings, or passing it all to the model.

Each resource has a unique URI (e.g., `file:///path/to/document.md`) and declares its MIME type for appropriate content handling.

Resources support two discovery patterns:

- **Direct Resources** - fixed URIs that point to specific data
* Example: `calendar://events/2024` - returns calendar availability for 2024
- **Resource Templates** - dynamic URIs with parameters for flexible queries
* Example:
  - `travel://activities/{city}/{category}` - returns activities by city and category
  - `travel://activities/barcelona/museums` - returns all museums in Barcelona

Resource Templates include metadata such as title, description, and expected MIME type, making them discoverable and self-documenting.

**Protocol operations:**

| Method                     | Purpose                         | Returns                                |
| -------------------------- | ------------------------------- | -------------------------------------- |
| `resources/list`           | List available direct resources | Array of resource descriptors          |
| `resources/templates/list` | Discover resource templates     | Array of resource template definitions |
| `resources/read`           | Retrieve resource contents      | Resource data with metadata            |
| `resources/subscribe`      | Monitor resource changes        | Subscription confirmation              |


#### Parameter Completion

Dynamic resources support parameter completion
* For example:

- Typing "Par" as input for `weather://forecast/{city}` might suggest "Paris" or "Park City"
- Typing "JFK" for `flights://search/{airport}` might suggest "JFK - John F
* Kennedy International"

The system helps discover valid values without requiring exact format knowledge.

#### User Interaction Model

Resources are application-driven, giving them flexibility in how they retrieve, process, and present available context
* Common interaction patterns include:

- Tree or list views for browsing resources in familiar folder-like structures
- Search and filter interfaces for finding specific resources
- Automatic context inclusion or smart suggestions based on heuristics or AI selection
- Manual or bulk selection interfaces for including single or multiple resources

Applications are free to implement resource discovery through any interface pattern that suits their needs
* The protocol doesn't mandate specific UI patterns, allowing for resource pickers with preview capabilities, smart suggestions based on current conversation context, bulk selection for including multiple resources, or integration with existing file browsers and data explorers.

### Prompts

Prompts provide reusable templates
* They allow MCP server authors to provide parameterized prompts for a domain, or showcase how to best use the MCP server.

#### How Prompts Work

Prompts are structured templates that define expected inputs and interaction patterns
* They are user-controlled, requiring explicit invocation rather than automatic triggering
* Prompts can be context-aware, referencing available resources and tools to create comprehensive workflows
* Similar to resources, prompts support parameter completion to help users discover valid argument values.

**Protocol operations:**

| Method         | Purpose                    | Returns                               |
| -------------- | -------------------------- | ------------------------------------- |
| `prompts/list` | Discover available prompts | Array of prompt descriptors           |
| `prompts/get`  | Retrieve prompt details    | Full prompt definition with arguments |

#### User Interaction Model

Prompts are user-controlled, requiring explicit invocation
* The protocol gives implementers freedom to design interfaces that feel natural within their application
* Key principles include:

- Easy discovery of available prompts
- Clear descriptions of what each prompt does
- Natural argument input with validation
- Transparent display of the prompt's underlying template

Applications typically expose prompts through various UI patterns such as:

- Slash commands (typing "/" to see available prompts like /plan-vacation)
- Command palettes for searchable access
- Dedicated UI buttons for frequently used prompts
- Context menus that suggest relevant prompts

## MULTIPLE Servers working together

* == 👀real power of MCP👀
  * Reason: 🧠combine servers' specialized capabilities -- through an -- unified interface🧠
