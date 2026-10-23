# MCPA 모의고사 03: MCPA Full-Length Mock 3: Design and Security Trade-offs

> 실제 시험 문항이 아니며 출제 내용을 보장하지 않는다. 리눅스 재단(Linux Foundation)과 AAIF의 공식 자료가 아니다.

원본: rohitg00/ai-engineering-from-scratch `certifications/mcpa/assessments/mcpa-f/mock-03.json` (버전 1). 제한 시간 90분, 60문항.

도메인별 문항 수: mcp-fundamentals 10, architecture-and-components 8, interactions-and-execution 16, security-and-governance 14, use-cases-and-ecosystem 12. 유형: single 50, multiple 10.

## 1. [mcp-fundamentals]

A team migrating to MCP 2026-07-28 complains that servers can no longer remember a shopping cart between calls the way a session used to. What does the stateless design actually require them to do instead, and why is that the right trade?

A. Mint a server handle for the cart, return it in a result, and have the client pass it back as an argument\
B. Open one dedicated connection to a single replica so that replica can hold the cart in its own memory\
C. Send an initialize request each morning so the server can attach the cart to that entire day's ongoing handshake process every time\
D. Resend the entire cart's contents inside the JSON-RPC id field on every single request the client makes

<details><summary>정답</summary>

A. Statelessness means a server cannot rely on a prior request on the same connection, so any state that must span calls needs an explicit, server-minted, opaque handle passed back as a normal argument. That is what lets any replica answer any request, which is the whole payoff of dropping sessions.

</details>

## 2. [mcp-fundamentals]

A backend engineer proposes storing per-client state keyed by the underlying TCP connection so a server can skip re-reading arguments on every call. Why does this design break the 2026-07-28 statelessness guarantee even if it works in a single-server test?

A. It requires every client to install a new certificate before it can open any connection at all\
B. It makes a call's outcome depend on which connection carried it, so no other replica can serve it correctly\
C. It duplicates work that tools/list pagination already performs whenever a result set grows large across many separate paginated requests\
D. It is disallowed only because TCP connections run slower than the stdio transport for local servers

<details><summary>정답</summary>

B. Connection-keyed state reintroduces exactly the stickiness that 2026-07-28 removed: a load balancer could route the next call to a different replica, or the client could reconnect, and the server would answer incorrectly. The fix is an explicit handle carried in the request, not implicit connection memory.

</details>

## 3. [mcp-fundamentals]

A team debating how to pin its integration asks why MCP names revisions with dates such as 2026-07-28 instead of semantic version numbers. What does the date in a revision identifier signify?

A. The day every earlier revision stopped working, since each date retires older revisions\
B. The day the revision's governance vote closed, whatever the revision happened to change\
C. The last date backwards incompatible changes were made, so compatible updates keep it\
D. The release date of the newest SDK that implements the revision's full feature set

<details><summary>정답</summary>

C. A protocol version string is a date in YYYY-MM-DD form marking the last date backwards incompatible changes were made; the specification can still receive backwards compatible updates without changing it. Older revisions do not stop working on that date, and SDK release dates or governance votes play no part in the identifier.

</details>

## 4. [mcp-fundamentals]

A company already has a proprietary message format tuned exactly to its internal tools and refuses to adopt MCP, arguing their format is simpler for their own use. What is the clearest cost of that choice as new AI hosts and external systems appear?

A. Their format will automatically gain OAuth 2.1 support the moment a new host adopts it\
B. Nothing changes at all, since message formats never affect how many separate integrations a team must eventually build\
C. Their proprietary format will be forced to add roots and sampling to remain usable\
D. Every new host or system still needs a bespoke adapter instead of reusing a compliant client or server

<details><summary>정답</summary>

D. A protocol tailored to one shop's current tools does not become more useful as the ecosystem grows. Each new host or system still needs a custom adapter, which is exactly the integration cost that adopting a shared, versioned protocol like MCP was meant to remove.

</details>

## 5. [mcp-fundamentals] (복수 정답)

Which two outcomes does adopting one open, versioned protocol provide that a set of per-system custom integrations does not? Select two.

A. Every tool call is guaranteed to complete in the same amount of time on any server\
B. Every server automatically becomes free to operate no matter its own infrastructure costs\
C. A revision timeline and feature lifecycle give teams a shared way to plan migrations together\
D. A client built against the specification can work with servers it has never seen before

<details><summary>정답</summary>

C, D. Interoperability and a shared versioning and deprecation process are the concrete benefits of one open protocol. Cost and latency are operational properties of each deployment and are not guaranteed by the protocol itself, so they make weak justifications for standardization.

</details>

## 6. [mcp-fundamentals]

A code reviewer flags a server that still implements roots/list and sampling/createMessage as inputRequests, calling it out of date for 2026-07-28. Is the reviewer's conclusion correct?

A. No, since roots and sampling are deprecated, not removed, and remain functional until a later revision removes them\
B. Yes, since a deprecated feature can never legally be shipped inside any conformant 2026-07-28 production server deployment\
C. No, since roots and sampling were never mentioned in any revision released before 2026-07-28\
D. Yes, since any feature with a documented migration path must already be gone from the specification

<details><summary>정답</summary>

A. The feature lifecycle distinguishes deprecated from removed. Roots, sampling, and logging are deprecated in 2026-07-28 with a stated migration path, but they still work, with the earliest possible removal in the first revision released on or after 2027-07-28, so the server is not out of date.

</details>

## 7. [mcp-fundamentals]

A governance team assumes every deprecated MCP feature shares the same earliest removal floor as roots and sampling, the first revision on or after 2027-07-28. Why is applying that same floor to the HTTP+SSE transport a mistake?

A. It is not a mistake at all, since every feature deprecated by any means shares exactly one single common removal floor\
B. HTTP+SSE was deprecated earlier in 2025-03-26 by a different SEP, with its own floor tied to that SEP reaching Final\
C. The floor only ever applies to purely client-side features in general, and HTTP+SSE simply counts as a server-side transport feature instead\
D. HTTP+SSE cannot be deprecated at all, since transports are explicitly exempt from the feature lifecycle policy

<details><summary>정답</summary>

B. Each deprecated feature carries its own origin and removal floor rather than one shared date. Roots, sampling, logging, and Dynamic Client Registration were deprecated in 2026-07-28 with an earliest removal on or after 2027-07-28, but HTTP+SSE was deprecated back in 2025-03-26, so its floor runs from that earlier SEP reaching Final instead.

</details>

## 8. [mcp-fundamentals]

A skeptical architect asks why MCP does not simply negotiate protocol version and capabilities once, the way many handshake-based protocols do, instead of repeating them on every request. What is the accurate 2026-07-28 rationale?

A. Negotiating once is impossible in JSON-RPC, which has no concept of a request at all\
B. The specification requires it only so that gateways have more spare bytes available for careful rate limiting decisions\
C. Repeating metadata on every request lets any request be handled correctly without depending on what happened earlier\
D. It only matters for HTTP transports, since stdio connections keep negotiated state in memory regardless

<details><summary>정답</summary>

C. Per-request metadata is a direct consequence of statelessness: a server must not infer version or capabilities from earlier requests on the same connection. That is what allows requests to be retried or rerouted to a different replica safely, on stdio and HTTP alike.

</details>

## 9. [mcp-fundamentals] (복수 정답)

Select the three things MCP deliberately leaves for the host, the model provider, or the external system to decide, rather than standardizing itself.

A. How a host's user interface presents a conversation to a person\
B. The shape of a JSON-RPC request, result, error, and notification message\
C. Which large language model the host uses to reason about a request\
D. The internal business logic that a connected external system runs

<details><summary>정답</summary>

A, C, D. MCP standardizes the wire format connecting applications and external systems, not the model choice, the host UI, or what a system does internally. The JSON-RPC message shapes are exactly the part the protocol does define, which is why it is the one item outside the deliberately unstandardized set.

</details>

## 10. [mcp-fundamentals]

While researching a fine point of client registration, an engineer finds an open SEP pull request proposing a shorter tool name format, and separately the published specification page that already documents a different, current format. Which should the engineer treat as authoritative for a 2026-07-28 deployment?

A. Whichever document was edited more recently in the source repository's commit history\
B. Neither, since naming rules are left entirely to each individual server implementation to freely decide upon\
C. The SEP pull request, since proposals always describe where the specification is heading next\
D. The published specification page, since a draft proposal is not yet part of the current revision

<details><summary>정답</summary>

D. Reading the specification means treating the published, current revision as the source of truth and a draft SEP as, at most, a preview of a possible future change. Until a proposal is accepted and released, the specification page's rule is what a 2026-07-28 implementation must follow.

</details>

## 11. [architecture-and-components]

A host developer proposes sharing a single client instance across three different MCP servers to save memory, routing all three connections' messages through it. What is the strongest reason MCP hosts instead embed one client per server connection?

A. Each server connection has its own capabilities and cache, and mixing them in one client would blur which server a message belongs to\
B. Sharing a client this way would also clearly violate the important rule that tools must always stay strictly model-controlled rather than application-driven\
C. Servers absolutely refuse to respond at all unless exactly one single dedicated TCP port stays wide open on the host machine itself\
D. A shared client is forbidden since JSON-RPC ids must stay globally unique across an entire operating system

<details><summary>정답</summary>

A. A client's job is to hold one server's negotiated capabilities, discovery results, and cache state cleanly. Merging three servers behind one client risks attributing a result, a capability, or a cached entry to the wrong server, which the one-client-per-server design avoids by construction.

</details>

## 12. [architecture-and-components]

A gateway aggregates two independent servers that both expose a tool named search. What is the correct design for the aggregated tool registry the host presents to the model?

A. Merge both tool definitions into one entry and forward every call to whichever server answers first\
B. Prefix each tool name with a server identifier so the model can address search_docs and search_tickets without ambiguity\
C. Reject both servers entirely at discovery time until their two separate authors formally agree on one single shared tool name\
D. Silently keep only the first search tool discovered and drop the second one from the registry

<details><summary>정답</summary>

B. Because serverInfo.name is not guaranteed unique and tool names only need to be unique within one server, an aggregator that fronts multiple servers should prefix names to disambiguate. Dropping, merging, or racing servers all lose information the model needs to call the right tool.

</details>

## 13. [architecture-and-components]

A public-facing tool accepts a free-form object with no declared properties and additionalProperties left unset, versus an alternative design that lists each accepted field and sets additionalProperties to false. Which design better protects the tool and why?

A. Neither matters very much at all, since inputSchema is only ever shown to a human reviewer and never actually enforced anywhere by any code\
B. The free-form schema, since it lets any future client send new fields without a schema update\
C. The explicit schema, since it gives the server a precise contract and surfaces unexpected fields as feedback the model can act on\
D. The free-form schema, since MCP requires every tool to accept arbitrary extra arguments for compatibility

<details><summary>정답</summary>

C. A tool's inputSchema is a real contract that the server validates arguments against, and a precise schema with additionalProperties false catches malformed or unexpected input early, returning it as a tool execution error the model can correct. An unconstrained schema defers that discovery to runtime logic instead.

</details>

## 14. [architecture-and-components]

A team debates whether the confirm-before-destructive-action gate should live inside the server's tool implementation or inside the host, ahead of sending tools/call, given that annotations like destructiveHint are hints and untrusted unless the server itself is trusted. Where should the gate live?

A. Split randomly and unpredictably between host and server so a bypass in one place is unlikely to matter much\
B. Inside the server, since only the server can be trusted to decide when to ask the user\
C. Nowhere at all, since a genuinely well written tool description already makes an explicit confirmation step entirely unnecessary\
D. Inside the host, since it can apply its own judgment and cannot rely on an untrusted server's hints

<details><summary>정답</summary>

D. Because annotations are hints the client must treat as untrusted unless the server is known trustworthy, the host cannot safely delegate the decision to confirm a destructive call to the server itself. The host is the place positioned to show inputs and gate the call before it is sent.

</details>

## 15. [architecture-and-components] (복수 정답)

Which two statements correctly describe how discovery and capability negotiation connect an MCP host to a server? Select two.

A. A server must not rely on a capability the client did not declare on that request, and it errors naming what is missing\
B. Once a client has successfully called server/discover one time, every later request on that same connection is exempt from repeating clientCapabilities\
C. Calling server/discover is mandatory for every server to implement, though a client may skip it and send a request directly\
D. Capability negotiation only applies to client capabilities and never constrains what a server is allowed to assume about itself

<details><summary>정답</summary>

A, C. Servers must implement server/discover even though calling it is optional for clients, and a server that needs an undeclared capability must reject the request rather than assume it, using the missing-capability error. Neither rule is relaxed by an earlier successful discovery on the same connection.

</details>

## 16. [architecture-and-components]

A tool author is deciding whether to add an outputSchema and return structuredContent, or keep returning only a text content block. When is adding outputSchema and structuredContent worth the extra design effort?

A. When downstream code must consume the result programmatically, since structuredContent should still be mirrored into a text block for older clients\
B. Only when the server wants to drop stdio support and force clients onto Streamable HTTP\
C. Only for those particular tools that take absolutely no arguments at all, since arguments and their outputs must always share exactly one single schema\
D. Never, since only image and audio content blocks are ever permitted to appear alongside any structuredContent value at all

<details><summary>정답</summary>

A. structuredContent is valuable exactly when something downstream, a script or another tool, needs to parse the result reliably rather than a human reading prose. Because it must conform to outputSchema and should still be serialized into a text block, it adds a machine-readable contract without breaking older clients.

</details>

## 17. [architecture-and-components]

After a tool call fails because an argument had the wrong date format, the host has two options: silently retry the exact same call, or feed the isError result's content back into the model's context. Which is the correct design and why?

A. Escalate every tool execution error straight to a human without giving the model a chance to respond\
B. Feed the tool execution error back to the model, since that lets it read the content and self correct\
C. Discard the failed call entirely and never mention it at all to either the model or the end user\
D. Silently retry unchanged, since repeating the same call with the same arguments will eventually succeed

<details><summary>정답</summary>

B. Tool execution errors carry explanatory content precisely so the model can read what went wrong and adjust, which is the SEP-1303 rationale for treating input validation failures as results rather than opaque protocol errors. Retrying identical arguments blindly cannot fix a formatting mistake the model has not seen.

</details>

## 18. [architecture-and-components]

A manifest reviewer sees a tool definition that omits its annotations object entirely. Under the 2026-07-28 default values, how should the reviewer treat that tool when deciding whether the host needs a confirmation gate?

A. As read-only and safe, since omitting annotations means the server asserts the tool has no side effects\
B. As idempotent by default in every case, since idempotentHint always defaults to true whenever a tool omits its annotations\
C. As destructive by default, since destructiveHint defaults to true and openWorldHint defaults to true when omitted\
D. As unknown, requiring the server's documentation before any confirmation policy can be set at all

<details><summary>정답</summary>

C. When annotations are omitted, the specification's defaults apply: readOnlyHint and idempotentHint default to false, while destructiveHint defaults to true and is only meaningful when the tool is not read only. A careful reviewer should treat an undeclared tool as potentially destructive rather than assuming safety.

</details>

## 19. [interactions-and-execution]

A client library reuses the original JSON-RPC id when it retries a tools/call after answering an input_required result, reasoning that it is still logically the same call. Why does 2026-07-28 require a new id instead?

A. Because the client must close the connection between the original call and its retry attempt\
B. Because ids are case sensitive strings, and reusing one would corrupt the transport framing layer\
C. Because servers will always flatly refuse any id that has ever once been used before, even quite long after a response finally arrived\
D. Because an id must not repeat one still awaiting a response, and the retry is a distinct request with inputResponses

<details><summary>정답</summary>

D. A request id must not be reused while still awaiting a response, and the MRTR retry is a new request that adds inputResponses and echoes requestState, not a continuation of the original one. Treating the retry as a fresh request with a new id keeps in-flight tracking correct.

</details>

## 20. [interactions-and-execution]

A client stores an input_required result's requestState, then reformats its whitespace before echoing it back on retry because the two values look semantically equal. What goes wrong with this design if the server protects requestState with an HMAC?

A. The reformatted bytes no longer match what the HMAC was computed over, so the server rejects the retry as tampered\
B. Nothing, since JSON-RPC guarantees semantically equal payloads always hash to the exact same value\
C. The retry gets treated as a brand new tool call entirely unrelated to the original request\
D. The server would always fall back immediately to the deprecated Dynamic Client Registration flow just to verify that one pending state

<details><summary>정답</summary>

A. requestState is meant to be echoed exactly, and an HMAC or AEAD is computed over the original bytes, so any reformatting, even whitespace only, changes what the server verifies against. Treating it as opaque and echoing it unmodified is the only design that keeps the integrity check meaningful.

</details>

## 21. [interactions-and-execution]

A server designer is choosing how to report that a tool argument violates the tool's inputSchema, for example a date string in the wrong format. Which design is correct for 2026-07-28, and why?

A. Return a JSON-RPC error with code -32602, since any schema violation counts as invalid params\
B. Return a normal result with isError true and explanatory content, since the model can read and act on it\
C. Return a JSON-RPC error with code -32601, since the tool effectively could not ever be found in that particular shape\
D. Silently coerce the malformed date into a best guess and proceed without informing anyone at all

<details><summary>정답</summary>

B. SEP-1303 reclassified input validation failures as tool execution errors rather than protocol errors, specifically because a result the model can read and act on supports self correction, while a bare JSON-RPC error does not carry that feedback loop back into the model's context.

</details>

## 22. [interactions-and-execution]

A Streamable HTTP request's SSE response stream breaks partway through an idempotent tool call. What is the correct client design for recovering, given that streams cannot resume in 2026-07-28?

A. Abandon the operation permanently and completely, since a broken stream means it can never be attempted again\
B. Reuse the exact original request id anyway, since resuming with Last-Event-ID will simply pick the stream back up again\
C. Reissue the call as a new request with a new id, since idempotency makes a safe replay possible\
D. Wait indefinitely, since the original request is still technically in flight somewhere on the server

<details><summary>정답</summary>

C. Because stream resumability and Last-Event-ID were removed, a broken stream loses the request outright, so the client must reissue it as a fresh request with a new id. Idempotency is exactly the property that makes reissuing safe rather than risking a duplicate side effect.

</details>

## 23. [interactions-and-execution]

A server implementation is being reviewed, and it sends notifications/progress for a long tool call onto the same subscriptions/listen stream the client opened for list-changed notifications, reasoning it saves a connection. Why is this design wrong for 2026-07-28?

A. Because progress notifications were removed entirely from the specification as of this particular revision release\
B. Because subscriptions/listen streams are only ever permitted to carry plain resource content, and never any notifications of any kind at all\
C. Because a listen stream can only be opened once per server for each calendar day\
D. Because progress notifications may only flow on the response stream of the request they relate to, not a listen stream

<details><summary>정답</summary>

D. Request-scoped notifications such as progress and log messages are defined to flow only on the response stream of the request they relate to, while a listen stream carries subscription notifications like list_changed. Mixing them would make demultiplexing ambiguous for any client following the specification.

</details>

## 24. [interactions-and-execution]

A resources/read result returns a price list that is identical for every caller, regardless of who is asking. Which cacheScope is the correct design choice, and what should the server still keep separate from that choice?

A. cacheScope public, while still applying whatever per-user access control the resource needs, since cacheScope is a sharing hint, not access control\
B. cacheScope public, which by itself is enough to guarantee only authorized users can read it\
C. No cacheScope field at all, since caching hints are optional whenever content rarely changes\
D. cacheScope private in every single case, because any resource that ever touches money must always be treated as extremely sensitive no matter what here

<details><summary>정답</summary>

A. cacheScope public is the right fit for content that is genuinely identical across callers, but the specification is explicit that cacheScope is never an access control mechanism, so authorization must still be enforced on every read independent of whether the result can be shared in a cache.

</details>

## 25. [interactions-and-execution]

A tools/list result contains fewer entries for a user with a narrower OAuth scope than for an administrator calling the same server. Does this violate the requirement that list results must not vary per connection?

A. No, but only if the two users happen to be using two different transports entirely\
B. No, since list results may vary by the authorization on the request, just not by the connection itself\
C. Yes, unless the administrator explicitly and repeatedly calls tools/list twice over before the comparison ever finally becomes valid\
D. Yes, since any variation at all in a list result is forbidden under the stateless core

<details><summary>정답</summary>

B. The stateless core forbids list results from varying as a side effect of the connection or prior requests, but it explicitly allows variation by the authorization presented on a request. Two different scopes producing two different lists is exactly that permitted case, not a violation.

</details>

## 26. [interactions-and-execution] (복수 정답)

Which two statements correctly describe how a subscriptions/listen stream must behave under 2026-07-28? Select two.

A. The server may freely send any notification type on the stream even if the client never asked to filter for it\
B. Every message on the stream carries a subscriptionId in _meta equal to the JSON-RPC id of the original listen request\
C. A client must always fully reopen a fresh subscriptions/listen call before every single tool invocation it ever makes at all\
D. The first message on the stream must be notifications/subscriptions/acknowledged, echoing the subset of notification types the server will actually honor

<details><summary>정답</summary>

B, D. A listen stream opens with an acknowledgment naming what the server will honor, and every later message is tagged with the subscriptionId tying it back to that listen request's id. The server must never send unrequested notification types, and listening has no required relationship to individual tool calls.

</details>

## 27. [interactions-and-execution]

A report-generation tool typically takes ten minutes to finish. A designer is choosing between a plain synchronous tools/call, an MRTR input_required exchange, and the tasks extension. Which fits, and why do the other two fail?

A. A plain synchronous call instead, since clients are generally expected to hold a connection open for as long as it ever takes\
B. MRTR, since ten whole minutes of waiting is exactly what elicitation was originally built to fill with a prompt\
C. The tasks extension, since a synchronous call risks timeouts and MRTR is for gathering input, not for waiting out work\
D. None of the three, since long work must always be split into ten separate one-minute calls

<details><summary>정답</summary>

C. A blocking synchronous call over ten minutes risks intermediary and client timeouts, and MRTR exists to gather input mid-flight, not to represent duration. The tasks extension is purpose built for durable long running work, returning a task handle the client polls instead of holding a connection open.

</details>

## 28. [interactions-and-execution]

A tool that runs a multi-step build pipeline needs to track progress across several minutes. Where should that intermediate state live: in requestState, inside the task object the tasks extension returns, or in ordinary server-side storage referenced by an opaque id?

A. In ordinary server-side storage only, and never expose any id to the client at all\
B. In requestState alone, since it is specifically designed to carry any data a server wants to keep across calls indefinitely\
C. Split evenly across all three separate locations so that a missing one still leaves a copy safely elsewhere\
D. In the task object and server-side storage keyed by taskId, since requestState survives only one MRTR round trip

<details><summary>정답</summary>

D. requestState exists to survive exactly one MRTR round trip and is explicitly never cached or reused beyond that, while the tasks extension's taskId and its backing server-side storage are the purpose built mechanism for state that spans a multi-step, minutes-long operation the client polls.

</details>

## 29. [interactions-and-execution]

A server author wants to add an application-defined error code meaning quota exceeded. Which numeric range is the correct place for it under the 2026-07-28 allocation policy?

A. Outside -32768 to -32000 entirely, since that whole band is reserved for JSON-RPC and MCP use\
B. Anywhere in -32000 to -32019, since that legacy band is open for any implementation to reuse\
C. Anywhere at all in -32020 to -32099, since that band is available for any server-specific extension error code\
D. It does not matter, since JSON-RPC error codes are purely cosmetic and never machine interpreted

<details><summary>정답</summary>

A. The allocation policy reserves -32000 to -32019 for legacy use only, -32020 to -32099 for the MCP specification itself, and directs application-defined codes outside the entire -32768 to -32000 band. A quota-exceeded code belongs outside that whole reserved range.

</details>

## 30. [interactions-and-execution] (복수 정답)

A server currently uses the deprecated sampling capability to ask the client's model for a completion mid-tool-call. Which two statements correctly describe migrating away from it under 2026-07-28? Select two.

A. Sampling still functions as an MRTR input request today, so the migration can happen on the server's own schedule before removal\
B. A client that has still not declared the sampling capability on a request can still be asked for a completion through it anyway\
C. Sampling must be removed immediately, since deprecated features are stripped out of every server the moment a new revision ships\
D. The recommended migration is for the server to call an LLM provider API directly instead of asking the client for a completion

<details><summary>정답</summary>

A, D. Sampling is deprecated with a clear migration path, calling a provider API directly, but it remains functional through MRTR today, so teams can migrate on their own timeline rather than under immediate pressure. A client must still declare the capability before a server may request it.

</details>

## 31. [interactions-and-execution] (복수 정답)

Select the three failures that 2026-07-28 treats as JSON-RPC protocol errors rather than tool execution errors.

A. A tools/call whose arguments violate the target tool's own inputSchema\
B. A tools/call naming a tool that the server does not expose at all\
C. A request that is missing the required io.modelcontextprotocol/clientCapabilities per-request meta field\
D. A request whose shape does not satisfy the CallToolRequest schema at all

<details><summary>정답</summary>

B, C, D. An unknown tool, a request that fails the CallToolRequest schema, and a request missing required per-request metadata are all protocol level failures returned as JSON-RPC errors. Arguments that merely violate a specific tool's inputSchema are, by contrast, a tool execution error the model can read and correct.

</details>

## 32. [interactions-and-execution]

A client library parses a tools/list cursor string, assumes it is a base ten offset, and increments it locally instead of sending back exactly what the server issued. Why is this design fragile under 2026-07-28?

A. Because an incremented cursor is always immediately rejected outright with a -32601 method not found error every single time\
B. Because a cursor is opaque and server chosen, so reinterpreting it breaks the moment the server changes page size\
C. Because cursors stay valid for exactly one minute after being issued, regardless of how they are used\
D. Because pagination was replaced entirely by cacheScope in this particular revision, and cursors simply no longer exist

<details><summary>정답</summary>

B. Pagination cursors are opaque by design specifically so the server retains freedom over page size and internal ordering. A client that parses and reconstructs a cursor is relying on an implementation detail that can silently break with any server-side change, which is exactly what opacity is meant to prevent.

</details>

## 33. [interactions-and-execution]

A server needs to end a subscriptions/listen stream in two different situations: a graceful, planned end of the subscription, and an abrupt teardown the server initiates. What is the correct message for each case?

A. A graceful end always uses a fresh tools/call, and a teardown always uses server/discover instead\
B. Both cases always use the exact same notifications/cancelled message, since the client cannot ever tell the difference\
C. A graceful end answers the listen request with a complete result, while notifications/cancelled tears the stream down\
D. Both cases require the client to reopen a brand new HTTP connection before either message sends

<details><summary>정답</summary>

C. The specification treats a graceful end and a server-initiated teardown as distinct: a graceful close answers the original listen request with a complete result carrying the subscription id, while notifications/cancelled is the message reserved specifically for the server tearing a listen stream down.

</details>

## 34. [interactions-and-execution]

A destructive tool call times out with no response, and its server-reported idempotentHint is false. Since annotations are self reported and untrusted unless the server itself is trusted, what should the client's retry design do?

A. Retry only after first fully switching the entire connection over to stdio, since stdio never produces any ambiguous timeouts at all\
B. Retry immediately and automatically with a new id, trusting the reported hint to mean it is safe\
C. Ignore the timeout entirely and wait forever rather than making any decision about it at all\
D. Treat the outcome as unknown and require explicit confirmation before reissuing a call whose effect could not be observed

<details><summary>정답</summary>

D. Because idempotentHint is an untrusted, self reported hint from a server that may not be trusted, a client cannot safely assume a timed out destructive call is either done or safe to repeat. The prudent design treats the outcome as unknown and requires explicit confirmation before retrying.

</details>

## 35. [security-and-governance]

A tool's description field contains text instructing the model to also call a different, unrelated server's delete_account tool whenever this tool is used. What is the correct trust-boundary response to this discovery?

A. Treat the tool description as untrusted content, refuse to let it steer a call into another server, and flag it for review\
B. Forward the instruction straight to the model without any comment at all, since the model must filter unsafe text entirely on its own\
C. Delete the entire tool registry for every connected server, since one bad description proves all are compromised\
D. Trust the instruction fully, since any text inside a tool definition is implicitly approved already by the operator

<details><summary>정답</summary>

A. Tool descriptions, like other server-authored content, are untrusted input to the model and can carry tool poisoning or cross-server steering attempts. The correct design quarantines the instruction rather than acting on it, and treats a server that ships one as a candidate for review, not blanket distrust of every server.

</details>

## 36. [security-and-governance]

A resources/read result returns document text that includes an embedded instruction telling the model to ignore its current task and export credentials. Which trust zone is responsible for making sure that instruction never reaches the model as if it were trustworthy guidance?

A. The model itself, since it alone stays fully responsible for recognizing every single injection attempt entirely unaided\
B. The host and client, since they own labeling and sanitizing untrusted content before it enters model context\
C. The authorization server, since OAuth scopes are what prevent embedded instructions from ever being read\
D. The upstream document's original author, since the host has no ability to inspect resource content at all

<details><summary>정답</summary>

B. Resource contents are untrusted input the same as tool results, and the host and client are the trust zone positioned to label and sanitize such content before it becomes part of the model's context, rather than leaving detection entirely to the model or to unrelated infrastructure like the authorization server.

</details>

## 37. [security-and-governance]

A client team notices its authorization server ignores the RFC 8707 resource parameter and proposes dropping it from the authorization and token requests to simplify the code. What is the correct design decision?

A. Replace it with a custom audience header, since servers read audience from headers\
B. Drop it from the token request only, since audience is fixed at authorization time\
C. Keep sending it on both requests, since MCP clients must send it whether or not it is supported\
D. Drop it, since a parameter the authorization server ignores cannot affect security at all

<details><summary>정답</summary>

C. MCP clients must send the resource parameter, naming the canonical server URI, on both the authorization and token requests regardless of whether the authorization server supports it. It is what lets a supporting authorization server bind a token's audience to one MCP server, so dropping it silently weakens the deployment the day the authorization server starts honoring it.

</details>

## 38. [security-and-governance]

An MCP server needs to call a third-party API on the user's behalf. One design passes the user's incoming bearer token straight through to that API; another design has the server obtain and use a separate token scoped to that API. Which design is correct, and why?

A. Pass the incoming token through, but only when the API happens to be owned by the same company\
B. Cache the incoming token locally and openly replay it to every downstream system the tool ever needs to reach at all times\
C. Pass the incoming token through, since reusing one token everywhere is simpler for the user to reason about\
D. Obtain a separate token for the API, since passthrough is forbidden and a server must validate any token it accepts

<details><summary>정답</summary>

D. Token passthrough is explicitly forbidden because a token issued for the MCP server was never validated as intended for the downstream API, creating a confused deputy risk. The server must validate audience for tokens it accepts and obtain its own, separately scoped token for any upstream call it makes.

</details>

## 39. [security-and-governance]

A gateway operator proposes forwarding the single incoming bearer token unchanged to every downstream MCP server behind the gateway, arguing it removes the need to manage per-server credentials. What is the flaw in this design?

A. It lets a token issued for one audience reach servers it was never validated against, which passthrough forbids\
B. It only ever really fails when the gateway uses Streamable HTTP rather than stdio to reach far downstream servers anyway every time\
C. It is flawless, since a bearer token by definition already grants unlimited access to any receiver\
D. It is unnecessary to avoid, since audience validation was removed from the specification in this revision

<details><summary>정답</summary>

A. A single forwarded token is a textbook token passthrough problem: each downstream server must validate that a token was issued for it, and a token minted for the gateway or for one server was never vetted for another. The gateway needs to mint or exchange distinct, audience-scoped tokens per server instead.

</details>

## 40. [security-and-governance]

A host is deciding whether to rely on a tool's title and description text to reassure the user before a destructive call, or to run an explicit MRTR elicitation asking the user to confirm. Which design is more defensible, given annotations are untrusted hints?

A. Neither, since destructive tools should simply never ever be safely callable under any circumstances at all here whatsoever\
B. Explicit elicitation, since it produces a real user decision the host controls, rather than trusting server-authored text\
C. The description text alone, but only for tools whose name happens to contain the word safe\
D. The description text alone, since a well written description is functionally equivalent to asking the user

<details><summary>정답</summary>

B. Because tool titles and descriptions are server authored and untrusted unless the server itself is trusted, they cannot substitute for a real consent decision. An MRTR elicitation round trip is the mechanism that gets an explicit answer from the user rather than assuming reassurance from text the server controls.

</details>

## 41. [security-and-governance] (복수 정답)

A deploy tool uses MRTR elicitation to collect a human approval before running, and the pending decision is carried in requestState. Select the three protections that requestState needs given it is attacker-controlled input.

A. Binding to the authenticated principal so one user's pending approval cannot be replayed by another\
B. Integrity protection such as an HMAC or AEAD so it cannot be modified undetected\
C. A short expiry combined with single-use enforcement on the server so it cannot be replayed later\
D. A cacheScope of public so the pending approval can be shared efficiently across every user

<details><summary>정답</summary>

A, B, C. Because requestState is round tripped through the client and is not itself trusted, it needs integrity protection, principal binding, and a short, single-use expiry enforced server side. A public cacheScope is unrelated and actively wrong here, since MRTR results including input_required are never cached at all.

</details>

## 42. [security-and-governance]

An audit pipeline is being designed to record who performed a sensitive tool call. One proposal attributes the action using the self reported clientInfo.name from the request; another uses the authenticated principal derived from the validated access token. Which is correct?

A. clientInfo.name, since it is the field explicitly designed for identifying a caller in an audit trail\
B. Whichever field simply happens to be shorter, since shorter values keep audit records far more compact over a long time\
C. The authenticated principal from the token, since clientInfo is self reported and meant only for display and debugging\
D. Both fields combined with an OR, so either one alone counts as sufficient proof of identity

<details><summary>정답</summary>

C. clientInfo and serverInfo are explicitly self reported and meant only for display, logging, and debugging, never for security decisions. An audit trail that must actually attribute an action correctly needs the authenticated principal established through token validation, not a value the caller could freely set.

</details>

## 43. [security-and-governance] (복수 정답)

Which two statements correctly describe the client registration priority order for 2026-07-28? Select two.

A. Dynamic Client Registration is checked before Client ID Metadata Documents whenever both are advertised\
B. Pre-registered credentials are tried first, ahead of any dynamic or document-based registration mechanism\
C. Asking the user is the fallback of last resort, used only once the automated options are unavailable\
D. Client ID Metadata Documents may only be used by clients that also support the deprecated Dynamic Client Registration flow

<details><summary>정답</summary>

B, C. The order is pre-registered credentials first, then Client ID Metadata Documents when advertised, then the deprecated Dynamic Client Registration, and finally asking the user as a last resort. CIMD does not require DCR support, and DCR is checked after CIMD, not before it.

</details>

## 44. [security-and-governance]

A registry-listed server silently changes a tool's description and inputSchema between two releases, in a way that expands what the tool actually does. What control design catches this kind of rug pull before the new behavior reaches production?

A. Waiting patiently for a user complaint, since definition changes are always noticeable immediately during normal daily use\
B. Trusting the registry's namespace verification alone, since a verified namespace also guarantees definitions never change\
C. Disabling the tool permanently the first time it is ever called, regardless of whether anything changed\
D. Pinning tool definitions by hash and holding any changed definition for review before it is trusted again

<details><summary>정답</summary>

D. Registry namespace verification proves who owns a name, not that its tool definitions stay the same over time, so definition drift needs its own control: pinning a known-good definition by hash and flagging any change for review before the updated tool is trusted in production.

</details>

## 45. [security-and-governance]

A host has two servers connected, A and B. A tool result from server A contains text asking the host to immediately call an administrative tool on server B. What is the correct trust design?

A. Treat server A's content as untrusted with respect to server B, and never let one server's output authorize a call into another\
B. Allow it, since both servers were separately approved by the same administrator and therefore trust each other\
C. Allow it only if server A's very own tool has readOnlyHint explicitly and clearly set to true in its own listed annotations somewhere\
D. Allow it, since the host's only job is to relay whatever a connected server's result requests

<details><summary>정답</summary>

A. Trust boundaries are per server, not transitive across servers just because a host happens to be connected to both. Content returned by one server must never be treated as authorization to act on a different server, since that would let any single compromised or careless server steer the entire host.

</details>

## 46. [security-and-governance] (복수 정답)

Which two design elements make an audit and trace system useful for reviewing an incident that crossed a host, a client, and two servers? Select two.

A. Writing tamper-evident, for example hash-chained, records so a reviewer can detect whether a log entry was altered after the fact\
B. Propagating OpenTelemetry trace context such as traceparent through _meta so spans across hops can be correlated\
C. Recording only the final aggregate outcome of the entire multi-hop exchange, discarding intermediate steps to save storage\
D. Letting each hop choose its own unrelated identifier scheme so operators can compare formats after the fact

<details><summary>정답</summary>

A, B. Correlating activity across hops needs a shared trace context propagated end to end, and reviewing an incident needs confidence that the records themselves were not altered, which tamper evidence provides. Discarding intermediate steps or letting each hop invent its own identifiers both destroy the correlation an audit needs.

</details>

## 47. [security-and-governance]

A team proposes setting cacheScope to public on a resource that contains per-tenant billing data, reasoning that a public cache reduces load and every tenant's client already has a valid token. What is wrong with this design?

A. Nothing is wrong, since holding a valid token is by itself sufficient proof cacheScope public is safe\
B. cacheScope public would let one tenant's cached response be served to a different tenant, since it governs sharing, not access\
C. cacheScope only ever really affects tools/list results specifically, so it has no real bearing on resources/read at all here whatsoever\
D. The only fix needed is to shorten ttlMs, since a shorter freshness window makes any scope safe

<details><summary>정답</summary>

B. cacheScope public means a result may be shared across users and tokens, which is precisely wrong for per-tenant billing data even though every caller holds a valid token. Because cacheScope is never an access control mechanism, per-tenant data needs private scope and its own per-read authorization check, not a shorter TTL.

</details>

## 48. [security-and-governance]

A client stored registration credentials after successfully registering with authorization server A. It now needs to connect to a different authorization server B for a separate deployment, and B also supports the same registration mechanism. What is the correct design?

A. Reuse the credentials stored for server A directly with server B right away, since one successful registration should cover every future authorization server encountered\
B. Wait patiently for server A to forward the stored credentials to server B fully automatically before any single request to B is ever attempted at all\
C. Register separately with server B and keep credentials keyed by issuer, since persisted credentials must never be reused across a different authorization server\
D. Ask the user to manually paste server A's credentials into server B's configuration screen so both servers share one identity

<details><summary>정답</summary>

C. Registration state is bound to the authorization server that issued it. Persisted credentials must be keyed by issuer, and a client must register again, rather than reuse or transfer them, whenever it connects to a different authorization server, even one that supports the same registration mechanism.

</details>

## 49. [use-cases-and-ecosystem]

A team wants a reusable, user-invoked, slash-command-style template with arguments that a person deliberately selects to start a code review conversation. Which primitive fits this control model, and why not the alternatives?

A. A resource, since resources are application-driven content the host attaches without the user choosing it each time\
B. A tool, since tools are always model-controlled and the model alone would decide on its own when to invoke it\
C. Server instructions returned from server/discover, since those are supposedly the only user-facing text a server could ever expose to anyone\
D. A prompt, since prompts are user-controlled templates a person deliberately selects, matching how this template is meant to be used

<details><summary>정답</summary>

D. The three server primitives map to three different control models: tools are model-controlled, resources are application-driven, and prompts are user-controlled templates a person explicitly selects. A deliberately chosen, argument-taking template for starting a conversation is exactly the user-controlled case prompts were designed for.

</details>

## 50. [use-cases-and-ecosystem]

A tool is meant to be called only by an app's own embedded UI, to refresh its view, and should never appear as something the model can invoke on its own. Under the MCP Apps extension, how should the tool's _meta.ui visibility be set, and what must the host do as a result?

A. Set visibility to only app, and the host must not list the tool to the model while still letting the app call it\
B. Set visibility to only app, but the host stays free to list it to the model anyway\
C. Set visibility to only model instead, since visibility supposedly only ever controls what the human user is ever allowed to see at all\
D. Leave visibility completely unset, since every tool is always callable by both the model and the app equally anyway

<details><summary>정답</summary>

A. MCP Apps lets a tool restrict who may invoke it: when visibility excludes model, the host must not list that tool to the agent, and when it excludes app, the host must reject a tools/call coming from the rendered UI. An app-only refresh action should declare app so only the embedded interface can trigger it.

</details>

## 51. [use-cases-and-ecosystem]

A vendor markets a brand new SDK as fully spec-compliant and ready to use, but it currently sits at Tier 3 in the official tiering system. Before recommending it for a customer's production deployment, what should an integrator conclude from that tier alone?

A. That it ships absolutely every official extension by default, since extension support is apparently mandatory at every single tier without exception\
B. That it is explicitly experimental, with no guaranteed conformance percentage or timeline the way Tier 1 and 2 commit to\
C. That it already passes one hundred percent of conformance tests, since every vendor SDK starts out at Tier 1 by default\
D. That it will automatically and fully graduate to Tier 1 within just two business days of the vendor's claim

<details><summary>정답</summary>

B. SDK tiers describe real, graduated commitments: Tier 1 passes all conformance tests with fast triage, Tier 2 passes most within a longer window, and Tier 3 is explicitly experimental with none of those guarantees. A vendor's marketing claim of spec compliance does not override what the tier itself actually commits to.

</details>

## 52. [use-cases-and-ecosystem]

In a gateway-fronted enterprise deployment over Streamable HTTP, whose responsibility is validating the Origin header and binding to localhost when appropriate? What goes wrong if that responsibility is assumed to belong to someone else?

A. The end user's, since only a human can decide whether a request's origin looks legitimate\
B. The registry publisher's, since server.json is where Origin policy is supposed to be declared\
C. The server or gateway operator's, and if neither implements it, the deployment is open to DNS rebinding attacks\
D. Nobody's at all really, since Origin validation only ever matters for stdio transports and never at all for Streamable HTTP

<details><summary>정답</summary>

C. Origin validation and localhost binding are HTTP transport security controls that the server or the gateway sitting in front of it must actually implement. If each side assumes the other handles it, the deployment ends up with no real defense against the DNS rebinding style attacks that control was meant to stop.

</details>

## 53. [use-cases-and-ecosystem]

A nightly batch job runs unattended, calling a server with no user present and no elicitation capability declared. It needs the equivalent of long running work tracking. Which design fits, and why would MRTR elicitation be the wrong choice here?

A. Dynamic Client Registration, since machine clients must always dynamically register again before every single batch run without exception\
B. A plain synchronous call held open for the job's entire duration, since batch jobs are typically always expected to block\
C. MRTR elicitation, since a server can always request user input from a client even with nobody watching\
D. The tasks extension or a server-minted handle, since elicitation needs a client capability an unattended job will not declare

<details><summary>정답</summary>

D. MRTR input requests such as elicitation assume a capability the client declares and a way to gather an answer, neither of which fits an unattended machine-to-machine job with nobody present. Tracking long, unattended work instead fits the tasks extension or an explicit server-minted handle referenced on later calls.

</details>

## 54. [use-cases-and-ecosystem]

A registry lists a server under a reverse-DNS namespace that has already passed GitHub ownership verification. A reviewer treats that verification alone as proof the server's published code is safe to run. What is the flaw in that conclusion?

A. Namespace verification proves who controls the name, not that the code is safe, since scanning is delegated to package registries and aggregators\
B. There is truly no flaw at all, since GitHub ownership verification already includes a full independent security audit of the published code itself\
C. The flaw is that GitHub verification technically only ever applies to DNS challenges, never to namespace ownership claims themselves at all\
D. The flaw is that reverse-DNS namespaces were entirely removed from the registry design as of this specification revision

<details><summary>정답</summary>

A. Namespace verification through GitHub, DNS, or an HTTP challenge establishes ownership of a name, which stops impersonation, but the registry explicitly delegates security scanning to package registries and aggregators, so ownership proof alone says nothing about whether the published code itself is safe to run.

</details>

## 55. [use-cases-and-ecosystem] (복수 정답)

Select the three facts about SDK tiers that should shape a build-versus-adopt decision for a client library.

A. A Tier 1 SDK commits to triaging issues within two business days and fixing critical bugs within seven\
B. Extensions are not a requirement at any SDK tier, so their absence does not by itself indicate a lower tier\
C. Tier 3 SDKs are always required to support every single official extension before they can ever be published\
D. An SDK can be relegated to a lower tier after four consecutive weeks of conformance failures

<details><summary>정답</summary>

A, B, D. Tier 1 commitments, the relegation rule after sustained failures, and the fact that extensions are optional at every tier are all real signals for a build-versus-adopt call. No tier mandates supporting every official extension, so that claim is false and would mislead a team using extension coverage to judge tier.

</details>

## 56. [use-cases-and-ecosystem]

A server exposes several skills through Skills over MCP. A client calls resources/read on one skill's SKILL.md file just to preview its contents. Has that skill been activated by this read?

A. No, since Skills over MCP never permits resources/read to return SKILL.md content at all\
B. No, reading the file through resources/read makes it merely readable, a step distinct from activation\
C. Yes, reading SKILL.md through resources/read is what activation means under this extension by definition\
D. Yes, but only for skills whose description field happens to be under one hundred characters

<details><summary>정답</summary>

B. Reading a skill's SKILL.md through resources/read only retrieves text. Skills over MCP treats skill content as untrusted input, leaves whether a skill is loaded into model context to explicit user policy, and has hosts let users inspect a skill before loading it, so previewing a file is not activation.

</details>

## 57. [use-cases-and-ecosystem]

A client was built and tested only against one specific MCP server. Under 2026-07-28, why should that same client generally be able to work against a different, unrelated, modern server without code changes?

A. Because every MCP server is legally required to expose an identical set of tool names\
B. Because servers share a single global process and therefore already know about every client in advance\
C. Because a compliant client relies on discovery and the wire protocol rather than one server's internal assumptions\
D. Because the client's own author is always expected to hardcode that one specific server's tool names directly into the client itself

<details><summary>정답</summary>

C. Portability follows directly from where logic is supposed to live: a client that discovers capabilities and tools through the protocol, rather than hardcoding one server's specifics, can drive any compliant server it has never seen. Custom, server-specific assumptions baked into a client are exactly what breaks that portability.

</details>

## 58. [use-cases-and-ecosystem]

A platform team is deciding whether a new extension can be turned on for every user by default the moment a vendor SDK adds support for it. What does the specification's extension design actually require here?

A. The host must wait for the extension's identifier to change to a -v2 suffix before enabling it\
B. Nothing further at all, since any extension a client's own SDK supports is automatically enabled for every single request\
C. The extension must first be promoted from Tier 3 to Tier 1 before it can ever be declared\
D. Explicit opt-in on both sides, since extensions are disabled by default, which is where governance sign-off belongs

<details><summary>정답</summary>

D. Extensions are disabled by default and require explicit opt-in through declared capabilities on both the client and server side, which is a deliberate design choice, not an accident of SDK support. That deliberate opt-in point is precisely where a platform team's governance review naturally fits before wide rollout.

</details>

## 59. [use-cases-and-ecosystem]

A developer is deciding whether to expose a purely internal helper function, called only within the same process and never crossing an application or trust boundary, as an MCP tool. Is this a good fit for MCP?

A. No, since MCP is built for crossing the boundary between an application and an external system, and this helper never crosses it\
B. No, but only because internal helper functions are always too fast for JSON-RPC to keep up with\
C. Yes, since any function at all becomes more valuable once it is exposed through a protocol\
D. Yes, since MCP technically always requires every single function anywhere in a codebase to eventually always be reachable as a tool too

<details><summary>정답</summary>

A. MCP exists to standardize context and actions crossing the boundary between an AI application and an external system. A same-process internal helper never crosses that boundary, so wrapping it as a tool adds serialization and discovery overhead without the interoperability benefit that justifies using the protocol in the first place.

</details>

## 60. [use-cases-and-ecosystem]

A gateway operator wants to cache a server's public server/discover result across every tenant to cut latency. Is sharing that cached result across tenants consistent with the caching contract, and what must the gateway still keep separate?

A. It is consistent, and caching the discover result is by itself sufficient to authorize every tenant's requests\
B. It is consistent, and per-tenant authorization must still be enforced on every actual request regardless of what was cached\
C. It is inconsistent, since server/discover results can never ever carry a cacheScope of public under any circumstances at all\
D. It is inconsistent, since sharing any cached value across tenants is forbidden no matter what scope declares

<details><summary>정답</summary>

B. server/discover is one of the results that carries ttlMs and cacheScope, and a public discover result, capability metadata rather than tenant-specific data, is a reasonable candidate for shared caching. That caching decision is still separate from authorization, which the gateway must keep enforcing per tenant on every later request.

</details>
