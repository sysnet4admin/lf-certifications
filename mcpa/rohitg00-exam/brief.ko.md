# MCP 2026-07-28 Protocol Brief

> 리눅스 재단(Linux Foundation)과 AAIF의 공식 자료가 아니다. 한국어 번역은 AI로 작성했으므로 틀린 곳이 있을 수 있다. 공식 스펙이 우선이다: https://modelcontextprotocol.io/specification/2026-07-28
> 원문 가운데 시험(exam)을 언급한 문장은 원작자의 설명을 그대로 옮긴 것이며 이 저장소의 의견이 아니다.

rohitg00/ai-engineering-from-scratch의 `certifications/mcpa/research/mcp-2026-07-28-brief.md`(커밋 `9f06b4b`, 2026-09-25, MIT)를 옮기고 블록마다 한국어 번역을 붙였다.
원문은 `brief.en.md`. rohitg00 저장소의 모의고사 문항 대부분이 이 문서에서 나온다.

The protocol source of truth for every MCPA lesson, quiz, and assessment question in this
curriculum. Every fact below was read from the primary specification pages on 2026-09-24.
If a lesson disagrees with this brief, the lesson is wrong. If this brief disagrees with the
specification, the specification wins and this brief must be corrected with a new date.

> 이 커리큘럼의 모든 MCPA 레슨, 퀴즈, 평가 문항이 기준으로 삼는 프로토콜 원천이다.
> 아래 사실은 모두 2026-09-24에 1차 명세 페이지에서 읽었다. 레슨이 이 요약과 다르면
> 레슨이 틀린 것이다. 이 요약이 명세와 다르면 명세가 우선한다. 그때는 이 요약을 새 날짜와
> 함께 고쳐야 한다.

- Specification: https://modelcontextprotocol.io/specification/2026-07-28
- TypeScript schema (source of truth for every message): https://github.com/modelcontextprotocol/modelcontextprotocol/blob/main/schema/2026-07-28/schema.ts
- Changelog since 2025-11-25: https://modelcontextprotocol.io/specification/2026-07-28/changelog
- Deprecated features registry: https://modelcontextprotocol.io/specification/2026-07-28/deprecated
- MCP Apps extension specification (stable 2026-01-26): https://github.com/modelcontextprotocol/ext-apps/blob/main/specification/2026-01-26/apps.mdx
- The MCPA exam page says to prepare against the 2026-07-28 release and that the exam focuses on
  "MCP internals, including architecture, message flow, client-server interactions, capabilities,
  tools, resources, prompts, transports, and security considerations."

> - 명세: https://modelcontextprotocol.io/specification/2026-07-28
> - TypeScript schema(모든 메시지의 원천): https://github.com/modelcontextprotocol/modelcontextprotocol/blob/main/schema/2026-07-28/schema.ts
> - 2025-11-25 이후 변경 이력: https://modelcontextprotocol.io/specification/2026-07-28/changelog
> - 폐기(deprecated) 기능 목록: https://modelcontextprotocol.io/specification/2026-07-28/deprecated
> - MCP Apps extension 명세(2026-01-26 stable): https://github.com/modelcontextprotocol/ext-apps/blob/main/specification/2026-01-26/apps.mdx
> - MCPA 시험 페이지는 2026-07-28 릴리스를 기준으로 준비하라고 적는다. 시험이 "아키텍처,
>   메시지 흐름, client-server 상호작용, capability, tool, resource, prompt, transport, 보안
>   고려 사항을 포함한 MCP 내부 구조"에 초점을 둔다고도 적는다.

Exam facts (fee, duration, domains, weights) live in `source-verification-ledger.md`, not here.

> 시험 정보(응시료, 시간, 도메인, 비중)는 여기가 아니라 `source-verification-ledger.md`에 있다.

## 1. The era model

**한국어 제목: 시대(era) 모델**

| Term | Meaning |
|---|---|
| Modern | 2026-07-28 and later. Version, identity, and capabilities travel as per-request metadata. |
| Legacy | 2025-11-25 and earlier. A connection starts with an `initialize` handshake that creates a session. |
| Dual-era | An implementation that supports both, with an explicit era decision before parsing. |

| 용어 | 의미 |
|---|---|
| Modern | 2026-07-28 이후. 버전, 신원, capability가 요청마다 메타데이터로 전달된다. |
| Legacy | 2025-11-25 이전. 연결이 `initialize` handshake로 시작하고 이 handshake가 세션을 만든다. |
| Dual-era | 둘 다 지원하는 구현. 파싱 전에 어느 era인지 명시적으로 결정한다. |

Revision identifiers are dates in YYYY-MM-DD form marking the last day backwards incompatible changes were made. A revision is Draft (in progress), Current (in use and still able to take backwards compatible changes; 2026-07-28 today), or Final (complete and frozen).

> 리비전 식별자는 YYYY-MM-DD 형식의 날짜다. 하위 호환이 깨지는 변경이 마지막으로 들어간 날을 나타낸다. 리비전 상태는 Draft(작업 중), Current(사용 중이며 하위 호환 변경은 아직 받을 수 있음. 현재는 2026-07-28), Final(완료되어 동결됨) 중 하나다.

Revision history, newest first:

> 리비전 이력이다. 최신순으로 적는다:

| Revision | Headline changes |
|---|---|
| 2026-07-28 | Stateless core; `initialize` and sessions removed; `server/discover`; Multi Round-Trip Requests (MRTR); `resultType`; `subscriptions/listen`; tasks moved to an extension; roots, sampling, logging, and Dynamic Client Registration deprecated; cacheable results; required HTTP headers; error-code allocation policy; feature lifecycle policy. |
| 2025-11-25 | Icons; incremental scope consent via `WWW-Authenticate`; tool-name guidance; URL-mode elicitation; tool calling inside sampling; Client ID Metadata Documents recommended; experimental tasks; input validation errors become tool execution errors (SEP-1303); JSON Schema 2020-12 default dialect; governance and SDK tiering. |
| 2025-06-18 | Structured tool output (`outputSchema`, `structuredContent`); resource links in tool results; elicitation; OAuth resource-server classification with Protected Resource Metadata; resource indicators required; `MCP-Protocol-Version` header on HTTP; JSON-RPC batching removed. |
| 2025-03-26 | Streamable HTTP replaces HTTP+SSE; OAuth 2.1 authorization framework; tool annotations; audio content; completions capability. |
| 2024-11-05 | First public revision: stdio and HTTP+SSE transports, `initialize` handshake. |

| 리비전 | 주요 변경 |
|---|---|
| 2026-07-28 | stateless 코어; `initialize`와 세션 제거; `server/discover`; Multi Round-Trip Requests(MRTR); `resultType`; `subscriptions/listen`; tasks를 extension으로 이동; roots, sampling, logging, Dynamic Client Registration 폐기(deprecated); 캐시 가능한 결과; 필수 HTTP 헤더; 오류 코드 할당 정책; 기능 수명 주기 정책. |
| 2025-11-25 | 아이콘; `WWW-Authenticate`를 통한 점진적 scope 동의; tool 이름 가이드; URL 모드 elicitation; sampling 안에서 tool 호출; Client ID Metadata Documents 권장; 실험적 tasks; 입력 검증 오류가 tool 실행 오류로 바뀜(SEP-1303); JSON Schema 2020-12가 기본 dialect; 거버넌스와 SDK 등급제. |
| 2025-06-18 | 구조화된 tool 출력(`outputSchema`, `structuredContent`); tool 결과 안의 resource 링크; elicitation; Protected Resource Metadata를 갖춘 OAuth resource server 분류; resource indicator 필수; HTTP에서 `MCP-Protocol-Version` 헤더; JSON-RPC 배칭 제거. |
| 2025-03-26 | Streamable HTTP가 HTTP+SSE를 대체; OAuth 2.1 인가 프레임워크; tool annotation; 오디오 콘텐츠; completions capability. |
| 2024-11-05 | 첫 공개 리비전: stdio와 HTTP+SSE transport, `initialize` handshake. |

What 2026-07-28 removed (never teach these as current):

> 2026-07-28이 제거한 것이다. 현행 기능으로 가르치지 않는다:

- `initialize` request and `notifications/initialized` (SEP-2575).
- Protocol sessions and the `Mcp-Session-Id` header (SEP-2567).
- The Streamable HTTP GET stream endpoint and session `DELETE`.
- `resources/subscribe` and `resources/unsubscribe` (replaced by `subscriptions/listen`).
- `ping`, `logging/setLevel`, `notifications/roots/list_changed`.
- SSE resumability and redelivery (`Last-Event-ID`, SSE event IDs).
- Server-initiated requests on any stream (replaced by MRTR).
- Tasks in the core protocol (moved to `io.modelcontextprotocol/tasks`, redesigned: no `tasks/result`, no `tasks/list`).
- `notifications/elicitation/complete` and the `elicitationId` field of URL-mode elicitation.
- Error codes `-32002` (resource not found, now `-32602`) and `-32042` (URL elicitation required).

> - `initialize` 요청과 `notifications/initialized` (SEP-2575).
> - 프로토콜 세션과 `Mcp-Session-Id` 헤더 (SEP-2567).
> - Streamable HTTP의 GET 스트림 엔드포인트와 세션 `DELETE`.
> - `resources/subscribe`와 `resources/unsubscribe` (`subscriptions/listen`으로 대체).
> - `ping`, `logging/setLevel`, `notifications/roots/list_changed`.
> - SSE 재개와 재전송(`Last-Event-ID`, SSE 이벤트 ID).
> - 모든 스트림에서 server가 시작하는 요청 (MRTR로 대체).
> - 코어 프로토콜의 tasks (`io.modelcontextprotocol/tasks`로 옮기고 재설계함. `tasks/result`와 `tasks/list`는 없다).
> - `notifications/elicitation/complete`와 URL 모드 elicitation의 `elicitationId` 필드.
> - 오류 코드 `-32002`(resource not found, 이제 `-32602`)와 `-32042`(URL elicitation required).

## 2. Messages

**한국어 제목: 메시지**

All messages follow JSON-RPC 2.0 with these MCP rules:

> 모든 메시지는 JSON-RPC 2.0을 따르며 다음 MCP 규칙이 더해진다:

- **Request**: `{"jsonrpc":"2.0","id":<string|integer>,"method":...,"params":{...}}`. The id MUST NOT be null and MUST NOT repeat an id still awaiting a response.
- **Result response**: same id as the request; `result` MUST include `resultType`.
  - `"complete"`: the final content.
  - `"input_required"`: an `InputRequiredResult` (MRTR, section 7).
  - Extensions MAY add values only when advertised through capabilities (the tasks extension adds `"task"`).
  - A client MUST treat an unrecognized `resultType` as invalid, and MUST treat an absent `resultType` (legacy server) as `"complete"`.
- **Error response**: `{"jsonrpc":"2.0","id":...,"error":{"code":<int>,"message":...,"data":...}}`. The id is omitted only when it could not be read from a malformed request.
- **Notification**: a `method` and optional `params`, and MUST NOT include an id. The receiver MUST NOT respond.
- No JSON-RPC batching (removed in 2025-06-18). On Streamable HTTP each POST body is exactly one request or notification.

> - **Request**: `{"jsonrpc":"2.0","id":<string|integer>,"method":...,"params":{...}}`. id는 null이면 안 된다(MUST NOT). 아직 응답을 기다리는 id를 다시 쓰면 안 된다(MUST NOT).
> - **Result response**: 요청과 같은 id를 쓴다. `result`에는 `resultType`이 있어야 한다(MUST).
>   - `"complete"`: 최종 콘텐츠.
>   - `"input_required"`: `InputRequiredResult` (MRTR, 7절).
>   - extension은 capability로 알린 경우에만 값을 추가할 수 있다(MAY). tasks extension은 `"task"`를 추가한다.
>   - client는 알 수 없는 `resultType`을 무효로 처리해야 한다(MUST). `resultType`이 없으면(legacy server) `"complete"`로 처리해야 한다(MUST).
> - **Error response**: `{"jsonrpc":"2.0","id":...,"error":{"code":<int>,"message":...,"data":...}}`. 잘못된 형식의 요청에서 id를 읽을 수 없을 때만 id를 생략한다.
> - **Notification**: `method`와 선택적 `params`로 이뤄지며 id를 넣으면 안 된다(MUST NOT). 수신자는 응답하면 안 된다(MUST NOT).
> - JSON-RPC 배칭은 없다(2025-06-18에 제거). Streamable HTTP에서 각 POST 본문은 정확히 요청 하나 또는 notification 하나다.

## 3. `_meta` and per-request protocol fields

**한국어 제목: `_meta`와 요청별 프로토콜 필드**

Key names: an optional prefix (dot-separated labels plus `/`, reverse DNS recommended) and a name
that begins and ends with an alphanumeric character. Any prefix whose second label is
`modelcontextprotocol` or `mcp` is reserved for MCP (`io.modelcontextprotocol/`, `dev.mcp/`); `com.example.mcp/` is not reserved.

> 키 이름은 선택적 접두사와 이름으로 이뤄진다. 접두사는 점으로 구분한 라벨 뒤에 `/`를 붙인
> 형태이며 역 DNS 표기를 권장한다. 이름은 영숫자로 시작하고 영숫자로 끝난다. 두 번째 라벨이
> `modelcontextprotocol` 또는 `mcp`인 접두사는 모두 MCP 예약이다(`io.modelcontextprotocol/`,
> `dev.mcp/`). `com.example.mcp/`는 예약이 아니다.

Required on every client request (missing ones make the request malformed: JSON-RPC `-32602`, and HTTP `400 Bad Request`):

> 모든 client 요청에 있어야 하는 키다. 빠지면 요청이 잘못된 형식이 된다(JSON-RPC `-32602`, HTTP `400 Bad Request`):

| Key | Required | Meaning |
|---|---|---|
| `io.modelcontextprotocol/protocolVersion` | yes | e.g. `"2026-07-28"` |
| `io.modelcontextprotocol/clientCapabilities` | yes | capabilities relevant to this request (may be `{}`) |
| `io.modelcontextprotocol/clientInfo` | no (SHOULD) | `Implementation`: `name`, `version`, optional `title`, `description`, `websiteUrl`, `icons` |
| `io.modelcontextprotocol/logLevel` | no | minimum log level for this request (deprecated logging feature) |
| `progressToken` | no | opts the request into `notifications/progress` |

| 키 | 필수 | 의미 |
|---|---|---|
| `io.modelcontextprotocol/protocolVersion` | 예 | 예: `"2026-07-28"` |
| `io.modelcontextprotocol/clientCapabilities` | 예 | 이 요청에 관련된 capability (`{}`일 수 있음) |
| `io.modelcontextprotocol/clientInfo` | 아니오 (SHOULD) | `Implementation`: `name`, `version`, 선택적 `title`, `description`, `websiteUrl`, `icons` |
| `io.modelcontextprotocol/logLevel` | 아니오 | 이 요청의 최소 로그 레벨 (폐기된 logging 기능) |
| `progressToken` | 아니오 | 이 요청이 `notifications/progress`를 받도록 한다 |

On results, servers SHOULD include `io.modelcontextprotocol/serverInfo` in `result._meta`.
`clientInfo` and `serverInfo` are self-reported: use them for display, logging, and debugging, never for security decisions.

> 결과에서 server는 `result._meta`에 `io.modelcontextprotocol/serverInfo`를 넣어야 한다(SHOULD).
> `clientInfo`와 `serverInfo`는 자기 신고 값이다. 표시, 로깅, 디버깅에만 쓰고 보안 판단에는 절대 쓰지 않는다.

Other reserved keys: `io.modelcontextprotocol/subscriptionId` (on every message of a `subscriptions/listen` stream), and `traceparent`, `tracestate`, `baggage` (OpenTelemetry context, W3C Trace Context and W3C Baggage formats; the one exception to the prefix rule, SEP-414).

> 그 밖의 예약 키는 다음과 같다. `io.modelcontextprotocol/subscriptionId`는 `subscriptions/listen` 스트림의 모든 메시지에 붙는다. `traceparent`, `tracestate`, `baggage`는 OpenTelemetry 컨텍스트이며 W3C Trace Context와 W3C Baggage 형식을 따른다. 이 셋은 접두사 규칙의 유일한 예외다(SEP-414).

Minimal modern request:

> 최소 형태의 modern 요청 예시다:

```json
{
  "jsonrpc": "2.0",
  "id": 7,
  "method": "tools/call",
  "params": {
    "name": "get_weather",
    "arguments": {"location": "Pune"},
    "_meta": {
      "io.modelcontextprotocol/protocolVersion": "2026-07-28",
      "io.modelcontextprotocol/clientCapabilities": {},
      "io.modelcontextprotocol/clientInfo": {"name": "ExampleClient", "version": "1.0.0"}
    }
  }
}
```

## 4. Statelessness

**한국어 제목: 무상태성(Statelessness)**

- All information needed to process a request is in the request. A server MUST NOT rely on prior requests on the same connection for capabilities, version, or identity.
- A stdio process or HTTP connection is not a session. Clients may interleave unrelated requests on one transport.
- State that spans requests (long-running work, carts, browser contexts) MUST be referenced by an explicit identifier the client passes on each request: a server-minted handle returned in a tool result and passed back as an ordinary argument (SEP-2567). Handles are names, not capabilities: authorize the caller against the handle on every call, keep handles opaque, state their lifetime in the tool description, and report an expired handle as a tool execution error.
- `tools/list`, `resources/list`, and `prompts/list` MUST NOT vary per connection or as a side effect of other requests. They MAY vary by the authorization presented on the request.
- Why: any replica can serve any request, retries are safe to route anywhere, and load balancers need no stickiness.

> - 요청 처리에 필요한 정보는 모두 요청 안에 있다. server는 capability, 버전, 신원을 같은 연결의 이전 요청에 의존하면 안 된다(MUST NOT).
> - stdio 프로세스나 HTTP 연결은 세션이 아니다. client는 transport 하나에서 서로 무관한 요청을 섞어 보낼 수 있다.
> - 여러 요청에 걸친 상태(오래 걸리는 작업, 장바구니, 브라우저 컨텍스트)는 client가 요청마다 넘기는 명시적 식별자로 참조해야 한다(MUST). 이 식별자는 server가 발급해 tool 결과로 돌려준 handle이며 client는 이를 일반 인자로 다시 넘긴다(SEP-2567). handle은 이름일 뿐 권한(capability)이 아니다. 호출마다 그 handle에 대해 호출자를 인가하고 handle을 불투명하게 유지한다. handle 수명은 tool 설명에 적고 만료된 handle은 tool 실행 오류로 보고한다.
> - `tools/list`, `resources/list`, `prompts/list`는 연결마다 달라지거나 다른 요청의 부수 효과로 달라지면 안 된다(MUST NOT). 요청에 제시된 인가 정보에 따라서는 달라질 수 있다(MAY).
> - 이유: 어떤 replica든 어떤 요청이든 처리할 수 있고 재시도를 어디로 보내도 안전하다. load balancer에 sticky 설정도 필요 없다.

## 5. Error codes

**한국어 제목: 오류 코드**

| Code | Name | Use |
|---|---|---|
| -32700 | Parse error | invalid JSON |
| -32600 | Invalid Request | not a valid request object |
| -32601 | Method not found | unknown method (on HTTP: `404 Not Found`) |
| -32602 | Invalid params | unknown tool; missing required `_meta`; resource not found; invalid prompt name or missing prompt argument; invalid cursor; invalid log level |
| -32603 | Internal error | server failure |
| -32020 | HeaderMismatch | HTTP header disagrees with body, or a required header is missing (HTTP `400`) |
| -32021 | MissingRequiredClientCapability | `data.requiredCapabilities` lists what the client must declare (HTTP `400`) |
| -32022 | UnsupportedProtocolVersion | `data.supported` (list) and `data.requested` (HTTP `400`) |

| 코드 | 이름 | 용도 |
|---|---|---|
| -32700 | Parse error | 잘못된 JSON |
| -32600 | Invalid Request | 유효한 요청 객체가 아님 |
| -32601 | Method not found | 알 수 없는 메서드 (HTTP에서는 `404 Not Found`) |
| -32602 | Invalid params | 알 수 없는 tool; 필수 `_meta` 누락; resource 없음; 잘못된 prompt 이름 또는 prompt 인자 누락; 잘못된 cursor; 잘못된 로그 레벨 |
| -32603 | Internal error | server 오류 |
| -32020 | HeaderMismatch | HTTP 헤더가 본문과 다르거나 필수 헤더가 없음 (HTTP `400`) |
| -32021 | MissingRequiredClientCapability | client가 선언해야 하는 것을 `data.requiredCapabilities`에 나열 (HTTP `400`) |
| -32022 | UnsupportedProtocolVersion | `data.supported`(목록)와 `data.requested` (HTTP `400`) |

Allocation policy (2026-07-28):

> 할당 정책(2026-07-28):

- `-32000` to `-32019`: legacy. New codes MUST NOT be allocated there and new implementations SHOULD NOT use it at all.
- `-32020` to `-32099`: reserved for the MCP specification. Emitting an undefined code in this range is forbidden.
- Implementations of this revision MUST NOT emit `-32002` or `-32042`. Clients SHOULD still accept `-32002` from older servers as "resource not found".
- Application-defined error codes SHOULD sit outside `-32768` to `-32000`.

> - `-32000`부터 `-32019`까지: legacy 구간. 새 코드를 여기에 할당하면 안 되고(MUST NOT) 새 구현은 이 구간을 아예 쓰지 않아야 한다(SHOULD NOT).
> - `-32020`부터 `-32099`까지: MCP 명세용 예약 구간. 이 구간에서 정의되지 않은 코드를 내보내는 것은 금지된다.
> - 이 리비전의 구현은 `-32002`나 `-32042`를 내보내면 안 된다(MUST NOT). 그래도 client는 이전 server가 보내는 `-32002`를 "resource not found"로 받아들여야 한다(SHOULD).
> - 애플리케이션이 정의하는 오류 코드는 `-32768`부터 `-32000`까지의 범위 밖에 두어야 한다(SHOULD).

Two error channels for tools (the most tested distinction):

> tool의 2가지 오류 경로다(원문 표현으로는 가장 많이 평가되는 구분):

1. **Protocol errors** (JSON-RPC `error`): unknown tool (`-32602`), a request that fails the `CallToolRequest` schema, server errors.
2. **Tool execution errors** (a normal result with `isError: true` and explanatory `content`): API failures, input validation errors such as a wrong date format or an out-of-range value, business-logic refusals, expired handles. These go back to the model so it can self-correct (SEP-1303, 2025-11-25).

> 1. **프로토콜 오류**(JSON-RPC `error`): 알 수 없는 tool(`-32602`), `CallToolRequest` schema를 통과하지 못한 요청, server 오류.
> 2. **Tool 실행 오류**(`isError: true`와 설명용 `content`를 담은 정상 결과): API 실패, 잘못된 날짜 형식이나 범위 밖 값 같은 입력 검증 오류, 비즈니스 로직상 거부, 만료된 handle. 이 오류는 모델에게 돌아가므로 모델이 스스로 고칠 수 있다(SEP-1303, 2025-11-25).

An unknown tool is `-32602`, not `-32601`. Arguments that violate the tool's `inputSchema` are a tool execution error, not `-32602`.

> 알 수 없는 tool은 `-32601`이 아니라 `-32602`다. tool의 `inputSchema`를 어기는 인자는 `-32602`가 아니라 tool 실행 오류다.

## 6. `server/discover` and version negotiation

**한국어 제목: `server/discover`와 버전 협상**

- Servers MUST implement `server/discover`. The request carries only the standard `_meta`.
- `DiscoverResult` (a cacheable result): `resultType`, `supportedVersions`, `capabilities`, optional `instructions` (natural-language guidance for the model), `_meta["io.modelcontextprotocol/serverInfo"]`, `ttlMs`, `cacheScope`.
- Calling it is optional. A client may send any request directly and handle `UnsupportedProtocolVersionError` by retrying with a version from `data.supported`.
- On stdio, a dual-era client SHOULD probe with `server/discover` first: a `DiscoverResult` means modern; a recognized modern error such as `-32022` means modern with a different version (do not fall back); any other error or a timeout means legacy (fall back to `initialize`). The fallback MUST NOT key on one specific error code.
- On HTTP, a dual-era client attempts a modern request and inspects a `400` body: a recognized modern JSON-RPC error means modern; anything else means legacy.
- Cache the era decision per server process (stdio) or origin (HTTP).
- A modern-only server SHOULD name its supported versions in any error it returns to `initialize`.

> - server는 `server/discover`를 구현해야 한다(MUST). 이 요청에는 표준 `_meta`만 담긴다.
> - `DiscoverResult`(캐시 가능한 결과): `resultType`, `supportedVersions`, `capabilities`, 선택적 `instructions`(모델을 위한 자연어 안내), `_meta["io.modelcontextprotocol/serverInfo"]`, `ttlMs`, `cacheScope`.
> - 호출은 선택 사항이다. client는 어떤 요청이든 바로 보낼 수 있다. `UnsupportedProtocolVersionError`를 받으면 `data.supported`에 있는 버전으로 재시도하면 된다.
> - stdio에서 dual-era client는 먼저 `server/discover`로 확인해야 한다(SHOULD). `DiscoverResult`가 오면 modern이다. `-32022` 같은 알려진 modern 오류가 오면 버전만 다른 modern이다(fallback하지 않는다). 그 밖의 오류나 timeout이면 legacy다(`initialize`로 fallback한다). fallback 판단을 특정 오류 코드 하나에 걸면 안 된다(MUST NOT).
> - HTTP에서 dual-era client는 modern 요청을 시도하고 `400` 응답 본문을 살핀다. 알려진 modern JSON-RPC 오류면 modern이고 그 밖의 것이면 legacy다.
> - era 판단은 server 프로세스(stdio) 또는 origin(HTTP) 단위로 캐시한다.
> - modern 전용 server는 `initialize`에 돌려주는 모든 오류에 지원 버전을 적어야 한다(SHOULD).

Server capabilities: `tools {listChanged}`, `resources {listChanged, subscribe}`, `prompts {listChanged}`, `completions {}`, `logging {}` (deprecated), `extensions {...}`, `experimental {...}`.
Client capabilities: `elicitation {form, url}`, `sampling {context, tools}` (deprecated), `roots {}` (deprecated), `extensions {...}`, `experimental {...}`.
A server MUST NOT rely on a capability the client did not declare on that request; if it needs one, it returns `-32021`.

> Server capability: `tools {listChanged}`, `resources {listChanged, subscribe}`, `prompts {listChanged}`, `completions {}`, `logging {}`(폐기), `extensions {...}`, `experimental {...}`.
> Client capability: `elicitation {form, url}`, `sampling {context, tools}`(폐기), `roots {}`(폐기), `extensions {...}`, `experimental {...}`.
> server는 client가 해당 요청에서 선언하지 않은 capability에 의존하면 안 된다(MUST NOT). 그 capability가 필요하면 `-32021`을 돌려준다.

## 7. Multi Round-Trip Requests (MRTR)

**한국어 제목: 다중 왕복 요청(MRTR)**

Replaces every server-initiated request (SEP-2322). Breaking change.

> server가 시작하던 모든 요청을 대체한다(SEP-2322). 하위 호환이 깨지는 변경이다.

1. The client sends a request.
2. The server returns `{"resultType":"input_required","inputRequests":{...},"requestState":"..."}`. It MUST include at least one of `inputRequests` or `requestState`.
3. `inputRequests` is a map from server-chosen keys to request objects, each exactly one of `elicitation/create`, `sampling/createMessage`, or `roots/list`. The server MUST NOT include a request type the client has not declared a capability for.
4. The client gathers the answers and **retries the original request with a new JSON-RPC id**, adding `params.inputResponses` (same keys) and echoing `params.requestState` byte for byte. It MUST NOT include `requestState` if none was given.
5. The server completes with a final result, or asks again with another `InputRequiredResult`.

> 1. client가 요청을 보낸다.
> 2. server가 `{"resultType":"input_required","inputRequests":{...},"requestState":"..."}`를 돌려준다. `inputRequests`와 `requestState` 중 적어도 하나를 넣어야 한다(MUST).
> 3. `inputRequests`는 server가 정한 키를 요청 객체에 대응시킨 map이다. 각 요청 객체는 `elicitation/create`, `sampling/createMessage`, `roots/list` 중 정확히 하나다. server는 client가 capability를 선언하지 않은 요청 유형을 넣으면 안 된다(MUST NOT).
> 4. client는 답을 모은 뒤 **새 JSON-RPC id로 원래 요청을 재시도한다**. 이때 `params.inputResponses`(같은 키)를 더하고 `params.requestState`를 바이트 단위로 똑같이 돌려보낸다. `requestState`를 받지 않았다면 넣으면 안 된다(MUST NOT).
> 5. server는 최종 결과로 끝내거나 다른 `InputRequiredResult`로 다시 묻는다.

Only `tools/call`, `prompts/get`, and `resources/read` may return `InputRequiredResult`.
`requestState` is attacker-controlled input: if it influences authorization, resource access, or business logic, protect it with an HMAC or AEAD, and bind it to the authenticated principal, a short expiry, and a digest of the originating request. Single-use must be enforced server-side.
`input_required` results and MRTR retries are never cached.

> `tools/call`, `prompts/get`, `resources/read`만 `InputRequiredResult`를 돌려줄 수 있다.
> `requestState`는 공격자가 조작할 수 있는 입력이다. 인가, resource 접근, 비즈니스 로직에 영향을 준다면 HMAC나 AEAD로 보호한다. 또 인증된 주체, 짧은 만료 시간, 원래 요청의 digest에 묶는다. 일회용 여부는 server 쪽에서 강제해야 한다.
> `input_required` 결과와 MRTR 재시도는 절대 캐시하지 않는다.

## 8. Notifications, subscriptions, progress, cancellation

**한국어 제목: notification, 구독, 진행 상황, 취소**

- `subscriptions/listen` opens a long-lived stream. `params.notifications` is a filter: `toolsListChanged`, `promptsListChanged`, `resourcesListChanged` (booleans) and `resourceSubscriptions` (URI list). The server MUST NOT send unrequested types.
- The first message MUST be `notifications/subscriptions/acknowledged`, echoing the subset the server will honor. Every message on the stream carries `_meta["io.modelcontextprotocol/subscriptionId"]`, whose value is the JSON-RPC id of the `subscriptions/listen` request.
- Stream notifications: `notifications/tools/list_changed`, `notifications/prompts/list_changed`, `notifications/resources/list_changed`, `notifications/resources/updated` (with `uri`).
- Request-scoped notifications (`notifications/progress`, `notifications/message`) flow only on the response stream of the request they relate to, never on a listen stream.
- Progress: the client sets `_meta.progressToken` (string or integer, unique among active requests); `notifications/progress` carries `progressToken`, a strictly increasing `progress`, optional `total` and `message`; notifications stop when the request completes.
- Cancellation: on Streamable HTTP, closing the request's SSE response stream is the cancellation (no message). On stdio, the client sends `notifications/cancelled` with `requestId` and optional `reason`. The server sends `notifications/cancelled` only to tear down a `subscriptions/listen` stream. Late responses to cancelled requests are ignored; races are normal.
- Timeouts SHOULD exist for every request, with a hard maximum even when progress arrives.
- A server ending a subscription gracefully SHOULD send a `subscriptions/listen` result with `resultType: "complete"` and the subscription id in `_meta`.

> - `subscriptions/listen`은 오래 유지되는 스트림을 연다. `params.notifications`는 필터다. `toolsListChanged`, `promptsListChanged`, `resourcesListChanged`(불리언)와 `resourceSubscriptions`(URI 목록)로 이뤄진다. server는 요청받지 않은 유형을 보내면 안 된다(MUST NOT).
> - 첫 메시지는 `notifications/subscriptions/acknowledged`여야 하며(MUST) server가 지킬 부분집합을 되돌려 준다. 스트림의 모든 메시지에는 `_meta["io.modelcontextprotocol/subscriptionId"]`가 붙는다. 그 값은 `subscriptions/listen` 요청의 JSON-RPC id다.
> - 스트림 notification: `notifications/tools/list_changed`, `notifications/prompts/list_changed`, `notifications/resources/list_changed`, `notifications/resources/updated`(`uri` 포함).
> - 요청 범위 notification(`notifications/progress`, `notifications/message`)은 관련 요청의 응답 스트림으로만 흐른다. listen 스트림으로는 절대 보내지 않는다.
> - Progress: client가 `_meta.progressToken`을 설정한다(문자열 또는 정수, 활성 요청 사이에서 고유). `notifications/progress`에는 `progressToken`, 엄격하게 증가하는 `progress`, 선택적 `total`과 `message`가 담긴다. 요청이 끝나면 notification도 멈춘다.
> - 취소: Streamable HTTP에서는 요청의 SSE 응답 스트림을 닫는 것이 곧 취소다(메시지 없음). stdio에서는 client가 `requestId`와 선택적 `reason`을 담아 `notifications/cancelled`를 보낸다. server는 `subscriptions/listen` 스트림을 끝낼 때만 `notifications/cancelled`를 보낸다. 취소된 요청에 늦게 온 응답은 무시한다. 경쟁 상태는 정상이다.
> - 모든 요청에 timeout을 두어야 한다(SHOULD). progress가 계속 오더라도 넘을 수 없는 최대 시간을 둔다.
> - subscription을 정상 종료하는 server는 `resultType: "complete"`와 `_meta` 안의 subscription id를 담은 `subscriptions/listen` 결과를 보내야 한다(SHOULD).

## 9. Transports

**한국어 제목: transport(전송 계층)**

**stdio**

- The client launches the server as a subprocess. Newline-delimited JSON-RPC, no embedded newlines.
- `stdout` carries only MCP messages; `stderr` carries logs (not necessarily errors).
- The server never writes requests to `stdout`. All metadata is inline in `_meta` (no header layer).
- Shutdown: the client closes `stdin`, waits, then escalates (SIGTERM, SIGKILL). On unexpected exit the client restarts the server, retries lost requests, and re-sends `subscriptions/listen`.
- Authorization: stdio implementations SHOULD NOT use the OAuth flow and instead read credentials from the environment.

> - client가 server를 하위 프로세스로 실행한다. 줄바꿈으로 구분한 JSON-RPC를 쓰며 메시지 안에는 줄바꿈을 넣지 않는다.
> - `stdout`에는 MCP 메시지만 흐른다. `stderr`에는 로그가 흐른다(반드시 오류인 것은 아니다).
> - server는 `stdout`에 요청을 절대 쓰지 않는다. 모든 메타데이터는 `_meta`에 인라인으로 들어간다(헤더 계층 없음).
> - 종료: client가 `stdin`을 닫고 기다린 뒤 단계적으로 강도를 높인다(SIGTERM, SIGKILL). server가 예기치 않게 종료되면 client는 server를 다시 시작하고 잃어버린 요청을 재시도하며 `subscriptions/listen`을 다시 보낸다.
> - 인가: stdio 구현은 OAuth 흐름을 쓰지 않아야 하며(SHOULD NOT) 대신 환경 변수에서 자격 증명을 읽는다.

**Streamable HTTP**

- One MCP endpoint that accepts POST (e.g. `https://example.com/mcp`). Every client message is its own POST.
- `Accept` lists both `application/json` and `text/event-stream`. The response to a request is either one JSON object or an SSE stream scoped to that request; the client MUST support both.
- A notification POST returns `202 Accepted` with no body.
- SSE streams carry request-related notifications, then the final response. The server MUST NOT send requests on any stream.
- No GET endpoint, no `Mcp-Session-Id`, no `Last-Event-ID`. A modern-only server answers GET or DELETE with `405` and ignores session and event-id headers.
- Security: validate `Origin` (reply `403` if present and invalid) to stop DNS rebinding; bind to `127.0.0.1` when local; authenticate.
- Required headers: `MCP-Protocol-Version` on every POST (must equal the `_meta` version, else `400` plus `-32020`); `Mcp-Method` on every request; `Mcp-Name` on `tools/call`, `resources/read`, and `prompts/get` (from `params.name` or `params.uri`).
- `x-mcp-header` in a tool's input schema mirrors a primitive argument (integer, string, or boolean, never `number`, and only properties statically reachable from the schema root) into `Mcp-Param-{Name}`. Server developers SHOULD NOT mark sensitive parameters (passwords, API keys, tokens, PII), because header values are visible to intermediaries. Values that are not plain ASCII use the sentinel `=?base64?...?=`. Clients on HTTP MUST reject a tool whose `x-mcp-header` values violate the constraints by excluding it from their `tools/list` result. Servers MUST reject a header that disagrees with the body (`-32020`).
- Status codes: unsupported version `400` plus `-32022`; missing capability `400` plus `-32021`; unknown method `404` plus `-32601`; missing or invalid token `401`; insufficient scope `403`.
- HTTP+SSE, the 2024-11-05 transport, has been Deprecated since 2025-03-26 (SEP-2596); its earliest removal is three months after SEP-2596 reaches Final.

> - POST를 받는 MCP 엔드포인트는 하나다(예: `https://example.com/mcp`). client 메시지는 각각 별도의 POST다.
> - `Accept`에는 `application/json`과 `text/event-stream`을 모두 나열한다. 요청에 대한 응답은 JSON 객체 하나이거나 그 요청 범위의 SSE 스트림이다. client는 둘 다 지원해야 한다(MUST).
> - notification POST는 본문 없이 `202 Accepted`를 돌려준다.
> - SSE 스트림은 요청 관련 notification을 보낸 뒤 최종 응답을 보낸다. server는 어떤 스트림에서도 요청을 보내면 안 된다(MUST NOT).
> - GET 엔드포인트, `Mcp-Session-Id`, `Last-Event-ID`는 없다. modern 전용 server는 GET이나 DELETE에 `405`로 답하고 세션 헤더와 event-id 헤더는 무시한다.
> - 보안: DNS rebinding을 막으려면 `Origin`을 검증한다(헤더가 있는데 유효하지 않으면 `403`으로 응답). 로컬이면 `127.0.0.1`에 바인딩한다. 인증을 수행한다.
> - 필수 헤더: 모든 POST에 `MCP-Protocol-Version`(`_meta`의 버전과 같아야 하며 다르면 `400`과 `-32020`). 모든 요청에 `Mcp-Method`. `tools/call`, `resources/read`, `prompts/get`에 `Mcp-Name`(`params.name` 또는 `params.uri`에서 가져옴).
> - tool의 input schema에 있는 `x-mcp-header`는 원시 타입 인자 하나를 `Mcp-Param-{Name}` 헤더로 복사한다. 대상은 integer, string, boolean이며 `number`는 안 된다. schema 루트에서 정적으로 닿을 수 있는 속성만 대상이 된다. 헤더 값은 중개자에게 보이므로 server 개발자는 민감한 매개변수(비밀번호, API 키, token, 개인 식별 정보)를 지정하지 않아야 한다(SHOULD NOT). 순수 ASCII가 아닌 값은 `=?base64?...?=` 표식을 쓴다. HTTP client는 `x-mcp-header` 값이 제약을 어기는 tool을 자신의 `tools/list` 결과에서 빼는 방식으로 거부해야 한다(MUST). server는 본문과 다른 헤더를 거부해야 한다(MUST, `-32020`).
> - 상태 코드: 지원하지 않는 버전은 `400`과 `-32022`. capability 누락은 `400`과 `-32021`. 알 수 없는 메서드는 `404`와 `-32601`. token이 없거나 유효하지 않으면 `401`. scope가 부족하면 `403`.
> - 2024-11-05 transport인 HTTP+SSE는 2025-03-26부터 Deprecated 상태다(SEP-2596). 가장 이른 제거 시점은 SEP-2596이 Final이 된 지 3개월 뒤다.

## 10. Server primitives

**한국어 제목: 10. 서버 primitive**

Control model: tools are **model-controlled**, resources are **application-driven**, prompts are **user-controlled**.

> 제어 모델: tool은 **모델이 제어**한다. resource는 **애플리케이션이 주도**한다. prompt는 **사용자가 제어**한다.

**Tools**

> **tool**

- `tools/list` supports pagination and caching, and SHOULD be deterministically ordered (client caching, LLM prompt-cache hits).
- Tool definition: `name`, optional `title`, `description`, optional `icons`, `inputSchema` (a JSON Schema object, never null; for no parameters use `{"type":"object","additionalProperties":false}`), optional `outputSchema`, optional `annotations`, optional `_meta`.
- Names SHOULD be 1 to 128 characters of `A-Za-z0-9_-.`, case-sensitive, unique within a server. Aggregators SHOULD prefix names with a server identifier; `serverInfo.name` is not unique.
- Annotations are hints and MUST be treated as untrusted unless the server is trusted: `title`, `readOnlyHint` (default false), `destructiveHint` (default true, meaningful only when not read-only), `idempotentHint` (default false), `openWorldHint` (default true).
- `tools/call` params: `name`, `arguments`, and on retry `inputResponses` and `requestState`.
- `CallToolResult`: `content` (list of content blocks), optional `structuredContent` (any JSON value; must conform to `outputSchema` when one exists; SHOULD also be serialized into a text block), optional `isError`.
- Content blocks: `text`; `image` (`data`, `mimeType`); `audio`; `resource_link` (`uri`, `name`, optional `description` and `mimeType`); `resource` (embedded contents). All accept `annotations` (`audience`: `user` or `assistant`; `priority` 0 to 1; `lastModified`).
- A human SHOULD be able to deny invocations. Servers MUST validate inputs, enforce access control, rate limit, and sanitize outputs. Clients SHOULD confirm sensitive operations, show inputs before calling, validate results, set timeouts, and log usage.

> - `tools/list`는 페이지네이션과 캐싱을 지원한다. 순서는 결정적이어야 한다(SHOULD). client 캐싱과 LLM prompt 캐시 적중을 위해서다.
> - tool 정의: `name`, 선택 항목 `title`, `description`, 선택 항목 `icons`, `inputSchema`(JSON Schema 객체이며 null이면 안 된다. 매개변수가 없으면 `{"type":"object","additionalProperties":false}`를 쓴다), 선택 항목 `outputSchema`, 선택 항목 `annotations`, 선택 항목 `_meta`.
> - 이름은 `A-Za-z0-9_-.` 문자로 된 1자에서 128자이고 대소문자를 구분하며 한 server 안에서 고유해야 한다(SHOULD). 집계기(aggregator)는 이름 앞에 server 식별자를 붙여야 한다(SHOULD). `serverInfo.name`은 고유하지 않다.
> - annotation은 힌트다. server를 신뢰하는 경우가 아니면 신뢰할 수 없는 값으로 다뤄야 한다(MUST). 항목: `title`, `readOnlyHint`(기본값 false), `destructiveHint`(기본값 true, read-only가 아닐 때만 의미가 있다), `idempotentHint`(기본값 false), `openWorldHint`(기본값 true).
> - `tools/call` 매개변수: `name`, `arguments`. 재시도할 때는 `inputResponses`와 `requestState`도 넣는다.
> - `CallToolResult`: `content`(content block 목록), 선택 항목 `structuredContent`(어떤 JSON 값이든 된다. `outputSchema`가 있으면 그에 맞아야 한다. text block으로도 직렬화해야 한다(SHOULD)), 선택 항목 `isError`.
> - content block: `text`, `image`(`data`, `mimeType`), `audio`, `resource_link`(`uri`, `name`, 선택 항목 `description`과 `mimeType`), `resource`(내장된 내용). 모두 `annotations`를 받는다(`audience`: `user` 또는 `assistant`, `priority` 0에서 1, `lastModified`).
> - 사람이 호출을 거부할 수 있어야 한다(SHOULD). server는 다음을 해야 한다(MUST): 입력 검증, 접근 제어 적용, 호출 빈도 제한(rate limit), 출력 정화. client는 다음을 해야 한다(SHOULD): 민감한 작업 확인, 호출 전 입력 표시, 결과 검증, 타임아웃 설정, 사용 내역 기록.

**Resources**

> **resource**

- `resources/list`, `resources/read`, `resources/templates/list` (RFC 6570 URI templates). Contents are `{uri, mimeType, text}` or `{uri, mimeType, blob}`.
- Capability `resources {listChanged, subscribe}`; updates arrive through `subscriptions/listen` with `resourceSubscriptions`.
- Not found: `-32602` (never an empty `contents` array). Internal: `-32603`. Accept legacy `-32002`.
- URI schemes: `https://` only when the client can fetch it directly; `file://` (need not be a real filesystem; sanitize paths against traversal); `git://`; custom schemes per RFC 3986.

> - `resources/list`, `resources/read`, `resources/templates/list`(RFC 6570 URI 템플릿). 내용은 `{uri, mimeType, text}` 또는 `{uri, mimeType, blob}`이다.
> - capability는 `resources {listChanged, subscribe}`이다. 갱신은 `resourceSubscriptions`를 담은 `subscriptions/listen`으로 도착한다.
> - 찾지 못함: `-32602`(빈 `contents` 배열을 돌려주지 않는다). 내부 오류: `-32603`. 이전 방식의 `-32002`도 받아들인다.
> - URI scheme: `https://`는 client가 직접 가져올 수 있을 때만 쓴다. `file://`는 실제 파일 시스템이 아니어도 된다. 경로 탐색(traversal)에 대비해 경로를 정화한다. 그 밖에 `git://`, RFC 3986에 따른 사용자 정의 scheme이 있다.

**Prompts**

> **prompt**

- `prompts/list`, `prompts/get` with `arguments`; results carry `messages` with a `role` (`user` or `assistant`) and a content block.
- Errors: invalid name or missing required argument is `-32602`; internal is `-32603`.

> - `prompts/list`, `prompts/get`(`arguments` 포함). 결과에는 `messages`가 담긴다. 각 메시지는 `role`(`user` 또는 `assistant`)과 content block 하나를 가진다.
> - 오류: 잘못된 이름이나 필수 인자 누락은 `-32602`이다. 내부 오류는 `-32603`이다.

**Completion**: `completion/complete` with `ref` (`ref/prompt` by name, or `ref/resource` by URI or template), `argument {name, value}`, and `context.arguments`; returns at most 100 `values` plus `total` and `hasMore`. Capability `completions {}`.

> **자동 완성(Completion)**: `completion/complete`는 `ref`(이름으로 지정하는 `ref/prompt`, 또는 URI나 템플릿으로 지정하는 `ref/resource`), `argument {name, value}`, `context.arguments`를 받는다. 최대 100개의 `values`와 `total`, `hasMore`를 돌려준다. capability는 `completions {}`이다.

**Pagination**: opaque `cursor` and `nextCursor`; the server picks page size; an empty string is a valid cursor; an invalid cursor is `-32602`.

> **페이지네이션**: `cursor`와 `nextCursor`는 불투명한 값이다. 페이지 크기는 server가 정한다. 빈 문자열도 유효한 cursor다. 잘못된 cursor는 `-32602`이다.

**Caching** (SEP-2549): `server/discover`, `tools/list`, `prompts/list`, `resources/list`, `resources/templates/list`, and `resources/read` results with `resultType: "complete"` MUST carry `ttlMs` (an integer, 0 or more) and `cacheScope` (`"public"` or `"private"`). `ttlMs` is a freshness hint, not a polling interval; a list-changed notification invalidates the cache immediately. `"private"` caches never cross authorization contexts. `cacheScope` is never an access control.

> **캐싱**(SEP-2549): `resultType: "complete"`인 `server/discover`, `tools/list`, `prompts/list`, `resources/list`, `resources/templates/list`, `resources/read` 결과는 `ttlMs`(0 이상의 정수)와 `cacheScope`(`"public"` 또는 `"private"`)를 담아야 한다(MUST). `ttlMs`는 신선도 힌트이지 폴링 간격이 아니다. list-changed notification이 오면 캐시는 즉시 무효가 된다. `"private"` 캐시는 서로 다른 인가 컨텍스트를 넘나들지 않는다. `cacheScope`는 접근 제어 수단이 아니다.

## 11. Client features

**한국어 제목: 11. client 기능**

**Elicitation** (via MRTR `inputRequests`, method `elicitation/create`)

> **elicitation**(MRTR `inputRequests`로 전달, 메서드 `elicitation/create`)

- Form mode: `mode: "form"` (optional), `message`, and `requestedSchema` (a flat object of primitive properties). URL mode: `mode: "url"`, `message`, `url` (out-of-band pages such as OAuth consent or payment, where sensitive data should not pass through the client).
- `ElicitResult.action`: `accept` (with `content` for form mode), `decline`, or `cancel`.
- Client capability: `elicitation {form, url}`.

> - 폼 모드: `mode: "form"`(선택), `message`, `requestedSchema`(원시 타입 속성으로만 된 평평한 객체). URL 모드: `mode: "url"`, `message`, `url`(OAuth 동의나 결제처럼 대역 외(out-of-band)로 여는 페이지다. 민감한 데이터가 client를 거치지 않아야 하는 경우에 쓴다).
> - `ElicitResult.action`: `accept`(폼 모드에서는 `content`를 함께 보낸다), `decline`, `cancel` 중 하나다.
> - client capability: `elicitation {form, url}`.

**Deprecated features** (the specification's deprecated registry; every one is still functional until removed):

> **지원 중단(deprecated) 기능**(명세의 deprecated 목록이다. 모두 제거되기 전까지는 계속 동작한다):

| Feature | Deprecated by | Deprecated in | Migration | Earliest removal |
|---|---|---|---|---|
| Roots | SEP-2577 | 2026-07-28 | pass directories via tool parameters, resource URIs, or server configuration | first revision released on or after 2027-07-28 |
| Sampling | SEP-2577 | 2026-07-28 | call LLM provider APIs directly | first revision released on or after 2027-07-28 |
| Logging | SEP-2577 | 2026-07-28 | log to `stderr` (stdio) or use OpenTelemetry | first revision released on or after 2027-07-28 |
| Dynamic Client Registration | PR #2858 | 2026-07-28 | Client ID Metadata Documents | first revision released on or after 2027-07-28 |
| `includeContext: "thisServer" \| "allServers"` | SEP-2596 | 2025-11-25 | omit, or use `"none"` | follows Sampling |
| HTTP+SSE transport | SEP-2596 | 2025-03-26 | Streamable HTTP | three months after SEP-2596 reaches Final |

| 기능 | 중단 근거 | 중단 시점 | 이전 방법 | 가장 이른 제거 시점 |
|---|---|---|---|---|
| Roots | SEP-2577 | 2026-07-28 | tool 매개변수, resource URI, server 설정으로 디렉터리를 전달한다 | 2027-07-28 당일 또는 그 이후에 처음 나오는 개정판 |
| Sampling | SEP-2577 | 2026-07-28 | LLM 제공자 API를 직접 호출한다 | 2027-07-28 당일 또는 그 이후에 처음 나오는 개정판 |
| Logging | SEP-2577 | 2026-07-28 | `stderr`(stdio)에 로그를 쓰거나 OpenTelemetry를 쓴다 | 2027-07-28 당일 또는 그 이후에 처음 나오는 개정판 |
| Dynamic Client Registration | PR #2858 | 2026-07-28 | Client ID Metadata Documents | 2027-07-28 당일 또는 그 이후에 처음 나오는 개정판 |
| `includeContext: "thisServer" \| "allServers"` | SEP-2596 | 2025-11-25 | 생략하거나 `"none"`을 쓴다 | Sampling을 따른다 |
| HTTP+SSE transport | SEP-2596 | 2025-03-26 | Streamable HTTP | SEP-2596이 Final이 된 지 3개월 뒤 |

Logging still works per request: a request with `_meta["io.modelcontextprotocol/logLevel"]` may receive `notifications/message` (syslog levels from RFC 5424) on its own response stream. Without that key, the server MUST NOT emit them.

> logging은 요청 단위로는 여전히 동작한다. `_meta["io.modelcontextprotocol/logLevel"]`을 담은 요청은 자기 응답 스트림으로 `notifications/message`(RFC 5424의 syslog 레벨)를 받을 수 있다. 이 키가 없으면 server는 이 메시지를 보내면 안 된다(MUST NOT).

## 12. Authorization (HTTP transports)

**한국어 제목: 12. 인가(HTTP transport)**

- Optional overall. HTTP implementations SHOULD conform; stdio SHOULD NOT (use environment credentials).
- Roles: the MCP server is an OAuth 2.1 resource server, the MCP client is an OAuth 2.1 client, and the authorization server issues tokens.
- MCP servers MUST implement Protected Resource Metadata (RFC 9728), listing at least one entry in `authorization_servers`. Discovery goes through `WWW-Authenticate: Bearer resource_metadata="..."` on a `401`, or the well-known URIs: first the path-specific `/.well-known/oauth-protected-resource/<path>`, then the root.
- Authorization server metadata: RFC 8414 or OpenID Connect Discovery. With a path issuer, the client tries in order `/.well-known/oauth-authorization-server/<path>`, then `/.well-known/openid-configuration/<path>`, then `<path>/.well-known/openid-configuration`. The metadata `issuer` MUST equal the issuer used to build the URL.

> - 전체적으로 선택 사항이다. HTTP 구현은 따라야 한다(SHOULD). stdio는 따르지 않아야 한다(SHOULD NOT). stdio는 환경에서 얻은 자격 증명을 쓴다.
> - 역할: MCP server는 OAuth 2.1 resource server이고 MCP client는 OAuth 2.1 client이다. 인가 server(authorization server)가 token을 발급한다.
> - MCP server는 Protected Resource Metadata(RFC 9728)를 구현해야 하며(MUST) `authorization_servers`에 항목을 하나 이상 넣는다. 탐색 경로는 두 가지다. 하나는 `401` 응답의 `WWW-Authenticate: Bearer resource_metadata="..."` 헤더다. 다른 하나는 well-known URI다. 경로별 `/.well-known/oauth-protected-resource/<path>`를 먼저 보고 그다음 루트를 본다.
> - 인가 server 메타데이터: RFC 8414 또는 OpenID Connect Discovery. issuer에 경로가 있으면 client는 `/.well-known/oauth-authorization-server/<path>`, `/.well-known/openid-configuration/<path>`, `<path>/.well-known/openid-configuration` 순서로 시도한다. 메타데이터의 `issuer`는 URL을 만들 때 쓴 issuer와 같아야 한다(MUST).
- PKCE is mandatory, with `S256` when capable. If `code_challenge_methods_supported` is absent, the client MUST refuse to proceed.
- Resource Indicators (RFC 8707): the `resource` parameter goes on both the authorization request and the token request, holding the canonical server URI (scheme plus host, no fragment, no trailing slash by convention). Clients send it even if the authorization server ignores it.
- Tokens travel in `Authorization: Bearer <token>` on every HTTP request, never in the query string.
- Servers MUST validate the audience (the token was issued for this server) and MUST NOT accept or pass through other tokens. Upstream API calls use a separate token (token passthrough is forbidden).

> - PKCE는 필수다. 가능하면 `S256`을 쓴다. `code_challenge_methods_supported`가 없으면 client는 진행을 거부해야 한다(MUST).
> - Resource Indicators(RFC 8707): `resource` 매개변수를 인가 요청과 token 요청 모두에 넣는다. 값은 server의 정규 URI다(scheme과 host, fragment 없음, 관례상 끝 슬래시 없음). 인가 server가 이 값을 무시하더라도 client는 보낸다.
> - token은 모든 HTTP 요청의 `Authorization: Bearer <token>` 헤더로 보낸다. 쿼리 문자열에는 절대 넣지 않는다.
> - server는 audience를 검증해야 한다(MUST). 즉 token이 이 server용으로 발급됐는지 확인한다. 다른 token은 받거나 그대로 넘기면 안 된다(MUST NOT). 상위(upstream) API 호출에는 별도 token을 쓴다(token passthrough 금지).
- Issuer validation (RFC 9207): the client records the expected issuer before redirecting and compares the returned `iss` exactly (no normalization); mismatches are rejected, including on error responses. This defends against mix-up attacks.
- Scopes: use the `scope` from the first `WWW-Authenticate` challenge, otherwise `scopes_supported`. At runtime, an insufficient scope returns `403` with `error="insufficient_scope"` and the full set of required scopes in one challenge. The client steps up with the union of old and new scopes and a limited number of retries.
- Refresh tokens: never assume one is issued; public clients' refresh tokens are rotated; servers should not demand `offline_access`.
- Status codes: `401` (no token or invalid token), `403` (insufficient scope), `400` (malformed authorization request).

> - issuer 검증(RFC 9207): client는 리디렉션 전에 기대하는 issuer를 기록한다. 돌아온 `iss`와 정확히 비교한다(정규화 없음). 일치하지 않으면 거부하며 오류 응답도 예외가 아니다. mix-up 공격을 막기 위한 장치다.
> - scope: 첫 `WWW-Authenticate` challenge의 `scope`를 쓰고 없으면 `scopes_supported`를 쓴다. 실행 중 scope가 부족하면 `403`과 `error="insufficient_scope"`를 돌려준다. 이때 필요한 scope 전체를 challenge 하나에 담는다. client는 기존 scope와 새 scope의 합집합으로 권한을 올려(step-up) 다시 요청한다. 재시도 횟수는 제한한다.
> - refresh token: 발급된다고 가정하지 않는다. public client의 refresh token은 교체(rotation)된다. server는 `offline_access`를 요구하지 않아야 한다.
> - 상태 코드: `401`(token 없음 또는 유효하지 않은 token), `403`(scope 부족), `400`(형식이 잘못된 인가 요청).

**Client registration priority**: (1) pre-registered credentials; (2) Client ID Metadata Documents when the authorization server advertises `client_id_metadata_document_supported`; (3) Dynamic Client Registration (deprecated) when a `registration_endpoint` exists; (4) ask the user.

> **client 등록 우선순위**: (1) 미리 등록한 자격 증명, (2) 인가 server가 `client_id_metadata_document_supported`를 알리면 Client ID Metadata Documents, (3) `registration_endpoint`가 있으면 Dynamic Client Registration(deprecated), (4) 사용자에게 묻기.

- CIMD: `client_id` is an HTTPS URL with a path, pointing at JSON containing at least `client_id` (exactly equal to the URL), `client_name`, and `redirect_uris`. The authorization server fetches it, validates it, validates redirect URIs, and guards against SSRF. It shows extra warnings for localhost-only redirect URIs and always displays the redirect hostname. CIMD ids are portable across authorization servers.
- DCR: the client MUST send `application_type` (`native` for desktop, CLI, and localhost apps; `web` for remote browser apps).
- Authorization-server binding: persisted credentials are keyed by the issuer, never reused with a different authorization server, and re-registered when the authorization server changes.
- Confused deputy: an MCP proxy using a static client id MUST get user consent for each dynamically registered client before forwarding to a third-party authorization server.
- Communication: every authorization-server endpoint uses HTTPS; redirect URIs are localhost or HTTPS and registered exactly; use and verify `state`.

> - CIMD: `client_id`는 경로가 있는 HTTPS URL이다. 이 URL은 최소한 `client_id`(URL과 정확히 같은 값), `client_name`, `redirect_uris`를 담은 JSON을 가리킨다. 인가 server는 이 문서를 가져와 검증하고 redirect URI도 검증한다. SSRF도 막는다. localhost 전용 redirect URI에는 추가 경고를 보여 주고 redirect 호스트 이름은 항상 표시한다. CIMD id는 여러 인가 server에서 그대로 쓸 수 있다.
> - DCR: client는 `application_type`을 보내야 한다(MUST). 데스크톱, CLI, localhost 앱은 `native`, 원격 브라우저 앱은 `web`이다.
> - 인가 server 바인딩: 저장한 자격 증명은 issuer를 키로 한다. 다른 인가 server에 재사용하지 않는다. 인가 server가 바뀌면 다시 등록한다.
> - confused deputy: 고정 client id를 쓰는 MCP 프록시는 제3자 인가 server로 전달하기 전에 동적으로 등록된 client마다 사용자 동의를 받아야 한다(MUST).
> - 통신: 모든 인가 server 엔드포인트는 HTTPS를 쓴다. redirect URI는 localhost 또는 HTTPS여야 하고 정확히 일치하게 등록한다. `state`를 쓰고 검증한다.

**Authorization extensions** (never active by default, negotiated through `extensions`): OAuth Client Credentials for machine-to-machine clients (CI, daemons) and Enterprise-Managed Authorization for central IdP policy.

> **인가 extension**(기본으로는 절대 켜지지 않는다. `extensions`로 협상한다): 기계 간(machine-to-machine) client(CI, 데몬)를 위한 OAuth Client Credentials, 그리고 중앙 IdP 정책을 위한 Enterprise-Managed Authorization.

## 13. Security checklist beyond authorization

**한국어 제목: 13. 인가 외 보안 점검 목록**

- Tool descriptions, annotations, icons, and results are untrusted input to the model (poisoning, prompt injection, rug pulls through changed definitions).
- Icons: only `https:` or `data:` URIs, no credentials when fetching, same origin as the server, treat SVG as potentially executable.
- JSON Schema: never auto-dereference a network `$ref` (opt-in only, with a host allowlist, no private addresses, timeouts); bound composition keywords against denial of service.
- Streamable HTTP: `Origin` validation, localhost binding, authentication.
- MRTR `requestState`: integrity protection, principal binding, expiry, request binding.
- Logs never contain credentials, secrets, or personal data.
- Caching: `"public"` responses can be shared across users and tokens; per-primitive access control is still required.

> - tool 설명, annotation, 아이콘, 결과는 모델에게 신뢰할 수 없는 입력이다(poisoning, prompt injection, 정의를 바꾸는 rug pull).
> - 아이콘: `https:` 또는 `data:` URI만 허용한다. 가져올 때 자격 증명을 보내지 않는다. server와 같은 origin이어야 한다. SVG는 실행될 수 있는 것으로 다룬다.
> - JSON Schema: 네트워크 `$ref`를 자동으로 역참조하지 않는다(명시적으로 켰을 때만 허용하며 호스트 허용 목록, 사설 주소 차단, 타임아웃을 둔다). 서비스 거부를 막기 위해 조합 키워드에 한도를 둔다.
> - Streamable HTTP: `Origin` 검증, localhost 바인딩, 인증.
> - MRTR `requestState`: 무결성 보호, principal 바인딩, 만료, 요청 바인딩.
> - 로그에는 자격 증명, 비밀 값, 개인 정보를 절대 남기지 않는다.
> - 캐싱: `"public"` 응답은 여러 사용자와 token 사이에서 공유될 수 있다. primitive별 접근 제어는 여전히 필요하다.

## 14. Extensions

**한국어 제목: 14. extension**

- Identifier: `{vendor-prefix}/{name}` under the `_meta` naming rules. Official extensions use `io.modelcontextprotocol/`; third parties use a reversed domain they own (`com.example/my-extension`).
- Negotiation: clients declare support in `io.modelcontextprotocol/clientCapabilities.extensions` on each request; servers declare theirs in `server/discover` `capabilities.extensions`. The value is the extension's settings object (`{}` means supported with no settings).
- If only one side supports an extension, the supporting side falls back to core behavior or rejects the request with an appropriate error. Extensions are disabled by default and need explicit opt-in. A breaking change needs a new identifier (e.g. `-v2`).
- Process: SEP-2133 (Extensions Track SEP, a reference implementation in an official SDK, review by the Core Maintainers). Experimental repositories use the `experimental-ext-` prefix and belong to a Working Group or Interest Group.

> - 식별자: `_meta` 이름 규칙을 따르는 `{vendor-prefix}/{name}`. 공식 extension은 `io.modelcontextprotocol/`을 쓴다. 제3자는 자기가 소유한 도메인을 뒤집어 쓴다(`com.example/my-extension`).
> - 협상: client는 요청마다 `io.modelcontextprotocol/clientCapabilities.extensions`에 지원하는 extension을 선언한다. server는 `server/discover`의 `capabilities.extensions`에 선언한다. 값은 extension의 설정 객체다(`{}`는 설정 없이 지원한다는 뜻).
> - 한쪽만 extension을 지원하면 지원하는 쪽이 core 동작으로 돌아가거나 적절한 오류로 요청을 거부한다. extension은 기본으로 꺼져 있고 명시적으로 켜야 한다. 호환이 깨지는 변경에는 새 식별자가 필요하다(예: `-v2`).
> - 절차: SEP-2133(Extensions Track SEP, 공식 SDK의 참조 구현, Core Maintainers 검토). 실험용 저장소는 `experimental-ext-` 접두사를 쓰며 Working Group이나 Interest Group에 속한다.
- Official extensions:
  - **Tasks** `io.modelcontextprotocol/tasks` (SEP-2663): the server may answer a supported request with `resultType: "task"` (`CreateTaskResult` with `taskId`, status, TTL, suggested poll interval). The client polls `tasks/get`, supplies mid-flight input with `tasks/update` (`inputResponses`), and may send `tasks/cancel` (cooperative). Statuses: `working`, `input_required`, `completed` (carries `result`), `failed` (carries `error`), `cancelled`. There is no `tasks/result` and no `tasks/list`.
  - **MCP Apps** `io.modelcontextprotocol/ui` (SEP-1865): a tool's `_meta.ui` carries `resourceUri`, pointing to a `ui://` resource of type `text/html;profile=mcp-app` fetched with an ordinary `resources/read`, and `visibility` (default `["model", "app"]`; the host MUST NOT list a tool to the agent when it lacks `"model"` and MUST reject an app's `tools/call` for a tool lacking `"app"`). The UI resource's own `_meta.ui` (on its `resources/read` contents entry) carries `csp`, an object of optional `connectDomains`, `resourceDomains`, `frameDomains`, and `baseUriDomains` origin lists, and `permissions`, an object of optional `camera`, `microphone`, `geolocation`, and `clipboardWrite` flags set to `{}`. The host MUST build its CSP from the declared domains, MUST NOT allow undeclared domains, MAY restrict further, and MUST apply a restrictive default (`connect-src 'none'`) when `csp` is omitted; it MAY honor `permissions`, and apps SHOULD NOT assume they were granted. The view runs in a sandboxed iframe (web hosts wrap it in a sandbox proxy on a different origin); the app and host talk through a JSON-RPC dialect with `ui/` methods, starting with the app's `ui/initialize`, which is unrelated to the removed core `initialize`; tools still return meaningful text for hosts without the extension.
  - **Skills over MCP** `io.modelcontextprotocol/skills` (SEP-2640): `skills/list` and `skills/get`, with skill files served through `resources/read`; reading `SKILL.md` does not activate a skill. Skill content is untrusted input tagged with its origin server, explicit user policy governs whether a skill is loaded at all, hosts SHOULD let users inspect a skill before loading it, MUST NOT let skill content trigger host-side code execution without per-skill approval, and MUST ignore permission-widening frontmatter such as `allowed-tools` on MCP-served skills unless the user approved that grant.
  - **Authorization extensions** (`ext-auth`): OAuth Client Credentials and Enterprise-Managed Authorization.

> - 공식 extension:
>   - **Tasks** `io.modelcontextprotocol/tasks`(SEP-2663): server는 지원하는 요청에 `resultType: "task"`로 답할 수 있다(`taskId`, 상태, TTL, 권장 폴링 간격을 담은 `CreateTaskResult`). client는 `tasks/get`으로 폴링한다. 진행 중 입력은 `tasks/update`(`inputResponses`)로 보낸다. `tasks/cancel`을 보낼 수도 있다(협조적 취소). 상태: `working`, `input_required`, `completed`(`result`를 담음), `failed`(`error`를 담음), `cancelled`. `tasks/result`와 `tasks/list`는 없다.
>   - **MCP Apps** `io.modelcontextprotocol/ui`(SEP-1865): tool의 `_meta.ui`에는 `resourceUri`와 `visibility`가 들어간다. `resourceUri`는 `text/html;profile=mcp-app` 타입의 `ui://` resource를 가리키며 이 resource는 일반 `resources/read`로 가져온다. `visibility`의 기본값은 `["model", "app"]`이다. host는 `"model"`이 없는 tool을 agent에게 목록으로 보여 주면 안 된다(MUST NOT). host는 `"app"`이 없는 tool에 대한 앱의 `tools/call`을 거부해야 한다(MUST). UI resource 자신의 `_meta.ui`(그 resource의 `resources/read` contents 항목에 있음)에는 `csp`와 `permissions`가 들어간다. `csp`는 선택 항목인 `connectDomains`, `resourceDomains`, `frameDomains`, `baseUriDomains` origin 목록을 담은 객체다. `permissions`는 선택 항목인 `camera`, `microphone`, `geolocation`, `clipboardWrite` 플래그를 `{}`로 설정하는 객체다. host는 선언된 도메인으로 CSP를 만들어야 한다(MUST). 선언되지 않은 도메인을 허용하면 안 된다(MUST NOT). 더 제한할 수는 있다(MAY). `csp`가 없으면 제한적인 기본값(`connect-src 'none'`)을 적용해야 한다(MUST). host는 `permissions`를 따를 수 있다(MAY). 앱은 권한을 받았다고 가정하지 않아야 한다(SHOULD NOT). 화면은 sandbox iframe에서 돈다(웹 host는 다른 origin의 sandbox 프록시로 한 번 더 감싼다). 앱과 host는 `ui/` 메서드를 쓰는 JSON-RPC 방언으로 통신한다. 첫 메시지는 앱의 `ui/initialize`이며 제거된 core `initialize`와는 관계가 없다. 이 extension이 없는 host를 위해 tool은 여전히 의미 있는 텍스트를 돌려준다.
>   - **Skills over MCP** `io.modelcontextprotocol/skills`(SEP-2640): `skills/list`와 `skills/get`. skill 파일은 `resources/read`로 제공한다. `SKILL.md`를 읽는다고 skill이 활성화되지는 않는다. skill 내용은 출처 server가 표시된 신뢰할 수 없는 입력이다. skill을 불러올지는 명시적인 사용자 정책이 정한다. host는 불러오기 전에 사용자가 skill을 살펴볼 수 있게 해야 한다(SHOULD). host는 skill별 승인 없이 skill 내용이 host 쪽 코드 실행을 일으키게 하면 안 된다(MUST NOT). 사용자가 그 권한 부여를 승인하지 않았다면 host는 MCP로 제공된 skill의 `allowed-tools` 같은 권한 확대 frontmatter를 무시해야 한다(MUST).
>   - **인가 extension**(`ext-auth`): OAuth Client Credentials와 Enterprise-Managed Authorization.

## 15. Governance, lifecycle, and ecosystem

**한국어 제목: 15. 거버넌스, 생애 주기, 생태계**

- Feature lifecycle (SEP-2596): Active, Deprecated (still specified; a migration path is required; at least a 12-month window measured from the release of the revision that deprecates it), Removed. The earliest removal is the first revision released on or after the window ends; actual removal is a Core Maintainer decision. A deprecated feature can be restored by a superseding SEP.
- SEPs: PR-based workflow with files in `seps/` (SEP-1850); governance (SEP-932); Working Groups and Interest Groups (SEP-1302); Final SEPs require conformance tests (SEP-2484).
- SDK tiers (SEP-1730): Tier 1 passes 100% of conformance tests, ships features before a spec release, triages within 2 business days, and fixes critical bugs within 7 days. Tier 2 passes 80% and ships features within 6 months. Tier 3 is experimental. Extensions are not required for any tier. An SDK is relegated after 4 weeks of failures.
- Registry (preview): metadata only (`server.json`) that points at packages (npm, PyPI, Docker, and others) or remote URLs; reverse-DNS namespaces verified through GitHub, DNS, or HTTP challenges (`io.github.user/server`, `com.example/server`); public servers only (run your own registry, implementing the OpenAPI spec, for private ones); meant for aggregators, not direct host consumption; security scanning is delegated to package registries and aggregators. The `server.json` schema version is independent of the MCP protocol version.
- Stewardship: MCP is a project of the Agentic AI Foundation (Linux Foundation).
- Scale (MCP blog, 2026-07-28 release post): close to half a billion downloads a month across Tier 1 SDKs; the TypeScript and Python SDKs each passed one billion total downloads.

> - 기능 생애 주기(SEP-2596): Active, Deprecated(명세에는 남아 있다. 이전 경로가 있어야 한다. 중단을 정한 개정판의 발표일부터 최소 12개월의 유예 기간을 둔다), Removed. 가장 이른 제거 시점은 유예 기간이 끝나는 날 또는 그 이후에 처음 나오는 개정판이다. 실제 제거는 Core Maintainer가 결정한다. 중단된 기능은 이를 대체하는 SEP로 되살릴 수 있다.
> - SEP: `seps/`에 파일을 두는 PR 기반 절차(SEP-1850), 거버넌스(SEP-932), Working Group과 Interest Group(SEP-1302). Final SEP에는 적합성 테스트가 필요하다(SEP-2484).
> - SDK 등급(SEP-1730): Tier 1은 적합성 테스트를 100% 통과하고 명세 발표 전에 기능을 내놓는다. 이슈 분류는 영업일 2일 안에, 치명적 버그 수정은 7일 안에 한다. Tier 2는 80%를 통과하고 6개월 안에 기능을 내놓는다. Tier 3은 실험 단계다. 어떤 등급도 extension을 요구하지 않는다. 4주 동안 실패하면 SDK의 등급이 내려간다.
> - Registry(미리보기): 메타데이터(`server.json`)만 담는다. 이 메타데이터는 패키지(npm, PyPI, Docker 등)나 원격 URL을 가리킨다. 역 DNS 네임스페이스는 GitHub, DNS, HTTP challenge로 검증한다(`io.github.user/server`, `com.example/server`). 공개 server만 등록한다(비공개 server라면 OpenAPI 명세를 구현해 자체 registry를 운영한다). 집계기용이며 host가 직접 소비하는 용도가 아니다. 보안 검사는 패키지 registry와 집계기에 맡긴다. `server.json` 스키마 버전은 MCP 프로토콜 버전과 별개다.
> - 관리 주체: MCP는 Agentic AI Foundation(Linux Foundation)의 프로젝트다.
> - 규모(MCP 블로그, 2026-07-28 릴리스 글): Tier 1 SDK 전체의 월간 다운로드가 5억 회에 가깝다. TypeScript SDK와 Python SDK는 각각 누적 다운로드 10억 회를 넘었다.

## 16. Traps the exam can exploit

**한국어 제목: 16. 시험이 노릴 수 있는 함정**

| Wrong belief | Correct 2026-07-28 fact |
|---|---|
| A connection starts with `initialize`. | No handshake. Every request carries version and capabilities in `_meta`; `server/discover` is optional for clients and mandatory for servers. |
| The session id keeps state between calls. | No sessions. Use explicit server-minted handles as arguments. |
| An unknown tool returns `-32601`. | `-32602`. `-32601` means the method itself is unknown. |
| A schema-invalid argument returns `-32602`. | A tool execution error: `isError: true` with feedback the model can act on. |
| Servers push requests (sampling, elicitation) to the client. | MRTR: an `input_required` result, then the client retries with `inputResponses` and a new id. |
| The retry reuses the original request id. | The retry MUST use a new id and echo `requestState` exactly. |
| `resources/subscribe` watches a resource. | `subscriptions/listen` with `resourceSubscriptions`. |
| Tool annotations are enforced guarantees. | Hints, untrusted unless the server is trusted. |
| `cacheScope: "public"` protects data. | It permits sharing across users; access control is separate. |
| Progress and log notifications arrive on the listen stream. | Only on the response stream of their own request. |
| Closing a stdio process cancels a request. | stdio cancels with `notifications/cancelled`; HTTP cancels by closing that request's stream. |
| New servers should use `-32001` for custom errors. | `-32000` to `-32019` is legacy; `-32020` to `-32099` is reserved; app codes go outside `-32768` to `-32000`. |
| Resource not found is `-32002`. | `-32602` in 2026-07-28 (accept `-32002` from older servers). |
| Roots, sampling, and logging were removed. | Deprecated, still functional, eligible for removal on or after 2027-07-28. |
| Dynamic Client Registration is the recommended registration. | Deprecated; Client ID Metadata Documents are preferred after pre-registration. |
| A server may forward the user's token upstream. | Token passthrough is forbidden; audience validation is mandatory. |
| `clientInfo` can drive authorization decisions. | Self-reported, for display and logging only. |
| Streams can resume with `Last-Event-ID`. | Removed. A broken stream loses the request; reissue with a new id. |

| 잘못된 믿음 | 2026-07-28 기준 사실 |
|---|---|
| 연결은 `initialize`로 시작한다. | handshake가 없다. 모든 요청이 `_meta`에 버전과 capability를 담는다. `server/discover`는 client에게는 선택이고 server에게는 필수다. |
| session id가 호출 사이의 상태를 유지한다. | session이 없다. server가 발급한 명시적 handle을 인자로 쓴다. |
| 알 수 없는 tool은 `-32601`을 돌려준다. | `-32602`다. `-32601`은 메서드 자체를 모른다는 뜻이다. |
| 스키마에 맞지 않는 인자는 `-32602`를 돌려준다. | tool 실행 오류다. 모델이 대응할 수 있는 피드백과 함께 `isError: true`를 돌려준다. |
| server가 client에게 요청(sampling, elicitation)을 밀어 보낸다. | MRTR: server가 `input_required` 결과를 돌려주면 client가 `inputResponses`와 새 id로 재시도한다. |
| 재시도는 원래 요청 id를 다시 쓴다. | 재시도는 새 id를 쓰고 `requestState`를 정확히 그대로 돌려보내야 한다(MUST). |
| `resources/subscribe`로 resource를 감시한다. | `resourceSubscriptions`를 담은 `subscriptions/listen`을 쓴다. |
| tool annotation은 강제되는 보장이다. | 힌트다. server를 신뢰하는 경우가 아니면 신뢰할 수 없다. |
| `cacheScope: "public"`이 데이터를 보호한다. | 여러 사용자 사이의 공유를 허용한다는 뜻이다. 접근 제어는 별개다. |
| progress notification과 log notification은 listen 스트림으로 온다. | 각자 속한 요청의 응답 스트림으로만 온다. |
| stdio 프로세스를 닫으면 요청이 취소된다. | stdio는 `notifications/cancelled`로 취소한다. HTTP는 그 요청의 스트림을 닫아 취소한다. |
| 새 server는 사용자 정의 오류에 `-32001`을 써야 한다. | `-32000`부터 `-32019`까지는 이전 방식(legacy)이다. `-32020`부터 `-32099`까지는 예약돼 있다. 앱 코드는 `-32768`부터 `-32000`까지의 범위 밖을 쓴다. |
| resource를 찾지 못하면 `-32002`다. | 2026-07-28에서는 `-32602`다(이전 server가 보낸 `-32002`도 받아들인다). |
| roots, sampling, logging은 제거됐다. | deprecated 상태다. 여전히 동작하며 2027-07-28 당일 또는 그 이후에 제거될 수 있다. |
| Dynamic Client Registration이 권장 등록 방식이다. | deprecated다. 사전 등록 다음으로는 Client ID Metadata Documents를 우선한다. |
| server가 사용자의 token을 상위(upstream) 서비스로 넘겨도 된다. | token passthrough는 금지다. audience 검증은 필수다. |
| `clientInfo`로 인가 결정을 내릴 수 있다. | 자기 신고 값이라 표시와 로깅에만 쓴다. |
| `Last-Event-ID`로 스트림을 이어 갈 수 있다. | 제거됐다. 스트림이 끊기면 그 요청은 잃는다. 새 id로 다시 보낸다. |

## 17. Source conflicts resolved during the build

**한국어 제목: 17. 이 문서를 만들며 해결한 출처 간 충돌**

Where two primary pages disagree, the normative record wins: `schema.ts` for message shapes, the
changelog and the current specification page for what exists in 2026-07-28, and never a historical
SEP draft over the published revision. Questions must not test the points marked "unspecified" as a
single rule.

> 두 1차 문서가 서로 다르게 말하면 규범 기록이 이긴다. 메시지 형태는 `schema.ts`가 기준이다. 2026-07-28에 무엇이 있는지는 변경 로그(changelog)와 현재 명세 페이지가 기준이다. 과거 SEP 초안을 발표된 개정판보다 우선하는 일은 없다. "unspecified"로 표시한 항목을 하나의 규칙으로 묻는 문제를 내면 안 된다.

| Point | What the pages say | Resolution |
|---|---|---|
| `logging/setLevel` and `notifications/roots/list_changed` | SEP-2577 lists them inside the deprecated Logging and Roots features; changelog item 5 and SEP-2575 say both methods are removed | The methods are removed in 2026-07-28. The Roots, Sampling, and Logging features are deprecated, and their surviving surface is `roots/list` and `sampling/createMessage` as MRTR input requests plus per-request `logLevel` with `notifications/message`. |
| Annotations on an embedded resource | The tools page example nests `annotations` inside `resource`; `schema.ts` defines `EmbeddedResource.annotations` beside `resource`, and `ResourceContents` has no `annotations` | Follow `schema.ts`: annotations sit beside `resource`. |
| Headers on a notification POST | SEP-2243's table lists `Mcp-Method` for notifications; the 2026-07-28 Streamable HTTP page says header requirements for notification POSTs are not defined, and the core protocol defines no client-to-server notification over HTTP | Unspecified in 2026-07-28. |
| Rejecting a `requestState` that fails verification | MRTR says the server MUST reject it but does not name the channel | Unspecified. A tool execution error or a fresh `input_required` result are both defensible. |
| HTTP status for `-32700`, `-32600`, `-32603` | Streamable HTTP names `400` for missing `_meta`, `-32020`, `-32021`, and `-32022`, and `404` for `-32601`, but nothing for these three | Unspecified. |
| A server ending a subscription | The subscriptions page says the server SHOULD answer the listen request with a `complete` result for a graceful end; the cancellation page says a server sends `notifications/cancelled` only to tear down a listen stream | Both hold. A graceful end is the `complete` result; tearing down a listen stream is the only purpose for which a server may send `notifications/cancelled`. |
| Tool name format | SEP-986 proposed a different length and allowed `/` | The published tools page wins: 1 to 128 characters of `A-Za-z0-9_-.`, no `/`. |

| 항목 | 문서들의 내용 | 결론 |
|---|---|---|
| `logging/setLevel`과 `notifications/roots/list_changed` | SEP-2577은 둘을 deprecated된 Logging과 Roots 기능 안에 넣는다. changelog 5번 항목과 SEP-2575는 두 메서드가 제거됐다고 한다 | 두 메서드는 2026-07-28에서 제거됐다. Roots, Sampling, Logging 기능은 deprecated다. 남은 부분은 MRTR input request로 쓰는 `roots/list`와 `sampling/createMessage`, 그리고 요청별 `logLevel`과 `notifications/message`이다. |
| 내장 resource의 annotation | tools 페이지 예시는 `annotations`를 `resource` 안에 넣는다. `schema.ts`는 `EmbeddedResource.annotations`를 `resource` 옆에 정의하며 `ResourceContents`에는 `annotations`가 없다 | `schema.ts`를 따른다. annotation은 `resource` 옆에 둔다. |
| notification POST의 헤더 | SEP-2243의 표는 notification에 `Mcp-Method`를 적는다. 2026-07-28 Streamable HTTP 페이지는 notification POST의 헤더 요구 사항이 정의되지 않았다고 한다. core 프로토콜은 HTTP에서 client가 server로 보내는 notification을 정의하지 않는다 | 2026-07-28에서는 정해지지 않았다(unspecified). |
| 검증에 실패한 `requestState` 거부 | MRTR은 server가 거부해야 한다(MUST)고 하지만 어떤 경로로 거부하는지는 정하지 않는다 | 정해지지 않았다(unspecified). tool 실행 오류와 새 `input_required` 결과 모두 타당하다. |
| `-32700`, `-32600`, `-32603`의 HTTP 상태 | Streamable HTTP는 `_meta` 누락, `-32020`, `-32021`, `-32022`에 `400`을, `-32601`에 `404`를 지정한다. 이 세 개에는 아무것도 정하지 않는다 | 정해지지 않았다(unspecified). |
| server가 subscription을 끝내는 경우 | subscriptions 페이지는 정상 종료 시 server가 listen 요청에 `complete` 결과로 답해야 한다(SHOULD)고 한다. cancellation 페이지는 server가 `notifications/cancelled`를 보내는 것은 listen 스트림을 끊을 때뿐이라고 한다 | 둘 다 맞다. 정상 종료는 `complete` 결과다. listen 스트림을 끊는 것이 server가 `notifications/cancelled`를 보낼 수 있는 유일한 목적이다. |
| tool 이름 형식 | SEP-986은 다른 길이를 제안했고 `/`를 허용했다 | 발표된 tools 페이지가 이긴다. `A-Za-z0-9_-.` 문자로 된 1자에서 128자이며 `/`는 쓸 수 없다. |
