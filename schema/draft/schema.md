* goal
  * schema reference

## JSON-RPC

### `JSONRPCMessage`

Refers to any valid JSON-RPC object that can be decoded off the wire, or encoded to be sent.

### `JSONRPCRequest`

A request that expects a response.

### `JSONRPCNotification`

A notification which does not expect a response.

### `JSONRPCResultResponse`

A successful (non-error) response to a request.

### `JSONRPCErrorResponse`

A response to a request that indicates an error occurred.

### `JSONRPCResponse`

A response to a request, containing either the result or error.

{/* @category JSON-RPC */}

## Common Types

### `MetaObject`

Represents the contents of a `_meta` field, which clients and servers use to attach additional metadata to their interactions.
Certain key names are reserved by MCP for protocol-level metadata; implementations MUST NOT make assumptions about values at these keys. Additionally, specific schema definitions may reserve particular names for purpose-specific metadata, as declared in those definitions.
Valid keys have two segments:
**Prefix:**
- Optional — if specified, MUST be a series of _labels_ separated by dots (`.`), followed by a slash (`/`).
- Labels MUST start with a letter and end with a letter or digit. Interior characters may be letters, digits, or hyphens (`-`).
- Any prefix consisting of zero or more labels, followed by `modelcontextprotocol` or `mcp`, followed by any label, is **reserved** for MCP use. For example: `modelcontextprotocol.io/`, `mcp.dev/`, `api.modelcontextprotocol.org/`, and `tools.mcp.com/` are all reserved.
**Name:**
- Unless empty, MUST start and end with an alphanumeric character (`[a-z0-9A-Z]`).
- Interior characters may be alphanumeric, hyphens (`-`), underscores (`_`), or dots (`.`).

### `RequestMetaObject`

Extends {@link MetaObject} with additional request-specific fields. All key naming rules from `MetaObject` apply.

### `ProgressToken`

A progress token, used to associate progress notifications with the original request.

### `Cursor`

An opaque token used to represent a cursor for pagination.

### `RequestParams`

Common params for any request.

### `NotificationParams`

Common params for any notification.

### `Result`

Common result fields.

### `RequestId`

A uniquely identifying ID for a request in JSON-RPC.

### `EmptyResult`

A result that indicates success but carries no data.

### `Icon`

An optionally-sized icon that can be displayed in a user interface.

### `PaginatedRequestParams`

Common params for paginated requests.

### `Role`

The sender or recipient of messages and data in a conversation.

### `LoggingLevel`

The severity of a log message.
These map to syslog message severities, as specified in RFC-5424:
https://datatracker.ietf.org/doc/html/rfc5424#section-6.2.1

### `Annotations`

Optional annotations for the client. The client can use annotations to inform how objects are used or displayed

{/* @category Common Types */}

## Errors

### `ParseError`

A JSON-RPC error indicating that invalid JSON was received by the server. This error is returned when the server cannot parse the JSON text of a message.

### `InvalidRequestError`

A JSON-RPC error indicating that the request is not a valid request object. This error is returned when the message structure does not conform to the JSON-RPC 2.0 specification requirements for a request (e.g., missing required fields like `jsonrpc` or `method`, or using invalid types for these fields).

### `MethodNotFoundError`

A JSON-RPC error indicating that the requested method does not exist or is not available.
In MCP, this error is returned when a request is made for a method that requires a capability that has not been declared. This can occur in either direction:
- A server returning this error when the client requests a capability it doesn't support (e.g., requesting completions when the `completions` capability was not advertised)
- A client returning this error when the server requests a capability it doesn't support (e.g., requesting roots when the client did not declare the `roots` capability)

### `InvalidParamsError`

A JSON-RPC error indicating that the method parameters are invalid or malformed.
In MCP, this error is returned in various contexts when request parameters fail validation:
- **Tools**: Unknown tool name or invalid tool arguments
- **Prompts**: Unknown prompt name or missing required arguments
- **Pagination**: Invalid or expired cursor values
- **Logging**: Invalid log level
- **Tasks**: Invalid or nonexistent task ID, invalid cursor, or attempting to cancel a task already in a terminal status
- **Elicitation**: Server requests an elicitation mode not declared in client capabilities
- **Sampling**: Missing tool result or tool results mixed with other content

### `InternalError`

A JSON-RPC error indicating that an internal error occurred on the receiver. This error is returned when the receiver encounters an unexpected condition that prevents it from fulfilling the request.

{/* @category Errors */}

## Content

### `ResourceLink`

A resource that the server is capable of reading, included in a prompt or tool call result.
Note: resource links returned by tools are not guaranteed to appear in the results of {@link ListResourcesRequest | resources/list} requests.

### `EmbeddedResource`

The contents of a resource, embedded into a prompt or tool call result.
It is up to the client how best to render embedded resources for the benefit
of the LLM and/or the user.

### `TextContent`

Text provided to or from an LLM.

### `ImageContent`

An image provided to or from an LLM.

### `AudioContent`

Audio provided to or from an LLM.

{/* @category Content */}

## `completion/complete`

### `CompleteRequestParams`

Parameters for a `completion/complete` request.

### `CompleteRequest`

A request from the client to the server, to ask for completion options.

### `CompleteResult`

The result returned by the server for a {@link CompleteRequest | completion/complete} request.

### `CompleteResultResponse`

A successful response from the server for a {@link CompleteRequest | completion/complete} request.

### `ResourceTemplateReference`

A reference to a resource or resource template definition.

### `PromptReference`

Identifies a prompt.

{/* @category `completion/complete` */}

## `elicitation/create`

### `ElicitRequestFormParams`

The parameters for a request to elicit non-sensitive information from the user via a form in the client.

### `ElicitRequestURLParams`

The parameters for a request to elicit information from the user via a URL in the client.

### `ElicitRequestParams`

The parameters for a request to elicit additional information from the user via the client.

### `ElicitRequest`

A request from the server to elicit additional information from the user via the client.

### `PrimitiveSchemaDefinition`

Restricted schema definitions that only allow primitive types
without nested objects or arrays.

### `UntitledSingleSelectEnumSchema`

Schema for single-selection enumeration without display titles for options.

### `TitledSingleSelectEnumSchema`

Schema for single-selection enumeration with display titles for each option.

### `UntitledMultiSelectEnumSchema`

Schema for multiple-selection enumeration without display titles for options.

### `TitledMultiSelectEnumSchema`

Schema for multiple-selection enumeration with display titles for each option.

### `LegacyTitledEnumSchema`

Use {@link TitledSingleSelectEnumSchema} instead.
This interface will be removed in a future version.

### `ElicitResult`

The result returned by the client for an {@link ElicitRequest | elicitation/create} request.

### `ElicitResultResponse`

A successful response from the client for a {@link ElicitRequest | elicitation/create} request.

{/* @category `elicitation/create` */}

## `initialize`

### `InitializeRequestParams`

Parameters for an `initialize` request.

### `InitializeRequest`

This request is sent from the client to the server when it first connects, asking it to begin initialization.

### `InitializeResult`

The result returned by the server for an {@link InitializeRequest | initialize} request.

### `InitializeResultResponse`

A successful response from the server for a {@link InitializeRequest | initialize} request.

### `ClientCapabilities`

Capabilities a client may support. Known capabilities are defined here, in this schema, but this is not a closed set: any client can define its own, additional capabilities.

### `ServerCapabilities`

Capabilities that a server may support. Known capabilities are defined here, in this schema, but this is not a closed set: any server can define its own, additional capabilities.

### `Implementation`

Describes the MCP implementation.

{/* @category `initialize` */}

## `logging/setLevel`

### `SetLevelRequestParams`

Parameters for a `logging/setLevel` request.

### `SetLevelRequest`

A request from the client to the server, to enable or adjust logging.

### `SetLevelResultResponse`

A successful response from the server for a {@link SetLevelRequest | logging/setLevel} request.

{/* @category `logging/setLevel` */}

## `notifications/cancelled`

### `CancelledNotificationParams`

Parameters for a `notifications/cancelled` notification.

### `CancelledNotification`

This notification can be sent by either side to indicate that it is cancelling a previously-issued request.
The request SHOULD still be in-flight, but due to communication latency, it is always possible that this notification MAY arrive after the request has already finished.
This notification indicates that the result will be unused, so any associated processing SHOULD cease.
A client MUST NOT attempt to cancel its `initialize` request.
For task cancellation, use the {@link CancelTaskRequest | tasks/cancel} request instead of this notification.

{/* @category `notifications/cancelled` */}

## `notifications/initialized`

### `InitializedNotification`

This notification is sent from the client to the server after initialization has finished.

{/* @category `notifications/initialized` */}

## `notifications/tasks/status`

### `TaskStatusNotificationParams`

Parameters for a `notifications/tasks/status` notification.

### `TaskStatusNotification`

An optional notification from the receiver to the requestor, informing them that a task's status has changed. Receivers are not required to send these notifications.

{/* @category `notifications/tasks/status` */}

## `notifications/message`

### `LoggingMessageNotificationParams`

Parameters for a `notifications/message` notification.

### `LoggingMessageNotification`

JSONRPCNotification of a log message passed from server to client. If no `logging/setLevel` request has been sent from the client, the server MAY decide which messages to send automatically.

{/* @category `notifications/message` */}

## `notifications/progress`

### `ProgressNotificationParams`

Parameters for a {@link ProgressNotification | notifications/progress} notification.

### `ProgressNotification`

An out-of-band notification used to inform the receiver of a progress update for a long-running request.

{/* @category `notifications/progress` */}

## `notifications/prompts/list_changed`

### `PromptListChangedNotification`

An optional notification from the server to the client, informing it that the list of prompts it offers has changed. This may be issued by servers without any previous subscription from the client.

{/* @category `notifications/prompts/list_changed` */}

## `notifications/resources/list_changed`

### `ResourceListChangedNotification`

An optional notification from the server to the client, informing it that the list of resources it can read from has changed. This may be issued by servers without any previous subscription from the client.

{/* @category `notifications/resources/list_changed` */}

## `notifications/resources/updated`

### `ResourceUpdatedNotificationParams`

Parameters for a `notifications/resources/updated` notification.

### `ResourceUpdatedNotification`

A notification from the server to the client, informing it that a resource has changed and may need to be read again. This should only be sent if the client previously sent a {@link SubscribeRequest | resources/subscribe} request.

{/* @category `notifications/resources/updated` */}

## `notifications/roots/list_changed`

### `RootsListChangedNotification`

A notification from the client to the server, informing it that the list of roots has changed.
This notification should be sent whenever the client adds, removes, or modifies any root.
The server should then request an updated list of roots using the {@link ListRootsRequest}.

{/* @category `notifications/roots/list_changed` */}

## `notifications/tools/list_changed`

### `ToolListChangedNotification`

An optional notification from the server to the client, informing it that the list of tools it offers has changed. This may be issued by servers without any previous subscription from the client.

{/* @category `notifications/tools/list_changed` */}

## `notifications/elicitation/complete`

### `ElicitationCompleteNotification`

An optional notification from the server to the client, informing it of a completion of a out-of-band elicitation request.

{/* @category `notifications/elicitation/complete` */}

## `ping`

### `PingRequest`

A ping, issued by either the server or the client, to check that the other party is still alive. The receiver must promptly respond, or else may be disconnected.

### `PingResultResponse`

A successful response for a {@link PingRequest | ping} request.

{/* @category `ping` */}

## `tasks`

### `TaskStatus`

The status of a task.

### `TaskMetadata`

Metadata for augmenting a request with task execution.
Include this in the `task` field of the request parameters.

### `RelatedTaskMetadata`

Metadata for associating messages with a task.
Include this in the `_meta` field under the key `io.modelcontextprotocol/related-task`.

### `Task`

Data associated with a task.

### `CreateTaskResult`

The result returned for a task-augmented request.

### `CreateTaskResultResponse`

A successful response for a task-augmented request.

{/* @category `tasks` */}

## `tasks/get`

### `GetTaskRequest`

A request to retrieve the state of a task.

### `GetTaskResult`

The result returned for a {@link GetTaskRequest | tasks/get} request.

### `GetTaskResultResponse`

A successful response for a {@link GetTaskRequest | tasks/get} request.

{/* @category `tasks/get` */}

## `tasks/result`

### `GetTaskPayloadRequest`

A request to retrieve the result of a completed task.

### `GetTaskPayloadResult`

The result returned for a {@link GetTaskPayloadRequest | tasks/result} request.
The structure matches the result type of the original request.
For example, a {@link CallToolRequest | tools/call} task would return the {@link CallToolResult} structure.

### `GetTaskPayloadResultResponse`

A successful response for a {@link GetTaskPayloadRequest | tasks/result} request.

{/* @category `tasks/result` */}

## `tasks/list`

### `ListTasksRequest`

A request to retrieve a list of tasks.

### `ListTasksResult`

The result returned for a {@link ListTasksRequest | tasks/list} request.

### `ListTasksResultResponse`

A successful response for a {@link ListTasksRequest | tasks/list} request.

{/* @category `tasks/list` */}

## `tasks/cancel`

### `CancelTaskRequest`

A request to cancel a task.

### `CancelTaskResult`

The result returned for a {@link CancelTaskRequest | tasks/cancel} request.

### `CancelTaskResultResponse`

A successful response for a {@link CancelTaskRequest | tasks/cancel} request.

{/* @category `tasks/cancel` */}

## `prompts/get`

### `GetPromptRequestParams`

Parameters for a `prompts/get` request.

### `GetPromptRequest`

Used by the client to get a prompt provided by the server.

### `GetPromptResult`

The result returned by the server for a {@link GetPromptRequest | prompts/get} request.

### `GetPromptResultResponse`

A successful response from the server for a {@link GetPromptRequest | prompts/get} request.

### `PromptMessage`

Describes a message returned as part of a prompt.
This is similar to {@link SamplingMessage}, but also supports the embedding of
resources from the MCP server.

{/* @category `prompts/get` */}

## `prompts/list`

### `ListPromptsRequest`

Sent from the client to request a list of prompts and prompt templates the server has.

### `ListPromptsResult`

The result returned by the server for a {@link ListPromptsRequest | prompts/list} request.

### `ListPromptsResultResponse`

A successful response from the server for a {@link ListPromptsRequest | prompts/list} request.

### `Prompt`

A prompt or prompt template that the server offers.

### `PromptArgument`

Describes an argument that a prompt can accept.

{/* @category `prompts/list` */}

## `resources/list`

### `ListResourcesRequest`

Sent from the client to request a list of resources the server has.

### `ListResourcesResult`

The result returned by the server for a {@link ListResourcesRequest | resources/list} request.

### `ListResourcesResultResponse`

A successful response from the server for a {@link ListResourcesRequest | resources/list} request.

### `Resource`

A known resource that the server is capable of reading.

{/* @category `resources/list` */}

## `resources/read`

### `ReadResourceRequest`

Sent from the client to the server, to read a specific resource URI.

### `ReadResourceResult`

The result returned by the server for a {@link ReadResourceRequest | resources/read} request.

### `ReadResourceResultResponse`

A successful response from the server for a {@link ReadResourceRequest | resources/read} request.

{/* @category `resources/read` */}

## `resources/subscribe`

### `SubscribeRequest`

Sent from the client to request {@link ResourceUpdatedNotification | resources/updated} notifications from the server whenever a particular resource changes.

### `SubscribeResultResponse`

A successful response from the server for a {@link SubscribeRequest | resources/subscribe} request.

{/* @category `resources/subscribe` */}

## `resources/templates/list`

### `ListResourceTemplatesRequest`

Sent from the client to request a list of resource templates the server has.

### `ListResourceTemplatesResult`

The result returned by the server for a {@link ListResourceTemplatesRequest | resources/templates/list} request.

### `ListResourceTemplatesResultResponse`

A successful response from the server for a {@link ListResourceTemplatesRequest | resources/templates/list} request.

### `ResourceTemplate`

A template description for resources available on the server.

{/* @category `resources/templates/list` */}

## `resources/unsubscribe`

### `UnsubscribeRequest`

Sent from the client to request cancellation of {@link ResourceUpdatedNotification | resources/updated} notifications from the server. This should follow a previous {@link SubscribeRequest | resources/subscribe} request.

### `UnsubscribeResultResponse`

A successful response from the server for a {@link UnsubscribeRequest | resources/unsubscribe} request.

{/* @category `resources/unsubscribe` */}

## `roots/list`

### `ListRootsRequest`

Sent from the server to request a list of root URIs from the client. Roots allow
servers to ask for specific directories or files to operate on. A common example
for roots is providing a set of repositories or directories a server should operate
on.
This request is typically used when the server needs to understand the file system
structure or access specific locations that the client has permission to read from.

### `ListRootsResult`

The result returned by the client for a {@link ListRootsRequest | roots/list} request.
This result contains an array of {@link Root} objects, each representing a root directory
or file that the server can operate on.

### `ListRootsResultResponse`

A successful response from the client for a {@link ListRootsRequest | roots/list} request.

### `Root`

Represents a root directory or file that the server can operate on.

{/* @category `roots/list` */}

## `sampling/createMessage`

### `CreateMessageRequestParams`

Parameters for a `sampling/createMessage` request.

### `ToolChoice`

Controls tool selection behavior for sampling requests.

### `CreateMessageRequest`

A request from the server to sample an LLM via the client. The client has full discretion over which model to select. The client should also inform the user before beginning sampling, to allow them to inspect the request (human in the loop) and decide whether to approve it.

### `CreateMessageResult`

The result returned by the client for a {@link CreateMessageRequest | sampling/createMessage} request.
The client should inform the user before returning the sampled message, to allow them
to inspect the response (human in the loop) and decide whether to allow the server to see it.

### `CreateMessageResultResponse`

A successful response from the client for a {@link CreateMessageRequest | sampling/createMessage} request.

### `SamplingMessage`

Describes a message issued to or received from an LLM API.

### `ToolUseContent`

A request from the assistant to call a tool.

### `ToolResultContent`

The result of a tool use, provided by the user back to the assistant.

### `ModelPreferences`

The server's preferences for model selection, requested of the client during sampling.
Because LLMs can vary along multiple dimensions, choosing the "best" model is
rarely straightforward.  Different models excel in different areas—some are
faster but less capable, others are more capable but more expensive, and so
on. This interface allows servers to express their priorities across multiple
dimensions to help clients make an appropriate selection for their use case.
These preferences are always advisory. The client MAY ignore them. It is also
up to the client to decide how to interpret these preferences and how to
balance them against other considerations.

### `ModelHint`

Hints to use for model selection.
Keys not declared here are currently left unspecified by the spec and are up
to the client to interpret.

{/* @category `sampling/createMessage` */}

## `tools/call`

### `CallToolResult`

The result returned by the server for a {@link CallToolRequest | tools/call} request.

### `CallToolResultResponse`

A successful response from the server for a {@link CallToolRequest | tools/call} request.

### `CallToolRequestParams`

Parameters for a `tools/call` request.

### `CallToolRequest`

Used by the client to invoke a tool provided by the server.

{/* @category `tools/call` */}

## `tools/list`

### `ListToolsRequest`

Sent from the client to request a list of tools the server has.

### `ListToolsResult`

The result returned by the server for a {@link ListToolsRequest | tools/list} request.

### `ListToolsResultResponse`

A successful response from the server for a {@link ListToolsRequest | tools/list} request.

### `ToolAnnotations`

Additional properties describing a {@link Tool} to clients.
NOTE: all properties in `ToolAnnotations` are **hints**.
They are not guaranteed to provide a faithful description of
tool behavior (including descriptive properties like `title`).
Clients should never make tool use decisions based on `ToolAnnotations`
received from untrusted servers.

### `ToolExecution`

Execution-related properties for a tool.

### `Tool`

Definition for a tool the client can call.

{/* @category `tools/list` */}
