# MCPA 연습 문제 (davila7)

> 실제 시험 문항이 아니며 출제 내용을 보장하지 않는다. 리눅스 재단(Linux Foundation)과 AAIF의 공식 자료가 아니다.

[davila7/model_context_protocol_associate_exam_prep](https://github.com/davila7/model_context_protocol_associate_exam_prep)
커밋 `8e8e7da`(2026-08-01)에서 옮겼다. MIT 라이선스이고 원문 `LICENSE`를 같은 폴더에 두었다.
사이트 페이지마다 흩어진 퀴즈를 한 파일로 모았다. 문제와 해설은 원문 그대로다.

실제 시험 기반이 아니다. 공식 도메인 구성을 따라 만든 연습 문제다. 2026-07-28 스펙을
반영했다(handshake 제거, Sampling과 Roots 폐기 예정 등). 첫 절 20문항은 도메인 비중에 맞춘
모의고사이고, 나머지는 세부 항목별 복습 문제다.

## Practice Questions

원본: `practice/practice-questions.mdx`

### 1. What integration problem does MCP primarily solve?

A. Slow LLM inference\
B. The N×M problem of custom integrations between AI apps and external systems\
C. Lack of GPU availability\
D. Prompt token limits

<details><summary>정답</summary>

B. MCP standardizes how AI applications connect to external tools and data, so N apps and M systems need one shared protocol instead of N×M bespoke integrations. (Fundamentals)

</details>

### 2. Which MCP primitive is model-controlled?

A. Resources\
B. Prompts\
C. Tools\
D. Roots

<details><summary>정답</summary>

C. Tools are model-controlled: the LLM discovers and invokes them (with human-in-the-loop approval). Resources are application-driven and prompts are user-controlled. (Fundamentals)

</details>

### 3. Which three primitives do MCP servers expose?

A. Tools, resources, and prompts\
B. Tools, sampling, and roots\
C. Resources, sessions, and schemas\
D. Prompts, models, and agents

<details><summary>정답</summary>

A. Servers provide tools (functions for the model), resources (context/data), and prompts (templated messages/workflows). Sampling and roots are client-side primitives. (Fundamentals)

</details>

### 4. What is the correct cardinality between MCP clients and servers?

A. One client, many servers\
B. Each client connects to exactly one server (1:1); a host runs many clients\
C. Many-to-many\
D. One client per host maximum

<details><summary>정답</summary>

B. Every client maintains a dedicated 1:1 relationship with a single server. The host creates and manages one client instance per server it connects to. (Architecture)

</details>

### 5. Which transport would a remote MCP server most likely use?

A. stdio\
B. Streamable HTTP\
C. gRPC\
D. WebRTC only

<details><summary>정답</summary>

B. Streamable HTTP is the standard transport for remote servers. stdio is for local servers launched as subprocesses; gRPC and WebRTC are not standard MCP transports. (Architecture)

</details>

### 6. Can an MCP server read the host's full conversation or see data from other connected servers?

A. Yes, all context is shared by design\
B. Yes, but only after user consent\
C. No — the host enforces isolation; servers receive only the context sent to them\
D. Only servers using the stdio transport can

<details><summary>정답</summary>

C. A core design principle: servers cannot read the whole conversation or "see into" other servers. The host aggregates context and enforces isolation between clients. (Architecture)

</details>

### 7. A client sends tools/call with a tool name that the server does not expose. How does the server respond?

A. A result with isError: true\
B. A JSON-RPC protocol error (invalid params, e.g. -32602)\
C. A list_changed notification\
D. It ignores the request

<details><summary>정답</summary>

B. An unknown tool is a protocol error, returned as a JSON-RPC error object. isError: true is reserved for errors during the execution of a valid tool. (Interactions)

</details>

### 8. A tool's external API call times out during execution. What should the server return?

A. A JSON-RPC protocol error\
B. Nothing; it should close the connection\
C. A successful JSON-RPC response whose result has isError: true and an explanatory message\
D. An elicitation request

<details><summary>정답</summary>

C. Execution failures are reported in-band as tool results with isError: true, so the model can see the error and self-correct. Protocol errors are for malformed or invalid requests. (Interactions)

</details>

### 9. A server wants an LLM completion but has no API key. Which client primitive does it use?

A. Sampling\
B. Elicitation\
C. Prompts\
D. Roots

<details><summary>정답</summary>

A. Sampling (sampling/createMessage) lets the server request a completion through the client, which controls model access and permissions. Note: Sampling was deprecated in the 2026-07-28 revision — new implementations should integrate with LLM provider APIs directly. (Interactions)

</details>

### 10. How does a client learn that a server's tool list changed?

A. It must reconnect\
B. Via a notifications/tools/list_changed notification, then re-fetching tools/list\
C. Tool lists can never change\
D. Via the ping response

<details><summary>정답</summary>

B. The server emits notifications/tools/list_changed and the client re-fetches tools/list. In the 2026-07-28 revision, the client must first open a subscriptions/listen stream to receive change notifications. (Interactions)

</details>

### 11. Which primitive would a server use to ask the user for a missing parameter mid-operation?

A. Sampling\
B. Roots\
C. Elicitation\
D. Progress

<details><summary>정답</summary>

C. Elicitation lets a server request structured input from the user via the client — either an in-band form or, for sensitive data, an out-of-band URL flow. The user can accept, decline, or cancel. (Interactions)

</details>

### 12. Which participant is responsible for obtaining user consent before a tool call?

A. The server\
B. The LLM\
C. The host\
D. The registry

<details><summary>정답</summary>

C. The host is the policy enforcement point: it enforces security policies, obtains explicit user consent before invoking tools, and controls permissions. Servers never present consent UIs. (Security)

</details>

### 13. "Tool poisoning" refers to:

A. Rate-limiting a tool until it fails\
B. Malicious instructions hidden in tool descriptions that manipulate the model\
C. Sending malformed JSON to crash a server\
D. Stealing OAuth tokens

<details><summary>정답</summary>

B. Tool descriptions are injected into model context, so a malicious server can hide instructions there. The spec's Tool Safety principle: treat tool descriptions as untrusted unless they come from a trusted server. (Security)

</details>

### 14. Why must an MCP server reject tokens that were not issued specifically for it?

A. To reduce latency\
B. To prevent token passthrough and confused-deputy attacks\
C. Tokens for other services are always expired\
D. The JSON-RPC spec forbids them

<details><summary>정답</summary>

B. Token passthrough is an explicitly forbidden anti-pattern: servers MUST validate the token audience and MUST NOT accept or forward tokens issued for other services. Accepting them breaks audit trails and lets one compromised service unlock others. (Security)

</details>

### 15. A stdio-based server should write its logs to:

A. stdout\
B. stderr — never stdout\
C. The tools/call result\
D. The client's console

<details><summary>정답</summary>

B. stdout carries only valid MCP JSON-RPC messages; anything else corrupts the stream. Servers MAY write logs to stderr, which the client can capture, forward, or ignore. (Security / Observability)

</details>

### 16. How should a client treat tool annotations such as readOnlyHint and destructiveHint?

A. As guarantees enforced by the protocol\
B. As untrusted hints unless they come from a trusted server\
C. As mandatory fields every tool must declare\
D. As instructions for the LLM only

<details><summary>정답</summary>

B. Annotations are optional behavior hints. Clients MUST NOT rely on them for security decisions unless the server is trusted — a malicious server can label a destructive tool readOnlyHint: true. (Security)

</details>

### 17. MCP protocol versions are:

A. Semantic version numbers (1.2.3)\
B. Date-based strings (e.g. 2026-07-28) agreed between client and server\
C. Implicit and never communicated\
D. Tied to the model version

<details><summary>정답</summary>

B. Versions use YYYY-MM-DD strings, incremented only on backwards-incompatible changes. Older revisions negotiated the version during the initialize handshake; the stateless 2026-07-28 revision declares it per request in _meta. (Ecosystem)

</details>

### 18. Who governs the Model Context Protocol today?

A. Anthropic, as a proprietary standard\
B. The Agentic AI Foundation (AAIF) under the Linux Foundation, via an open maintainer structure\
C. A consortium of cloud vendors with paid membership\
D. The W3C

<details><summary>정답</summary>

B. Anthropic donated MCP to the Linux Foundation's Agentic AI Foundation in December 2025. Changes are proposed through SEPs (Specification Enhancement Proposals), and the spec is Apache 2.0 licensed. The AAIF also issues the MCPA certification. (Ecosystem)

</details>

### 19. What is the official MCP Registry?

A. A package manager that hosts and executes MCP server code\
B. A centralized metadata repository of publicly available MCP servers, described via server.json\
C. A private directory for enterprise-only servers\
D. A runtime that proxies all MCP traffic

<details><summary>정답</summary>

B. The registry stores metadata (not code) about public servers in standardized server.json files, with reverse-DNS names verified via GitHub/DNS/HTTP challenges. It is intended for downstream aggregators and is still in preview. (Ecosystem)

</details>

### 20. An organization starting MCP adoption should pilot with:

A. Write-capable automation across all teams\
B. Low-risk, read-only servers in a controlled host\
C. Unvetted community servers with full permissions\
D. Direct model-to-database connections

<details><summary>정답</summary>

B. Start with least-privilege, read-only integrations in a host you control, then expand scope as governance, auditing, and consent flows mature. (Use Cases)

</details>

## MCP Purpose & Scope

원본: `fundamentals/purpose-and-scope.mdx`

### 1. What integration problem does MCP solve?

A. Slow LLM inference times\
B. The N×M problem of building a custom integration for every app–data source pair\
C. The lack of a standard prompt-engineering format\
D. Incompatibility between different LLM vendors' model weights

<details><summary>정답</summary>

B. Without a standard, N applications each need bespoke integrations with M systems (N×M). MCP replaces this with one shared protocol, reducing the cost to roughly N+M.

</details>

### 2. Which of the following is IN scope for the MCP specification?

A. How the host renders its user interface\
B. Which AI model the host must use\
C. The JSON-RPC 2.0 message format between clients and servers\
D. The programming language servers must be written in

<details><summary>정답</summary>

C. MCP standardizes the interface — message format, primitives, negotiation, and transports. UI, model choice, and server implementation details are explicitly out of scope.

</details>

### 3. Is MCP tied to a specific AI model or vendor?

A. Yes, it only works with Claude\
B. Yes, it requires Anthropic-hosted models\
C. No, it is an open, model-agnostic standard\
D. No, but it only supports open-weight models

<details><summary>정답</summary>

C. MCP is open source and model-agnostic: any MCP-compatible host can use any LLM. Anthropic created it, but it is not tied to Claude.

</details>

### 4. Who governs MCP today?

A. Anthropic alone, as a proprietary protocol\
B. The Agentic AI Foundation (AAIF), part of the Linux Foundation, after Anthropic donated it in December 2025\
C. The W3C\
D. OpenAI, via the AGENTS.md working group

<details><summary>정답</summary>

B. Anthropic introduced MCP in November 2024 and donated it to the Linux Foundation's newly formed Agentic AI Foundation in December 2025.

</details>

### 5. Which pair correctly matches MCP primitives to the side that provides them?

A. Tools and prompts are client primitives; elicitation is a server primitive\
B. Tools, resources, and prompts are server primitives; elicitation is a client primitive\
C. Resources are a transport; tools are a message format\
D. All primitives are provided by the host application

<details><summary>정답</summary>

B. Servers expose tools, resources, and prompts; clients provide capabilities such as elicitation (and, in earlier spec revisions, sampling and roots).

</details>

## Core MCP Concepts

원본: `fundamentals/core-concepts.mdx`

### 1. Which participant maintains a 1:1 relationship with exactly one server?

A. The host\
B. The client\
C. The LLM\
D. The registry

<details><summary>정답</summary>

B. Each client connects to exactly one server. The host manages many clients, one per server connection.

</details>

### 2. Who controls when a Tool is invoked?

A. The user, by clicking it in the UI\
B. The host application\
C. The model (LLM), with human-in-the-loop approval\
D. The server

<details><summary>정답</summary>

C. Tools are model-controlled: the LLM discovers and invokes them, though a human should always be able to deny an invocation.

</details>

### 3. Which server-side primitive is application-controlled?

A. Tools\
B. Prompts\
C. Resources\
D. Elicitation

<details><summary>정답</summary>

C. Resources are application-driven: the host application decides how to incorporate them (UI pickers, search, automatic inclusion).

</details>

### 4. Which client-side primitive lets a server request an LLM completion through the client?

A. Roots\
B. Elicitation\
C. Prompts\
D. Sampling

<details><summary>정답</summary>

D. Sampling lets a server request generation via the client's model access — no server-side API key needed. Note it was deprecated in the 2026-07-28 revision.

</details>

### 5. What message format does MCP use at the data layer?

A. Protocol Buffers\
B. JSON-RPC 2.0\
C. GraphQL\
D. REST with JSON bodies

<details><summary>정답</summary>

B. All MCP messages are JSON-RPC 2.0: requests, responses, and notifications.

</details>

### 6. In the current (2026-07-28) spec revision, how does a client learn a server's capabilities?

A. Through the initialize handshake\
B. Via the server/discover request\
C. By reading the server's README\
D. Capabilities are no longer negotiated

<details><summary>정답</summary>

B. The 2026-07-28 revision removed the initialize handshake; servers advertise capabilities via server/discover, and clients send their own capabilities in per-request _meta fields.

</details>

## Interoperability & Value

원본: `fundamentals/interoperability-and-value.mdx`

### 1. What format do MCP protocol version identifiers use?

A. Semantic versioning (e.g. 1.2.0)\
B. Date-based strings (e.g. 2026-07-28)\
C. Incrementing integers (e.g. v3)\
D. Git commit hashes

<details><summary>정답</summary>

B. Versions are YYYY-MM-DD date strings, incremented only when a backwards-incompatible change is made.

</details>

### 2. In the 2026-07-28 revision, how does a client communicate its protocol version and capabilities?

A. Once per connection via the initialize handshake\
B. Through an out-of-band configuration file\
C. As _meta metadata attached to every request\
D. It never does; only servers declare capabilities

<details><summary>정답</summary>

C. MCP became stateless: there is no initialize handshake, so version, client info, and capabilities travel in _meta on each request.

</details>

### 3. What happens when a server does not support the protocol version a request declares?

A. It silently processes the request with its own version\
B. It returns UnsupportedProtocolVersionError (-32022) with the versions it supports\
C. It closes the transport permanently\
D. It downgrades the client automatically

<details><summary>정답</summary>

B. The server rejects the request with JSON-RPC error -32022, including data.supported; the client should retry with a mutually supported version.

</details>

### 4. What is the primary source of MCP's interoperability?

A. A single shared reference implementation everyone must use\
B. A standardized protocol that any implementation can follow\
C. A mandatory certification program for servers\
D. All servers being written in TypeScript

<details><summary>정답</summary>

B. The protocol is what is standardized, not the implementation — compliant hosts and servers interoperate regardless of language or SDK.

</details>

### 5. What value does MCP offer a tool/API provider compared to building per-app plugins?

A. It removes the need for any authentication\
B. One MCP server integration reaches every MCP-enabled application\
C. It guarantees a listing in every app marketplace\
D. It makes the API free to operate

<details><summary>정답</summary>

B. Build once, integrate everywhere: a single compliant server works with any compliant host, replacing N per-app integrations.

</details>

## Schemas & Structured Data

원본: `architecture/schemas-and-structured-data.mdx`

### 1. What field distinguishes a JSON-RPC request from a notification?

A. The method field\
B. The id field\
C. The params field\
D. The jsonrpc field

<details><summary>정답</summary>

B. Requests carry an id and expect a correlated response; notifications have no id and never receive a response.

</details>

### 2. Which tool definition field is required and describes valid arguments?

A. outputSchema\
B. annotations\
C. inputSchema\
D. icons

<details><summary>정답</summary>

C. Every tool must declare an inputSchema (a JSON Schema object); outputSchema, annotations, and icons are optional.

</details>

### 3. What JSON Schema dialect do MCP tool schemas use by default?

A. Draft-04\
B. Draft-07\
C. 2019-09\
D. 2020-12

<details><summary>정답</summary>

D. MCP tool schemas default to the JSON Schema 2020-12 dialect.

</details>

### 4. How are MCP resources uniquely identified?

A. By a numeric index\
B. By a URI (RFC 3986)\
C. By their mimeType\
D. By a UUID assigned by the client

<details><summary>정답</summary>

B. Each resource has a unique URI following RFC 3986; custom schemes are allowed.

</details>

### 5. How should a client treat tool annotations like readOnlyHint?

A. As guaranteed by the protocol\
B. As untrusted unless the server is trusted\
C. As enforced by the model\
D. As mandatory validation rules

<details><summary>정답</summary>

B. Annotations are hints; clients must treat them as untrusted unless they come from a trusted server.

</details>

## Hosts, Clients & Servers

원본: `architecture/hosts-clients-servers.mdx`

### 1. Which participant enforces user consent and security policy?

A. The server\
B. The client\
C. The host\
D. The transport layer

<details><summary>정답</summary>

C. The host is the AI application that enforces security policies, permissions, and user consent. Servers never present consent UIs.

</details>

### 2. What is the relationship between a client and a server?

A. One client can connect to many servers\
B. Each client has a 1:1 connection with exactly one server\
C. Servers connect to multiple clients within one host\
D. Clients and servers share a single global connection

<details><summary>정답</summary>

B. Each client maintains a dedicated 1:1 relationship with one server; the host creates one client per server it connects to.

</details>

### 3. Which transport would a locally installed filesystem server typically use?

A. Streamable HTTP\
B. WebSockets\
C. stdio\
D. gRPC

<details><summary>정답</summary>

C. Local servers run as subprocesses of the client and exchange newline-delimited JSON-RPC messages over stdin/stdout.

</details>

### 4. On the stdio transport, where should a server write its log output?

A. stdout\
B. stderr\
C. A log file required by the spec\
D. It must not log at all

<details><summary>정답</summary>

B. stdout is reserved exclusively for valid MCP messages; the server may write UTF-8 log output to stderr.

</details>

### 5. Can a server read the host's full conversation history?

A. Yes, servers receive the whole conversation for context\
B. Only if the server declares a special capability\
C. No — the host isolates servers, which see only what is sent to them\
D. Only over the stdio transport

<details><summary>정답</summary>

C. Isolation is a core design principle: servers cannot read the whole conversation or see into other servers; the host enforces this boundary.

</details>

## Model Interaction Flow

원본: `architecture/model-interaction-flow.mdx`

### 1. In a typical tool-using flow, when does user consent happen?

A. Before discovery, when the server is first listed\
B. After the model requests a tool but before the client calls the server\
C. After the server returns the tool result\
D. Consent is handled by the server during execution

<details><summary>정답</summary>

B. The host checks policy or asks the user for approval after the model emits a tool-use request and before the client sends tools/call.

</details>

### 2. Who converts MCP tool definitions into the model's native tool-calling format?

A. The server\
B. The LLM itself\
C. The host\
D. The registry

<details><summary>정답</summary>

C. The host assembles context and translates MCP tool definitions into whatever tool-call format the specific LLM expects.

</details>

### 3. How does a client discover a server's capabilities in the 2026-07-28 revision?

A. By sending an initialize request and waiting for InitializeResult\
B. By calling server/discover, then listing tools, resources, and prompts\
C. By reading the server's manifest from the MCP Registry\
D. Capabilities are pushed automatically over an SSE stream

<details><summary>정답</summary>

B. The initialize handshake was removed in 2026-07-28; servers advertise versions and capabilities via the server/discover RPC, and clients then call tools/list, resources/list, and prompts/list.

</details>

### 4. What must a client do in the current spec to receive notifications/tools/list_changed?

A. Nothing — servers can send it at any time on any connection\
B. Open a subscriptions/listen stream and opt into that notification type\
C. Call tools/list with a subscribe flag\
D. Poll server/discover every few seconds

<details><summary>정답</summary>

B. In 2026-07-28, change notifications flow over a long-lived subscriptions/listen stream that the client opens and opts into.

</details>

### 5. Can the LLM call an MCP server directly?

A. Yes, once the tool is approved\
B. Yes, but only over stdio\
C. No — the host/client mediates every call; the model only sees names, descriptions, and schemas\
D. Only for read-only tools

<details><summary>정답</summary>

C. The model never talks to servers. It emits a tool-use request; the host handles consent and the client sends tools/call.

</details>

### 6. How do server-initiated needs like sampling reach the client in 2026-07-28?

A. As server-initiated JSON-RPC requests over the same connection\
B. Inside an InputRequiredResult returned in a reply (the MRTR pattern)\
C. Via a webhook the client registers during discovery\
D. They cannot — servers have no way to request client input

<details><summary>정답</summary>

B. Servers no longer initiate requests; sampling/createMessage, elicitation/create, and roots/list are delivered inside InputRequiredResult, and the client retries the original request with the responses. Sampling itself is deprecated in this revision.

</details>

## Interaction Patterns & Response Handling

원본: `interactions/interaction-patterns.mdx`

### 1. In the 2026-07-28 revision, how does a server request LLM sampling or user input from the client?

A. It sends a server-initiated JSON-RPC request like sampling/createMessage\
B. It returns an InputRequiredResult inside its reply, and the client retries with the answers (MRTR)\
C. It opens a WebSocket back to the client\
D. It emits a notifications/input_required notification

<details><summary>정답</summary>

B. Server-initiated requests were removed in 2026-07-28; sampling, elicitation, and roots requests now travel inside InputRequiredResult via the Multi Round-Trip Requests pattern.

</details>

### 2. Which sequence describes the legacy (2025-11-25 and earlier) session handshake?

A. server/discover, then normal requests\
B. initialize request, initialize response, notifications/initialized\
C. ping, pong, notifications/ready\
D. subscriptions/listen followed by initialize

<details><summary>정답</summary>

B. Legacy revisions used the initialize request/response exchange followed by the notifications/initialized notification. The 2026-07-28 revision removed the handshake entirely.

</details>

### 3. How does a client learn that a server's tool list changed at runtime?

A. The server closes the connection to force a refresh\
B. The client must poll tools/list on a fixed schedule required by the spec\
C. The server sends notifications/tools/list_changed and the client re-fetches the list\
D. The server pushes the new tool definitions directly in the notification body

<details><summary>정답</summary>

C. Servers with the listChanged capability send notifications/tools/list_changed; the client then re-fetches with tools/list. In 2026-07-28 the client must have opened a subscriptions/listen stream to receive it.

</details>

### 4. What must a client do with a nextCursor value returned by a paginated list operation?

A. Parse it to compute the next page number\
B. Treat it as opaque and pass it back unchanged as the cursor parameter\
C. Increment it by the page size\
D. Ignore it — pagination is server-driven only

<details><summary>정답</summary>

B. Cursors are opaque tokens: clients MUST NOT parse or modify them, only echo them back in params.cursor to fetch the next page.

</details>

### 5. How does a client cancel an in-flight request over Streamable HTTP in 2026-07-28?

A. It sends notifications/cancelled on a new POST\
B. It sends a DELETE to the MCP endpoint\
C. It closes the SSE response stream for that request\
D. It sends ping with a cancel flag

<details><summary>정답</summary>

C. On Streamable HTTP, closing the request's SSE response stream is the cancellation signal; notifications/cancelled is used on stdio.

</details>

### 6. What happened to the ping utility in the 2026-07-28 revision?

A. It became mandatory for servers\
B. It was removed from the protocol\
C. It was renamed to health/check\
D. It now requires a progressToken

<details><summary>정답</summary>

B. ping (along with logging/setLevel) was removed in 2026-07-28. Through 2025-11-25, either side could send ping and the receiver had to respond with an empty result.

</details>

## Error Handling

원본: `interactions/error-handling.mdx`

### 1. A tool call references a tool name that doesn't exist on the server. How is this reported?

A. A result with isError: true so the model can retry\
B. A JSON-RPC error object (Invalid params, -32602)\
C. HTTP status 404 with no body\
D. A notifications/cancelled notification

<details><summary>정답</summary>

B. An unknown tool means the request itself cannot be fulfilled — a protocol error, reported as a JSON-RPC error object with code -32602 (Invalid params).

</details>

### 2. A weather API inside a tool times out. How should the server report this?

A. Return a JSON-RPC error with code -32603\
B. Close the connection so the client restarts the server\
C. Return a successful JSON-RPC response with isError: true and the error text in content\
D. Send a log notification and no response

<details><summary>정답</summary>

C. The tool ran but its operation failed — a tool execution error. It goes in a normal result with isError: true so the model can see it and self-correct.

</details>

### 3. Why are tool execution errors returned inside a successful JSON-RPC response?

A. Because JSON-RPC has no error object\
B. So the LLM can see the failure details and adjust its next attempt\
C. To avoid HTTP status codes other than 200\
D. Because clients ignore JSON-RPC error objects

<details><summary>정답</summary>

B. In-band errors reach the model, which can retry with different arguments or pick another tool. JSON-RPC protocol errors are handled by client code instead.

</details>

### 4. The model calls a tool with arguments that fail the tool's input schema validation. Per current spec guidance, this is a...

A. Protocol error with code -32700\
B. Protocol error with code -32601\
C. Tool execution error (isError: true) so the model can fix its arguments\
D. Reason to terminate the connection

<details><summary>정답</summary>

C. Since the 2025-11-25 revision, input-validation failures are tool execution errors, keeping the feedback visible to the model.

</details>

### 5. Which JSON-RPC code means "Method not found"?

A. -32700\
B. -32601\
C. -32602\
D. -32603

<details><summary>정답</summary>

B. -32700 is a parse error, -32601 is method not found, -32602 is invalid params, -32603 is an internal error.

</details>

### 6. A server does not support the protocol version a request declares. What does the current (2026-07-28) spec say it must return?

A. An isError: true result\
B. UnsupportedProtocolVersionError, JSON-RPC code -32022\
C. HTTP 500 with an empty body\
D. JSON-RPC code -32002

<details><summary>정답</summary>

B. The 2026-07-28 spec defines -32022 UnsupportedProtocolVersionError, with data listing the supported versions so the client can retry with one of them.

</details>

## Tool Invocation Lifecycle

원본: `interactions/tool-invocation-lifecycle.mdx`

### 1. In the tool invocation lifecycle, when does user consent/authorization happen?

A. After the server executes the tool but before the result is returned\
B. Before the tools/call request is sent to the server\
C. Only when the tool sets destructiveHint to true\
D. It is enforced by the server during execution

<details><summary>정답</summary>

B. The host applies policy and obtains consent before invocation — the call never reaches the server without authorization.

</details>

### 2. Which method does a client use to invoke a tool?

A. tools/invoke\
B. tools/run\
C. tools/call\
D. tools/execute

<details><summary>정답</summary>

C. tools/call takes the tool name and an arguments object; tools/list is used for discovery.

</details>

### 3. How does a client cancel an in-flight tool call over Streamable HTTP (2026-07-28)?

A. It sends notifications/cancelled on a new POST\
B. It closes the SSE response stream\
C. It sends a DELETE request to the MCP endpoint\
D. It waits for the server timeout

<details><summary>정답</summary>

B. On Streamable HTTP, closing the SSE response stream is the cancellation signal; notifications/cancelled is used on stdio.

</details>

### 4. What does structuredContent in a tool result contain?

A. A base64-encoded binary payload\
B. A JSON value conforming to the tool's outputSchema\
C. The JSON-RPC error object\
D. A list of resource URIs

<details><summary>정답</summary>

B. If the tool defines an outputSchema, structuredContent must conform to it; clients should validate the result against the schema.

</details>

### 5. Why can't a host rely on readOnlyHint for a security decision?

A. Because it defaults to true\
B. Because annotations are untrusted hints unless the server itself is trusted\
C. Because readOnlyHint only applies to resources\
D. Because the spec forbids hosts from reading annotations

<details><summary>정답</summary>

B. Annotations are self-reported by the server. A malicious server can claim readOnlyHint: true while performing writes, so hosts must treat them as UX/policy hints only.

</details>

### 6. A tool execution fails because an external API is down. What should the server return?

A. A JSON-RPC error with code -32603\
B. A normal result with isError: true and the error details in content\
C. An empty content array\
D. An UnsupportedProtocolVersionError

<details><summary>정답</summary>

B. Execution failures are reported in-band with isError: true so the model can see the error text and self-correct; JSON-RPC errors are reserved for protocol-level problems like an unknown tool.

</details>

## Protocol Primitives

원본: `interactions/protocol-primitives.mdx`

### 1. Which primitive lets a server ask the user for missing information mid-operation?

A. Sampling\
B. Roots\
C. Elicitation\
D. Prompts

<details><summary>정답</summary>

C. Elicitation (elicitation/create) requests input from the user via the client — in form mode or URL mode. The user can accept, decline, or cancel.

</details>

### 2. A client wants to read a document a server exposes. Which methods does it use?

A. tools/list then tools/call\
B. resources/list then resources/read\
C. prompts/list then prompts/get\
D. roots/list then resources/read

<details><summary>정답</summary>

B. Resources are discovered with resources/list and fetched by URI with resources/read.

</details>

### 3. Who controls whether a sampling request is fulfilled?

A. The server that sent it\
B. The client/host, with human-in-the-loop review\
C. The model provider\
D. The registry

<details><summary>정답</summary>

B. The client keeps control over model access, selection, and permissions; users should be able to review and approve sampling. Servers never hold the API keys.

</details>

### 4. Which pairing of primitive and control model is correct?

A. Tools — user-controlled\
B. Resources — model-controlled\
C. Prompts — user-controlled\
D. Prompts — application-controlled

<details><summary>정답</summary>

C. Tools are model-controlled, resources are application-driven, and prompts are user-controlled (e.g. slash commands).

</details>

### 5. In the 2026-07-28 revision, how does per-resource update subscription work?

A. resources/subscribe per URI\
B. A long-lived subscriptions/listen stream the client opts into\
C. Polling resources/list on a timer\
D. notifications/roots/list_changed

<details><summary>정답</summary>

B. resources/subscribe and resources/unsubscribe were removed in 2026-07-28; clients open subscriptions/listen and opt into resource updates and list-change notifications.

</details>

### 6. Which client-offered primitives were deprecated in the 2026-07-28 revision?

A. Elicitation and Sampling\
B. Roots and Elicitation\
C. Sampling and Roots\
D. None — all remain current

<details><summary>정답</summary>

C. Sampling and Roots were deprecated (SEP-2577); Elicitation remains a current client primitive.

</details>

## Trust Boundaries

원본: `security/trust-boundaries.mdx`

### 1. Why are tool descriptions considered untrusted input?

A. They are compressed and may decode incorrectly\
B. They come from third-party server code and can carry injected instructions\
C. The spec requires them to be empty\
D. They are generated by the LLM

<details><summary>정답</summary>

B. Tool descriptions and annotations are authored by the server. Unless the server is trusted, they must be treated as potentially adversarial content that could manipulate the model.

</details>

### 2. Which component is responsible for isolating one server's data from another?

A. The server itself\
B. The LLM\
C. The host\
D. The external API

<details><summary>정답</summary>

C. The host is the policy enforcement point: it maintains isolation between servers, gates consent, and controls permissions. Servers cannot see into other servers.

</details>

### 3. How should a tool call proposed by the LLM be treated?

A. As a command to execute immediately\
B. As a request subject to host policy and explicit user consent\
C. As trusted because it came from the model\
D. As a server-side error

<details><summary>정답</summary>

B. Model output is data, not commands. The host must obtain explicit user consent before invoking any tool.

</details>

### 4. What does the MCP spec say about authorization on stdio transports?

A. They MUST implement OAuth 2.1\
B. They SHOULD NOT use the OAuth authorization spec and should retrieve credentials from the environment\
C. They must use session cookies\
D. They require Dynamic Client Registration

<details><summary>정답</summary>

B. OAuth 2.1-based authorization applies to HTTP-based transports; stdio servers should instead get credentials from the environment (e.g., environment variables).

</details>

### 5. An MCP server receives a bearer token from its client and forwards it to a downstream API. What is this?

A. A recommended pattern for single sign-on\
B. Token passthrough — an explicitly forbidden anti-pattern\
C. Required by RFC 8707\
D. Only allowed for local servers

<details><summary>정답</summary>

B. Token passthrough breaks trust boundaries and audit trails. Servers MUST NOT accept tokens that were not explicitly issued for them; upstream calls need a separate token obtained by the server as its own OAuth client.

</details>

## Permissions & Consent

원본: `security/permissions-and-consent.mdx`

### 1. Which participant is responsible for obtaining user consent?

A. The MCP server\
B. The host application\
C. The authorization server\
D. The LLM

<details><summary>정답</summary>

B. The host presents consent UIs and enforces decisions. Servers never present consent prompts; the protocol itself cannot enforce consent.

</details>

### 2. In MCP authorization, which OAuth 2.1 role does the MCP server play?

A. Authorization server\
B. OAuth client\
C. Resource server\
D. Resource owner

<details><summary>정답</summary>

C. The MCP server is the resource server; the MCP client is the OAuth client acting on behalf of the resource owner, and a separate (or co-hosted) authorization server issues tokens.

</details>

### 3. Why must an MCP server reject a token that was issued for a different service?

A. Tokens for other services are always expired\
B. Accepting foreign tokens enables token passthrough, breaking audit trails and trust boundaries\
C. OAuth 2.1 forbids more than one token per user\
D. The token would be too large for the Authorization header

<details><summary>정답</summary>

B. Token passthrough is an explicitly forbidden anti-pattern: it circumvents rate limiting and validation, breaks accountability, and lets one compromised service unlock others. Servers must validate the token audience.

</details>

### 4. How does an MCP client ensure a token is audience-bound to a specific server?

A. By including the resource parameter (RFC 8707) with the server's canonical URI in authorization and token requests\
B. By hashing the token with the server name\
C. By registering the token in the MCP Registry\
D. By sending the token in the URL query string

<details><summary>정답</summary>

A. RFC 8707 Resource Indicators require the client to send a resource parameter identifying the MCP server; the server then validates it is the intended audience. Tokens must never appear in the query string.

</details>

### 5. How should a stdio-transported MCP server obtain credentials?

A. Via the OAuth 2.1 authorization flow\
B. From the environment — stdio transports SHOULD NOT use the HTTP authorization spec\
C. By eliciting the user's password through form-mode elicitation\
D. By reusing the host's tokens

<details><summary>정답</summary>

B. The authorization spec targets HTTP transports. Stdio servers retrieve credentials from the environment. Elicitation must never be used to collect passwords or API keys in form mode.

</details>

### 6. Which statement about the roots primitive and least privilege is accurate?

A. Roots cryptographically enforce filesystem boundaries\
B. Roots are mandatory for all servers\
C. Roots are informational guidance, not an enforced access control, and are deprecated in the 2026-07-28 revision\
D. Roots replace OAuth scopes for remote servers

<details><summary>정답</summary>

C. The protocol never enforced that servers stay within roots — they were guidance only — and the 2026-07-28 revision deprecates them in favor of passing paths via tool parameters or server configuration.

</details>

## Risk & Safety Controls

원본: `security/risk-and-safety-controls.mdx`

### 1. A server accepts a token that was issued for a different service and forwards it to a downstream API. What is this called?

A. Confused deputy\
B. Token passthrough\
C. Rug pull\
D. Session hijacking

<details><summary>정답</summary>

B. Token passthrough is an explicitly forbidden anti-pattern: MCP servers MUST NOT accept any tokens that were not explicitly issued for them.

</details>

### 2. Which setup is the target of the confused deputy attack described in the MCP security best practices?

A. A local stdio server with filesystem access\
B. An MCP proxy server using a static client ID with a third-party authorization server\
C. A client that stores refresh tokens in plaintext\
D. A registry hosting unvetted server packages

<details><summary>정답</summary>

B. The attack abuses the third-party auth server's consent cookie: after one legitimate consent, a maliciously registered client can get the consent screen skipped and steal the authorization code. Proxies MUST obtain per-client user consent.

</details>

### 3. A previously approved server silently changes its tool definitions to include malicious instructions. This is best described as:

A. Prompt injection\
B. SSRF\
C. A rug pull\
D. Token theft

<details><summary>정답</summary>

C. A rug pull is a post-approval change to tool definitions. The mitigation is re-prompting for consent when definitions change and pinning versions.

</details>

### 4. Under the stateless 2026-07-28 revision, what replaces the session hijacking guidance?

A. Nothing — the attack is no longer possible\
B. State-handle hijacking: handles must not be treated as authentication and should be bound to the verified user\
C. Mandatory Mcp-Session-Id headers\
D. SSE stream resumability rules

<details><summary>정답</summary>

B. With protocol-level sessions removed, the parallel rules apply to state handles: possession of a handle MUST NOT authenticate, and handles should be securely random and bound server-side to the authenticated user.

</details>

### 5. How can a malicious server trigger SSRF against a client?

A. By sending oversized tool results\
B. By pointing OAuth discovery URLs at internal or cloud-metadata addresses\
C. By registering a duplicate tool name\
D. By emitting log notifications too frequently

<details><summary>정답</summary>

B. Discovery metadata (resource_metadata, authorization server endpoints) can point at internal targets like 169.254.169.254. Clients should enforce HTTPS, block private and link-local IP ranges, and validate redirect targets.

</details>

### 6. Why are tool descriptions considered an attack surface (tool poisoning)?

A. They are executed as code by the host\
B. The model reads and follows them, and they are untrusted unless they come from a trusted server\
C. They contain OAuth tokens\
D. They bypass JSON-RPC validation

<details><summary>정답</summary>

B. The spec's Tool Safety principle flags tool descriptions and annotations as untrusted unless obtained from a trusted server; malicious text in them can steer the model.

</details>

## Auditability & Observability

원본: `security/auditability-and-observability.mdx`

### 1. Why must a stdio-based server avoid writing logs to stdout?

A. stdout is reserved for the client's messages\
B. Only valid MCP JSON-RPC messages may go to stdout; anything else corrupts the protocol stream\
C. stdout is not captured by most operating systems\
D. The spec requires all logs to be sent over HTTP

<details><summary>정답</summary>

B. The server MUST NOT write anything to stdout that is not a valid MCP message. Logs belong on stderr, which the client may capture, forward, or ignore.

</details>

### 2. Which standard defines the severity levels used by MCP's logging utility?

A. RFC 6570\
B. RFC 8707\
C. RFC 5424\
D. RFC 9728

<details><summary>정답</summary>

C. MCP log levels follow the RFC 5424 syslog severities: debug, info, notice, warning, error, critical, alert, emergency.

</details>

### 3. Which notification carries a server's log entries to the client?

A. notifications/progress\
B. notifications/message\
C. notifications/tools/list_changed\
D. notifications/cancelled

<details><summary>정답</summary>

B. Servers with the logging capability emit notifications/message, containing a required level, optional logger name, and arbitrary JSON data.

</details>

### 4. What does the MCP spec say about log message content?

A. Logs should include full request payloads for traceability\
B. Logs must not contain credentials, secrets, or personal information\
C. Logs must always be encrypted at rest\
D. Logs must be written in plain English

<details><summary>정답</summary>

B. Log messages MUST NOT contain credentials/secrets, PII, or internal details that could aid attacks, and servers should rate-limit emission.

</details>

### 5. Why does token passthrough undermine auditability?

A. It makes tokens expire faster\
B. Downstream audit logs record the wrong identity, breaking accountability\
C. It disables the logging capability\
D. It forces logs onto stdout

<details><summary>정답</summary>

B. A server forwarding tokens not issued to it makes downstream services attribute actions to the wrong party, which is one reason the spec forbids the practice.

</details>

### 6. Why is monitoring list_changed events an audit practice?

A. It measures server latency\
B. It detects rug-pull attacks where a server silently changes its tool definitions\
C. It is required to keep the connection alive\
D. It replaces consent logging

<details><summary>정답</summary>

B. Logging definition changes lets you spot a server that swaps a benign tool description for a malicious one after approval (a rug pull).

</details>

## Roles, Responsibilities & Adoption

원본: `use-cases/roles-responsibilities-adoption.mdx`

### 1. Who is responsible for validating tool inputs?

A. The host developer\
B. The server developer\
C. The end user\
D. The registry operator

<details><summary>정답</summary>

B. Servers own correct tool behavior, including validating the inputs they receive. Hosts own consent UX and policy enforcement.

</details>

### 2. Which role builds the consent UX that lets users approve or deny tool invocations?

A. Server developer\
B. Platform engineer\
C. Host/client developer\
D. Governance team

<details><summary>정답</summary>

C. The host application presents consent flows to the user; servers never present consent UIs.

</details>

### 3. What kind of server is recommended for an initial organizational pilot?

A. A write-capable automation server for maximum value\
B. A read-only, low-risk server in a controlled host\
C. A public community server with broad permissions\
D. A server that bypasses consent prompts to reduce friction

<details><summary>정답</summary>

B. Start with low-risk, read-only integrations; add write-capable tools later with human-in-the-loop controls.

</details>

### 4. Which organization governs MCP today?

A. It is solely controlled by Anthropic\
B. The Agentic AI Foundation (AAIF), part of the Linux Foundation\
C. The OpenAI Developer Consortium\
D. The W3C

<details><summary>정답</summary>

B. Anthropic donated MCP to the Linux Foundation's newly formed Agentic AI Foundation in December 2025.

</details>

### 5. What must a governance/compliance function maintain for safe MCP adoption?

A. A single shared API key for all servers\
B. An inventory of vetted, approved servers with audit requirements\
C. A copy of every model prompt\
D. Direct write access to all production servers

<details><summary>정답</summary>

B. A maintained inventory of approved servers, plus consent policies and audit logging, is the core governance control.

</details>

## Operational Use Cases

원본: `use-cases/operational-use-cases.mdx`

### 1. An assistant needs to search internal wikis and pull documents into context. Which primitives fit best?

A. Prompts + elicitation\
B. Resources + tools\
C. Roots + sampling\
D. Elicitation only

<details><summary>정답</summary>

B. Knowledge access is about feeding data as context (resources) plus search/query actions (tools).

</details>

### 2. A user wants to trigger a standardized code-review workflow from a slash command. Which primitive?

A. Tool\
B. Resource\
C. Prompt\
D. Elicitation

<details><summary>정답</summary>

C. Prompts are user-controlled, explicitly selected templated workflows — slash commands are the classic example.

</details>

### 3. Under the 2026-07-28 revision, how should a new server design scope filesystem access instead of using roots?

A. Use resources/subscribe on each directory\
B. Pass directories via tool parameters, resource URIs, or server configuration\
C. Request roots/list on every call\
D. Use sampling to ask the model which files are allowed

<details><summary>정답</summary>

B. Roots are deprecated in 2026-07-28; the migration path is tool parameters, resource URIs, or server config. Roots were informational anyway, never an enforcement mechanism.

</details>

### 4. When is Streamable HTTP the better transport choice over stdio?

A. When the server is a local subprocess accessing the user's files\
B. When the server is a shared remote service used by many clients, typically behind OAuth\
C. Whenever tools return large results\
D. Only when the server needs to write logs to stderr

<details><summary>정답</summary>

B. stdio suits local subprocess servers; Streamable HTTP suits remote/shared deployments with token-based authorization.

</details>

### 5. A server mid-task needs a piece of structured information from the user. Which primitive?

A. Sampling\
B. Prompt\
C. Elicitation\
D. Resource template

<details><summary>정답</summary>

C. Elicitation lets a server request user input through the client; it remains an active client feature in 2026-07-28.

</details>

## Ecosystem & Portability

원본: `use-cases/ecosystem-and-portability.mdx`

### 1. Why can a host written in Python use an MCP server written in TypeScript?

A. Both SDKs share a common runtime\
B. The wire protocol is the contract, so implementation language is irrelevant\
C. The registry transpiles servers between languages\
D. Hosts embed each server's interpreter

<details><summary>정답</summary>

B. MCP standardizes the JSON-RPC wire protocol; any compliant implementation interoperates regardless of language.

</details>

### 2. Which languages are in Tier 1 of the official MCP SDKs?

A. Java, Rust, Swift, Ruby\
B. TypeScript, Python, C#, Go\
C. TypeScript, Python, Java, Kotlin\
D. Python, Rust, PHP, Swift

<details><summary>정답</summary>

B. Tier 1 is TypeScript, Python, C#, and Go; Java and Rust are Tier 2; Swift, Ruby, PHP, and Kotlin are Tier 3.

</details>

### 3. What does the official MCP Registry actually store?

A. Executable server binaries, fully security-vetted\
B. Standardized metadata (server.json) about publicly accessible servers\
C. Private enterprise server catalogs\
D. Cached LLM responses for popular tools

<details><summary>정답</summary>

B. The registry is a metadata repository (in preview); security scanning is delegated to package registries and aggregators, and private servers are out of scope.

</details>

### 4. In the 2026-07-28 (modern) era, how is the protocol version communicated?

A. Once per connection during the initialize handshake\
B. In the _meta fields of every request, evaluated per request\
C. Through an environment variable read by the server\
D. Via the registry's server.json entry

<details><summary>정답</summary>

B. MCP became stateless in 2026-07-28: version, client info, and capabilities travel as per-request metadata, and servers accept or reject each request independently.

</details>

### 5. Who governs the Model Context Protocol today?

A. Anthropic alone, as the original author\
B. The Agentic AI Foundation under the Linux Foundation, with changes proposed via SEPs\
C. A consortium of package registries (npm, PyPI, Docker Hub)\
D. Each SDK team independently

<details><summary>정답</summary>

B. Anthropic donated MCP to the Linux Foundation's Agentic AI Foundation in December 2025; the project evolves through Specification Enhancement Proposals (SEPs).

</details>

## About the MCPA Exam

원본: `getting-started/about-the-exam.mdx`

### 1. How long do you have to complete the MCPA exam?

A. 60 minutes\
B. 90 minutes\
C. 120 minutes\
D. 180 minutes

<details><summary>정답</summary>

C. The MCPA is a 120-minute online, proctored, multiple-choice exam.

</details>

### 2. How long is the MCPA certification valid once earned?

A. 1 year\
B. 2 years\
C. 3 years\
D. It never expires

<details><summary>정답</summary>

B. The certification is valid for 2 years. Separately, you have 12 months from purchase to take the exam.

</details>

### 3. Which organization issues the MCPA certification?

A. Anthropic\
B. The Agentic AI Foundation (AAIF), part of the Linux Foundation\
C. OpenAI\
D. The Cloud Native Computing Foundation

<details><summary>정답</summary>

B. The MCPA is issued by the Agentic AI Foundation (AAIF), a Linux Foundation project that stewards MCP.

</details>

### 4. Which two domains together make up half of the exam weight?

A. MCP Fundamentals and Architecture & Components\
B. Use Cases & Ecosystem and MCP Fundamentals\
C. Interactions & Execution and Security & Governance\
D. Architecture & Components and Security & Governance

<details><summary>정답</summary>

C. Interactions & Execution (26%) plus Security & Governance (24%) equal 50% of the exam.

</details>

### 5. What does the exam purchase include if you do not pass on the first attempt?

A. Unlimited retakes for 12 months\
B. One retake\
C. No retakes; a new exam must be purchased\
D. Two retakes within 90 days

<details><summary>정답</summary>

B. One retake is included with the exam purchase, and there is a 12-month eligibility window from purchase.

</details>
