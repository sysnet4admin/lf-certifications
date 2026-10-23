# MCPA 모의고사 03: MCPA Full-Length Mock 3: Design and Security Trade-offs

> 실제 시험 문항이 아니며 출제 내용을 보장하지 않는다. 리눅스 재단(Linux Foundation)과 AAIF의 공식 자료가 아니다.
> 한국어 번역과 덧붙인 메모는 AI로 작성했으므로 틀린 곳이 있을 수 있다. 공식 스펙이 우선이다: https://modelcontextprotocol.io/specification/2026-07-28
> 원문 가운데 시험(exam)을 언급한 문장은 원작자의 설명을 그대로 옮긴 것이며 이 저장소의 의견이 아니다.

원본: rohitg00/ai-engineering-from-scratch `certifications/mcpa/assessments/mcpa-f/mock-03.json` (버전 1). 제한 시간 90분, 60문항.

도메인별 문항 수: mcp-fundamentals 10, architecture-and-components 8, interactions-and-execution 16, security-and-governance 14, use-cases-and-ecosystem 12. 유형: single 50, multiple 10.

영어 원문 아래 한국어 번역을 붙였다. 원문은 `mock-03.en.md`.

## 1. [mcp-fundamentals]

A team migrating to MCP 2026-07-28 complains that servers can no longer remember a shopping cart between calls the way a session used to. What does the stateless design actually require them to do instead, and why is that the right trade?

A. Mint a server handle for the cart, return it in a result, and have the client pass it back as an argument\
B. Open one dedicated connection to a single replica so that replica can hold the cart in its own memory\
C. Send an initialize request each morning so the server can attach the cart to that entire day's ongoing handshake process every time\
D. Resend the entire cart's contents inside the JSON-RPC id field on every single request the client makes

**한국어**

MCP 2026-07-28로 옮기는 팀이, 예전에 session이 하던 것처럼 server가 호출과 호출 사이에 장바구니를 기억할 수 없게 됐다고 불평한다. stateless 설계는 대신 무엇을 하도록 요구하는가? 그리고 왜 그것이 올바른 절충인가?

A. 장바구니용 server handle을 발급해 결과로 돌려준다. client는 그 handle을 인자로 다시 넘긴다\
B. replica 하나와 전용 연결을 하나 열어, 그 replica가 자기 메모리에 장바구니를 보관하게 한다\
C. 매일 아침 initialize 요청을 보내, server가 매번 그날 하루 동안 이어지는 handshake 과정 전체에 장바구니를 붙이게 한다\
D. client가 보내는 모든 요청마다 JSON-RPC id 필드 안에 장바구니 내용 전체를 다시 보낸다

<details><summary>정답</summary>

A. Statelessness means a server cannot rely on a prior request on the same connection, so any state that must span calls needs an explicit, server-minted, opaque handle passed back as a normal argument. That is what lets any replica answer any request, which is the whole payoff of dropping sessions.

A. stateless라는 것은 server가 같은 연결의 이전 요청에 기댈 수 없다는 뜻이다. 그래서 여러 호출에 걸쳐 유지해야 하는 상태는 server가 발급한 명시적인 opaque handle로 만들어 일반 인자로 다시 넘겨야 한다. 그래야 어떤 replica든 어떤 요청에든 응답할 수 있다. 이것이 session을 없애서 얻는 이득의 전부다.

</details>

## 2. [mcp-fundamentals]

A backend engineer proposes storing per-client state keyed by the underlying TCP connection so a server can skip re-reading arguments on every call. Why does this design break the 2026-07-28 statelessness guarantee even if it works in a single-server test?

A. It requires every client to install a new certificate before it can open any connection at all\
B. It makes a call's outcome depend on which connection carried it, so no other replica can serve it correctly\
C. It duplicates work that tools/list pagination already performs whenever a result set grows large across many separate paginated requests\
D. It is disallowed only because TCP connections run slower than the stdio transport for local servers

**한국어**

한 백엔드 엔지니어가 client별 상태를 하위 TCP 연결을 키로 저장해서, server가 호출마다 인자를 다시 읽지 않아도 되게 하자고 제안한다. 이 설계가 server 한 대로 한 테스트에서는 동작하더라도 2026-07-28의 stateless 보장을 깨는 이유는 무엇인가?

A. 모든 client가 연결을 하나라도 열기 전에 새 인증서를 설치해야 한다\
B. 호출 결과가 그 호출을 실어 나른 연결에 따라 달라지므로, 다른 replica는 그 호출을 올바르게 처리할 수 없다\
C. 결과 집합이 여러 번의 페이지 요청에 걸쳐 커질 때 tools/list 페이지네이션이 이미 하는 일을 중복한다\
D. 로컬 server에서 TCP 연결이 stdio transport보다 느리다는 이유 하나 때문에 금지된다

<details><summary>정답</summary>

B. Connection-keyed state reintroduces exactly the stickiness that 2026-07-28 removed: a load balancer could route the next call to a different replica, or the client could reconnect, and the server would answer incorrectly. The fix is an explicit handle carried in the request, not implicit connection memory.

B. 연결을 키로 한 상태는 2026-07-28이 없앤 고정성(stickiness)을 그대로 다시 들여온다. load balancer가 다음 호출을 다른 replica로 보내거나 client가 다시 연결하면 server는 틀린 답을 낸다. 해결책은 암묵적인 연결 메모리가 아니라 요청 안에 실어 보내는 명시적 handle이다.

</details>

## 3. [mcp-fundamentals]

A team debating how to pin its integration asks why MCP names revisions with dates such as 2026-07-28 instead of semantic version numbers. What does the date in a revision identifier signify?

A. The day every earlier revision stopped working, since each date retires older revisions\
B. The day the revision's governance vote closed, whatever the revision happened to change\
C. The last date backwards incompatible changes were made, so compatible updates keep it\
D. The release date of the newest SDK that implements the revision's full feature set

**한국어**

연동 버전을 어떻게 고정할지 논의하던 팀이, MCP는 왜 시맨틱 버전 번호 대신 2026-07-28 같은 날짜로 revision 이름을 붙이는지 묻는다. revision 식별자의 날짜는 무엇을 뜻하는가?

A. 이전 revision이 모두 동작을 멈춘 날이다. 날짜가 새로 나올 때마다 이전 revision을 폐기하기 때문이다\
B. 그 revision이 무엇을 바꿨든 상관없이, revision의 거버넌스 투표가 마감된 날이다\
C. 하위 호환되지 않는 변경이 마지막으로 이뤄진 날이다. 그래서 호환되는 업데이트에서는 날짜가 그대로다\
D. 그 revision의 기능 전체를 구현한 최신 SDK의 출시일이다

<details><summary>정답</summary>

C. A protocol version string is a date in YYYY-MM-DD form marking the last date backwards incompatible changes were made; the specification can still receive backwards compatible updates without changing it. Older revisions do not stop working on that date, and SDK release dates or governance votes play no part in the identifier.

C. 프로토콜 버전 문자열은 YYYY-MM-DD 형식의 날짜로, 하위 호환되지 않는 변경이 마지막으로 이뤄진 날을 표시한다. 사양은 이 값을 바꾸지 않고도 하위 호환되는 업데이트를 받을 수 있다. 이전 revision이 그날 동작을 멈추지는 않는다. SDK 출시일이나 거버넌스 투표도 식별자와 관계가 없다.

</details>

## 4. [mcp-fundamentals]

A company already has a proprietary message format tuned exactly to its internal tools and refuses to adopt MCP, arguing their format is simpler for their own use. What is the clearest cost of that choice as new AI hosts and external systems appear?

A. Their format will automatically gain OAuth 2.1 support the moment a new host adopts it\
B. Nothing changes at all, since message formats never affect how many separate integrations a team must eventually build\
C. Their proprietary format will be forced to add roots and sampling to remain usable\
D. Every new host or system still needs a bespoke adapter instead of reusing a compliant client or server

**한국어**

한 회사가 자사 내부 도구에 딱 맞춘 독자 메시지 형식을 이미 갖고 있다. 자기들이 쓰기에는 그 형식이 더 단순하다며 MCP 도입을 거부한다. 새 AI host와 외부 시스템이 등장할수록 이 선택의 가장 분명한 비용은 무엇인가?

A. 새 host가 그 형식을 채택하는 순간 OAuth 2.1 지원이 자동으로 생긴다\
B. 아무것도 바뀌지 않는다. 메시지 형식은 팀이 결국 만들어야 하는 개별 연동의 수에 전혀 영향을 주지 않기 때문이다\
C. 독자 형식이 계속 쓰이려면 roots와 sampling을 억지로 추가해야 한다\
D. 새 host나 시스템마다 규격을 따르는 client나 server를 재사용하지 못하고 전용 어댑터를 따로 만들어야 한다

<details><summary>정답</summary>

D. A protocol tailored to one shop's current tools does not become more useful as the ecosystem grows. Each new host or system still needs a custom adapter, which is exactly the integration cost that adopting a shared, versioned protocol like MCP was meant to remove.

D. 한 회사의 현재 도구에 맞춘 프로토콜은 생태계가 커져도 더 유용해지지 않는다. 새 host나 시스템마다 여전히 맞춤 어댑터가 필요하다. 이것이 바로 MCP처럼 공유되고 버전이 관리되는 프로토콜을 도입해 없애려던 연동 비용이다.

</details>

## 5. [mcp-fundamentals] (복수 정답)

Which two outcomes does adopting one open, versioned protocol provide that a set of per-system custom integrations does not? Select two.

A. Every tool call is guaranteed to complete in the same amount of time on any server\
B. Every server automatically becomes free to operate no matter its own infrastructure costs\
C. A revision timeline and feature lifecycle give teams a shared way to plan migrations together\
D. A client built against the specification can work with servers it has never seen before

**한국어**

시스템별 맞춤 연동 여러 개로는 얻지 못하지만 개방되고 버전이 관리되는 프로토콜 하나를 도입하면 얻는 결과 두 가지는 무엇인가? 두 개 고르시오.

A. 어떤 server에서든 모든 tool 호출이 같은 시간 안에 끝난다는 것이 보장된다\
B. 모든 server가 자체 인프라 비용과 관계없이 자동으로 무료로 운영된다\
C. revision 일정과 기능 수명 주기 덕분에 팀들이 마이그레이션을 함께 계획할 공통 방식이 생긴다\
D. 사양에 맞춰 만든 client는 처음 보는 server와도 동작할 수 있다

<details><summary>정답</summary>

C, D. Interoperability and a shared versioning and deprecation process are the concrete benefits of one open protocol. Cost and latency are operational properties of each deployment and are not guaranteed by the protocol itself, so they make weak justifications for standardization.

C, D. 상호운용성, 그리고 공통 버전 관리와 지원 중단 절차가 개방 프로토콜 하나의 구체적인 이점이다. 비용과 지연 시간은 배포마다 다른 운영 특성이고 프로토콜이 보장하지 않는다. 그래서 표준화를 정당화하는 근거로는 약하다.

</details>

## 6. [mcp-fundamentals]

A code reviewer flags a server that still implements roots/list and sampling/createMessage as inputRequests, calling it out of date for 2026-07-28. Is the reviewer's conclusion correct?

A. No, since roots and sampling are deprecated, not removed, and remain functional until a later revision removes them\
B. Yes, since a deprecated feature can never legally be shipped inside any conformant 2026-07-28 production server deployment\
C. No, since roots and sampling were never mentioned in any revision released before 2026-07-28\
D. Yes, since any feature with a documented migration path must already be gone from the specification

**한국어**

코드 리뷰어가 roots/list와 sampling/createMessage를 아직 inputRequests로 구현한 server를 지적하며 2026-07-28 기준으로 낡았다고 한다. 리뷰어의 결론은 맞는가?

A. 아니다. roots와 sampling은 제거가 아니라 지원 중단(deprecated) 상태다. 이후 revision이 제거할 때까지 계속 동작한다\
B. 맞다. 지원 중단된 기능은 2026-07-28을 준수하는 어떤 프로덕션 server 배포에도 정식으로 넣을 수 없다\
C. 아니다. roots와 sampling은 2026-07-28 이전에 나온 어떤 revision에도 언급된 적이 없다\
D. 맞다. 마이그레이션 경로가 문서화된 기능은 이미 사양에서 빠져 있어야 한다

<details><summary>정답</summary>

A. The feature lifecycle distinguishes deprecated from removed. Roots, sampling, and logging are deprecated in 2026-07-28 with a stated migration path, but they still work, with the earliest possible removal in the first revision released on or after 2027-07-28, so the server is not out of date.

A. 기능 수명 주기는 지원 중단(deprecated)과 제거(removed)를 구분한다. roots, sampling, logging은 2026-07-28에서 마이그레이션 경로와 함께 지원 중단됐지만 여전히 동작한다. 가장 이른 제거 시점은 2027-07-28 당일 또는 그 이후에 나오는 첫 revision이다. 따라서 이 server는 낡지 않았다.

</details>

## 7. [mcp-fundamentals]

A governance team assumes every deprecated MCP feature shares the same earliest removal floor as roots and sampling, the first revision on or after 2027-07-28. Why is applying that same floor to the HTTP+SSE transport a mistake?

A. It is not a mistake at all, since every feature deprecated by any means shares exactly one single common removal floor\
B. HTTP+SSE was deprecated earlier in 2025-03-26 by a different SEP, with its own floor tied to that SEP reaching Final\
C. The floor only ever applies to purely client-side features in general, and HTTP+SSE simply counts as a server-side transport feature instead\
D. HTTP+SSE cannot be deprecated at all, since transports are explicitly exempt from the feature lifecycle policy

**한국어**

한 거버넌스 팀이, 지원 중단된 MCP 기능은 모두 roots, sampling과 같은 가장 이른 제거 하한을 공유한다고 가정한다. 그 하한은 2027-07-28 당일 또는 그 이후의 첫 revision이다. 같은 하한을 HTTP+SSE transport에 적용하는 것은 왜 잘못인가?

A. 전혀 잘못이 아니다. 어떤 방식으로 지원 중단됐든 모든 기능은 단 하나의 공통 제거 하한을 공유한다\
B. HTTP+SSE는 그보다 앞선 2025-03-26에 다른 SEP로 지원 중단됐다. 그 SEP가 Final에 도달한 시점에 묶인 자체 하한이 있다\
C. 이 하한은 일반적으로 순수 client 측 기능에만 적용된다. HTTP+SSE는 server 측 transport 기능으로 분류된다\
D. HTTP+SSE는 아예 지원 중단될 수 없다. transport는 기능 수명 주기 정책에서 명시적으로 제외된다

<details><summary>정답</summary>

B. Each deprecated feature carries its own origin and removal floor rather than one shared date. Roots, sampling, logging, and Dynamic Client Registration were deprecated in 2026-07-28 with an earliest removal on or after 2027-07-28, but HTTP+SSE was deprecated back in 2025-03-26, so its floor runs from that earlier SEP reaching Final instead.

B. 지원 중단된 기능은 공통 날짜 하나가 아니라 각자의 출처와 제거 하한을 가진다. roots, sampling, logging, Dynamic Client Registration은 2026-07-28에서 지원 중단됐고 가장 이른 제거는 2027-07-28 당일 또는 그 이후다. 하지만 HTTP+SSE는 이미 2025-03-26에 지원 중단됐으므로, 그 하한은 그보다 앞선 SEP가 Final에 도달한 시점부터 계산한다.

</details>

## 8. [mcp-fundamentals]

A skeptical architect asks why MCP does not simply negotiate protocol version and capabilities once, the way many handshake-based protocols do, instead of repeating them on every request. What is the accurate 2026-07-28 rationale?

A. Negotiating once is impossible in JSON-RPC, which has no concept of a request at all\
B. The specification requires it only so that gateways have more spare bytes available for careful rate limiting decisions\
C. Repeating metadata on every request lets any request be handled correctly without depending on what happened earlier\
D. It only matters for HTTP transports, since stdio connections keep negotiated state in memory regardless

**한국어**

회의적인 아키텍트가 묻는다. 많은 handshake 기반 프로토콜은 프로토콜 버전과 capability를 한 번만 협상하는데, MCP는 왜 그렇게 하지 않고 요청마다 반복하는가? 2026-07-28의 정확한 근거는 무엇인가?

A. JSON-RPC에는 요청이라는 개념이 아예 없으므로 한 번 협상하는 것이 불가능하다\
B. 사양이 이를 요구하는 이유는 gateway가 신중한 rate limiting 판단에 쓸 여유 바이트를 더 갖게 하려는 것뿐이다\
C. 요청마다 메타데이터를 반복하면 이전에 무슨 일이 있었는지에 기대지 않고도 어떤 요청이든 올바르게 처리할 수 있다\
D. HTTP transport에서만 의미가 있다. stdio 연결은 어차피 협상한 상태를 메모리에 유지한다

<details><summary>정답</summary>

C. Per-request metadata is a direct consequence of statelessness: a server must not infer version or capabilities from earlier requests on the same connection. That is what allows requests to be retried or rerouted to a different replica safely, on stdio and HTTP alike.

C. 요청별 메타데이터는 stateless 설계의 직접적인 결과다. server는 같은 연결의 이전 요청에서 버전이나 capability를 추론해서는 안 된다. 그래서 stdio와 HTTP 모두에서 요청을 안전하게 재시도하거나 다른 replica로 다시 보낼 수 있다.

</details>

## 9. [mcp-fundamentals] (복수 정답)

Select the three things MCP deliberately leaves for the host, the model provider, or the external system to decide, rather than standardizing itself.

A. How a host's user interface presents a conversation to a person\
B. The shape of a JSON-RPC request, result, error, and notification message\
C. Which large language model the host uses to reason about a request\
D. The internal business logic that a connected external system runs

**한국어**

MCP가 직접 표준화하지 않고 host, 모델 제공자, 외부 시스템이 정하도록 일부러 남겨 둔 것 세 가지를 고르시오.

A. host의 사용자 인터페이스가 대화를 사람에게 보여 주는 방식\
B. JSON-RPC 요청, 결과, 오류, 알림 메시지의 형태\
C. host가 요청을 추론하는 데 쓰는 대규모 언어 모델의 종류\
D. 연결된 외부 시스템이 실행하는 내부 비즈니스 로직

<details><summary>정답</summary>

A, C, D. MCP standardizes the wire format connecting applications and external systems, not the model choice, the host UI, or what a system does internally. The JSON-RPC message shapes are exactly the part the protocol does define, which is why it is the one item outside the deliberately unstandardized set.

A, C, D. MCP는 애플리케이션과 외부 시스템을 잇는 와이어 형식을 표준화한다. 모델 선택, host UI, 시스템 내부 동작은 표준화하지 않는다. JSON-RPC 메시지 형태는 프로토콜이 실제로 정의하는 부분이므로, 일부러 표준화하지 않은 항목에서 유일하게 빠진다.

</details>

## 10. [mcp-fundamentals]

While researching a fine point of client registration, an engineer finds an open SEP pull request proposing a shorter tool name format, and separately the published specification page that already documents a different, current format. Which should the engineer treat as authoritative for a 2026-07-28 deployment?

A. Whichever document was edited more recently in the source repository's commit history\
B. Neither, since naming rules are left entirely to each individual server implementation to freely decide upon\
C. The SEP pull request, since proposals always describe where the specification is heading next\
D. The published specification page, since a draft proposal is not yet part of the current revision

**한국어**

client 등록의 세부 사항을 조사하던 엔지니어가 더 짧은 tool 이름 형식을 제안하는 열린 SEP pull request를 찾았다. 따로, 이미 다른 현행 형식을 문서화한 게시된 사양 페이지도 찾았다. 2026-07-28 배포에서 엔지니어는 어느 쪽을 권위 있는 기준으로 삼아야 하는가?

A. 소스 저장소 커밋 기록에서 더 최근에 수정된 문서\
B. 둘 다 아니다. 이름 규칙은 각 server 구현이 자유롭게 정하도록 전적으로 맡겨져 있다\
C. SEP pull request. 제안은 항상 사양이 다음에 향할 방향을 설명하기 때문이다\
D. 게시된 사양 페이지. 초안 제안은 아직 현행 revision의 일부가 아니기 때문이다

<details><summary>정답</summary>

D. Reading the specification means treating the published, current revision as the source of truth and a draft SEP as, at most, a preview of a possible future change. Until a proposal is accepted and released, the specification page's rule is what a 2026-07-28 implementation must follow.

D. 사양을 읽는다는 것은 게시된 현행 revision을 기준으로 삼는다는 뜻이다. 초안 SEP는 기껏해야 앞으로 있을 수 있는 변경의 미리 보기로 다룬다. 제안이 승인되어 릴리스되기 전까지는 사양 페이지의 규칙이 2026-07-28 구현이 따라야 할 규칙이다.

</details>

## 11. [architecture-and-components]

A host developer proposes sharing a single client instance across three different MCP servers to save memory, routing all three connections' messages through it. What is the strongest reason MCP hosts instead embed one client per server connection?

A. Each server connection has its own capabilities and cache, and mixing them in one client would blur which server a message belongs to\
B. Sharing a client this way would also clearly violate the important rule that tools must always stay strictly model-controlled rather than application-driven\
C. Servers absolutely refuse to respond at all unless exactly one single dedicated TCP port stays wide open on the host machine itself\
D. A shared client is forbidden since JSON-RPC ids must stay globally unique across an entire operating system

**한국어**

host 개발자가 메모리를 아끼려고 서로 다른 MCP server 세 개가 client 인스턴스 하나를 공유하게 하자고 제안한다. 세 연결의 메시지를 모두 그 client로 보내는 방식이다. MCP host가 대신 server 연결마다 client를 하나씩 두는 가장 강력한 이유는 무엇인가?

A. server 연결마다 capability와 캐시가 따로 있다. 이를 client 하나에 섞으면 메시지가 어느 server의 것인지 흐려진다\
B. 이렇게 client를 공유하면 tool은 애플리케이션 주도가 아니라 항상 엄격하게 모델 제어여야 한다는 중요한 규칙도 분명히 어기게 된다\
C. host 머신에서 전용 TCP 포트 딱 하나가 활짝 열려 있지 않으면 server는 아예 응답하지 않는다\
D. JSON-RPC id는 운영체제 전체에서 전역적으로 고유해야 하므로 공유 client는 금지된다

<details><summary>정답</summary>

A. A client's job is to hold one server's negotiated capabilities, discovery results, and cache state cleanly. Merging three servers behind one client risks attributing a result, a capability, or a cached entry to the wrong server, which the one-client-per-server design avoids by construction.

A. client의 역할은 server 하나와 협상한 capability, discovery 결과, 캐시 상태를 깔끔하게 보관하는 것이다. server 세 개를 client 하나 뒤에 합치면 결과나 capability, 캐시 항목을 엉뚱한 server의 것으로 볼 위험이 생긴다. server마다 client를 하나 두는 설계는 구조상 이 문제를 피한다.

</details>

## 12. [architecture-and-components]

A gateway aggregates two independent servers that both expose a tool named search. What is the correct design for the aggregated tool registry the host presents to the model?

A. Merge both tool definitions into one entry and forward every call to whichever server answers first\
B. Prefix each tool name with a server identifier so the model can address search_docs and search_tickets without ambiguity\
C. Reject both servers entirely at discovery time until their two separate authors formally agree on one single shared tool name\
D. Silently keep only the first search tool discovered and drop the second one from the registry

**한국어**

gateway가 독립된 server 두 개를 모으는데, 두 server 모두 search라는 이름의 tool을 노출한다. host가 모델에게 보여 줄 통합 tool registry의 올바른 설계는 무엇인가?

A. 두 tool 정의를 항목 하나로 합치고 모든 호출을 먼저 응답하는 server로 보낸다\
B. 각 tool 이름 앞에 server 식별자를 붙여, 모델이 search_docs와 search_tickets를 헷갈리지 않고 지정할 수 있게 한다\
C. 두 server의 작성자가 공통 tool 이름 하나에 공식적으로 합의할 때까지 discovery 단계에서 두 server를 모두 거부한다\
D. 처음 발견한 search tool만 조용히 남기고 두 번째는 registry에서 뺀다

<details><summary>정답</summary>

B. Because serverInfo.name is not guaranteed unique and tool names only need to be unique within one server, an aggregator that fronts multiple servers should prefix names to disambiguate. Dropping, merging, or racing servers all lose information the model needs to call the right tool.

B. serverInfo.name은 고유하다는 보장이 없고 tool 이름은 server 하나 안에서만 고유하면 된다. 그래서 여러 server를 앞단에서 묶는 aggregator는 이름에 접두사를 붙여 구분해야 한다. 하나를 버리거나, 합치거나, server끼리 먼저 응답하기 경쟁을 시키면 모델이 올바른 tool을 호출하는 데 필요한 정보를 잃는다.

</details>

## 13. [architecture-and-components]

A public-facing tool accepts a free-form object with no declared properties and additionalProperties left unset, versus an alternative design that lists each accepted field and sets additionalProperties to false. Which design better protects the tool and why?

A. Neither matters very much at all, since inputSchema is only ever shown to a human reviewer and never actually enforced anywhere by any code\
B. The free-form schema, since it lets any future client send new fields without a schema update\
C. The explicit schema, since it gives the server a precise contract and surfaces unexpected fields as feedback the model can act on\
D. The free-form schema, since MCP requires every tool to accept arbitrary extra arguments for compatibility

**한국어**

공개용 tool 하나는 선언된 property 없이 자유 형식 객체를 받고 additionalProperties도 설정하지 않았다. 다른 설계안은 받을 필드를 모두 나열하고 additionalProperties를 false로 설정한다. 어느 설계가 tool을 더 잘 보호하는가? 그 이유는 무엇인가?

A. 둘 다 크게 상관없다. inputSchema는 사람 리뷰어에게만 보이고 어떤 코드도 실제로 검증하지 않는다\
B. 자유 형식 스키마. 앞으로 어떤 client든 스키마 업데이트 없이 새 필드를 보낼 수 있다\
C. 명시적 스키마. server에 정확한 계약을 준다. 예상치 못한 필드도 모델이 대응할 수 있는 피드백으로 알려 준다\
D. 자유 형식 스키마. MCP는 호환성을 위해 모든 tool이 임의의 추가 인자를 받도록 요구한다

<details><summary>정답</summary>

C. A tool's inputSchema is a real contract that the server validates arguments against, and a precise schema with additionalProperties false catches malformed or unexpected input early, returning it as a tool execution error the model can correct. An unconstrained schema defers that discovery to runtime logic instead.

C. tool의 inputSchema는 server가 인자를 검증하는 실제 계약이다. additionalProperties를 false로 둔 정확한 스키마는 잘못되거나 예상치 못한 입력을 일찍 잡아낸다. 그리고 이를 모델이 고칠 수 있는 tool 실행 오류로 돌려준다. 제약 없는 스키마는 이런 발견을 런타임 로직으로 미룬다.

</details>

## 14. [architecture-and-components]

A team debates whether the confirm-before-destructive-action gate should live inside the server's tool implementation or inside the host, ahead of sending tools/call, given that annotations like destructiveHint are hints and untrusted unless the server itself is trusted. Where should the gate live?

A. Split randomly and unpredictably between host and server so a bypass in one place is unlikely to matter much\
B. Inside the server, since only the server can be trusted to decide when to ask the user\
C. Nowhere at all, since a genuinely well written tool description already makes an explicit confirmation step entirely unnecessary\
D. Inside the host, since it can apply its own judgment and cannot rely on an untrusted server's hints

**한국어**

파괴적 작업 전에 확인을 받는 관문을 server의 tool 구현 안에 둘지, host 안에서 tools/call을 보내기 전에 둘지 팀이 논의한다. destructiveHint 같은 annotation은 힌트일 뿐이며 server 자체를 신뢰할 때가 아니면 신뢰할 수 없다. 관문은 어디에 두어야 하는가?

A. host와 server 사이에 무작위로, 예측할 수 없게 나눈다. 그러면 한 곳이 우회되어도 큰 문제가 될 가능성이 낮다\
B. server 안. 사용자에게 언제 물어볼지는 server만 믿고 정할 수 있다\
C. 어디에도 두지 않는다. 정말 잘 쓴 tool 설명이 있으면 명시적 확인 단계는 전혀 필요 없다\
D. host 안. host는 자체 판단을 적용할 수 있고 신뢰할 수 없는 server의 힌트에 기댈 수 없다

<details><summary>정답</summary>

D. Because annotations are hints the client must treat as untrusted unless the server is known trustworthy, the host cannot safely delegate the decision to confirm a destructive call to the server itself. The host is the place positioned to show inputs and gate the call before it is sent.

D. annotation은 server를 신뢰할 수 있다고 알려진 경우가 아니면 client가 신뢰할 수 없는 것으로 다뤄야 하는 힌트다. 그래서 host는 파괴적 호출을 확인할지 결정하는 일을 server에 안전하게 맡길 수 없다. 호출을 보내기 전에 입력을 보여 주고 관문을 걸 수 있는 위치는 host다.

</details>

## 15. [architecture-and-components] (복수 정답)

Which two statements correctly describe how discovery and capability negotiation connect an MCP host to a server? Select two.

A. A server must not rely on a capability the client did not declare on that request, and it errors naming what is missing\
B. Once a client has successfully called server/discover one time, every later request on that same connection is exempt from repeating clientCapabilities\
C. Calling server/discover is mandatory for every server to implement, though a client may skip it and send a request directly\
D. Capability negotiation only applies to client capabilities and never constrains what a server is allowed to assume about itself

**한국어**

discovery와 capability 협상이 MCP host와 server를 어떻게 연결하는지 바르게 설명한 것 두 가지는 무엇인가? 두 개 고르시오.

A. server는 그 요청에서 client가 선언하지 않은 capability에 기대서는 안 된다. 무엇이 빠졌는지 명시한 오류를 낸다\
B. client가 server/discover를 한 번 성공적으로 호출하면, 같은 연결의 이후 요청은 모두 clientCapabilities를 반복하지 않아도 된다\
C. 모든 server는 server/discover를 반드시 구현해야 한다. 하지만 client는 이를 건너뛰고 바로 요청을 보낼 수 있다\
D. capability 협상은 client capability에만 적용된다. server가 자기 자신에 대해 가정할 수 있는 범위는 전혀 제한하지 않는다

<details><summary>정답</summary>

A, C. Servers must implement server/discover even though calling it is optional for clients, and a server that needs an undeclared capability must reject the request rather than assume it, using the missing-capability error. Neither rule is relaxed by an earlier successful discovery on the same connection.

A, C. client가 server/discover를 호출하는 것은 선택이지만 server는 이를 반드시 구현해야 한다. 그리고 선언되지 않은 capability가 필요한 server는 그것을 가정하지 말고 missing-capability 오류로 요청을 거부해야 한다. 같은 연결에서 앞서 discovery가 성공했다고 해서 두 규칙 중 어느 것도 완화되지 않는다.

</details>

## 16. [architecture-and-components]

A tool author is deciding whether to add an outputSchema and return structuredContent, or keep returning only a text content block. When is adding outputSchema and structuredContent worth the extra design effort?

A. When downstream code must consume the result programmatically, since structuredContent should still be mirrored into a text block for older clients\
B. Only when the server wants to drop stdio support and force clients onto Streamable HTTP\
C. Only for those particular tools that take absolutely no arguments at all, since arguments and their outputs must always share exactly one single schema\
D. Never, since only image and audio content blocks are ever permitted to appear alongside any structuredContent value at all

**한국어**

tool 작성자가 outputSchema를 추가하고 structuredContent를 반환할지, 아니면 계속 text content block만 반환할지 고민한다. outputSchema와 structuredContent를 추가하는 설계 수고는 언제 가치가 있는가?

A. 후속 코드가 결과를 프로그램으로 처리해야 할 때다. 단, 이전 client를 위해 structuredContent를 text block에도 그대로 담아야 한다\
B. server가 stdio 지원을 끊고 client가 Streamable HTTP만 쓰게 만들려 할 때만이다\
C. 인자를 전혀 받지 않는 특정 tool에만 해당한다. 인자와 출력은 항상 정확히 하나의 스키마를 공유해야 하기 때문이다\
D. 절대 없다. structuredContent 값과 함께 나올 수 있는 content block은 image와 audio뿐이다

<details><summary>정답</summary>

A. structuredContent is valuable exactly when something downstream, a script or another tool, needs to parse the result reliably rather than a human reading prose. Because it must conform to outputSchema and should still be serialized into a text block, it adds a machine-readable contract without breaking older clients.

A. structuredContent는 사람이 산문을 읽는 대신 스크립트나 다른 tool 같은 후속 처리가 결과를 안정적으로 파싱해야 할 때 가치가 있다. structuredContent는 outputSchema를 따라야 하고 text block으로도 직렬화해야 한다. 그래서 이전 client를 깨지 않으면서 기계가 읽을 수 있는 계약을 더한다.

</details>

## 17. [architecture-and-components]

After a tool call fails because an argument had the wrong date format, the host has two options: silently retry the exact same call, or feed the isError result's content back into the model's context. Which is the correct design and why?

A. Escalate every tool execution error straight to a human without giving the model a chance to respond\
B. Feed the tool execution error back to the model, since that lets it read the content and self correct\
C. Discard the failed call entirely and never mention it at all to either the model or the end user\
D. Silently retry unchanged, since repeating the same call with the same arguments will eventually succeed

**한국어**

인자의 날짜 형식이 잘못되어 tool 호출이 실패했다. host에는 두 가지 선택지가 있다. 똑같은 호출을 조용히 다시 시도하거나, isError 결과의 content를 모델 컨텍스트에 다시 넣는 것이다. 어느 쪽이 올바른 설계인가? 그 이유는 무엇인가?

A. 모델에게 대응할 기회를 주지 않고 모든 tool 실행 오류를 곧장 사람에게 올린다\
B. tool 실행 오류를 모델에게 돌려준다. 그러면 모델이 content를 읽고 스스로 고칠 수 있다\
C. 실패한 호출을 완전히 버리고 모델에게도 최종 사용자에게도 전혀 알리지 않는다\
D. 바꾸지 않고 조용히 다시 시도한다. 같은 인자로 같은 호출을 반복하면 결국 성공한다

<details><summary>정답</summary>

B. Tool execution errors carry explanatory content precisely so the model can read what went wrong and adjust, which is the SEP-1303 rationale for treating input validation failures as results rather than opaque protocol errors. Retrying identical arguments blindly cannot fix a formatting mistake the model has not seen.

B. tool 실행 오류에 설명 content가 담기는 이유는 모델이 무엇이 잘못됐는지 읽고 고치게 하려는 것이다. 이것이 SEP-1303이 입력 검증 실패를 불투명한 프로토콜 오류가 아니라 결과로 다루는 근거다. 같은 인자로 무작정 재시도해서는 모델이 보지 못한 형식 오류를 고칠 수 없다.

</details>

## 18. [architecture-and-components]

A manifest reviewer sees a tool definition that omits its annotations object entirely. Under the 2026-07-28 default values, how should the reviewer treat that tool when deciding whether the host needs a confirmation gate?

A. As read-only and safe, since omitting annotations means the server asserts the tool has no side effects\
B. As idempotent by default in every case, since idempotentHint always defaults to true whenever a tool omits its annotations\
C. As destructive by default, since destructiveHint defaults to true and openWorldHint defaults to true when omitted\
D. As unknown, requiring the server's documentation before any confirmation policy can be set at all

**한국어**

manifest 리뷰어가 annotations 객체를 아예 생략한 tool 정의를 본다. 2026-07-28 기본값에 따르면, host에 확인 관문이 필요한지 판단할 때 리뷰어는 이 tool을 어떻게 다뤄야 하는가?

A. 읽기 전용이고 안전한 것으로 본다. annotation을 생략하면 server가 그 tool에 부작용이 없다고 주장하는 셈이다\
B. 모든 경우에 기본적으로 멱등(idempotent)으로 본다. tool이 annotation을 생략하면 idempotentHint의 기본값은 항상 true다\
C. 기본적으로 파괴적인 것으로 본다. 생략하면 destructiveHint와 openWorldHint의 기본값이 true다\
D. 알 수 없는 것으로 본다. 확인 정책을 하나라도 정하려면 먼저 server 문서가 있어야 한다

<details><summary>정답</summary>

C. When annotations are omitted, the specification's defaults apply: readOnlyHint and idempotentHint default to false, while destructiveHint defaults to true and is only meaningful when the tool is not read only. A careful reviewer should treat an undeclared tool as potentially destructive rather than assuming safety.

C. annotation이 생략되면 사양의 기본값이 적용된다. readOnlyHint와 idempotentHint는 기본값이 false다. destructiveHint는 기본값이 true이고 tool이 읽기 전용이 아닐 때만 의미가 있다. 신중한 리뷰어라면 선언이 없는 tool을 안전하다고 가정하지 말고 파괴적일 수 있는 것으로 다뤄야 한다.

</details>

## 19. [interactions-and-execution]

A client library reuses the original JSON-RPC id when it retries a tools/call after answering an input_required result, reasoning that it is still logically the same call. Why does 2026-07-28 require a new id instead?

A. Because the client must close the connection between the original call and its retry attempt\
B. Because ids are case sensitive strings, and reusing one would corrupt the transport framing layer\
C. Because servers will always flatly refuse any id that has ever once been used before, even quite long after a response finally arrived\
D. Because an id must not repeat one still awaiting a response, and the retry is a distinct request with inputResponses

**한국어**

한 client 라이브러리가 input_required 결과에 답한 뒤 tools/call을 재시도할 때 원래 JSON-RPC id를 다시 쓴다. 논리적으로는 여전히 같은 호출이라는 이유다. 2026-07-28이 새 id를 요구하는 이유는 무엇인가?

A. client가 원래 호출과 재시도 사이에 연결을 닫아야 하기 때문이다\
B. id는 대소문자를 구분하는 문자열이라서 재사용하면 transport 프레이밍 계층이 망가지기 때문이다\
C. server는 한 번이라도 쓰인 id를, 응답이 도착하고 한참 지난 뒤라도 항상 단호하게 거부하기 때문이다\
D. id는 아직 응답을 기다리는 요청의 id와 겹쳐서는 안 된다. 그리고 재시도는 inputResponses를 담은 별개의 요청이기 때문이다

<details><summary>정답</summary>

D. A request id must not be reused while still awaiting a response, and the MRTR retry is a new request that adds inputResponses and echoes requestState, not a continuation of the original one. Treating the retry as a fresh request with a new id keeps in-flight tracking correct.

D. 요청 id는 아직 응답을 기다리는 동안에는 재사용해서는 안 된다. MRTR 재시도는 원래 요청의 연속이 아니라 inputResponses를 추가하고 requestState를 그대로 돌려보내는 새 요청이다. 재시도를 새 id를 가진 새 요청으로 다뤄야 진행 중인 요청을 정확하게 추적할 수 있다.

</details>

## 20. [interactions-and-execution]

A client stores an input_required result's requestState, then reformats its whitespace before echoing it back on retry because the two values look semantically equal. What goes wrong with this design if the server protects requestState with an HMAC?

A. The reformatted bytes no longer match what the HMAC was computed over, so the server rejects the retry as tampered\
B. Nothing, since JSON-RPC guarantees semantically equal payloads always hash to the exact same value\
C. The retry gets treated as a brand new tool call entirely unrelated to the original request\
D. The server would always fall back immediately to the deprecated Dynamic Client Registration flow just to verify that one pending state

**한국어**

client가 input_required 결과의 requestState를 저장한다. 그리고 재시도 때 돌려보내기 전에, 두 값이 의미상 같아 보인다며 공백을 다시 정리한다. server가 requestState를 HMAC으로 보호한다면 이 설계에서 무엇이 잘못되는가?

A. 다시 정리한 바이트가 HMAC 계산에 쓰인 바이트와 더 이상 일치하지 않는다. 그래서 server가 재시도를 변조된 것으로 보고 거부한다\
B. 아무 문제도 없다. JSON-RPC는 의미상 같은 페이로드가 항상 정확히 같은 해시값을 낸다고 보장한다\
C. 재시도가 원래 요청과 전혀 관계없는 완전히 새로운 tool 호출로 처리된다\
D. server가 대기 중인 그 상태 하나를 검증하려고 항상 즉시 지원 중단된 Dynamic Client Registration 흐름으로 돌아간다

<details><summary>정답</summary>

A. requestState is meant to be echoed exactly, and an HMAC or AEAD is computed over the original bytes, so any reformatting, even whitespace only, changes what the server verifies against. Treating it as opaque and echoing it unmodified is the only design that keeps the integrity check meaningful.

A. requestState는 받은 그대로 돌려보내야 하는 값이다. HMAC이나 AEAD는 원래 바이트를 대상으로 계산된다. 그래서 공백만 바꾸더라도 어떤 재정리든 server가 검증하는 대상을 바꾼다. requestState를 opaque한 값으로 다루고 수정 없이 그대로 돌려보내는 것만이 무결성 검사를 의미 있게 유지하는 설계다.

</details>

## 21. [interactions-and-execution]

A server designer is choosing how to report that a tool argument violates the tool's inputSchema, for example a date string in the wrong format. Which design is correct for 2026-07-28, and why?

A. Return a JSON-RPC error with code -32602, since any schema violation counts as invalid params\
B. Return a normal result with isError true and explanatory content, since the model can read and act on it\
C. Return a JSON-RPC error with code -32601, since the tool effectively could not ever be found in that particular shape\
D. Silently coerce the malformed date into a best guess and proceed without informing anyone at all

**한국어**

server 설계자가 tool 인자가 tool의 inputSchema를 어긴 경우를 어떻게 보고할지 정하고 있다. 예를 들어 날짜 문자열의 형식이 잘못된 경우다. 2026-07-28에서 올바른 설계는 무엇인가? 그 이유는 무엇인가?

A. 코드 -32602인 JSON-RPC 오류를 반환한다. 스키마 위반은 모두 invalid params에 해당한다\
B. isError가 true이고 설명 content가 담긴 일반 결과를 반환한다. 모델이 이를 읽고 대응할 수 있다\
C. 코드 -32601인 JSON-RPC 오류를 반환한다. 그 형태로는 사실상 tool을 결코 찾을 수 없기 때문이다\
D. 잘못된 날짜를 짐작으로 조용히 바꾸고 아무에게도 알리지 않고 진행한다

<details><summary>정답</summary>

B. SEP-1303 reclassified input validation failures as tool execution errors rather than protocol errors, specifically because a result the model can read and act on supports self correction, while a bare JSON-RPC error does not carry that feedback loop back into the model's context.

B. SEP-1303은 입력 검증 실패를 프로토콜 오류가 아닌 tool 실행 오류로 다시 분류했다. 구체적인 이유는 모델이 읽고 대응할 수 있는 결과가 스스로 고치는 것을 돕기 때문이다. 반면 그냥 JSON-RPC 오류는 그 피드백을 모델 컨텍스트로 되돌려 보내지 못한다.

</details>

## 22. [interactions-and-execution]

A Streamable HTTP request's SSE response stream breaks partway through an idempotent tool call. What is the correct client design for recovering, given that streams cannot resume in 2026-07-28?

A. Abandon the operation permanently and completely, since a broken stream means it can never be attempted again\
B. Reuse the exact original request id anyway, since resuming with Last-Event-ID will simply pick the stream back up again\
C. Reissue the call as a new request with a new id, since idempotency makes a safe replay possible\
D. Wait indefinitely, since the original request is still technically in flight somewhere on the server

**한국어**

idempotent tool 호출 도중 Streamable HTTP 요청의 SSE 응답 스트림이 끊긴다. 2026-07-28에서는 스트림을 재개할 수 없다. 이 점을 고려할 때 복구를 위한 올바른 client 설계는 무엇인가?

A. 작업을 완전히 영구 포기한다. 스트림이 끊기면 다시는 시도할 수 없다\
B. 그래도 원래 요청 id를 그대로 다시 쓴다. Last-Event-ID로 재개하면 스트림이 그냥 다시 이어진다\
C. 새 id를 가진 새 요청으로 호출을 다시 보낸다. idempotent하므로 안전하게 다시 실행할 수 있다\
D. 무기한 기다린다. 원래 요청은 엄밀히 말해 server 어딘가에서 아직 진행 중이다

<details><summary>정답</summary>

C. Because stream resumability and Last-Event-ID were removed, a broken stream loses the request outright, so the client must reissue it as a fresh request with a new id. Idempotency is exactly the property that makes reissuing safe rather than risking a duplicate side effect.

C. 스트림 재개와 Last-Event-ID가 제거됐으므로 스트림이 끊기면 그 요청은 그대로 사라진다. 그래서 client는 새 id를 가진 새 요청으로 다시 보내야 한다. 다시 보내도 중복 부작용이 생기지 않고 안전한 것은 바로 idempotent하다는 성질 덕분이다.

</details>

## 23. [interactions-and-execution]

A server implementation is being reviewed, and it sends notifications/progress for a long tool call onto the same subscriptions/listen stream the client opened for list-changed notifications, reasoning it saves a connection. Why is this design wrong for 2026-07-28?

A. Because progress notifications were removed entirely from the specification as of this particular revision release\
B. Because subscriptions/listen streams are only ever permitted to carry plain resource content, and never any notifications of any kind at all\
C. Because a listen stream can only be opened once per server for each calendar day\
D. Because progress notifications may only flow on the response stream of the request they relate to, not a listen stream

**한국어**

검토 중인 server 구현이 긴 tool 호출의 notifications/progress를, client가 list-changed 알림용으로 열어 둔 subscriptions/listen 스트림으로 보낸다. 연결 하나를 아낀다는 이유다. 2026-07-28에서 이 설계가 틀린 이유는 무엇인가?

A. progress 알림이 이 revision 릴리스부터 사양에서 완전히 제거됐기 때문이다\
B. subscriptions/listen 스트림은 순수한 resource content만 실을 수 있고 어떤 종류의 알림도 실을 수 없기 때문이다\
C. listen 스트림은 server마다 하루에 한 번만 열 수 있기 때문이다\
D. progress 알림은 관련된 요청의 응답 스트림으로만 흐를 수 있고 listen 스트림으로는 흐를 수 없기 때문이다

<details><summary>정답</summary>

D. Request-scoped notifications such as progress and log messages are defined to flow only on the response stream of the request they relate to, while a listen stream carries subscription notifications like list_changed. Mixing them would make demultiplexing ambiguous for any client following the specification.

D. progress와 log 메시지 같은 요청 범위 알림은 관련된 요청의 응답 스트림으로만 흐르도록 정의되어 있다. listen 스트림은 list_changed 같은 구독 알림을 전달한다. 둘을 섞으면 사양을 따르는 client가 메시지를 어느 쪽으로 나눌지 모호해진다.

</details>

## 24. [interactions-and-execution]

A resources/read result returns a price list that is identical for every caller, regardless of who is asking. Which cacheScope is the correct design choice, and what should the server still keep separate from that choice?

A. cacheScope public, while still applying whatever per-user access control the resource needs, since cacheScope is a sharing hint, not access control\
B. cacheScope public, which by itself is enough to guarantee only authorized users can read it\
C. No cacheScope field at all, since caching hints are optional whenever content rarely changes\
D. cacheScope private in every single case, because any resource that ever touches money must always be treated as extremely sensitive no matter what here

**한국어**

resources/read 결과가 누가 요청하든 모든 호출자에게 똑같은 가격표를 반환한다. 올바른 cacheScope 선택은 무엇인가? 그리고 server가 그 선택과 별도로 여전히 지켜야 할 것은 무엇인가?

A. cacheScope public. 단, 그 resource에 필요한 사용자별 접근 제어는 그대로 적용한다. cacheScope는 공유 힌트이지 접근 제어가 아니다\
B. cacheScope public. 이것만으로 인가된 사용자만 읽을 수 있다는 것이 보장된다\
C. cacheScope 필드를 아예 두지 않는다. 내용이 거의 바뀌지 않으면 캐싱 힌트는 선택 사항이다\
D. 모든 경우에 cacheScope private. 돈과 조금이라도 관련된 resource는 무슨 일이 있어도 항상 매우 민감한 것으로 다뤄야 한다

<details><summary>정답</summary>

A. cacheScope public is the right fit for content that is genuinely identical across callers, but the specification is explicit that cacheScope is never an access control mechanism, so authorization must still be enforced on every read independent of whether the result can be shared in a cache.

A. 모든 호출자에게 정말로 똑같은 content라면 cacheScope public이 맞다. 하지만 사양은 cacheScope가 결코 접근 제어 메커니즘이 아니라고 명시한다. 그래서 결과를 캐시에서 공유할 수 있는지와 관계없이 읽기 요청마다 인가를 계속 검사해야 한다.

</details>

## 25. [interactions-and-execution]

A tools/list result contains fewer entries for a user with a narrower OAuth scope than for an administrator calling the same server. Does this violate the requirement that list results must not vary per connection?

A. No, but only if the two users happen to be using two different transports entirely\
B. No, since list results may vary by the authorization on the request, just not by the connection itself\
C. Yes, unless the administrator explicitly and repeatedly calls tools/list twice over before the comparison ever finally becomes valid\
D. Yes, since any variation at all in a list result is forbidden under the stateless core

**한국어**

같은 server를 호출할 때, OAuth scope가 더 좁은 사용자의 tools/list 결과는 관리자의 결과보다 항목이 적다. 이것은 목록 결과가 연결마다 달라져서는 안 된다는 요구 사항을 어기는가?

A. 아니다. 단, 두 사용자가 마침 완전히 다른 transport를 쓰는 경우에만 그렇다\
B. 아니다. 목록 결과는 요청에 담긴 인가에 따라 달라질 수 있다. 연결 자체에 따라 달라지면 안 될 뿐이다\
C. 그렇다. 비교가 유효해지기 전에 관리자가 명시적으로 tools/list를 두 번 반복해 호출하지 않는 한 그렇다\
D. 그렇다. stateless 핵심 규칙에서는 목록 결과가 조금이라도 달라지는 것이 금지된다

<details><summary>정답</summary>

B. The stateless core forbids list results from varying as a side effect of the connection or prior requests, but it explicitly allows variation by the authorization presented on a request. Two different scopes producing two different lists is exactly that permitted case, not a violation.

B. stateless 핵심 규칙은 목록 결과가 연결이나 이전 요청의 부수 효과로 달라지는 것을 금지한다. 하지만 요청에 제시된 인가에 따라 달라지는 것은 명시적으로 허용한다. scope가 달라서 목록이 다른 것은 바로 그 허용된 경우이며 위반이 아니다.

</details>

## 26. [interactions-and-execution] (복수 정답)

Which two statements correctly describe how a subscriptions/listen stream must behave under 2026-07-28? Select two.

A. The server may freely send any notification type on the stream even if the client never asked to filter for it\
B. Every message on the stream carries a subscriptionId in _meta equal to the JSON-RPC id of the original listen request\
C. A client must always fully reopen a fresh subscriptions/listen call before every single tool invocation it ever makes at all\
D. The first message on the stream must be notifications/subscriptions/acknowledged, echoing the subset of notification types the server will actually honor

**한국어**

2026-07-28에서 subscriptions/listen 스트림이 따라야 할 동작을 바르게 설명한 것 두 가지는 무엇인가? 두 개 고르시오.

A. client가 받겠다고 지정한 적이 없는 알림 유형이라도 server는 스트림으로 자유롭게 보낼 수 있다\
B. 스트림의 모든 메시지는 _meta에 subscriptionId를 담는다. 그 값은 원래 listen 요청의 JSON-RPC id와 같다\
C. client는 tool을 호출할 때마다 매번 반드시 새 subscriptions/listen 호출을 완전히 다시 열어야 한다\
D. 스트림의 첫 메시지는 반드시 notifications/subscriptions/acknowledged여야 한다. 이 메시지는 server가 실제로 처리할 알림 유형의 부분 집합을 그대로 돌려준다

<details><summary>정답</summary>

B, D. A listen stream opens with an acknowledgment naming what the server will honor, and every later message is tagged with the subscriptionId tying it back to that listen request's id. The server must never send unrequested notification types, and listening has no required relationship to individual tool calls.

B, D. listen 스트림은 server가 처리할 항목을 밝히는 확인 메시지로 시작한다. 이후 모든 메시지에는 그 listen 요청의 id와 연결하는 subscriptionId가 붙는다. server는 요청받지 않은 알림 유형을 절대 보내서는 안 되며 listen은 개별 tool 호출과 필수 관계가 없다.

</details>

## 27. [interactions-and-execution]

A report-generation tool typically takes ten minutes to finish. A designer is choosing between a plain synchronous tools/call, an MRTR input_required exchange, and the tasks extension. Which fits, and why do the other two fail?

A. A plain synchronous call instead, since clients are generally expected to hold a connection open for as long as it ever takes\
B. MRTR, since ten whole minutes of waiting is exactly what elicitation was originally built to fill with a prompt\
C. The tasks extension, since a synchronous call risks timeouts and MRTR is for gathering input, not for waiting out work\
D. None of the three, since long work must always be split into ten separate one-minute calls

**한국어**

보고서 생성 tool은 끝나는 데 보통 10분이 걸린다. 설계자가 일반 동기 tools/call, MRTR input_required 교환, tasks extension 중에서 고르고 있다. 어느 것이 맞는가? 나머지 둘은 왜 맞지 않는가?

A. 일반 동기 호출. client는 대개 얼마가 걸리든 연결을 열어 둘 것으로 기대된다\
B. MRTR. 꼬박 10분의 대기는 elicitation이 원래 프롬프트로 채우려고 만든 바로 그 시간이다\
C. tasks extension. 동기 호출은 타임아웃 위험이 있다. MRTR은 작업을 기다리는 용도가 아니라 입력을 모으는 용도다\
D. 셋 다 아니다. 긴 작업은 항상 1분짜리 호출 10개로 나눠야 한다

<details><summary>정답</summary>

C. A blocking synchronous call over ten minutes risks intermediary and client timeouts, and MRTR exists to gather input mid-flight, not to represent duration. The tasks extension is purpose built for durable long running work, returning a task handle the client polls instead of holding a connection open.

C. 10분 넘게 막고 있는 동기 호출은 중간 장비와 client의 타임아웃 위험이 있다. MRTR은 실행 도중 입력을 모으려고 있는 것이지 작업 시간을 표현하려는 것이 아니다. tasks extension은 오래 걸리는 작업을 지속적으로 관리하려고 만든 것이다. client가 연결을 열어 두는 대신 폴링할 task handle을 돌려준다.

</details>

## 28. [interactions-and-execution]

A tool that runs a multi-step build pipeline needs to track progress across several minutes. Where should that intermediate state live: in requestState, inside the task object the tasks extension returns, or in ordinary server-side storage referenced by an opaque id?

A. In ordinary server-side storage only, and never expose any id to the client at all\
B. In requestState alone, since it is specifically designed to carry any data a server wants to keep across calls indefinitely\
C. Split evenly across all three separate locations so that a missing one still leaves a copy safely elsewhere\
D. In the task object and server-side storage keyed by taskId, since requestState survives only one MRTR round trip

**한국어**

여러 단계로 된 빌드 파이프라인을 실행하는 tool이 몇 분에 걸쳐 진행 상황을 추적해야 한다. 이 중간 상태는 어디에 두어야 하는가? 후보는 requestState, tasks extension이 반환하는 task 객체, opaque id로 참조하는 일반 server 측 저장소다.

A. 일반 server 측 저장소에만 둔다. client에게는 어떤 id도 노출하지 않는다\
B. requestState에만 둔다. requestState는 server가 여러 호출에 걸쳐 무기한 유지하려는 어떤 데이터든 담도록 설계됐다\
C. 세 곳에 고르게 나눠 둔다. 하나가 없어져도 다른 곳에 사본이 안전하게 남는다\
D. task 객체와, taskId를 키로 한 server 측 저장소에 둔다. requestState는 MRTR 왕복 한 번만 유지된다

<details><summary>정답</summary>

D. requestState exists to survive exactly one MRTR round trip and is explicitly never cached or reused beyond that, while the tasks extension's taskId and its backing server-side storage are the purpose built mechanism for state that spans a multi-step, minutes-long operation the client polls.

D. requestState는 MRTR 왕복 정확히 한 번을 넘기려고 있는 값이다. 그 이상 캐시되거나 재사용되지 않는다고 명시되어 있다. 반면 tasks extension의 taskId와 그 뒤의 server 측 저장소는 여러 단계에 몇 분씩 걸리고 client가 폴링하는 작업의 상태를 위해 만든 메커니즘이다.

</details>

## 29. [interactions-and-execution]

A server author wants to add an application-defined error code meaning quota exceeded. Which numeric range is the correct place for it under the 2026-07-28 allocation policy?

A. Outside -32768 to -32000 entirely, since that whole band is reserved for JSON-RPC and MCP use\
B. Anywhere in -32000 to -32019, since that legacy band is open for any implementation to reuse\
C. Anywhere at all in -32020 to -32099, since that band is available for any server-specific extension error code\
D. It does not matter, since JSON-RPC error codes are purely cosmetic and never machine interpreted

**한국어**

server 작성자가 할당량 초과를 뜻하는 애플리케이션 정의 오류 코드를 추가하려 한다. 2026-07-28 할당 정책에서 이 코드를 둘 올바른 숫자 범위는 어디인가?

A. -32768에서 -32000 범위의 완전히 바깥. 그 범위 전체는 JSON-RPC와 MCP용으로 예약되어 있다\
B. -32000에서 -32019 사이 아무 곳. 이 레거시 범위는 어떤 구현이든 재사용할 수 있게 열려 있다\
C. -32020에서 -32099 사이 아무 곳. 이 범위는 server별 확장 오류 코드라면 무엇이든 쓸 수 있다\
D. 상관없다. JSON-RPC 오류 코드는 순전히 표시용이며 기계가 해석하지 않는다

<details><summary>정답</summary>

A. The allocation policy reserves -32000 to -32019 for legacy use only, -32020 to -32099 for the MCP specification itself, and directs application-defined codes outside the entire -32768 to -32000 band. A quota-exceeded code belongs outside that whole reserved range.

A. 할당 정책은 -32000에서 -32019를 레거시 용도로만, -32020에서 -32099를 MCP 사양 자체용으로 예약한다. 애플리케이션 정의 코드는 -32768에서 -32000 범위 전체의 바깥에 두도록 한다. 할당량 초과 코드는 그 예약 범위 전체의 바깥에 있어야 한다.

</details>

## 30. [interactions-and-execution] (복수 정답)

A server currently uses the deprecated sampling capability to ask the client's model for a completion mid-tool-call. Which two statements correctly describe migrating away from it under 2026-07-28? Select two.

A. Sampling still functions as an MRTR input request today, so the migration can happen on the server's own schedule before removal\
B. A client that has still not declared the sampling capability on a request can still be asked for a completion through it anyway\
C. Sampling must be removed immediately, since deprecated features are stripped out of every server the moment a new revision ships\
D. The recommended migration is for the server to call an LLM provider API directly instead of asking the client for a completion

**한국어**

한 server가 tool 호출 도중 client의 모델에게 completion을 요청하려고 지원 중단된 sampling capability를 쓰고 있다. 2026-07-28에서 sampling을 걷어 내는 마이그레이션을 바르게 설명한 것 두 가지는 무엇인가? 두 개 고르시오.

A. sampling은 지금도 MRTR input request로 동작한다. 그래서 제거 전까지 server 자체 일정에 맞춰 마이그레이션할 수 있다\
B. 요청에서 아직 sampling capability를 선언하지 않은 client에게도 sampling으로 completion을 요청할 수 있다\
C. sampling은 즉시 제거해야 한다. 새 revision이 나오는 순간 지원 중단된 기능은 모든 server에서 제거된다\
D. 권장 마이그레이션은 client에게 completion을 요청하는 대신 server가 LLM 제공자 API를 직접 호출하는 것이다

<details><summary>정답</summary>

A, D. Sampling is deprecated with a clear migration path, calling a provider API directly, but it remains functional through MRTR today, so teams can migrate on their own timeline rather than under immediate pressure. A client must still declare the capability before a server may request it.

A, D. sampling은 제공자 API를 직접 호출한다는 분명한 마이그레이션 경로와 함께 지원 중단됐지만 지금도 MRTR을 통해 동작한다. 그래서 팀은 당장의 압박 없이 자기 일정에 맞춰 옮길 수 있다. server가 sampling을 요청하려면 client가 여전히 그 capability를 먼저 선언해야 한다.

</details>

## 31. [interactions-and-execution] (복수 정답)

Select the three failures that 2026-07-28 treats as JSON-RPC protocol errors rather than tool execution errors.

A. A tools/call whose arguments violate the target tool's own inputSchema\
B. A tools/call naming a tool that the server does not expose at all\
C. A request that is missing the required io.modelcontextprotocol/clientCapabilities per-request meta field\
D. A request whose shape does not satisfy the CallToolRequest schema at all

**한국어**

2026-07-28이 tool 실행 오류가 아닌 JSON-RPC 프로토콜 오류로 다루는 실패 세 가지를 고르시오.

A. 인자가 대상 tool 자체의 inputSchema를 어긴 tools/call\
B. server가 아예 노출하지 않는 tool 이름을 지정한 tools/call\
C. 필수 요청별 meta 필드인 io.modelcontextprotocol/clientCapabilities가 빠진 요청\
D. 형태가 CallToolRequest 스키마를 전혀 만족하지 않는 요청

<details><summary>정답</summary>

B, C, D. An unknown tool, a request that fails the CallToolRequest schema, and a request missing required per-request metadata are all protocol level failures returned as JSON-RPC errors. Arguments that merely violate a specific tool's inputSchema are, by contrast, a tool execution error the model can read and correct.

B, C, D. 알 수 없는 tool, CallToolRequest 스키마를 통과하지 못한 요청, 필수 요청별 메타데이터가 빠진 요청은 모두 프로토콜 수준의 실패이며 JSON-RPC 오류로 반환된다. 반면 특정 tool의 inputSchema만 어긴 인자는 모델이 읽고 고칠 수 있는 tool 실행 오류다.

</details>

## 32. [interactions-and-execution]

A client library parses a tools/list cursor string, assumes it is a base ten offset, and increments it locally instead of sending back exactly what the server issued. Why is this design fragile under 2026-07-28?

A. Because an incremented cursor is always immediately rejected outright with a -32601 method not found error every single time\
B. Because a cursor is opaque and server chosen, so reinterpreting it breaks the moment the server changes page size\
C. Because cursors stay valid for exactly one minute after being issued, regardless of how they are used\
D. Because pagination was replaced entirely by cacheScope in this particular revision, and cursors simply no longer exist

**한국어**

client 라이브러리가 tools/list cursor 문자열을 파싱해 10진수 offset이라고 가정한다. 그리고 server가 발급한 값을 그대로 돌려보내는 대신 로컬에서 값을 늘린다. 2026-07-28에서 이 설계가 취약한 이유는 무엇인가?

A. 증가시킨 cursor는 매번 즉시 -32601 method not found 오류로 거부되기 때문이다\
B. cursor는 opaque하고 server가 정하는 값이다. 그래서 이를 다르게 해석하면 server가 페이지 크기를 바꾸는 순간 깨지기 때문이다\
C. cursor는 사용 방식과 관계없이 발급 후 정확히 1분 동안만 유효하기 때문이다\
D. 이 revision에서 페이지네이션이 cacheScope로 완전히 대체되어 cursor가 더 이상 존재하지 않기 때문이다

<details><summary>정답</summary>

B. Pagination cursors are opaque by design specifically so the server retains freedom over page size and internal ordering. A client that parses and reconstructs a cursor is relying on an implementation detail that can silently break with any server-side change, which is exactly what opacity is meant to prevent.

B. 페이지네이션 cursor는 server가 페이지 크기와 내부 정렬을 자유롭게 정할 수 있도록 일부러 opaque하게 설계됐다. cursor를 파싱해 다시 만드는 client는 server 쪽이 바뀌면 조용히 깨질 수 있는 구현 세부 사항에 기대는 것이다. opaque하게 만든 목적이 바로 이것을 막는 데 있다.

</details>

## 33. [interactions-and-execution]

A server needs to end a subscriptions/listen stream in two different situations: a graceful, planned end of the subscription, and an abrupt teardown the server initiates. What is the correct message for each case?

A. A graceful end always uses a fresh tools/call, and a teardown always uses server/discover instead\
B. Both cases always use the exact same notifications/cancelled message, since the client cannot ever tell the difference\
C. A graceful end answers the listen request with a complete result, while notifications/cancelled tears the stream down\
D. Both cases require the client to reopen a brand new HTTP connection before either message sends

**한국어**

server가 두 가지 상황에서 subscriptions/listen 스트림을 끝내야 한다. 하나는 계획된 정상 종료다. 다른 하나는 server가 시작하는 갑작스러운 해제다. 각 경우에 맞는 메시지는 무엇인가?

A. 정상 종료에는 항상 새 tools/call을 쓰고 해제에는 항상 server/discover를 쓴다\
B. 두 경우 모두 항상 똑같은 notifications/cancelled 메시지를 쓴다. client는 둘의 차이를 결코 구분할 수 없다\
C. 정상 종료는 listen 요청에 완료 결과로 응답한다. notifications/cancelled는 스트림을 해제한다\
D. 두 경우 모두 어느 메시지든 보내기 전에 client가 새 HTTP 연결을 다시 열어야 한다

<details><summary>정답</summary>

C. The specification treats a graceful end and a server-initiated teardown as distinct: a graceful close answers the original listen request with a complete result carrying the subscription id, while notifications/cancelled is the message reserved specifically for the server tearing a listen stream down.

C. 사양은 정상 종료와 server가 시작하는 해제를 구분한다. 정상 종료는 원래 listen 요청에 subscription id를 담은 완료 결과로 응답한다. notifications/cancelled는 server가 listen 스트림을 해제할 때만 쓰도록 정해진 메시지다.

</details>

## 34. [interactions-and-execution]

A destructive tool call times out with no response, and its server-reported idempotentHint is false. Since annotations are self reported and untrusted unless the server itself is trusted, what should the client's retry design do?

A. Retry only after first fully switching the entire connection over to stdio, since stdio never produces any ambiguous timeouts at all\
B. Retry immediately and automatically with a new id, trusting the reported hint to mean it is safe\
C. Ignore the timeout entirely and wait forever rather than making any decision about it at all\
D. Treat the outcome as unknown and require explicit confirmation before reissuing a call whose effect could not be observed

**한국어**

파괴적인 tool 호출이 응답 없이 타임아웃됐다. server가 보고한 idempotentHint는 false다. annotation은 자체 보고된 값이며 server 자체를 신뢰할 때가 아니면 신뢰할 수 없다. client의 재시도 설계는 어떻게 해야 하는가?

A. 먼저 연결 전체를 stdio로 완전히 바꾼 뒤에만 재시도한다. stdio에서는 모호한 타임아웃이 전혀 생기지 않는다\
B. 보고된 힌트가 안전하다는 뜻이라고 믿고 새 id로 즉시 자동 재시도한다\
C. 타임아웃을 완전히 무시하고 아무 결정도 내리지 않은 채 영원히 기다린다\
D. 결과를 알 수 없는 것으로 본다. 효과를 확인하지 못한 호출을 다시 보내기 전에 명시적 확인을 요구한다

<details><summary>정답</summary>

D. Because idempotentHint is an untrusted, self reported hint from a server that may not be trusted, a client cannot safely assume a timed out destructive call is either done or safe to repeat. The prudent design treats the outcome as unknown and requires explicit confirmation before retrying.

D. idempotentHint는 신뢰할 수 없을 수도 있는 server가 자체 보고한, 신뢰할 수 없는 힌트다. 그래서 client는 타임아웃된 파괴적 호출이 이미 실행됐다고도, 반복해도 안전하다고도 가정할 수 없다. 신중한 설계는 결과를 알 수 없는 것으로 보고 재시도 전에 명시적 확인을 요구한다.

</details>

## 35. [security-and-governance]

A tool's description field contains text instructing the model to also call a different, unrelated server's delete_account tool whenever this tool is used. What is the correct trust-boundary response to this discovery?

A. Treat the tool description as untrusted content, refuse to let it steer a call into another server, and flag it for review\
B. Forward the instruction straight to the model without any comment at all, since the model must filter unsafe text entirely on its own\
C. Delete the entire tool registry for every connected server, since one bad description proves all are compromised\
D. Trust the instruction fully, since any text inside a tool definition is implicitly approved already by the operator

**한국어**

한 tool의 description 필드에, 이 tool을 쓸 때마다 관계없는 다른 server의 delete_account tool도 호출하라고 모델에게 지시하는 문구가 들어 있다. 이를 발견했을 때 신뢰 경계 관점에서 올바른 대응은 무엇인가?

A. tool 설명을 신뢰할 수 없는 content로 다룬다. 그 설명이 다른 server로 호출을 유도하지 못하게 막고 검토 대상으로 표시한다\
B. 아무 말 없이 그 지시를 모델에게 그대로 전달한다. 위험한 문구는 모델이 전적으로 스스로 걸러야 한다\
C. 연결된 모든 server의 tool registry 전체를 삭제한다. 나쁜 설명 하나가 모두 침해됐다는 증거다\
D. 지시를 전적으로 신뢰한다. tool 정의 안의 문구는 무엇이든 운영자가 이미 암묵적으로 승인한 것이다

<details><summary>정답</summary>

A. Tool descriptions, like other server-authored content, are untrusted input to the model and can carry tool poisoning or cross-server steering attempts. The correct design quarantines the instruction rather than acting on it, and treats a server that ships one as a candidate for review, not blanket distrust of every server.

A. tool 설명은 server가 작성한 다른 content와 마찬가지로 모델에게 신뢰할 수 없는 입력이다. tool poisoning이나 server 간 유도 시도를 담을 수 있다. 올바른 설계는 그 지시에 따라 행동하지 않고 격리한다. 그런 설명을 내보낸 server는 검토 후보로 다룬다. 모든 server를 일괄로 불신하는 것은 아니다.

</details>

## 36. [security-and-governance]

A resources/read result returns document text that includes an embedded instruction telling the model to ignore its current task and export credentials. Which trust zone is responsible for making sure that instruction never reaches the model as if it were trustworthy guidance?

A. The model itself, since it alone stays fully responsible for recognizing every single injection attempt entirely unaided\
B. The host and client, since they own labeling and sanitizing untrusted content before it enters model context\
C. The authorization server, since OAuth scopes are what prevent embedded instructions from ever being read\
D. The upstream document's original author, since the host has no ability to inspect resource content at all

**한국어**

resources/read 결과로 돌아온 문서 텍스트에, 모델에게 현재 작업을 무시하고 자격 증명을 내보내라는 지시가 숨어 있다. 그 지시가 믿을 만한 안내인 것처럼 모델에게 전달되지 않도록 책임지는 신뢰 영역(trust zone)은 어디인가?

A. 모델 자신. 모든 주입 시도를 아무 도움 없이 알아내는 책임은 모델에게만 있다\
B. host와 client. 신뢰할 수 없는 content가 모델 컨텍스트에 들어가기 전에 표시하고 정제하는 일을 맡는다\
C. authorization server. 숨은 지시가 아예 읽히지 않게 막는 것이 OAuth scope다\
D. 원래 문서 작성자. host는 resource content를 전혀 들여다볼 수 없다

<details><summary>정답</summary>

B. Resource contents are untrusted input the same as tool results, and the host and client are the trust zone positioned to label and sanitize such content before it becomes part of the model's context, rather than leaving detection entirely to the model or to unrelated infrastructure like the authorization server.

B. resource content는 tool 결과와 마찬가지로 신뢰할 수 없는 입력이다. host와 client는 그런 content가 모델 컨텍스트에 들어가기 전에 표시하고 정제할 수 있는 위치에 있는 신뢰 영역이다. 탐지를 전적으로 모델에게 맡기거나 authorization server처럼 관계없는 인프라에 맡길 일이 아니다.

</details>

## 37. [security-and-governance]

A client team notices its authorization server ignores the RFC 8707 resource parameter and proposes dropping it from the authorization and token requests to simplify the code. What is the correct design decision?

A. Replace it with a custom audience header, since servers read audience from headers\
B. Drop it from the token request only, since audience is fixed at authorization time\
C. Keep sending it on both requests, since MCP clients must send it whether or not it is supported\
D. Drop it, since a parameter the authorization server ignores cannot affect security at all

**한국어**

한 client 팀이 자기 authorization server가 RFC 8707 resource 파라미터를 무시한다는 것을 알았다. 그래서 코드를 단순하게 하려고 authorization 요청과 token 요청에서 이 파라미터를 빼자고 제안한다. 올바른 설계 결정은 무엇인가?

A. 맞춤 audience 헤더로 바꾼다. server는 헤더에서 audience를 읽는다\
B. token 요청에서만 뺀다. audience는 authorization 시점에 고정된다\
C. 두 요청 모두에 계속 보낸다. MCP client는 지원 여부와 관계없이 이 파라미터를 보내야 한다\
D. 뺀다. authorization server가 무시하는 파라미터는 보안에 전혀 영향을 줄 수 없다

<details><summary>정답</summary>

C. MCP clients must send the resource parameter, naming the canonical server URI, on both the authorization and token requests regardless of whether the authorization server supports it. It is what lets a supporting authorization server bind a token's audience to one MCP server, so dropping it silently weakens the deployment the day the authorization server starts honoring it.

C. MCP client는 authorization server의 지원 여부와 관계없이 authorization 요청과 token 요청 모두에 resource 파라미터를 보내야 한다. 이 파라미터에는 canonical server URI를 지정한다. 이를 지원하는 authorization server는 이 파라미터로 token의 audience를 MCP server 하나에 묶는다. 그래서 이를 빼면 authorization server가 이 파라미터를 반영하기 시작하는 날 배포의 보안이 조용히 약해진다.

</details>

## 38. [security-and-governance]

An MCP server needs to call a third-party API on the user's behalf. One design passes the user's incoming bearer token straight through to that API; another design has the server obtain and use a separate token scoped to that API. Which design is correct, and why?

A. Pass the incoming token through, but only when the API happens to be owned by the same company\
B. Cache the incoming token locally and openly replay it to every downstream system the tool ever needs to reach at all times\
C. Pass the incoming token through, since reusing one token everywhere is simpler for the user to reason about\
D. Obtain a separate token for the API, since passthrough is forbidden and a server must validate any token it accepts

**한국어**

MCP server가 사용자를 대신해 서드파티 API를 호출해야 한다. 한 설계는 들어온 사용자의 bearer token을 그 API에 그대로 넘긴다. 다른 설계는 server가 그 API용 scope를 가진 별도 token을 받아 쓴다. 어느 설계가 올바른가? 그 이유는 무엇인가?

A. 들어온 token을 그대로 넘긴다. 단, 그 API가 마침 같은 회사 소유인 경우에만 그렇게 한다\
B. 들어온 token을 로컬에 캐시해 둔다. tool이 접근해야 하는 모든 downstream 시스템에 언제나 공공연히 재사용한다\
C. 들어온 token을 그대로 넘긴다. token 하나를 어디서나 재사용하는 편이 사용자가 이해하기 더 쉽다\
D. API용 별도 token을 받는다. passthrough는 금지되어 있고 server는 받아들이는 모든 token을 검증해야 한다

<details><summary>정답</summary>

D. Token passthrough is explicitly forbidden because a token issued for the MCP server was never validated as intended for the downstream API, creating a confused deputy risk. The server must validate audience for tokens it accepts and obtain its own, separately scoped token for any upstream call it makes.

D. token passthrough는 명시적으로 금지되어 있다. MCP server용으로 발급된 token은 downstream API용인지 검증된 적이 없으므로 confused deputy 위험이 생기기 때문이다. server는 받아들이는 token의 audience를 검증해야 한다. 또 자신이 하는 모든 upstream 호출에는 별도 scope를 가진 자기 token을 받아야 한다.

</details>

## 39. [security-and-governance]

A gateway operator proposes forwarding the single incoming bearer token unchanged to every downstream MCP server behind the gateway, arguing it removes the need to manage per-server credentials. What is the flaw in this design?

A. It lets a token issued for one audience reach servers it was never validated against, which passthrough forbids\
B. It only ever really fails when the gateway uses Streamable HTTP rather than stdio to reach far downstream servers anyway every time\
C. It is flawless, since a bearer token by definition already grants unlimited access to any receiver\
D. It is unnecessary to avoid, since audience validation was removed from the specification in this revision

**한국어**

gateway 운영자가 들어온 bearer token 하나를 gateway 뒤의 모든 downstream MCP server에 그대로 전달하자고 제안한다. 그러면 server별 자격 증명을 관리할 필요가 없어진다는 주장이다. 이 설계의 결함은 무엇인가?

A. 한 audience용으로 발급된 token이 검증된 적 없는 server에 전달된다. passthrough 금지가 막는 것이 바로 이것이다\
B. gateway가 멀리 있는 downstream server에 연결할 때 stdio가 아닌 Streamable HTTP를 쓰는 경우에만 실제로 문제가 된다\
C. 결함이 없다. bearer token은 정의상 이미 어떤 수신자에게든 무제한 접근을 허용한다\
D. 피할 필요가 없다. 이 revision에서 audience 검증이 사양에서 제거됐다

<details><summary>정답</summary>

A. A single forwarded token is a textbook token passthrough problem: each downstream server must validate that a token was issued for it, and a token minted for the gateway or for one server was never vetted for another. The gateway needs to mint or exchange distinct, audience-scoped tokens per server instead.

A. token 하나를 그대로 전달하는 것은 전형적인 token passthrough 문제다. 각 downstream server는 token이 자신을 위해 발급됐는지 검증해야 한다. gateway나 한 server용으로 발급된 token은 다른 server용으로 검증된 적이 없다. gateway는 server마다 audience 범위가 다른 별개의 token을 발급하거나 교환해야 한다.

</details>

## 40. [security-and-governance]

A host is deciding whether to rely on a tool's title and description text to reassure the user before a destructive call, or to run an explicit MRTR elicitation asking the user to confirm. Which design is more defensible, given annotations are untrusted hints?

A. Neither, since destructive tools should simply never ever be safely callable under any circumstances at all here whatsoever\
B. Explicit elicitation, since it produces a real user decision the host controls, rather than trusting server-authored text\
C. The description text alone, but only for tools whose name happens to contain the word safe\
D. The description text alone, since a well written description is functionally equivalent to asking the user

**한국어**

host가 파괴적 호출 전에 사용자를 안심시키려고 tool의 title과 description 문구에 기댈지, 아니면 사용자에게 확인을 요청하는 명시적 MRTR elicitation을 실행할지 정하고 있다. annotation이 신뢰할 수 없는 힌트라는 점을 고려하면 어느 설계가 더 타당한가?

A. 둘 다 아니다. 파괴적 tool은 어떤 상황에서도 절대 안전하게 호출될 수 없어야 한다\
B. 명시적 elicitation. server가 작성한 문구를 믿는 대신 host가 통제하는 실제 사용자 결정을 만들어 낸다\
C. description 문구만. 단, 이름에 safe라는 단어가 마침 들어 있는 tool에만 해당한다\
D. description 문구만. 잘 쓴 설명은 기능상 사용자에게 묻는 것과 같다

<details><summary>정답</summary>

B. Because tool titles and descriptions are server authored and untrusted unless the server itself is trusted, they cannot substitute for a real consent decision. An MRTR elicitation round trip is the mechanism that gets an explicit answer from the user rather than assuming reassurance from text the server controls.

B. tool의 title과 description은 server가 작성한 것이고 server 자체를 신뢰할 때가 아니면 신뢰할 수 없다. 그래서 실제 동의 결정을 대신할 수 없다. MRTR elicitation 왕복은 server가 통제하는 문구로 안심했다고 가정하지 않고 사용자에게서 명시적인 답을 받아 내는 메커니즘이다.

</details>

## 41. [security-and-governance] (복수 정답)

A deploy tool uses MRTR elicitation to collect a human approval before running, and the pending decision is carried in requestState. Select the three protections that requestState needs given it is attacker-controlled input.

A. Binding to the authenticated principal so one user's pending approval cannot be replayed by another\
B. Integrity protection such as an HMAC or AEAD so it cannot be modified undetected\
C. A short expiry combined with single-use enforcement on the server so it cannot be replayed later\
D. A cacheScope of public so the pending approval can be shared efficiently across every user

**한국어**

배포 tool이 실행 전에 MRTR elicitation으로 사람의 승인을 받는다. 대기 중인 결정은 requestState에 담긴다. requestState가 공격자가 조작할 수 있는 입력이라는 점을 고려할 때 필요한 보호 장치 세 가지를 고르시오.

A. 인증된 principal에 묶는다. 그래야 한 사용자의 대기 중인 승인을 다른 사용자가 재사용(replay)하지 못한다\
B. HMAC이나 AEAD 같은 무결성 보호를 적용한다. 그래야 들키지 않고 수정되지 못한다\
C. 짧은 만료와 server 측에서 한 번만 쓸 수 있게 막는 처리를 함께 적용한다. 그래야 나중에 재사용되지 못한다\
D. cacheScope를 public으로 설정한다. 그래야 대기 중인 승인을 모든 사용자가 효율적으로 공유할 수 있다

<details><summary>정답</summary>

A, B, C. Because requestState is round tripped through the client and is not itself trusted, it needs integrity protection, principal binding, and a short, single-use expiry enforced server side. A public cacheScope is unrelated and actively wrong here, since MRTR results including input_required are never cached at all.

A, B, C. requestState는 client를 거쳐 왕복하고 그 자체로는 신뢰할 수 없다. 그래서 무결성 보호, principal 바인딩, server 측에서 적용하는 짧은 일회성 만료가 필요하다. public cacheScope는 관계가 없을 뿐 아니라 여기서는 명백히 틀리다. input_required를 포함한 MRTR 결과는 아예 캐시되지 않기 때문이다.

</details>

## 42. [security-and-governance]

An audit pipeline is being designed to record who performed a sensitive tool call. One proposal attributes the action using the self reported clientInfo.name from the request; another uses the authenticated principal derived from the validated access token. Which is correct?

A. clientInfo.name, since it is the field explicitly designed for identifying a caller in an audit trail\
B. Whichever field simply happens to be shorter, since shorter values keep audit records far more compact over a long time\
C. The authenticated principal from the token, since clientInfo is self reported and meant only for display and debugging\
D. Both fields combined with an OR, so either one alone counts as sufficient proof of identity

**한국어**

민감한 tool 호출을 누가 수행했는지 기록하는 감사 파이프라인을 설계하고 있다. 한 제안은 요청에 담긴, 자체 보고된 clientInfo.name으로 행위자를 판별한다. 다른 제안은 검증된 access token에서 얻은 인증된 principal을 쓴다. 어느 쪽이 올바른가?

A. clientInfo.name. 감사 기록에서 호출자를 식별하도록 명시적으로 설계된 필드다\
B. 그냥 더 짧은 쪽. 값이 짧을수록 오랜 기간 감사 기록이 훨씬 간결해진다\
C. token에서 얻은 인증된 principal. clientInfo는 자체 보고된 값이고 표시와 디버깅 용도일 뿐이다\
D. 두 필드를 OR로 결합한다. 어느 하나만 있어도 신원 증명으로 충분하다

<details><summary>정답</summary>

C. clientInfo and serverInfo are explicitly self reported and meant only for display, logging, and debugging, never for security decisions. An audit trail that must actually attribute an action correctly needs the authenticated principal established through token validation, not a value the caller could freely set.

C. clientInfo와 serverInfo는 자체 보고된 값이며 표시, 로깅, 디버깅 용도일 뿐이다. 보안 결정에 써서는 안 된다고 명시되어 있다. 행위를 정확하게 귀속해야 하는 감사 기록에는 호출자가 마음대로 설정할 수 있는 값이 아니라 token 검증으로 확립된 인증된 principal이 필요하다.

</details>

## 43. [security-and-governance] (복수 정답)

Which two statements correctly describe the client registration priority order for 2026-07-28? Select two.

A. Dynamic Client Registration is checked before Client ID Metadata Documents whenever both are advertised\
B. Pre-registered credentials are tried first, ahead of any dynamic or document-based registration mechanism\
C. Asking the user is the fallback of last resort, used only once the automated options are unavailable\
D. Client ID Metadata Documents may only be used by clients that also support the deprecated Dynamic Client Registration flow

**한국어**

2026-07-28의 client 등록 우선순위를 바르게 설명한 것 두 가지는 무엇인가? 두 개 고르시오.

A. 둘 다 광고되어 있으면 Dynamic Client Registration을 Client ID Metadata Documents보다 먼저 확인한다\
B. 사전 등록된 자격 증명을 가장 먼저 시도한다. 동적 등록이나 문서 기반 등록 메커니즘보다 앞선다\
C. 사용자에게 묻는 것은 마지막 수단이다. 자동화된 방법을 쓸 수 없을 때만 쓴다\
D. Client ID Metadata Documents는 지원 중단된 Dynamic Client Registration 흐름도 지원하는 client만 쓸 수 있다

<details><summary>정답</summary>

B, C. The order is pre-registered credentials first, then Client ID Metadata Documents when advertised, then the deprecated Dynamic Client Registration, and finally asking the user as a last resort. CIMD does not require DCR support, and DCR is checked after CIMD, not before it.

B, C. 순서는 사전 등록된 자격 증명, 광고된 경우 Client ID Metadata Documents, 지원 중단된 Dynamic Client Registration 순이다. 마지막 수단으로 사용자에게 묻는다. CIMD는 DCR 지원을 요구하지 않는다. DCR은 CIMD보다 먼저가 아니라 나중에 확인한다.

</details>

## 44. [security-and-governance]

A registry-listed server silently changes a tool's description and inputSchema between two releases, in a way that expands what the tool actually does. What control design catches this kind of rug pull before the new behavior reaches production?

A. Waiting patiently for a user complaint, since definition changes are always noticeable immediately during normal daily use\
B. Trusting the registry's namespace verification alone, since a verified namespace also guarantees definitions never change\
C. Disabling the tool permanently the first time it is ever called, regardless of whether anything changed\
D. Pinning tool definitions by hash and holding any changed definition for review before it is trusted again

**한국어**

registry에 등재된 server가 두 릴리스 사이에 tool의 description과 inputSchema를 조용히 바꿔서 tool이 실제로 하는 일을 넓힌다. 새 동작이 프로덕션에 도달하기 전에 이런 rug pull을 잡아내는 통제 설계는 무엇인가?

A. 사용자 불만이 나올 때까지 참고 기다린다. 정의 변경은 평소 일상적으로 쓰다 보면 항상 바로 눈에 띈다\
B. registry의 namespace 검증만 믿는다. 검증된 namespace는 정의가 절대 바뀌지 않는다는 것도 보장한다\
C. 무엇이 바뀌었는지와 관계없이 tool이 처음 호출되는 순간 영구히 비활성화한다\
D. tool 정의를 해시로 고정해 둔다. 바뀐 정의는 다시 신뢰하기 전에 검토 대상으로 보류한다

<details><summary>정답</summary>

D. Registry namespace verification proves who owns a name, not that its tool definitions stay the same over time, so definition drift needs its own control: pinning a known-good definition by hash and flagging any change for review before the updated tool is trusted in production.

D. registry namespace 검증은 이름의 소유자가 누구인지를 증명할 뿐이다. tool 정의가 시간이 지나도 그대로라는 것은 증명하지 않는다. 그래서 정의 변경에는 별도 통제가 필요하다. 검증된 정의를 해시로 고정해 두고 바뀐 tool을 프로덕션에서 신뢰하기 전에 모든 변경을 검토 대상으로 표시한다.

</details>

## 45. [security-and-governance]

A host has two servers connected, A and B. A tool result from server A contains text asking the host to immediately call an administrative tool on server B. What is the correct trust design?

A. Treat server A's content as untrusted with respect to server B, and never let one server's output authorize a call into another\
B. Allow it, since both servers were separately approved by the same administrator and therefore trust each other\
C. Allow it only if server A's very own tool has readOnlyHint explicitly and clearly set to true in its own listed annotations somewhere\
D. Allow it, since the host's only job is to relay whatever a connected server's result requests

**한국어**

host에 server A와 B가 연결되어 있다. server A의 tool 결과에, host가 server B의 관리용 tool을 즉시 호출하라고 요청하는 문구가 들어 있다. 올바른 신뢰 설계는 무엇인가?

A. server A의 content를 server B와 관련해서는 신뢰할 수 없는 것으로 다룬다. 한 server의 출력이 다른 server로의 호출을 승인하게 두지 않는다\
B. 허용한다. 두 server는 같은 관리자가 각각 승인했으므로 서로 신뢰한다\
C. server A 자체 tool의 annotation 어딘가에 readOnlyHint가 명시적으로 분명하게 true로 설정된 경우에만 허용한다\
D. 허용한다. host의 유일한 역할은 연결된 server의 결과가 요청하는 것을 무엇이든 전달하는 것이다

<details><summary>정답</summary>

A. Trust boundaries are per server, not transitive across servers just because a host happens to be connected to both. Content returned by one server must never be treated as authorization to act on a different server, since that would let any single compromised or careless server steer the entire host.

A. 신뢰 경계는 server 단위다. host가 두 server에 함께 연결되어 있다고 해서 신뢰가 server 사이로 전이되지 않는다. 한 server가 반환한 content를 다른 server에서 행동할 권한으로 다뤄서는 절대 안 된다. 그렇게 하면 침해되거나 부주의한 server 하나가 host 전체를 조종할 수 있다.

</details>

## 46. [security-and-governance] (복수 정답)

Which two design elements make an audit and trace system useful for reviewing an incident that crossed a host, a client, and two servers? Select two.

A. Writing tamper-evident, for example hash-chained, records so a reviewer can detect whether a log entry was altered after the fact\
B. Propagating OpenTelemetry trace context such as traceparent through _meta so spans across hops can be correlated\
C. Recording only the final aggregate outcome of the entire multi-hop exchange, discarding intermediate steps to save storage\
D. Letting each hop choose its own unrelated identifier scheme so operators can compare formats after the fact

**한국어**

host, client, server 두 개를 거친 사고를 검토할 때 감사와 추적 시스템을 유용하게 만드는 설계 요소 두 가지는 무엇인가? 두 개 고르시오.

A. 해시 체인처럼 변조를 감지할 수 있는(tamper-evident) 기록을 남긴다. 그래야 리뷰어가 로그 항목이 나중에 바뀌었는지 알아낼 수 있다\
B. traceparent 같은 OpenTelemetry trace context를 _meta로 전파한다. 그래야 여러 hop에 걸친 span을 연관 지을 수 있다\
C. 여러 hop에 걸친 교환 전체의 최종 집계 결과만 기록한다. 저장 공간을 아끼려고 중간 단계는 버린다\
D. hop마다 서로 관계없는 식별자 체계를 고르게 한다. 그러면 운영자가 나중에 형식을 비교할 수 있다

<details><summary>정답</summary>

A, B. Correlating activity across hops needs a shared trace context propagated end to end, and reviewing an incident needs confidence that the records themselves were not altered, which tamper evidence provides. Discarding intermediate steps or letting each hop invent its own identifiers both destroy the correlation an audit needs.

A, B. hop 사이의 활동을 연관 지으려면 처음부터 끝까지 전파되는 공통 trace context가 필요하다. 사고를 검토하려면 기록 자체가 바뀌지 않았다는 확신도 필요하다. 변조 감지가 그 확신을 준다. 중간 단계를 버리거나 hop마다 식별자를 따로 만들게 하면 감사에 필요한 연관 관계가 모두 사라진다.

</details>

## 47. [security-and-governance]

A team proposes setting cacheScope to public on a resource that contains per-tenant billing data, reasoning that a public cache reduces load and every tenant's client already has a valid token. What is wrong with this design?

A. Nothing is wrong, since holding a valid token is by itself sufficient proof cacheScope public is safe\
B. cacheScope public would let one tenant's cached response be served to a different tenant, since it governs sharing, not access\
C. cacheScope only ever really affects tools/list results specifically, so it has no real bearing on resources/read at all here whatsoever\
D. The only fix needed is to shorten ttlMs, since a shorter freshness window makes any scope safe

**한국어**

한 팀이 테넌트별 청구 데이터를 담은 resource에 cacheScope를 public으로 설정하자고 제안한다. public 캐시로 부하를 줄일 수 있고 모든 테넌트의 client가 이미 유효한 token을 갖고 있다는 이유다. 이 설계의 문제는 무엇인가?

A. 문제없다. 유효한 token을 가졌다는 것만으로 cacheScope public이 안전하다는 충분한 증거다\
B. cacheScope public이면 한 테넌트의 캐시된 응답이 다른 테넌트에게 전달될 수 있다. cacheScope는 접근이 아니라 공유를 다룬다\
C. cacheScope는 사실 tools/list 결과에만 영향을 준다. 그래서 여기서 resources/read와는 전혀 관계가 없다\
D. ttlMs만 줄이면 된다. 캐시 유효 시간이 짧으면 어떤 scope든 안전해진다

<details><summary>정답</summary>

B. cacheScope public means a result may be shared across users and tokens, which is precisely wrong for per-tenant billing data even though every caller holds a valid token. Because cacheScope is never an access control mechanism, per-tenant data needs private scope and its own per-read authorization check, not a shorter TTL.

B. cacheScope public은 결과를 여러 사용자와 token에 걸쳐 공유할 수 있다는 뜻이다. 모든 호출자가 유효한 token을 갖고 있더라도 테넌트별 청구 데이터에는 정확히 틀린 선택이다. cacheScope는 결코 접근 제어 메커니즘이 아니다. 그래서 테넌트별 데이터에는 더 짧은 TTL이 아니라 private scope와 읽기마다 하는 별도 인가 검사가 필요하다.

</details>

## 48. [security-and-governance]

A client stored registration credentials after successfully registering with authorization server A. It now needs to connect to a different authorization server B for a separate deployment, and B also supports the same registration mechanism. What is the correct design?

A. Reuse the credentials stored for server A directly with server B right away, since one successful registration should cover every future authorization server encountered\
B. Wait patiently for server A to forward the stored credentials to server B fully automatically before any single request to B is ever attempted at all\
C. Register separately with server B and keep credentials keyed by issuer, since persisted credentials must never be reused across a different authorization server\
D. Ask the user to manually paste server A's credentials into server B's configuration screen so both servers share one identity

**한국어**

client가 authorization server A에 성공적으로 등록한 뒤 등록 자격 증명을 저장했다. 이제 별도 배포를 위해 다른 authorization server B에 연결해야 한다. B도 같은 등록 메커니즘을 지원한다. 올바른 설계는 무엇인가?

A. server A용으로 저장한 자격 증명을 server B에 바로 재사용한다. 한 번 성공한 등록은 앞으로 만나는 모든 authorization server에 통해야 한다\
B. B로 요청을 하나라도 보내기 전에, server A가 저장된 자격 증명을 server B로 완전히 자동 전달할 때까지 참고 기다린다\
C. server B에 따로 등록하고 issuer를 키로 자격 증명을 보관한다. 저장된 자격 증명은 다른 authorization server에 절대 재사용해서는 안 된다\
D. 사용자에게 server A의 자격 증명을 server B 설정 화면에 직접 붙여 넣게 한다. 그러면 두 server가 신원 하나를 공유한다

<details><summary>정답</summary>

C. Registration state is bound to the authorization server that issued it. Persisted credentials must be keyed by issuer, and a client must register again, rather than reuse or transfer them, whenever it connects to a different authorization server, even one that supports the same registration mechanism.

C. 등록 상태는 그것을 발급한 authorization server에 묶인다. 저장된 자격 증명은 issuer를 키로 보관해야 한다. client가 다른 authorization server에 연결할 때는 같은 등록 메커니즘을 지원하는 곳이라도 자격 증명을 재사용하거나 옮기지 말고 다시 등록해야 한다.

</details>

## 49. [use-cases-and-ecosystem]

A team wants a reusable, user-invoked, slash-command-style template with arguments that a person deliberately selects to start a code review conversation. Which primitive fits this control model, and why not the alternatives?

A. A resource, since resources are application-driven content the host attaches without the user choosing it each time\
B. A tool, since tools are always model-controlled and the model alone would decide on its own when to invoke it\
C. Server instructions returned from server/discover, since those are supposedly the only user-facing text a server could ever expose to anyone\
D. A prompt, since prompts are user-controlled templates a person deliberately selects, matching how this template is meant to be used

**한국어**

한 팀이 코드 리뷰 대화를 시작할 때 사람이 직접 골라 호출하는, 인자를 받는 재사용 가능한 슬래시 명령 형태의 템플릿을 원한다. 이 제어 모델에 맞는 primitive는 무엇인가? 다른 선택지는 왜 아닌가?

A. resource. resource는 사용자가 매번 고르지 않아도 host가 붙이는 애플리케이션 주도 content다\
B. tool. tool은 항상 모델이 제어하므로 언제 호출할지는 모델 혼자 정한다\
C. server/discover가 반환하는 server instructions. 그것이 server가 누구에게든 노출할 수 있는 유일한 사용자용 텍스트라고 한다\
D. prompt. prompt는 사람이 직접 고르는 사용자 제어 템플릿이므로 이 템플릿의 쓰임새와 맞는다

<details><summary>정답</summary>

D. The three server primitives map to three different control models: tools are model-controlled, resources are application-driven, and prompts are user-controlled templates a person explicitly selects. A deliberately chosen, argument-taking template for starting a conversation is exactly the user-controlled case prompts were designed for.

D. server primitive 세 가지는 서로 다른 제어 모델에 대응한다. tool은 모델 제어, resource는 애플리케이션 주도, prompt는 사람이 명시적으로 고르는 사용자 제어 템플릿이다. 대화를 시작하려고 일부러 고르는, 인자를 받는 템플릿은 prompt가 설계된 사용자 제어 사례에 정확히 해당한다.

</details>

## 50. [use-cases-and-ecosystem]

A tool is meant to be called only by an app's own embedded UI, to refresh its view, and should never appear as something the model can invoke on its own. Under the MCP Apps extension, how should the tool's _meta.ui visibility be set, and what must the host do as a result?

A. Set visibility to only app, and the host must not list the tool to the model while still letting the app call it\
B. Set visibility to only app, but the host stays free to list it to the model anyway\
C. Set visibility to only model instead, since visibility supposedly only ever controls what the human user is ever allowed to see at all\
D. Leave visibility completely unset, since every tool is always callable by both the model and the app equally anyway

**한국어**

한 tool은 앱에 내장된 UI가 화면을 새로 고칠 때만 호출하도록 만들었다. 모델이 스스로 호출할 수 있는 것으로 보여서는 절대 안 된다. MCP Apps extension에서 이 tool의 _meta.ui visibility는 어떻게 설정해야 하는가? 그 결과 host는 무엇을 해야 하는가?

A. visibility를 app만으로 설정한다. host는 이 tool을 모델에게 목록으로 보여 주면 안 된다. 앱은 계속 호출할 수 있게 해야 한다\
B. visibility를 app만으로 설정하되, host는 그래도 이 tool을 모델에게 자유롭게 보여 줄 수 있다\
C. 대신 visibility를 model만으로 설정한다. visibility는 사람 사용자가 볼 수 있는 것만 제어한다고 한다\
D. visibility를 아예 설정하지 않는다. 모든 tool은 어차피 항상 모델과 앱 모두가 똑같이 호출할 수 있다

<details><summary>정답</summary>

A. MCP Apps lets a tool restrict who may invoke it: when visibility excludes model, the host must not list that tool to the agent, and when it excludes app, the host must reject a tools/call coming from the rendered UI. An app-only refresh action should declare app so only the embedded interface can trigger it.

A. MCP Apps에서는 tool을 누가 호출할 수 있는지 제한할 수 있다. visibility에 model이 빠져 있으면 host는 그 tool을 agent에게 보여 주면 안 된다. app이 빠져 있으면 host는 렌더링된 UI에서 온 tools/call을 거부해야 한다. 앱 전용 새로 고침 동작은 app을 선언해 내장 인터페이스만 호출할 수 있게 해야 한다.

</details>

## 51. [use-cases-and-ecosystem]

> 변동 가능(경계): Tier 3가 experimental이고 conformance 보장이 없다는 것은 SDK 등급 기준에서 오는 정보입니다. 스펙 본문이 아니라 별도 문서에서 오는 내용입니다.

A vendor markets a brand new SDK as fully spec-compliant and ready to use, but it currently sits at Tier 3 in the official tiering system. Before recommending it for a customer's production deployment, what should an integrator conclude from that tier alone?

A. That it ships absolutely every official extension by default, since extension support is apparently mandatory at every single tier without exception\
B. That it is explicitly experimental, with no guaranteed conformance percentage or timeline the way Tier 1 and 2 commit to\
C. That it already passes one hundred percent of conformance tests, since every vendor SDK starts out at Tier 1 by default\
D. That it will automatically and fully graduate to Tier 1 within just two business days of the vendor's claim

**한국어**

한 벤더가 새 SDK를 사양을 완전히 준수하고 바로 쓸 수 있다고 홍보한다. 그런데 이 SDK는 현재 공식 tier 체계에서 Tier 3다. 고객의 프로덕션 배포에 추천하기 전에, 통합 담당자는 tier만 보고 무엇을 결론 내려야 하는가?

A. 모든 공식 extension을 기본으로 빠짐없이 제공한다. extension 지원은 예외 없이 모든 tier에서 필수인 것으로 보이기 때문이다\
B. 명시적으로 실험 단계다. Tier 1과 2가 약속하는 것 같은 적합성 비율이나 일정 보장이 없다\
C. 이미 적합성 테스트를 100퍼센트 통과한다. 모든 벤더 SDK는 기본적으로 Tier 1에서 시작한다\
D. 벤더가 주장한 뒤 영업일 이틀 안에 자동으로 완전히 Tier 1로 올라간다

<details><summary>정답</summary>

B. SDK tiers describe real, graduated commitments: Tier 1 passes all conformance tests with fast triage, Tier 2 passes most within a longer window, and Tier 3 is explicitly experimental with none of those guarantees. A vendor's marketing claim of spec compliance does not override what the tier itself actually commits to.

B. SDK tier는 단계별로 실제 약속을 나타낸다. Tier 1은 모든 적합성 테스트를 통과하고 이슈를 빠르게 분류(triage)한다. Tier 2는 더 긴 기간 안에 대부분을 통과한다. Tier 3는 명시적으로 실험 단계이며 그런 보장이 전혀 없다. 벤더가 마케팅에서 사양 준수를 주장하더라도 tier 자체가 실제로 약속하는 내용을 뒤집지는 못한다.

</details>

## 52. [use-cases-and-ecosystem]

In a gateway-fronted enterprise deployment over Streamable HTTP, whose responsibility is validating the Origin header and binding to localhost when appropriate? What goes wrong if that responsibility is assumed to belong to someone else?

A. The end user's, since only a human can decide whether a request's origin looks legitimate\
B. The registry publisher's, since server.json is where Origin policy is supposed to be declared\
C. The server or gateway operator's, and if neither implements it, the deployment is open to DNS rebinding attacks\
D. Nobody's at all really, since Origin validation only ever matters for stdio transports and never at all for Streamable HTTP

**한국어**

Streamable HTTP를 쓰고 gateway가 앞단에 있는 기업 배포에서, Origin 헤더를 검증하고 필요할 때 localhost에 바인딩하는 일은 누구의 책임인가? 그 책임이 다른 쪽에 있다고 가정하면 무엇이 잘못되는가?

A. 최종 사용자. 요청의 출처가 정상으로 보이는지는 사람만 판단할 수 있다\
B. registry 게시자. server.json이 Origin 정책을 선언하는 곳이다\
C. server 또는 gateway 운영자. 둘 다 구현하지 않으면 배포가 DNS rebinding 공격에 노출된다\
D. 사실상 아무도 아니다. Origin 검증은 stdio transport에서만 의미가 있고 Streamable HTTP에서는 전혀 의미가 없다

<details><summary>정답</summary>

C. Origin validation and localhost binding are HTTP transport security controls that the server or the gateway sitting in front of it must actually implement. If each side assumes the other handles it, the deployment ends up with no real defense against the DNS rebinding style attacks that control was meant to stop.

C. Origin 검증과 localhost 바인딩은 server나 그 앞의 gateway가 실제로 구현해야 하는 HTTP transport 보안 통제다. 양쪽이 서로 상대가 처리한다고 가정하면, 배포에는 그 통제가 막으려던 DNS rebinding 유형의 공격에 대한 실질적인 방어가 남지 않는다.

</details>

## 53. [use-cases-and-ecosystem]

A nightly batch job runs unattended, calling a server with no user present and no elicitation capability declared. It needs the equivalent of long running work tracking. Which design fits, and why would MRTR elicitation be the wrong choice here?

A. Dynamic Client Registration, since machine clients must always dynamically register again before every single batch run without exception\
B. A plain synchronous call held open for the job's entire duration, since batch jobs are typically always expected to block\
C. MRTR elicitation, since a server can always request user input from a client even with nobody watching\
D. The tasks extension or a server-minted handle, since elicitation needs a client capability an unattended job will not declare

**한국어**

야간 배치 작업이 무인으로 실행된다. 사용자가 없고 elicitation capability도 선언하지 않은 채 server를 호출한다. 이 작업에는 오래 걸리는 작업 추적에 해당하는 기능이 필요하다. 어떤 설계가 맞는가? 여기서 MRTR elicitation은 왜 잘못된 선택인가?

A. Dynamic Client Registration. 기계 client는 배치를 실행할 때마다 예외 없이 항상 다시 동적 등록해야 한다\
B. 작업 시간 내내 열어 두는 일반 동기 호출. 배치 작업은 대개 항상 블로킹으로 동작할 것으로 기대된다\
C. MRTR elicitation. 지켜보는 사람이 없어도 server는 언제나 client에게 사용자 입력을 요청할 수 있다\
D. tasks extension 또는 server가 발급한 handle. elicitation에는 무인 작업이 선언하지 않을 client capability가 필요하다

<details><summary>정답</summary>

D. MRTR input requests such as elicitation assume a capability the client declares and a way to gather an answer, neither of which fits an unattended machine-to-machine job with nobody present. Tracking long, unattended work instead fits the tasks extension or an explicit server-minted handle referenced on later calls.

D. elicitation 같은 MRTR input request는 client가 선언한 capability와 답을 받아 낼 방법이 있다고 전제한다. 사람이 없는 무인 기계 간 작업에는 둘 다 맞지 않는다. 오래 걸리는 무인 작업을 추적하는 데는 tasks extension이나, 이후 호출에서 참조하는 server 발급 명시적 handle이 맞다.

</details>

## 54. [use-cases-and-ecosystem]

> 변동 가능(경계): 네임스페이스 검증과 보안 스캔 위임은 preview 상태인 Registry의 운영 방식입니다. 스펙 본문이 아니라 별도 문서에서 오는 내용입니다.

A registry lists a server under a reverse-DNS namespace that has already passed GitHub ownership verification. A reviewer treats that verification alone as proof the server's published code is safe to run. What is the flaw in that conclusion?

A. Namespace verification proves who controls the name, not that the code is safe, since scanning is delegated to package registries and aggregators\
B. There is truly no flaw at all, since GitHub ownership verification already includes a full independent security audit of the published code itself\
C. The flaw is that GitHub verification technically only ever applies to DNS challenges, never to namespace ownership claims themselves at all\
D. The flaw is that reverse-DNS namespaces were entirely removed from the registry design as of this specification revision

**한국어**

registry가 GitHub 소유권 검증을 이미 통과한 역방향 DNS namespace 아래에 server를 등재하고 있다. 리뷰어는 그 검증만으로 server가 게시한 코드를 실행해도 안전하다는 증거로 본다. 이 결론의 결함은 무엇인가?

A. namespace 검증은 이름을 누가 통제하는지를 증명할 뿐 코드가 안전하다는 것은 증명하지 않는다. 보안 스캔은 package registry와 aggregator에 맡겨져 있다\
B. 결함이 전혀 없다. GitHub 소유권 검증에는 게시된 코드 자체에 대한 완전한 독립 보안 감사가 이미 포함되어 있다\
C. GitHub 검증은 엄밀히 말해 DNS challenge에만 적용될 뿐 namespace 소유권 주장 자체에는 전혀 적용되지 않는다는 것이 결함이다\
D. 이 사양 revision부터 역방향 DNS namespace가 registry 설계에서 완전히 제거됐다는 것이 결함이다

<details><summary>정답</summary>

A. Namespace verification through GitHub, DNS, or an HTTP challenge establishes ownership of a name, which stops impersonation, but the registry explicitly delegates security scanning to package registries and aggregators, so ownership proof alone says nothing about whether the published code itself is safe to run.

A. GitHub, DNS, HTTP challenge를 통한 namespace 검증은 이름의 소유권을 확립해 사칭을 막는다. 하지만 registry는 보안 스캔을 package registry와 aggregator에 맡긴다고 명시한다. 그래서 소유권 증명만으로는 게시된 코드 자체를 실행해도 안전한지 전혀 알 수 없다.

</details>

## 55. [use-cases-and-ecosystem] (복수 정답)

> 변동 가능(경계): 정답 A, B, D가 모두 SDK 등급 기준(triage와 수정 기한, extension 비요구, 4주 실패 시 강등)입니다. 스펙 본문이 아니라 별도 문서에서 오는 내용입니다.

Select the three facts about SDK tiers that should shape a build-versus-adopt decision for a client library.

A. A Tier 1 SDK commits to triaging issues within two business days and fixing critical bugs within seven\
B. Extensions are not a requirement at any SDK tier, so their absence does not by itself indicate a lower tier\
C. Tier 3 SDKs are always required to support every single official extension before they can ever be published\
D. An SDK can be relegated to a lower tier after four consecutive weeks of conformance failures

**한국어**

client 라이브러리를 직접 만들지 기존 것을 도입할지 결정할 때 고려해야 할 SDK tier 관련 사실 세 가지를 고르시오.

A. Tier 1 SDK는 이슈를 영업일 이틀 안에 분류하고 치명적 버그를 7일 안에 고치기로 약속한다\
B. extension은 어떤 SDK tier에서도 요구 사항이 아니다. 그래서 extension이 없다는 것만으로 더 낮은 tier라는 뜻은 아니다\
C. Tier 3 SDK는 게시되기 전에 모든 공식 extension을 반드시 빠짐없이 지원해야 한다\
D. SDK는 적합성 실패가 4주 연속 이어지면 더 낮은 tier로 강등될 수 있다

<details><summary>정답</summary>

A, B, D. Tier 1 commitments, the relegation rule after sustained failures, and the fact that extensions are optional at every tier are all real signals for a build-versus-adopt call. No tier mandates supporting every official extension, so that claim is false and would mislead a team using extension coverage to judge tier.

A, B, D. Tier 1의 약속, 실패가 이어질 때의 강등 규칙, extension이 모든 tier에서 선택 사항이라는 사실은 모두 직접 만들지 도입할지 판단할 때 실제로 쓸 수 있는 신호다. 모든 공식 extension 지원을 의무로 정한 tier는 없다. 그래서 그 주장은 틀렸다. extension 지원 범위로 tier를 판단하려는 팀을 잘못 이끌 수 있다.

</details>

## 56. [use-cases-and-ecosystem]

A server exposes several skills through Skills over MCP. A client calls resources/read on one skill's SKILL.md file just to preview its contents. Has that skill been activated by this read?

A. No, since Skills over MCP never permits resources/read to return SKILL.md content at all\
B. No, reading the file through resources/read makes it merely readable, a step distinct from activation\
C. Yes, reading SKILL.md through resources/read is what activation means under this extension by definition\
D. Yes, but only for skills whose description field happens to be under one hundred characters

**한국어**

server가 Skills over MCP로 여러 skill을 노출한다. client가 한 skill의 SKILL.md 파일 내용을 미리 보려고 resources/read를 호출한다. 이 읽기로 그 skill이 활성화됐는가?

A. 아니다. Skills over MCP는 resources/read가 SKILL.md 내용을 반환하는 것을 아예 허용하지 않는다\
B. 아니다. resources/read로 파일을 읽으면 읽을 수 있는 상태가 될 뿐이다. 이는 활성화와 별개의 단계다\
C. 그렇다. 이 extension에서는 정의상 resources/read로 SKILL.md를 읽는 것이 곧 활성화다\
D. 그렇다. 단, description 필드가 마침 100자 미만인 skill에만 해당한다

<details><summary>정답</summary>

B. Reading a skill's SKILL.md through resources/read only retrieves text. Skills over MCP treats skill content as untrusted input, leaves whether a skill is loaded into model context to explicit user policy, and has hosts let users inspect a skill before loading it, so previewing a file is not activation.

B. resources/read로 skill의 SKILL.md를 읽는 것은 텍스트를 가져오는 것일 뿐이다. Skills over MCP는 skill content를 신뢰할 수 없는 입력으로 다룬다. skill을 모델 컨텍스트에 올릴지는 명시적인 사용자 정책에 맡긴다. host는 사용자가 올리기 전에 skill을 살펴볼 수 있게 한다. 그래서 파일을 미리 보는 것은 활성화가 아니다.

</details>

## 57. [use-cases-and-ecosystem]

A client was built and tested only against one specific MCP server. Under 2026-07-28, why should that same client generally be able to work against a different, unrelated, modern server without code changes?

A. Because every MCP server is legally required to expose an identical set of tool names\
B. Because servers share a single global process and therefore already know about every client in advance\
C. Because a compliant client relies on discovery and the wire protocol rather than one server's internal assumptions\
D. Because the client's own author is always expected to hardcode that one specific server's tool names directly into the client itself

**한국어**

한 client를 특정 MCP server 하나만 상대로 만들고 테스트했다. 2026-07-28에서 이 client가 관계없는 다른 최신 server에서도 코드 변경 없이 대체로 동작해야 하는 이유는 무엇인가?

A. 모든 MCP server는 똑같은 tool 이름 집합을 노출하도록 법적으로 요구되기 때문이다\
B. server들이 전역 프로세스 하나를 공유하므로 이미 모든 client를 미리 알고 있기 때문이다\
C. 규격을 따르는 client는 server 하나의 내부 가정이 아니라 discovery와 와이어 프로토콜에 기대기 때문이다\
D. client 작성자가 그 특정 server의 tool 이름을 client에 직접 하드코딩할 것으로 항상 기대되기 때문이다

<details><summary>정답</summary>

C. Portability follows directly from where logic is supposed to live: a client that discovers capabilities and tools through the protocol, rather than hardcoding one server's specifics, can drive any compliant server it has never seen. Custom, server-specific assumptions baked into a client are exactly what breaks that portability.

C. 이식성은 로직이 어디에 있어야 하는지에서 바로 나온다. server 하나의 세부 사항을 하드코딩하지 않고 프로토콜로 capability와 tool을 찾아내는 client는 처음 보는 어떤 규격 준수 server든 다룰 수 있다. client에 박아 넣은 server 전용 맞춤 가정이 바로 이 이식성을 깨뜨린다.

</details>

## 58. [use-cases-and-ecosystem]

A platform team is deciding whether a new extension can be turned on for every user by default the moment a vendor SDK adds support for it. What does the specification's extension design actually require here?

A. The host must wait for the extension's identifier to change to a -v2 suffix before enabling it\
B. Nothing further at all, since any extension a client's own SDK supports is automatically enabled for every single request\
C. The extension must first be promoted from Tier 3 to Tier 1 before it can ever be declared\
D. Explicit opt-in on both sides, since extensions are disabled by default, which is where governance sign-off belongs

**한국어**

플랫폼 팀이, 벤더 SDK가 새 extension 지원을 추가하는 즉시 모든 사용자에게 그 extension을 기본으로 켤 수 있는지 판단하고 있다. 사양의 extension 설계는 여기서 실제로 무엇을 요구하는가?

A. host는 extension 식별자에 -v2 접미사가 붙을 때까지 기다렸다가 켜야 한다\
B. 더 필요한 것이 없다. client의 SDK가 지원하는 extension은 모든 요청에서 자동으로 켜진다\
C. extension은 선언되기 전에 먼저 Tier 3에서 Tier 1로 승격되어야 한다\
D. 양쪽 모두의 명시적 opt-in. extension은 기본적으로 꺼져 있으므로 거버넌스 승인은 바로 이 지점에 들어간다

<details><summary>정답</summary>

D. Extensions are disabled by default and require explicit opt-in through declared capabilities on both the client and server side, which is a deliberate design choice, not an accident of SDK support. That deliberate opt-in point is precisely where a platform team's governance review naturally fits before wide rollout.

D. extension은 기본적으로 꺼져 있다. client와 server 양쪽에서 선언한 capability로 명시적으로 opt-in해야 한다. 이는 SDK 지원에 따라 우연히 생긴 것이 아니라 의도된 설계다. 이 의도된 opt-in 지점이 플랫폼 팀이 넓게 배포하기 전에 거버넌스 검토를 넣기에 자연스러운 자리다.

</details>

## 59. [use-cases-and-ecosystem]

A developer is deciding whether to expose a purely internal helper function, called only within the same process and never crossing an application or trust boundary, as an MCP tool. Is this a good fit for MCP?

A. No, since MCP is built for crossing the boundary between an application and an external system, and this helper never crosses it\
B. No, but only because internal helper functions are always too fast for JSON-RPC to keep up with\
C. Yes, since any function at all becomes more valuable once it is exposed through a protocol\
D. Yes, since MCP technically always requires every single function anywhere in a codebase to eventually always be reachable as a tool too

**한국어**

개발자가 순수 내부 헬퍼 함수를 MCP tool로 노출할지 고민한다. 이 함수는 같은 프로세스 안에서만 호출되고 애플리케이션 경계나 신뢰 경계를 전혀 넘지 않는다. 이것은 MCP에 잘 맞는가?

A. 아니다. MCP는 애플리케이션과 외부 시스템 사이의 경계를 넘기 위해 만들어졌다. 이 헬퍼는 그 경계를 넘지 않는다\
B. 아니다. 다만 그 이유는 내부 헬퍼 함수가 항상 너무 빨라서 JSON-RPC가 따라가지 못하기 때문일 뿐이다\
C. 그렇다. 어떤 함수든 프로토콜로 노출하면 가치가 더 커진다\
D. 그렇다. MCP는 엄밀히 말해 코드베이스 어디에 있든 모든 함수가 결국 tool로도 호출 가능해야 한다고 항상 요구한다

<details><summary>정답</summary>

A. MCP exists to standardize context and actions crossing the boundary between an AI application and an external system. A same-process internal helper never crosses that boundary, so wrapping it as a tool adds serialization and discovery overhead without the interoperability benefit that justifies using the protocol in the first place.

A. MCP는 AI 애플리케이션과 외부 시스템 사이의 경계를 넘는 컨텍스트와 동작을 표준화하려고 존재한다. 같은 프로세스 안의 내부 헬퍼는 그 경계를 넘지 않는다. 그래서 이를 tool로 감싸면 직렬화와 discovery 부담만 늘 뿐, 애초에 프로토콜을 쓸 이유가 되는 상호운용성 이점은 얻지 못한다.

</details>

## 60. [use-cases-and-ecosystem]

A gateway operator wants to cache a server's public server/discover result across every tenant to cut latency. Is sharing that cached result across tenants consistent with the caching contract, and what must the gateway still keep separate?

A. It is consistent, and caching the discover result is by itself sufficient to authorize every tenant's requests\
B. It is consistent, and per-tenant authorization must still be enforced on every actual request regardless of what was cached\
C. It is inconsistent, since server/discover results can never ever carry a cacheScope of public under any circumstances at all\
D. It is inconsistent, since sharing any cached value across tenants is forbidden no matter what scope declares

**한국어**

gateway 운영자가 지연 시간을 줄이려고 server의 공개 server/discover 결과를 모든 테넌트에 걸쳐 캐시하려 한다. 캐시한 결과를 테넌트끼리 공유하는 것은 캐싱 계약에 부합하는가? 그리고 gateway가 여전히 따로 지켜야 할 것은 무엇인가?

A. 부합한다. discover 결과를 캐시하는 것만으로 모든 테넌트의 요청을 인가하기에 충분하다\
B. 부합한다. 그리고 무엇이 캐시됐든 관계없이 테넌트별 인가는 실제 요청마다 계속 검사해야 한다\
C. 부합하지 않는다. server/discover 결과는 어떤 상황에서도 절대 cacheScope public을 가질 수 없다\
D. 부합하지 않는다. scope가 무엇을 선언하든 캐시된 값을 테넌트끼리 공유하는 것은 금지된다

<details><summary>정답</summary>

B. server/discover is one of the results that carries ttlMs and cacheScope, and a public discover result, capability metadata rather than tenant-specific data, is a reasonable candidate for shared caching. That caching decision is still separate from authorization, which the gateway must keep enforcing per tenant on every later request.

B. server/discover는 ttlMs와 cacheScope를 담는 결과 중 하나다. public discover 결과는 테넌트별 데이터가 아니라 capability 메타데이터이므로 공유 캐싱의 타당한 후보다. 그래도 캐싱 결정은 인가와 별개다. gateway는 이후 모든 요청에서 테넌트별 인가를 계속 검사해야 한다.

</details>
