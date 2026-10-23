# MCPA 연습 문제 (davila7)

> 실제 시험 문항이 아니며 출제 내용을 보장하지 않는다. 리눅스 재단(Linux Foundation)과 AAIF의 공식 자료가 아니다.
> 한국어 번역과 자세한 설명은 AI로 작성했으므로 틀린 곳이 있을 수 있다. 공식 스펙이 우선이다: https://modelcontextprotocol.io/specification/2026-07-28
> 원문 가운데 시험(exam)을 언급한 문장은 원작자의 설명을 그대로 옮긴 것이며 이 저장소의 의견이 아니다.

[davila7/model_context_protocol_associate_exam_prep](https://github.com/davila7/model_context_protocol_associate_exam_prep)
커밋 `8e8e7da`(2026-08-01)에서 옮겼다. MIT 라이선스이고 원문 `LICENSE`를 같은 폴더에 두었다.
사이트 페이지마다 흩어진 퀴즈를 한 파일로 모았다. 문제와 해설은 원문 그대로다.

실제 시험 기반이 아니다. 공식 도메인 구성을 따라 만든 연습 문제다. 2026-07-28 스펙을
반영했다(handshake 제거, Sampling과 Roots 폐기 예정 등). 첫 절 20문항은 도메인 비중에 맞춘
모의고사이고 나머지는 세부 항목별 복습 문제다.

영어 원문 아래 한국어 번역을 붙였다. 문항마다 정답 뒤에 접히는 자세한 설명을 넣었다. 원문은 `questions.en.md`.

## Practice Questions

원본: `practice/practice-questions.mdx`

### 1. What integration problem does MCP primarily solve?

A. Slow LLM inference\
B. The N×M problem of custom integrations between AI apps and external systems\
C. Lack of GPU availability\
D. Prompt token limits

**한국어**

MCP가 주로 해결하는 통합 문제는 무엇입니까?

A. 느린 LLM 추론\
B. AI 앱과 외부 시스템 사이 맞춤 통합의 N×M 문제\
C. GPU 부족\
D. 프롬프트 토큰 한도

<details><summary>정답</summary>

B. MCP standardizes how AI applications connect to external tools and data, so N apps and M systems need one shared protocol instead of N×M bespoke integrations. (Fundamentals)

B. MCP는 AI 애플리케이션이 외부 tool과 데이터에 연결하는 방식을 표준화합니다. 그래서 앱 N개와 시스템 M개가 N×M개의 개별 통합을 만들 필요가 없습니다. 공통 프로토콜 하나만 쓰면 됩니다. (Fundamentals)

</details>

<details><summary>자세한 설명</summary>

MCP가 왜 생겼는지 묻는 문제입니다. MCP가 푸는 것은 성능이 아니라 연결 방식의 문제라는 점을 알면 풀립니다.

표준이 없으면 AI 앱마다, 외부 시스템마다 연동 코드를 따로 만들어야 합니다. 앱이 N개이고 시스템이 M개면 연동이 N×M개 생깁니다. MCP를 쓰면 앱은 MCP client를 한 번 구현하고 시스템은 MCP server를 한 번 구현하면 됩니다. 그래서 B가 정답입니다.

A의 추론 속도와 C의 GPU 부족, D의 프롬프트 토큰 한도는 모델이나 인프라의 문제입니다. MCP는 메시지 형식과 연결 방식을 정하는 프로토콜이라 이런 자원 문제는 다루지 않습니다.

</details>

### 2. Which MCP primitive is model-controlled?

A. Resources\
B. Prompts\
C. Tools\
D. Roots

**한국어**

model-controlled인 MCP primitive는 무엇입니까?

A. Resources\
B. Prompts\
C. Tools\
D. Roots

<details><summary>정답</summary>

C. Tools are model-controlled: the LLM discovers and invokes them (with human-in-the-loop approval). Resources are application-driven and prompts are user-controlled. (Fundamentals)

C. Tools가 model-controlled입니다. LLM이 tool을 찾아서 호출합니다. 이때 사람의 승인을 거칩니다. Resources는 애플리케이션이 주도하고 prompts는 사용자가 제어합니다. (Fundamentals)

</details>

<details><summary>자세한 설명</summary>

server primitive 세 가지를 각각 누가 제어하는지 묻는 문제입니다. tools는 모델, resources는 애플리케이션, prompts는 사용자라는 짝을 알면 풀립니다.

tool은 모델이 대화 흐름을 보고 직접 골라 호출합니다. 스펙은 사람이 호출을 거부할 수 있어야 한다고 권합니다(SHOULD). 그래도 언제 호출할지 정하는 쪽은 모델입니다. 그래서 C가 정답입니다.

resource는 host 애플리케이션이 어떤 데이터를 맥락에 넣을지 정합니다. prompt는 사용자가 직접 골라 씁니다. 그래서 A와 B는 답이 아닙니다.

roots는 server primitive가 아니라 client 쪽 기능입니다. client가 server에 작업 범위가 되는 디렉터리를 알려 주는 용도입니다. 2026-07-28에서 폐기 예정이 됐습니다. 그래서 D도 답이 아닙니다.

user-controlled인 primitive는 prompts이고 application-driven인 primitive는 resources입니다.

</details>

### 3. Which three primitives do MCP servers expose?

A. Tools, resources, and prompts\
B. Tools, sampling, and roots\
C. Resources, sessions, and schemas\
D. Prompts, models, and agents

**한국어**

MCP server가 노출하는 primitive 세 가지는 무엇입니까?

A. Tools, resources, prompts\
B. Tools, sampling, roots\
C. Resources, sessions, schemas\
D. Prompts, models, agents

<details><summary>정답</summary>

A. Servers provide tools (functions for the model), resources (context/data), and prompts (templated messages/workflows). Sampling and roots are client-side primitives. (Fundamentals)

A. server는 tools, resources, prompts를 제공합니다. tools는 모델이 쓰는 함수입니다. resources는 맥락과 데이터입니다. prompts는 템플릿 메시지와 워크플로입니다. Sampling과 roots는 client 쪽 primitive입니다. (Fundamentals)

</details>

<details><summary>자세한 설명</summary>

server 쪽 primitive와 client 쪽 기능을 구분하는 문제입니다. server가 노출하는 것은 tools, resources, prompts 세 가지입니다.

A가 이 세 가지를 정확히 나열합니다. 그래서 A가 정답입니다.

B의 sampling과 roots는 client 쪽 기능입니다. C의 sessions는 primitive가 아닙니다. 2026-07-28에서는 프로토콜 세션 자체가 없어졌습니다. D의 models와 agents도 MCP primitive가 아닙니다.

반대로 client 쪽 기능도 구분해 둡니다. 2026-07-28 기준 client capability는 elicitation, sampling, roots입니다. 이 가운데 sampling과 roots는 폐기 예정이지만 아직 동작합니다. "sampling과 roots가 삭제됐다"는 설명은 틀립니다.

</details>

### 4. What is the correct cardinality between MCP clients and servers?

A. One client, many servers\
B. Each client connects to exactly one server (1:1); a host runs many clients\
C. Many-to-many\
D. One client per host maximum

**한국어**

MCP client와 server 사이의 올바른 대응 관계는 무엇입니까?

A. client 하나에 server 여럿\
B. 각 client는 정확히 server 하나에 연결합니다(1:1). host 하나가 client 여럿을 실행합니다\
C. 다대다\
D. host당 client 최대 하나

<details><summary>정답</summary>

B. Every client maintains a dedicated 1:1 relationship with a single server. The host creates and manages one client instance per server it connects to. (Architecture)

B. 모든 client는 server 하나와 전용 1:1 관계를 유지합니다. host는 연결하는 server마다 client 인스턴스를 하나씩 만들고 관리합니다. (Architecture)

</details>

<details><summary>자세한 설명</summary>

host, client, server 세 참여자의 관계를 묻는 문제입니다. client와 server는 1:1이고 host와 client는 1:N이라는 점을 알면 풀립니다.

host가 server 세 개에 연결하려면 client 인스턴스를 세 개 만듭니다. client 하나는 server 하나만 맡습니다. 그래서 B가 정답입니다.

A는 client 하나가 server 여럿을 맡는다고 해서 틀립니다. C의 다대다도 같은 이유로 틀립니다. D는 host가 client를 하나만 둔다고 해서 틀립니다. host는 연결하는 server 수만큼 client를 둡니다.

server와 1:1 관계를 유지하는 참여자는 client입니다. host와 구분해 둡니다.

</details>

### 5. Which transport would a remote MCP server most likely use?

A. stdio\
B. Streamable HTTP\
C. gRPC\
D. WebRTC only

**한국어**

원격 MCP server가 쓸 가능성이 가장 큰 transport는 무엇입니까?

A. stdio\
B. Streamable HTTP\
C. gRPC\
D. WebRTC만

<details><summary>정답</summary>

B. Streamable HTTP is the standard transport for remote servers. stdio is for local servers launched as subprocesses; gRPC and WebRTC are not standard MCP transports. (Architecture)

B. 원격 server의 표준 transport는 Streamable HTTP입니다. stdio는 하위 프로세스로 띄우는 로컬 server용입니다. gRPC와 WebRTC는 MCP 표준 transport가 아닙니다. (Architecture)

</details>

<details><summary>자세한 설명</summary>

로컬 server와 원격 server가 각각 어떤 transport를 쓰는지 묻는 문제입니다. MCP의 표준 transport는 stdio와 Streamable HTTP 두 가지뿐입니다.

stdio는 같은 컴퓨터에서 쓰는 방식입니다. client가 server를 하위 프로세스로 띄우고 표준 입력과 표준 출력으로 메시지를 주고받습니다. 네트워크 너머에 있는 server에는 쓸 수 없으므로 A는 답이 아닙니다.

Streamable HTTP는 원격 server용입니다. server는 `https://example.com/mcp` 같은 주소 하나를 열어 두고 POST 요청을 받습니다. 그래서 B가 정답입니다.

gRPC와 WebRTC는 MCP 표준 transport가 아닙니다. 스펙이 직접 만든 transport를 허용하기는 하지만 표준으로 정한 것은 위 두 가지입니다. 그래서 C와 D도 답이 아닙니다.

예전에 쓰던 HTTP+SSE transport는 2025-03-26부터 폐기 예정이고 Streamable HTTP가 그 자리를 대신합니다. "원격 server는 HTTP+SSE를 쓴다"는 설명은 틀립니다. 인가 방식도 transport마다 다릅니다. OAuth는 HTTP transport에서 쓰고 stdio server는 환경 변수에서 자격 증명을 읽습니다.

</details>

### 6. Can an MCP server read the host's full conversation or see data from other connected servers?

A. Yes, all context is shared by design\
B. Yes, but only after user consent\
C. No — the host enforces isolation; servers receive only the context sent to them\
D. Only servers using the stdio transport can

**한국어**

MCP server가 host의 전체 대화를 읽거나 연결된 다른 server의 데이터를 볼 수 있습니까?

A. 예. 설계상 모든 맥락을 공유합니다\
B. 예. 다만 사용자 동의를 받은 뒤에만 가능합니다\
C. 아니요. host가 server를 서로 격리하고 server는 자기에게 보낸 맥락만 받습니다\
D. stdio transport를 쓰는 server만 가능합니다

<details><summary>정답</summary>

C. A core design principle: servers cannot read the whole conversation or "see into" other servers. The host aggregates context and enforces isolation between clients. (Architecture)

C. MCP의 기본 설계 원칙입니다. server는 대화 전체를 읽을 수 없습니다. 다른 server를 "들여다볼" 수도 없습니다. host가 맥락을 모으고 client 사이를 격리합니다. (Architecture)

</details>

<details><summary>자세한 설명</summary>

server가 볼 수 있는 정보의 범위를 묻는 문제입니다. 맥락을 모으는 쪽은 host이고 server는 자기에게 온 것만 본다는 점을 알면 풀립니다.

host는 대화 전체와 여러 server의 결과를 한곳에 모읍니다. 각 server에는 그 server가 처리할 내용만 보냅니다. server마다 client가 따로 있으므로 server끼리 서로의 데이터를 볼 수도 없습니다. 그래서 C가 정답입니다.

A는 모든 맥락을 공유한다고 해서 틀립니다. B는 그럴듯한 함정입니다. 사용자 동의가 있어도 server가 대화 전체를 읽는 구조는 아닙니다. D도 틀립니다. transport 종류와 관계없이 격리는 host가 담당합니다.

</details>

### 7. A client sends tools/call with a tool name that the server does not expose. How does the server respond?

A. A result with isError: true\
B. A JSON-RPC protocol error (invalid params, e.g. -32602)\
C. A list_changed notification\
D. It ignores the request

**한국어**

client가 server에 없는 tool 이름으로 `tools/call`을 보냅니다. server는 어떻게 응답합니까?

A. `isError: true`인 결과\
B. JSON-RPC 프로토콜 오류(invalid params, 예: `-32602`)\
C. `list_changed` notification\
D. 요청 무시

<details><summary>정답</summary>

B. An unknown tool is a protocol error, returned as a JSON-RPC error object. isError: true is reserved for errors during the execution of a valid tool. (Interactions)

B. 알 수 없는 tool은 프로토콜 오류입니다. server는 JSON-RPC 오류 객체로 돌려줍니다. `isError: true`는 유효한 tool을 실행하다가 생긴 오류에만 씁니다. (Interactions)

</details>

<details><summary>자세한 설명</summary>

tool 오류를 보고하는 두 경로를 구분하는 문제입니다. 요청 자체가 잘못됐으면 프로토콜 오류입니다. tool이 실행되다가 실패하면 tool 실행 오류입니다.

없는 tool을 부르면 실행할 대상이 없습니다. 그래서 server는 JSON-RPC `error` 객체를 돌려줍니다. 오류 코드는 `-32602`(Invalid params)입니다. B가 정답입니다.

A의 `isError: true`는 tool이 실제로 실행된 뒤 실패했을 때 쓰는 결과입니다. C의 `notifications/tools/list_changed`는 tool 목록이 바뀌었다고 알리는 notification이고 오류 응답이 아닙니다. D처럼 요청을 무시하는 동작은 스펙에 없습니다. client는 응답을 받지 못하고 시간 초과로 끝납니다.

오류 코드는 특히 주의해서 구분합니다. 알 수 없는 tool은 `-32601`이 아니라 `-32602`입니다. `-32601`은 메서드 자체를 모를 때 씁니다. 인자가 tool의 `inputSchema`에 맞지 않을 때는 `isError: true`인 tool 실행 오류로 보고합니다. 이 규칙은 2025-11-25 개정(SEP-1303)에서 정해졌습니다.

</details>

### 8. A tool's external API call times out during execution. What should the server return?

A. A JSON-RPC protocol error\
B. Nothing; it should close the connection\
C. A successful JSON-RPC response whose result has isError: true and an explanatory message\
D. An elicitation request

**한국어**

tool 실행 중 외부 API 호출이 시간 초과됐습니다. server는 무엇을 돌려줘야 합니까?

A. JSON-RPC 프로토콜 오류\
B. 아무것도 돌려주지 않고 연결을 닫습니다\
C. result에 `isError: true`와 설명 메시지를 담은 성공 JSON-RPC 응답\
D. elicitation 요청

<details><summary>정답</summary>

C. Execution failures are reported in-band as tool results with isError: true, so the model can see the error and self-correct. Protocol errors are for malformed or invalid requests. (Interactions)

C. 실행 실패는 `isError: true`인 tool 결과로 대역 안에서(in-band) 보고합니다. 그래야 모델이 오류를 보고 스스로 고칠 수 있습니다. 프로토콜 오류는 형식이 잘못됐거나 유효하지 않은 요청에 씁니다. (Interactions)

</details>

<details><summary>자세한 설명</summary>

7번과 짝을 이루는 문제입니다. tool이 실행되다가 실패하면 프로토콜 오류가 아니라 tool 실행 오류로 보고한다는 점을 알면 풀립니다.

외부 API 시간 초과는 요청이 잘못된 경우가 아닙니다. tool은 제대로 불렸고 실행 도중에 실패했습니다. 그래서 server는 정상 `result`를 돌려주고 그 안에 `isError: true`와 설명을 `content`로 담습니다. 이 결과는 모델에게 전달됩니다. 모델은 다시 시도하거나 다른 방법을 고를 수 있습니다. C가 정답입니다.

A의 프로토콜 오류는 없는 tool이나 `CallToolRequest` 형식에 맞지 않는 요청처럼 요청 자체가 잘못됐을 때 씁니다. B처럼 연결을 닫으면 client는 무슨 일이 생겼는지 알 수 없습니다. D의 elicitation은 사용자에게 입력을 받는 기능이고 오류를 보고하는 수단이 아닙니다.

tool 실행 오류로 보고하는 경우는 API 실패 말고도 더 있습니다. 날짜 형식이 틀린 입력처럼 검증에 실패한 인자, 업무 규칙에 따른 거절, 만료된 handle도 `isError: true`로 보고합니다.

</details>

### 9. A server wants an LLM completion but has no API key. Which client primitive does it use?

A. Sampling\
B. Elicitation\
C. Prompts\
D. Roots

**한국어**

server가 LLM 응답 생성(completion)을 원하지만 API 키가 없습니다. 어떤 client primitive를 씁니까?

A. Sampling\
B. Elicitation\
C. Prompts\
D. Roots

<details><summary>정답</summary>

A. Sampling (sampling/createMessage) lets the server request a completion through the client, which controls model access and permissions. Note: Sampling was deprecated in the 2026-07-28 revision — new implementations should integrate with LLM provider APIs directly. (Interactions)

A. Sampling(`sampling/createMessage`)을 쓰면 server가 client를 거쳐 completion을 요청할 수 있습니다. 모델 접근과 권한은 client가 제어합니다. 참고로 Sampling은 2026-07-28 개정에서 폐기 예정(deprecated)이 됐습니다. 새 구현은 LLM 제공자 API와 직접 연동해야 합니다. (Interactions)

</details>

<details><summary>자세한 설명</summary>

client 쪽 기능의 용도를 구분하는 문제입니다. 모델 호출을 client에 맡기는 기능이 sampling이라는 점을 알면 풀립니다.

sampling을 쓰면 server는 자기 API 키 없이 client가 가진 모델 접근을 빌려 씁니다. 어떤 모델을 쓸지와 요청을 허용할지는 client가 정합니다. 그래서 A가 정답입니다.

B의 elicitation은 사용자에게 입력을 받는 기능입니다. C의 prompts는 server가 제공하는 템플릿이고 client 쪽 기능이 아닙니다. D의 roots는 작업 범위가 되는 디렉터리를 알려 주는 기능입니다. 셋 다 모델 응답 생성과 관계가 없습니다.

2026-07-28 기준으로 두 가지를 더 알아 둡니다. 첫 번째로 sampling은 폐기 예정이지만 아직 동작합니다. 2027-07-28 이후에 나오는 개정부터 없어질 수 있습니다. 두 번째로 server가 client에 요청을 먼저 보내는 방식은 없어졌습니다. server는 `resultType: "input_required"` 결과 안에 `sampling/createMessage`를 담아 돌려줍니다. client는 답을 `inputResponses`에 넣어 원래 요청을 다시 보냅니다. 이 방식을 MRTR(Multi Round-Trip Requests)이라고 합니다.

MRTR이 sampling을 대체했다고 보면 안 됩니다. MRTR이 바꾼 것은 server가 client에 요청을 전달하는 방식입니다. `inputRequests`에는 `sampling/createMessage`뿐 아니라 `elicitation/create`와 `roots/list`도 담을 수 있습니다. 그래서 MRTR은 세 요청이 같이 쓰는 전달 방식입니다.

sampling 기능은 MRTR과 따로 SEP-2577로 폐기 예정이 됐습니다. 스펙이 권하는 대안은 server가 LLM 제공자 API를 직접 호출하는 것입니다. roots와 logging도 같은 SEP로 폐기 예정이 됐습니다. elicitation은 폐기 대상이 아니고 MRTR로 전달됩니다.

정리하면 다음과 같습니다.

- server가 client에 먼저 보내던 요청: MRTR로 바뀌었습니다.
- sampling 기능: 폐기 예정이지만 지금도 MRTR 위에서 동작합니다. 대안은 LLM API 직접 호출입니다.
- elicitation: 폐기 대상이 아닙니다. MRTR로 전달됩니다.

"sampling은 MRTR로 대체됐다"처럼 2가지 변경을 하나로 묶은 설명은 정확하지 않습니다.

</details>

### 10. How does a client learn that a server's tool list changed?

A. It must reconnect\
B. Via a notifications/tools/list_changed notification, then re-fetching tools/list\
C. Tool lists can never change\
D. Via the ping response

**한국어**

client는 server의 tool 목록이 바뀐 것을 어떻게 압니까?

A. 다시 연결해야 합니다\
B. `notifications/tools/list_changed` notification을 받은 뒤 `tools/list`를 다시 가져옵니다\
C. tool 목록은 절대 바뀌지 않습니다\
D. `ping` 응답으로 압니다

<details><summary>정답</summary>

B. The server emits notifications/tools/list_changed and the client re-fetches tools/list. In the 2026-07-28 revision, the client must first open a subscriptions/listen stream to receive change notifications. (Interactions)

B. server가 `notifications/tools/list_changed`를 보내면 client가 `tools/list`를 다시 가져옵니다. 2026-07-28 개정에서는 client가 먼저 `subscriptions/listen` 스트림을 열어야 변경 notification을 받을 수 있습니다. (Interactions)

</details>

<details><summary>자세한 설명</summary>

tool 목록 변경을 알리는 방법을 묻는 문제입니다. list_changed notification을 받고 목록을 다시 조회한다는 흐름을 알면 풀립니다.

server는 tool 목록이 바뀌면 `notifications/tools/list_changed`를 보냅니다. client는 이 notification을 받은 뒤 `tools/list`를 다시 호출해 새 목록을 가져옵니다. 그래서 B가 정답입니다.

A처럼 다시 연결할 필요는 없습니다. C는 틀립니다. server는 `tools {listChanged}` capability로 목록이 바뀔 수 있다고 알립니다. D의 `ping`은 2026-07-28에서 없어진 메서드입니다.

2026-07-28에서 바뀐 점도 확인해 둡니다. 이 notification은 `subscriptions/listen` 스트림으로 옵니다. client는 `params.notifications`에 `toolsListChanged: true`를 넣어 구독을 엽니다. resource 변경도 같은 스트림으로 받습니다. `resources/subscribe`는 없어졌습니다. 또 `tools/list` 결과에는 `ttlMs`가 붙습니다. list_changed notification을 받으면 그 캐시는 바로 무효가 됩니다.

</details>

### 11. Which primitive would a server use to ask the user for a missing parameter mid-operation?

A. Sampling\
B. Roots\
C. Elicitation\
D. Progress

**한국어**

작업 도중 빠진 파라미터를 사용자에게 물어보려면 server는 어떤 primitive를 씁니까?

A. Sampling\
B. Roots\
C. Elicitation\
D. Progress

<details><summary>정답</summary>

C. Elicitation lets a server request structured input from the user via the client — either an in-band form or, for sensitive data, an out-of-band URL flow. The user can accept, decline, or cancel. (Interactions)

C. Elicitation을 쓰면 server가 client를 거쳐 사용자에게 구조화된 입력을 요청할 수 있습니다. 대역 안(in-band) 폼을 쓸 수 있습니다. 민감한 데이터라면 대역 밖(out-of-band) URL 흐름을 씁니다. 사용자는 수락, 거절, 취소할 수 있습니다. (Interactions)

</details>

<details><summary>자세한 설명</summary>

사용자에게 입력을 받는 기능을 묻는 문제입니다. 사용자에게 묻는 것은 elicitation이고 모델에게 맡기는 것은 sampling이라고 구분하면 풀립니다.

elicitation에는 두 방식이 있습니다. form 모드는 `requestedSchema`로 받을 항목을 정하고 client가 폼을 띄웁니다. url 모드는 사용자를 외부 페이지로 보냅니다. OAuth 동의나 결제처럼 민감한 데이터가 client를 거치면 안 될 때 씁니다. 사용자의 답은 `accept`, `decline`, `cancel` 중 하나입니다. 그래서 C가 정답입니다.

A의 sampling은 사용자가 아니라 모델에게 응답 생성을 맡기는 기능입니다. B의 roots는 작업 범위가 되는 디렉터리를 알려 주는 기능입니다. D의 progress는 오래 걸리는 요청의 진행 상황을 알리는 notification이고 입력을 받지 못합니다.

2026-07-28에서는 elicitation도 MRTR로 동작합니다. server는 `input_required` 결과에 `elicitation/create`를 담아 돌려줍니다. client는 사용자의 답을 `inputResponses`에 넣어 원래 요청을 새 id로 다시 보냅니다. server는 client가 그 요청에 `elicitation` capability를 선언했을 때만 이 요청을 담을 수 있습니다. "server가 client에 elicitation 요청을 먼저 보낸다"는 설명은 옛 스펙 기준입니다.

</details>

### 12. Which participant is responsible for obtaining user consent before a tool call?

A. The server\
B. The LLM\
C. The host\
D. The registry

**한국어**

tool 호출 전에 사용자 동의를 받을 책임은 어느 참여자에게 있습니까?

A. server\
B. LLM\
C. host\
D. registry

<details><summary>정답</summary>

C. The host is the policy enforcement point: it enforces security policies, obtains explicit user consent before invoking tools, and controls permissions. Servers never present consent UIs. (Security)

C. host가 정책을 집행하는 지점입니다. host는 보안 정책을 적용합니다. tool을 호출하기 전에 사용자의 명시적 동의를 받고 권한을 제어합니다. server는 동의 UI를 띄우지 않습니다. (Security)

</details>

<details><summary>자세한 설명</summary>

보안 책임이 어느 참여자에게 있는지 묻는 문제입니다. 사용자와 직접 마주하는 쪽이 host라는 점을 알면 풀립니다.

tool 호출을 사용자에게 보여 주고 승인이나 거부를 받는 일은 host가 맡습니다. 스펙도 민감한 작업은 확인을 받고 호출 전에 입력값을 보여 주라고 권합니다(SHOULD). 그래서 C가 정답입니다.

A의 server는 요청을 받아 처리할 뿐 사용자 화면이 없습니다. B의 LLM은 tool 호출을 결정하는 쪽이라 자기 호출을 스스로 승인할 수 없습니다. D의 registry는 server 메타데이터를 모아 두는 곳이고 실행 흐름에 끼지 않습니다.

동의와 인가의 시점은 다릅니다. 동의는 연결할 때 받습니다. 인가 확인은 tool을 호출할 때 합니다.

</details>

### 13. "Tool poisoning" refers to:

A. Rate-limiting a tool until it fails\
B. Malicious instructions hidden in tool descriptions that manipulate the model\
C. Sending malformed JSON to crash a server\
D. Stealing OAuth tokens

**한국어**

"Tool poisoning"은 무엇을 가리킵니까?

A. tool이 실패할 때까지 호출 속도를 제한하는 것\
B. tool 설명에 악성 지시를 숨겨 모델을 조종하는 것\
C. 잘못된 JSON을 보내 server를 다운시키는 것\
D. OAuth token 탈취

<details><summary>정답</summary>

B. Tool descriptions are injected into model context, so a malicious server can hide instructions there. The spec's Tool Safety principle: treat tool descriptions as untrusted unless they come from a trusted server. (Security)

B. tool 설명은 모델 맥락에 그대로 들어갑니다. 그래서 악성 server가 그 안에 지시를 숨길 수 있습니다. 스펙의 Tool Safety 원칙은 신뢰하는 server에서 온 것이 아니면 tool 설명을 신뢰하지 말라고 합니다. (Security)

</details>

<details><summary>자세한 설명</summary>

MCP에서 자주 거론되는 공격 이름을 묻는 문제입니다. tool 설명이 모델에게 그대로 읽힌다는 점을 알면 풀립니다.

host는 `tools/list`로 받은 tool 이름과 설명을 모델 맥락에 넣습니다. 악성 server는 설명 안에 사용자가 원하지 않는 동작을 시키는 지시를 숨길 수 있습니다. 모델은 그 지시를 따를 수 있습니다. 그래서 B가 정답입니다.

A는 호출 속도 제한이고 C는 잘못된 입력으로 server를 멈추게 하는 서비스 거부 공격입니다. D는 token 탈취입니다. 셋 다 보안 위협이지만 tool poisoning과는 다릅니다.

brief는 tool 설명과 annotation, 아이콘, 결과를 모두 모델에게 신뢰할 수 없는 입력으로 봅니다. 비슷한 공격으로 rug pull이 있습니다. 처음에는 정상이던 tool 정의를 나중에 바꾸는 공격입니다.

rug pull은 "깔고 있던 양탄자를 갑자기 잡아당긴다"는 영어 표현에서 왔습니다. 공격 순서는 다음과 같습니다.

1. server가 처음에는 무해한 설명을 가진 tool을 제공합니다.
2. 사용자는 그 설명을 보고 server를 연결하거나 tool 사용을 승인합니다.
3. 나중에 server는 같은 이름의 tool 설명이나 입력 스키마를 악성으로 바꿉니다.
4. client는 `tools/list`를 다시 받아 바뀐 정의를 모델 맥락에 넣습니다. 사용자는 이미 승인했으므로 바뀐 것을 알아채지 못합니다.

2026-07-28 스펙에서는 tool 목록이 바뀌면 server가 `subscriptions/listen` 스트림으로 `notifications/tools/list_changed`를 보냅니다. client는 이 알림을 받거나 `ttlMs`가 지나면 목록을 다시 받습니다. 정상 기능인 목록 갱신이 rug pull의 통로가 됩니다.

tool poisoning과 rug pull의 차이는 시점입니다. tool poisoning은 악성 지시가 처음부터 설명에 들어 있습니다. rug pull은 승인을 받은 뒤에 정의를 바꿉니다. 그래서 처음 연결할 때 설명을 검토하는 것만으로는 rug pull을 막지 못합니다.

흔히 거론되는 대응은 다음과 같습니다. 스펙이 MUST로 정한 절차는 아니고 보안 권고 수준의 내용입니다.

- 승인한 시점의 tool 정의를 해시 등으로 고정(pinning)해 두고 바뀌면 감지합니다.
- 정의가 바뀌면 사용자에게 바뀐 내용을 보여 주고 다시 승인을 받습니다.
- 신뢰하는 server만 연결하고 출처와 버전을 관리합니다.

"승인 후 tool 설명이 바뀌는 공격"은 rug pull이고 "처음부터 설명에 숨긴 지시"는 tool poisoning입니다.

</details>

### 14. Why must an MCP server reject tokens that were not issued specifically for it?

A. To reduce latency\
B. To prevent token passthrough and confused-deputy attacks\
C. Tokens for other services are always expired\
D. The JSON-RPC spec forbids them

**한국어**

MCP server는 왜 자신을 위해 발급되지 않은 token을 거부해야 합니까?

A. 지연 시간을 줄이려고\
B. token passthrough와 confused deputy 공격을 막으려고\
C. 다른 서비스용 token은 항상 만료돼 있어서\
D. JSON-RPC 스펙이 금지해서

<details><summary>정답</summary>

B. Token passthrough is an explicitly forbidden anti-pattern: servers MUST validate the token audience and MUST NOT accept or forward tokens issued for other services. Accepting them breaks audit trails and lets one compromised service unlock others. (Security)

B. token passthrough는 명시적으로 금지된 안티패턴입니다. server는 token의 audience를 반드시 검증해야 합니다(MUST). 다른 서비스용으로 발급된 token을 받아들이거나 전달해서도 안 됩니다(MUST NOT). 그런 token을 받아들이면 감사 추적이 끊깁니다. 서비스 하나가 뚫리면 다른 서비스까지 열립니다. (Security)

</details>

<details><summary>자세한 설명</summary>

audience 검증이 왜 필요한지 묻는 문제입니다. audience는 token이 어느 서비스용으로 발급됐는지 나타내는 값입니다. server는 자기용으로 발급된 token만 받아야 한다는 점을 알면 풀립니다.

server가 다른 서비스용 token을 받아 주면 그 token이 원래 범위를 벗어나 쓰입니다. 받은 token을 상위 API로 그대로 넘기면 상위 API는 누가 실제로 요청했는지 알 수 없습니다. confused deputy는 권한을 가진 중개자가 다른 주체의 요청을 대신 실행해 주는 문제입니다. 그래서 B가 정답입니다.

A의 지연 시간은 관계가 없습니다. C는 전제가 틀렸습니다. 다른 서비스용 token도 유효 기간 안이면 만료되지 않았습니다. 문제가 되는 것은 만료가 아니라 audience입니다. D도 틀립니다. 이 규칙은 JSON-RPC가 아니라 MCP 인가 스펙에 있습니다.

server가 상위 API를 부를 때 사용자 token을 그대로 사용하면 안 됩니다. 상위 API 호출에는 별도 token을 사용합니다. client 쪽에서는 RFC 8707의 `resource` 파라미터에 server의 정식 URI를 넣어 audience를 지정합니다.

</details>

### 15. A stdio-based server should write its logs to:

A. stdout\
B. stderr — never stdout\
C. The tools/call result\
D. The client's console

**한국어**

stdio 기반 server는 로그를 어디에 써야 합니까?

A. stdout\
B. stderr. stdout에는 절대 쓰지 않습니다\
C. `tools/call` 결과\
D. client의 콘솔

<details><summary>정답</summary>

B. stdout carries only valid MCP JSON-RPC messages; anything else corrupts the stream. Servers MAY write logs to stderr, which the client can capture, forward, or ignore. (Security / Observability)

B. stdout에는 유효한 MCP JSON-RPC 메시지만 흐릅니다. 다른 것이 섞이면 스트림이 깨집니다. server는 stderr에 로그를 쓸 수 있습니다(MAY). client는 그 로그를 수집하거나 전달하거나 무시할 수 있습니다. (Security / Observability)

</details>

<details><summary>자세한 설명</summary>

stdio transport의 출력 규칙을 묻는 문제입니다. stdout은 MCP 메시지 전용이라는 점을 알면 풀립니다.

stdio에서 client는 server의 stdout을 줄 단위 JSON-RPC 메시지로 읽습니다. 로그 한 줄이 stdout에 섞이면 client는 그 줄을 메시지로 해석하다가 실패합니다. 그래서 로그는 stderr로 보냅니다. B가 정답입니다.

A는 바로 그 잘못된 방법입니다. C처럼 로그를 tool 결과에 넣으면 모델 맥락으로 들어가 버립니다. tool 결과는 로그를 담는 자리가 아닙니다. D는 server가 직접 쓸 수 있는 곳이 아닙니다. server는 stderr에 쓰고 그 내용을 콘솔에 보여 줄지는 client가 정합니다.

2026-07-28에서 MCP Logging 기능이 폐기 예정이 됐습니다. 대신 stdio에서는 stderr에 쓰거나 OpenTelemetry를 쓰라고 안내합니다. stderr에 찍힌 내용이 꼭 오류라는 뜻은 아니라는 점도 기억해 둡니다. 로그에는 자격 증명이나 개인 정보를 남기지 않습니다.

</details>

### 16. How should a client treat tool annotations such as readOnlyHint and destructiveHint?

A. As guarantees enforced by the protocol\
B. As untrusted hints unless they come from a trusted server\
C. As mandatory fields every tool must declare\
D. As instructions for the LLM only

**한국어**

client는 `readOnlyHint`, `destructiveHint` 같은 tool annotation을 어떻게 다뤄야 합니까?

A. 프로토콜이 보장하는 사항\
B. 신뢰하는 server에서 온 것이 아니면 신뢰할 수 없는 힌트\
C. 모든 tool이 선언해야 하는 필수 필드\
D. LLM만을 위한 지시

<details><summary>정답</summary>

B. Annotations are optional behavior hints. Clients MUST NOT rely on them for security decisions unless the server is trusted — a malicious server can label a destructive tool readOnlyHint: true. (Security)

B. annotation은 선택 사항인 동작 힌트입니다. server를 신뢰하지 않는 한 client는 annotation을 보안 판단의 근거로 삼으면 안 됩니다(MUST NOT). 악성 server는 파괴적인 tool에 `readOnlyHint: true`를 붙일 수 있습니다. (Security)

</details>

<details><summary>자세한 설명</summary>

tool annotation의 성격을 묻는 문제입니다. annotation은 server가 직접 적는 값이라 검증되지 않는다는 점을 알면 풀립니다.

annotation은 tool이 읽기만 하는지, 파괴적인지, 같은 호출을 반복해도 안전한지 같은 정보를 담습니다. 그런데 이 값은 server가 적은 것입니다. 프로토콜은 값이 사실인지 확인하지 않습니다. 그래서 B가 정답입니다.

A는 보증이라고 해서 틀립니다. C도 틀립니다. `annotations`는 tool 정의에서 선택 필드입니다. D도 틀립니다. annotation은 client가 읽는 힌트이고 LLM에게 주는 지시가 아닙니다.

annotation은 tool 정의의 `annotations` 필드에 들어갑니다.

```json
{
  "name": "delete_file",
  "description": "...",
  "inputSchema": { ... },
  "annotations": {
    "title": "파일 삭제",
    "readOnlyHint": false,
    "destructiveHint": true,
    "idempotentHint": true,
    "openWorldHint": false
  }
}
```

- `readOnlyHint`: 환경을 바꾸지 않고 읽기만 하는지 나타냅니다.
- `destructiveHint`: 데이터를 지우거나 덮어쓰는지 나타냅니다. 읽기 전용이 아닐 때만 의미가 있습니다.
- `idempotentHint`: 같은 인자로 여러 번 호출해도 결과가 같은지 나타냅니다.
- `openWorldHint`: 인터넷처럼 바깥 세계와 상호작용하는지 나타냅니다.

기본값도 알아 둡니다. "annotation이 없는 tool"은 server가 이 필드를 생략한 tool입니다. 이때는 다음 기본값이 적용됩니다.

| 필드 | 기본값 | 의미 |
|---|---|---|
| `readOnlyHint` | false | 쓰기를 할 수 있습니다 |
| `destructiveHint` | true | 파괴적일 수 있습니다 |
| `idempotentHint` | false | 반복 호출이 안전하지 않습니다 |
| `openWorldHint` | true | 외부 세계에 영향을 줄 수 있습니다 |

네 값이 모두 위험한 쪽입니다. 그래서 annotation이 없는 tool을 무해하다고 보면 안 됩니다. 쓰기와 삭제를 하고 외부에도 접근할 수 있는 tool로 다뤄야 합니다. host는 이런 tool을 호출하기 전에 사용자 확인을 받는 것이 맞습니다. rohitg00 mock-03에 이 내용을 묻는 문항이 있습니다.

주의할 점이 2가지 있습니다. annotation이 없으면 위험하다고 가정합니다. annotation이 있어도 신뢰하는 server가 보낸 값이 아니면 믿지 않습니다. 신뢰하지 않는 server가 `readOnlyHint: true`를 붙였다고 해서 확인 단계를 건너뛰면 안 됩니다. rohitg00 mock-01에 이 내용을 묻는 문항이 있습니다.

</details>

### 17. MCP protocol versions are:

A. Semantic version numbers (1.2.3)\
B. Date-based strings (e.g. 2026-07-28) agreed between client and server\
C. Implicit and never communicated\
D. Tied to the model version

**한국어**

MCP 프로토콜 버전은 어떤 형식입니까?

A. 시맨틱 버전 번호(1.2.3)\
B. client와 server가 합의하는 날짜 기반 문자열(예: 2026-07-28)\
C. 암묵적이고 주고받지 않습니다\
D. 모델 버전에 묶여 있습니다

<details><summary>정답</summary>

B. Versions use YYYY-MM-DD strings, incremented only on backwards-incompatible changes. Older revisions negotiated the version during the initialize handshake; the stateless 2026-07-28 revision declares it per request in _meta. (Ecosystem)

B. 버전은 YYYY-MM-DD 문자열입니다. 하위 호환이 깨지는 변경이 있을 때만 올라갑니다. 이전 개정에서는 `initialize` handshake 중에 버전을 협상했습니다. stateless인 2026-07-28 개정에서는 요청마다 `_meta`에 버전을 선언합니다. (Ecosystem)

</details>

<details><summary>자세한 설명</summary>

프로토콜 버전 표기법을 묻는 문제입니다. MCP 버전은 날짜 문자열이라는 점을 알면 풀립니다.

버전 날짜는 하위 호환이 깨지는 변경이 마지막으로 들어간 날입니다. 그래서 B가 정답입니다.

A의 시맨틱 버전은 MCP가 쓰지 않습니다. C도 틀립니다. 2026-07-28에서는 모든 요청의 `_meta`에 `io.modelcontextprotocol/protocolVersion`이 반드시 들어갑니다. HTTP에서는 `MCP-Protocol-Version` 헤더에도 같은 값을 넣습니다. D도 틀립니다. 프로토콜 버전은 모델 버전과 따로 정해집니다.

버전이 맞지 않을 때의 동작도 알아 둡니다. server가 그 버전을 지원하지 않으면 `-32022`(UnsupportedProtocolVersion)를 돌려줍니다. `data.supported`에는 지원하는 버전 목록이 들어 있습니다. client는 그 가운데 하나로 다시 요청합니다. 개정 상태로는 Draft, Current, Final이 있습니다. 지금 Current인 개정은 2026-07-28입니다.

</details>

### 18. Who governs the Model Context Protocol today?

> 변동 정보: MCP의 AAIF 기증 시점과 관리 주체는 연혁과 조직 정보입니다. 바뀔 수 있으므로 최신 공식 문서로 확인합니다.

A. Anthropic, as a proprietary standard\
B. The Agentic AI Foundation (AAIF) under the Linux Foundation, via an open maintainer structure\
C. A consortium of cloud vendors with paid membership\
D. The W3C

**한국어**

현재 Model Context Protocol을 관리하는 곳은 어디입니까?

A. 독점 표준으로서 Anthropic\
B. 개방형 메인테이너 구조로 운영하는 Linux Foundation 산하 Agentic AI Foundation(AAIF)\
C. 유료 회원제인 클라우드 벤더 컨소시엄\
D. W3C

<details><summary>정답</summary>

B. Anthropic donated MCP to the Linux Foundation's Agentic AI Foundation in December 2025. Changes are proposed through SEPs (Specification Enhancement Proposals), and the spec is Apache 2.0 licensed. The AAIF also issues the MCPA certification. (Ecosystem)

B. Anthropic은 2025년 12월 MCP를 Linux Foundation의 Agentic AI Foundation에 기증했습니다. 변경은 SEP(Specification Enhancement Proposal)로 제안합니다. 스펙은 Apache 2.0 라이선스입니다. AAIF는 MCPA 자격증도 발급합니다. (Ecosystem)

</details>

<details><summary>자세한 설명</summary>

MCP의 관리 주체를 묻는 문제입니다. 지금 MCP는 Linux Foundation 산하 AAIF의 프로젝트라는 점을 알면 풀립니다.

Anthropic이 처음 만들었지만 지금은 AAIF가 관리합니다. 스펙 변경은 SEP 절차를 거칩니다. SEP는 `seps/` 폴더에 파일을 두는 PR 방식으로 진행합니다. 그래서 B가 정답입니다.

A는 기증 이전의 상황입니다. C의 유료 컨소시엄과 D의 W3C는 MCP를 관리하지 않습니다.

거버넌스와 관련해 SDK 등급과 기능 폐기 정책도 알아 둡니다. Tier 1 SDK는 conformance 테스트를 100% 통과해야 하고 Tier 2는 80%를 통과해야 합니다. 기능 폐기 정책은 SEP-2596으로 생겼습니다. 폐기 예정이 된 기능은 최소 12개월 동안 남습니다.

</details>

### 19. What is the official MCP Registry?

> 변동 가능(경계): Registry의 preview 상태는 생태계 서비스의 현재 상태입니다. 정답인 Registry의 성격(공개 server 메타데이터 저장소)은 잘 바뀌지 않습니다. 스펙 본문이 아니라 별도 문서에서 오는 내용입니다.

A. A package manager that hosts and executes MCP server code\
B. A centralized metadata repository of publicly available MCP servers, described via server.json\
C. A private directory for enterprise-only servers\
D. A runtime that proxies all MCP traffic

**한국어**

공식 MCP Registry는 무엇입니까?

A. MCP server 코드를 호스팅하고 실행하는 패키지 관리자\
B. 공개된 MCP server의 메타데이터를 `server.json`으로 기술해 모아 두는 중앙 저장소\
C. 기업 전용 server를 위한 비공개 디렉터리\
D. 모든 MCP 트래픽을 중계하는 런타임

<details><summary>정답</summary>

B. The registry stores metadata (not code) about public servers in standardized server.json files, with reverse-DNS names verified via GitHub/DNS/HTTP challenges. It is intended for downstream aggregators and is still in preview. (Ecosystem)

B. registry는 공개 server에 관한 메타데이터를 표준화된 `server.json` 파일에 저장합니다. 코드는 저장하지 않습니다. 역방향 DNS 이름은 GitHub, DNS, HTTP 챌린지로 검증합니다. registry는 하위 집계 서비스(aggregator)가 쓰도록 만든 것이고 아직 프리뷰 단계입니다. (Ecosystem)

</details>

<details><summary>자세한 설명</summary>

MCP Registry가 무엇을 저장하는지 묻는 문제입니다. 코드가 아니라 메타데이터만 둔다는 점을 알면 풀립니다.

`server.json`은 실제 패키지나 원격 URL을 가리킵니다. 패키지는 npm, PyPI, Docker 같은 기존 저장소에 있습니다. registry는 그 위치만 알려 줍니다. 그래서 B가 정답입니다.

A는 코드를 호스팅하고 실행한다고 해서 틀립니다. C도 틀립니다. 공식 registry는 공개 server만 받습니다. 비공개 server가 필요하면 OpenAPI 스펙을 구현한 자체 registry를 운영합니다. D도 틀립니다. registry는 MCP 트래픽을 중계하지 않습니다.

세부 사항도 구분해 둡니다. registry는 host가 직접 쓰는 용도가 아니라 aggregator용입니다. 보안 검사는 패키지 저장소와 aggregator에 맡깁니다. `server.json` 스키마 버전은 MCP 프로토콜 버전과 별개입니다. 이름은 `io.github.user/server`나 `com.example/server` 같은 역방향 DNS 형식입니다.

</details>

### 20. An organization starting MCP adoption should pilot with:

A. Write-capable automation across all teams\
B. Low-risk, read-only servers in a controlled host\
C. Unvetted community servers with full permissions\
D. Direct model-to-database connections

**한국어**

MCP 도입을 시작하는 조직은 무엇으로 시범 운영해야 합니까?

A. 모든 팀에 걸친 쓰기 가능 자동화\
B. 통제된 host 안에서 위험이 낮은 읽기 전용 server\
C. 검증되지 않은 커뮤니티 server에 전체 권한 부여\
D. 모델과 데이터베이스의 직접 연결

<details><summary>정답</summary>

B. Start with least-privilege, read-only integrations in a host you control, then expand scope as governance, auditing, and consent flows mature. (Use Cases)

B. 직접 통제하는 host에서 최소 권한의 읽기 전용 통합부터 시작합니다. 그다음 거버넌스, 감사, 동의 흐름이 자리를 잡는 만큼 범위를 넓힙니다. (Use Cases)

</details>

<details><summary>자세한 설명</summary>

도입 전략을 묻는 문제입니다. 위험이 낮은 것부터 작게 시작한다는 원칙을 알면 풀립니다.

읽기 전용 server는 데이터를 바꾸지 않습니다. 그래서 잘못 동작해도 피해가 작습니다. 직접 통제하는 host를 쓰면 어떤 server를 연결할지와 어떤 tool 호출을 승인할지 조직이 정할 수 있습니다. 그래서 B가 정답입니다.

A처럼 쓰기 가능한 자동화를 모든 팀에 한 번에 여는 것은 가장 위험한 출발입니다. C처럼 검증되지 않은 커뮤니티 server에 전체 권한을 주면 tool poisoning 같은 공격에 그대로 노출됩니다. D의 모델과 데이터베이스 직접 연결은 MCP를 쓰는 방식이 아닙니다. host의 동의 절차와 격리도 거치지 않습니다.

같은 원칙은 다른 상황에도 적용됩니다. 최소 권한, 읽기 전용, 통제된 host, 감사 로그를 갖춘 방식이 대개 맞는 선택입니다. 2026-07-28에서는 세션이 없어졌습니다. 그래서 감사 로그에 어느 에이전트가 누구의 권한으로 무엇을 했는지 남겨야 합니다.

</details>

## MCP Purpose & Scope

원본: `fundamentals/purpose-and-scope.mdx`

### 1. What integration problem does MCP solve?

A. Slow LLM inference times\
B. The N×M problem of building a custom integration for every app–data source pair\
C. The lack of a standard prompt-engineering format\
D. Incompatibility between different LLM vendors' model weights

**한국어**

MCP는 어떤 통합 문제를 해결합니까?

A. 느린 LLM 추론 시간\
B. 앱과 데이터 소스 조합마다 맞춤 통합을 만들어야 하는 N×M 문제\
C. 표준 프롬프트 엔지니어링 형식의 부재\
D. LLM 벤더마다 모델 가중치가 호환되지 않는 문제

<details><summary>정답</summary>

B. Without a standard, N applications each need bespoke integrations with M systems (N×M). MCP replaces this with one shared protocol, reducing the cost to roughly N+M.

B. 표준이 없으면 애플리케이션 N개가 시스템 M개와 각각 맞춤 통합을 해야 합니다(N×M). MCP는 이를 공통 프로토콜 하나로 바꿔 비용을 대략 N+M으로 줄입니다.

</details>

<details><summary>자세한 설명</summary>

연습 문제 1번과 같은 내용을 묻는 문제입니다. 통합 수가 N×M에서 N+M으로 준다는 계산을 알면 풀립니다.

AI 앱이 3개, 데이터 소스가 4개라고 해 봅니다. 표준이 없으면 연동이 12개 필요합니다. MCP를 쓰면 앱마다 client를 한 번, 데이터 소스마다 server를 한 번 만들면 됩니다. 그러면 7개로 끝납니다. 그래서 B가 정답입니다.

A는 성능 문제이고 D는 모델 내부의 문제입니다. C는 그럴듯하지만 틀립니다. MCP에 prompts primitive가 있기는 해도 프롬프트 작성법을 표준화하는 프로토콜은 아닙니다.

MCP를 사용하면 통합 비용은 대략 N+M입니다.

</details>

### 2. Which of the following is IN scope for the MCP specification?

A. How the host renders its user interface\
B. Which AI model the host must use\
C. The JSON-RPC 2.0 message format between clients and servers\
D. The programming language servers must be written in

**한국어**

다음 중 MCP 스펙의 범위 안에 있는 것은 무엇입니까?

A. host가 사용자 인터페이스를 렌더링하는 방식\
B. host가 써야 하는 AI 모델\
C. client와 server 사이의 JSON-RPC 2.0 메시지 형식\
D. server를 작성해야 하는 프로그래밍 언어

<details><summary>정답</summary>

C. MCP standardizes the interface — message format, primitives, negotiation, and transports. UI, model choice, and server implementation details are explicitly out of scope.

C. MCP는 인터페이스를 표준화합니다. 메시지 형식, primitive, 협상, transport가 여기에 속합니다. UI, 모델 선택, server 구현 세부 사항은 명시적으로 범위 밖입니다.

</details>

<details><summary>자세한 설명</summary>

MCP 스펙이 정하는 것과 정하지 않는 것을 구분하는 문제입니다. MCP는 client와 server 사이에 오가는 것만 정한다는 점을 알면 풀립니다.

client와 server가 주고받는 메시지는 JSON-RPC 2.0 형식입니다. 스펙은 이 형식과 primitive, capability, transport를 정합니다. 그래서 C가 정답입니다.

A의 UI는 host가 알아서 정합니다. B의 모델 선택도 host가 정할 일입니다. D의 프로그래밍 언어도 스펙이 정하지 않습니다. 공식 SDK가 TypeScript, Python, Go, C# 같은 여러 언어로 나와 있습니다.

"host가 tool 승인 화면을 어떻게 그리는가" 같은 UI 쪽 내용은 스펙 범위 밖입니다. 반대로 transport 규칙, 오류 코드, `_meta` 필드는 범위 안입니다.

</details>

### 3. Is MCP tied to a specific AI model or vendor?

A. Yes, it only works with Claude\
B. Yes, it requires Anthropic-hosted models\
C. No, it is an open, model-agnostic standard\
D. No, but it only supports open-weight models

**한국어**

MCP는 특정 AI 모델이나 벤더에 묶여 있습니까?

A. 예. Claude에서만 동작합니다\
B. 예. Anthropic이 호스팅하는 모델이 필요합니다\
C. 아니요. 개방형이고 모델에 구애받지 않는 표준입니다\
D. 아니요. 다만 오픈 웨이트 모델만 지원합니다

<details><summary>정답</summary>

C. MCP is open source and model-agnostic: any MCP-compatible host can use any LLM. Anthropic created it, but it is not tied to Claude.

C. MCP는 오픈 소스이고 모델에 구애받지 않습니다. MCP를 지원하는 host라면 어떤 LLM이든 쓸 수 있습니다. Anthropic이 만들었지만 Claude에 묶여 있지 않습니다.

</details>

<details><summary>자세한 설명</summary>

MCP가 특정 모델과 무관한지 묻는 문제입니다. MCP는 client와 server 사이의 규약이고 모델을 정하지 않는다는 점을 알면 풀립니다.

MCP 메시지에는 특정 모델을 요구하는 부분이 없습니다. host가 어떤 LLM을 쓰든 같은 server에 연결할 수 있습니다. 그래서 C가 정답입니다.

A와 B는 Anthropic이 MCP를 만들었다는 사실에서 나온 오해입니다. D는 "아니요"로 시작해 그럴듯해 보입니다. 하지만 오픈 웨이트 모델만 지원한다는 제한은 사실이 아닙니다.

</details>

### 4. Who governs MCP today?

> 변동 정보: AAIF 기증 시점과 관리 주체는 연혁과 조직 정보입니다. 바뀔 수 있으므로 최신 공식 문서로 확인합니다.

A. Anthropic alone, as a proprietary protocol\
B. The Agentic AI Foundation (AAIF), part of the Linux Foundation, after Anthropic donated it in December 2025\
C. The W3C\
D. OpenAI, via the AGENTS.md working group

**한국어**

현재 MCP를 관리하는 곳은 어디입니까?

A. 독점 프로토콜로서 Anthropic 단독\
B. Anthropic이 2025년 12월에 기증한 뒤로는 Linux Foundation 소속 Agentic AI Foundation(AAIF)\
C. W3C\
D. AGENTS.md 워킹 그룹을 통한 OpenAI

<details><summary>정답</summary>

B. Anthropic introduced MCP in November 2024 and donated it to the Linux Foundation's newly formed Agentic AI Foundation in December 2025.

B. Anthropic은 2024년 11월 MCP를 공개했습니다. 2025년 12월에는 Linux Foundation이 새로 만든 Agentic AI Foundation에 기증했습니다.

</details>

<details><summary>자세한 설명</summary>

연습 문제 18번과 같은 내용을 묻는 문제입니다. MCP가 지금 AAIF의 프로젝트라는 점을 알면 풀립니다.

brief도 MCP를 Linux Foundation 산하 Agentic AI Foundation의 프로젝트로 적습니다. 그래서 B가 정답입니다.

A는 기증 이전의 상황입니다. C의 W3C는 MCP를 관리하지 않습니다. D는 AGENTS.md라는 다른 이름을 끌어온 함정입니다. OpenAI는 MCP를 관리하지 않습니다.

연도와 달도 알아 둡니다. Anthropic은 2024-11-25에 MCP를 공개했습니다. 2025-12-09에는 Linux Foundation 산하에 새로 생긴 AAIF에 MCP를 기증한다고 발표했습니다. AAIF는 Anthropic, Block, OpenAI가 함께 세웠습니다. 이때 MCP와 함께 goose와 AGENTS.md도 AAIF 프로젝트가 됐습니다. 그래서 D의 AGENTS.md는 MCP와 같은 재단의 다른 프로젝트입니다.

</details>

### 5. Which pair correctly matches MCP primitives to the side that provides them?

A. Tools and prompts are client primitives; elicitation is a server primitive\
B. Tools, resources, and prompts are server primitives; elicitation is a client primitive\
C. Resources are a transport; tools are a message format\
D. All primitives are provided by the host application

**한국어**

MCP primitive와 그것을 제공하는 쪽을 바르게 짝지은 것은 무엇입니까?

A. Tools와 prompts는 client primitive, elicitation은 server primitive\
B. Tools, resources, prompts는 server primitive, elicitation은 client primitive\
C. Resources는 transport, tools는 메시지 형식\
D. 모든 primitive를 host 애플리케이션이 제공

<details><summary>정답</summary>

B. Servers expose tools, resources, and prompts; clients provide capabilities such as elicitation (and, in earlier spec revisions, sampling and roots).

B. server는 tools, resources, prompts를 노출합니다. client는 elicitation 같은 capability를 제공합니다. 이전 스펙 개정에서는 client가 sampling과 roots도 제공했습니다.

</details>

<details><summary>자세한 설명</summary>

primitive를 제공하는 쪽을 묻는 문제입니다. server는 tools, resources, prompts를 노출하고 client는 elicitation 같은 기능을 제공한다는 점을 알면 풀립니다.

B가 이 짝을 정확히 말합니다. 그래서 B가 정답입니다.

A는 server와 client를 거꾸로 짝지었습니다. C는 primitive를 transport나 메시지 형식과 혼동했습니다. transport는 stdio와 Streamable HTTP이고 메시지 형식은 JSON-RPC 2.0입니다. D도 틀립니다. tools, resources, prompts는 server가 제공합니다.

원 해설은 sampling과 roots를 이전 개정의 기능처럼 적었습니다. 2026-07-28 스펙 본문 기준으로는 sampling과 roots가 2026-07-28에서 폐기 예정이 됐을 뿐 아직 스펙에 남아 있고 동작합니다. 2027-07-28 이후에 나오는 개정부터 없어질 수 있습니다. "sampling과 roots는 삭제됐다"는 설명은 틀립니다.

</details>

## Core MCP Concepts

원본: `fundamentals/core-concepts.mdx`

### 1. Which participant maintains a 1:1 relationship with exactly one server?

A. The host\
B. The client\
C. The LLM\
D. The registry

**한국어**

정확히 server 하나와 1:1 관계를 유지하는 참여자는 무엇입니까?

A. host\
B. client\
C. LLM\
D. registry

<details><summary>정답</summary>

B. Each client connects to exactly one server. The host manages many clients, one per server connection.

B. 각 client는 정확히 server 하나에 연결합니다. host는 server 연결마다 client를 하나씩 두고 여러 client를 관리합니다.

</details>

<details><summary>자세한 설명</summary>

연습 문제 4번을 다른 쪽에서 묻는 문제입니다. server와 1:1인 쪽은 client라는 점을 알면 풀립니다.

host가 server 둘에 연결하면 client도 둘이 생깁니다. 각 client는 자기 server 하나만 상대합니다. 그래서 B가 정답입니다.

A의 host는 client 여럿을 관리하므로 server와 1:N 관계입니다. C의 LLM은 server와 직접 연결하지 않습니다. D의 registry는 server 메타데이터를 모아 두는 곳이고 연결 관계에 끼지 않습니다.

</details>

### 2. Who controls when a Tool is invoked?

A. The user, by clicking it in the UI\
B. The host application\
C. The model (LLM), with human-in-the-loop approval\
D. The server

**한국어**

Tool이 언제 호출될지 누가 제어합니까?

A. UI에서 클릭하는 사용자\
B. host 애플리케이션\
C. 사람의 승인을 거치는 모델(LLM)\
D. server

<details><summary>정답</summary>

C. Tools are model-controlled: the LLM discovers and invokes them, though a human should always be able to deny an invocation.

C. Tools는 model-controlled입니다. LLM이 tool을 찾아서 호출합니다. 다만 사람이 언제든 호출을 거부할 수 있어야 합니다.

</details>

<details><summary>자세한 설명</summary>

연습 문제 2번과 같은 제어 모델을 묻는 문제입니다. tool의 호출 시점은 모델이 정한다는 점을 알면 풀립니다.

모델은 대화를 보고 어떤 tool을 부를지 정합니다. 사람은 그 호출을 승인하거나 거부합니다. 거부할 수 있다고 해서 호출 시점을 사람이 정하는 것은 아닙니다. 그래서 C가 정답입니다.

A는 prompts의 제어 방식에 가깝습니다. prompt는 사용자가 직접 고릅니다. B는 resources의 제어 방식입니다. host 애플리케이션이 어떤 resource를 맥락에 넣을지 정합니다. D의 server는 tool을 제공할 뿐 호출 시점을 정하지 않습니다.

스펙은 사람이 호출을 거부할 수 있어야 한다고 권합니다(SHOULD). "사람이 모든 tool 호출을 반드시(MUST) 승인해야 한다"는 설명은 스펙보다 표현이 강합니다.

</details>

### 3. Which server-side primitive is application-controlled?

A. Tools\
B. Prompts\
C. Resources\
D. Elicitation

**한국어**

application-controlled인 server 쪽 primitive는 무엇입니까?

A. Tools\
B. Prompts\
C. Resources\
D. Elicitation

<details><summary>정답</summary>

C. Resources are application-driven: the host application decides how to incorporate them (UI pickers, search, automatic inclusion).

C. Resources는 애플리케이션이 주도합니다. host 애플리케이션이 resource를 어떻게 포함할지 정합니다. UI 선택기, 검색, 자동 포함 같은 방식이 있습니다.

</details>

<details><summary>자세한 설명</summary>

server primitive의 제어 모델을 묻는 문제입니다. 애플리케이션이 제어하는 primitive는 resources라는 점을 알면 풀립니다.

resource는 파일이나 데이터처럼 맥락으로 쓰는 내용입니다. 어떤 resource를 모델에 보여 줄지는 host가 정합니다. 사용자가 고르게 할 수도 있고 자동으로 넣을 수도 있습니다. 그래서 C가 정답입니다.

A의 tools는 model-controlled이고 B의 prompts는 user-controlled입니다. D의 elicitation은 server 쪽 primitive가 아니라 client 쪽 기능입니다.

원문은 "application-controlled"라고 쓰고 brief는 "application-driven"이라고 씁니다. 같은 뜻입니다. 어느 표현이든 답은 resources입니다.

</details>

### 4. Which client-side primitive lets a server request an LLM completion through the client?

A. Roots\
B. Elicitation\
C. Prompts\
D. Sampling

**한국어**

server가 client를 거쳐 LLM completion을 요청할 수 있게 하는 client 쪽 primitive는 무엇입니까?

A. Roots\
B. Elicitation\
C. Prompts\
D. Sampling

<details><summary>정답</summary>

D. Sampling lets a server request generation via the client's model access — no server-side API key needed. Note it was deprecated in the 2026-07-28 revision.

D. Sampling을 쓰면 server가 client의 모델 접근 권한으로 생성을 요청할 수 있습니다. server 쪽 API 키가 필요 없습니다. 참고로 Sampling은 2026-07-28 개정에서 폐기 예정(deprecated)이 됐습니다.

</details>

<details><summary>자세한 설명</summary>

연습 문제 9번과 같은 내용입니다. 모델 호출을 client에 맡기는 기능이 sampling이라는 점을 알면 풀립니다.

server는 `sampling/createMessage`로 모델 응답을 요청합니다. client는 자기 모델로 응답을 만들어 돌려줍니다. 그래서 D가 정답입니다.

A의 roots는 작업 범위가 되는 디렉터리를 알려 주는 기능입니다. B의 elicitation은 사용자에게 입력을 받는 기능입니다. C의 prompts는 server 쪽 primitive라 문제 조건에도 맞지 않습니다.

2026-07-28 기준으로 sampling은 폐기 예정이지만 아직 동작합니다. 새 구현은 LLM 제공자 API를 직접 부르라고 안내합니다. 요청 방식도 MRTR로 바뀌었습니다. server는 `input_required` 결과에 `sampling/createMessage`를 담아 돌려줍니다. client는 답을 `inputResponses`에 넣어 원래 요청을 다시 보냅니다.

</details>

### 5. What message format does MCP use at the data layer?

A. Protocol Buffers\
B. JSON-RPC 2.0\
C. GraphQL\
D. REST with JSON bodies

**한국어**

MCP는 데이터 계층에서 어떤 메시지 형식을 씁니까?

A. Protocol Buffers\
B. JSON-RPC 2.0\
C. GraphQL\
D. JSON 본문을 쓰는 REST

<details><summary>정답</summary>

B. All MCP messages are JSON-RPC 2.0: requests, responses, and notifications.

B. 모든 MCP 메시지는 JSON-RPC 2.0입니다. 요청, 응답, notification이 모두 그렇습니다.

</details>

<details><summary>자세한 설명</summary>

MCP의 메시지 형식을 묻는 문제입니다. 답은 JSON-RPC 2.0입니다.

요청에는 `jsonrpc`, `id`, `method`, `params`가 들어갑니다. 응답은 같은 `id`로 `result`나 `error`를 돌려줍니다. notification에는 `id`가 없고 받는 쪽은 응답하지 않습니다. 그래서 B가 정답입니다.

A의 Protocol Buffers와 C의 GraphQL은 MCP가 쓰지 않습니다. D는 Streamable HTTP가 HTTP를 쓰기 때문에 헷갈리는 보기입니다. HTTP는 메시지를 나르는 수단이고 본문은 JSON-RPC 메시지입니다. MCP는 REST처럼 자원마다 URL을 나누지 않고 엔드포인트 하나에 POST합니다.

세부 규칙도 알아 둡니다. 요청 `id`는 null이면 안 됩니다. JSON-RPC 배치는 2025-06-18에서 없어졌습니다. Streamable HTTP의 POST 하나에는 요청이나 notification이 정확히 하나만 들어갑니다. 2026-07-28에서는 결과에 `resultType`이 반드시 들어갑니다.

</details>

### 6. In the current (2026-07-28) spec revision, how does a client learn a server's capabilities?

A. Through the initialize handshake\
B. Via the server/discover request\
C. By reading the server's README\
D. Capabilities are no longer negotiated

**한국어**

현재(2026-07-28) 스펙 개정에서 client는 server의 capability를 어떻게 알아냅니까?

A. `initialize` handshake로\
B. `server/discover` 요청으로\
C. server의 README를 읽어서\
D. capability는 더 이상 협상하지 않습니다

<details><summary>정답</summary>

B. The 2026-07-28 revision removed the initialize handshake; servers advertise capabilities via server/discover, and clients send their own capabilities in per-request _meta fields.

B. 2026-07-28 개정은 `initialize` handshake를 없앴습니다. server는 `server/discover`로 capability를 알립니다. client는 요청마다 `_meta` 필드에 자기 capability를 담아 보냅니다.

</details>

<details><summary>자세한 설명</summary>

2026-07-28에서 가장 크게 바뀐 부분을 묻는 문제입니다. handshake가 없어지고 `server/discover`가 생겼다는 점을 알면 풀립니다.

server는 `server/discover`를 반드시 구현해야 합니다(MUST). 결과에는 지원 버전 목록, capability, server 정보가 들어 있습니다. client는 이 요청으로 server가 무엇을 지원하는지 알아냅니다. 그래서 B가 정답입니다.

A의 `initialize` handshake는 2025-11-25 이전의 방식이고 2026-07-28에서 없어졌습니다. C처럼 README를 읽는 것은 프로토콜 동작이 아닙니다. D는 그럴듯한 함정입니다. 연결 단위의 협상은 없어졌지만 capability는 여전히 주고받습니다. client는 요청마다 `io.modelcontextprotocol/clientCapabilities`를 보냅니다.

`server/discover`는 server에게는 필수이지만 client에게는 선택입니다. client는 바로 요청을 보내도 됩니다. `-32022`가 오면 `data.supported`에 있는 버전으로 다시 보냅니다. server가 필요한 capability를 client가 그 요청에 선언하지 않았다면 server는 `-32021`을 돌려줍니다. 옛 자료에서 본 "연결은 `initialize`로 시작한다"는 설명은 2026-07-28 기준으로 틀립니다.

</details>

## Interoperability & Value

원본: `fundamentals/interoperability-and-value.mdx`

### 1. What format do MCP protocol version identifiers use?

A. Semantic versioning (e.g. 1.2.0)\
B. Date-based strings (e.g. 2026-07-28)\
C. Incrementing integers (e.g. v3)\
D. Git commit hashes

**한국어**

MCP 프로토콜 버전 식별자는 어떤 형식입니까?

A. 시맨틱 버저닝(예: 1.2.0)\
B. 날짜 기반 문자열(예: 2026-07-28)\
C. 증가하는 정수(예: v3)\
D. Git 커밋 해시

<details><summary>정답</summary>

B. Versions are YYYY-MM-DD date strings, incremented only when a backwards-incompatible change is made.

B. 버전은 YYYY-MM-DD 형식의 날짜 문자열입니다. 하위 호환이 되지 않는 변경이 있을 때만 새 버전이 나옵니다.

</details>

<details><summary>자세한 설명</summary>

MCP가 프로토콜 버전을 어떻게 표기하는지 묻는 문제입니다. 버전 이름이 날짜라는 것만 알면 풀립니다.

MCP 개정판의 식별자는 YYYY-MM-DD 형식의 날짜입니다. 이 날짜는 하위 호환이 되지 않는 변경을 마지막으로 넣은 날을 뜻합니다. 지금 Current 개정인 2026-07-28도 이런 날짜 문자열입니다. 그래서 B가 정답입니다.

A의 시맨틱 버저닝은 1.2.0처럼 주 버전, 부 버전, 패치 번호로 나눕니다. MCP는 이 방식을 쓰지 않습니다. C의 정수 번호와 D의 커밋 해시도 MCP가 쓰는 방식이 아닙니다. 그래서 A, C, D는 답이 아닙니다.

개정판의 상태도 함께 알아 둡니다. 개정판은 Draft, Current, Final 중 하나입니다. Draft는 작업 중인 판입니다. Final은 확정되어 더 바뀌지 않는 판입니다. Current는 지금 쓰는 판이며 하위 호환을 지키는 변경은 계속 들어갈 수 있습니다. MCP Registry의 `server.json` 스키마 버전이 프로토콜 버전과 따로 정해진다는 점도 헷갈리기 쉽습니다.

</details>

### 2. In the 2026-07-28 revision, how does a client communicate its protocol version and capabilities?

A. Once per connection via the initialize handshake\
B. Through an out-of-band configuration file\
C. As _meta metadata attached to every request\
D. It never does; only servers declare capabilities

**한국어**

2026-07-28 개정에서 client는 자신의 프로토콜 버전과 capability를 어떻게 전달합니까?

A. 연결마다 한 번, `initialize` handshake로\
B. 대역 밖(out-of-band) 설정 파일로\
C. 모든 요청에 붙는 `_meta` 메타데이터로\
D. 전달하지 않습니다. capability는 server만 선언합니다

<details><summary>정답</summary>

C. MCP became stateless: there is no initialize handshake, so version, client info, and capabilities travel in _meta on each request.

C. MCP는 상태를 두지 않는(stateless) 방식이 되었습니다. `initialize` handshake가 없으므로 버전, client 정보, capability는 요청마다 `_meta`에 담겨 전달됩니다.

</details>

<details><summary>자세한 설명</summary>

2026-07-28 개정에서 client 정보가 어디에 실리는지 묻는 문제입니다. 이 개정이 handshake를 없애고 요청마다 정보를 넣는 방식으로 바뀌었다는 것을 알면 풀립니다.

2026-07-28부터 server는 같은 연결의 이전 요청에서 버전이나 capability를 알아내면 안 됩니다(MUST NOT). 대신 client는 모든 요청의 `params._meta`에 `io.modelcontextprotocol/protocolVersion`과 `io.modelcontextprotocol/clientCapabilities`를 넣습니다. 이 둘은 필수입니다. `io.modelcontextprotocol/clientInfo`는 넣는 것이 권장(SHOULD)됩니다. 그래서 C가 정답입니다.

A의 `initialize` handshake는 2025-11-25까지 쓰던 방식입니다. 2026-07-28에서 제거되었으므로 A는 답이 아닙니다. B처럼 설정 파일로 따로 전달하는 방식은 스펙에 없습니다. D도 틀렸습니다. client capability는 요청마다 선언합니다. server는 client가 그 요청에 선언하지 않은 capability에 기대면 안 됩니다.

필수 `_meta`가 빠진 요청은 형식이 잘못된 요청으로 처리되어 `-32602` 오류를 받습니다. HTTP에서는 `400 Bad Request`가 함께 옵니다. server에 필요한 capability를 client가 선언하지 않았다면 server는 `-32021`을 돌려줍니다. `clientInfo`는 client가 스스로 적은 값입니다. 그래서 표시와 로그에만 쓰고 보안 판단에는 쓰지 않습니다.

</details>

### 3. What happens when a server does not support the protocol version a request declares?

A. It silently processes the request with its own version\
B. It returns UnsupportedProtocolVersionError (-32022) with the versions it supports\
C. It closes the transport permanently\
D. It downgrades the client automatically

**한국어**

요청에 선언된 프로토콜 버전을 server가 지원하지 않으면 어떻게 됩니까?

A. 자기 버전으로 요청을 조용히 처리합니다\
B. 지원하는 버전 목록과 함께 `UnsupportedProtocolVersionError`(`-32022`)를 돌려줍니다\
C. transport를 영구히 닫습니다\
D. client를 자동으로 다운그레이드합니다

<details><summary>정답</summary>

B. The server rejects the request with JSON-RPC error -32022, including data.supported; the client should retry with a mutually supported version.

B. server는 `data.supported`를 담은 JSON-RPC 오류 `-32022`로 요청을 거부합니다. client는 양쪽이 모두 지원하는 버전으로 다시 시도해야 합니다.

</details>

<details><summary>자세한 설명</summary>

버전이 맞지 않을 때 server가 어떻게 응답하는지 묻는 문제입니다. `-32022` 오류와 그 안의 `data.supported` 필드를 알면 풀립니다.

server는 요청이 선언한 버전을 지원하지 않으면 `UnsupportedProtocolVersionError`로 거부합니다. 오류 코드는 `-32022`입니다. 오류의 `data`에는 server가 지원하는 버전 목록인 `supported`가 들어갑니다. client가 요청한 버전인 `requested`도 들어갑니다. client는 이 목록에서 자기도 지원하는 버전을 선택해 다시 요청합니다. 그래서 B가 정답입니다.

A처럼 조용히 처리하면 client는 버전이 맞지 않는다는 것을 알 수 없습니다. C처럼 transport를 닫을 이유도 없습니다. 오류 하나로 알리면 충분합니다. D의 자동 다운그레이드도 없습니다. 어느 버전으로 다시 보낼지는 client가 정합니다.

이 오류는 버전 협상 방식과 이어집니다. client는 `server/discover`를 먼저 부르지 않아도 됩니다. 아무 요청이나 보낸 뒤 `-32022`를 받으면 `data.supported`의 버전으로 다시 보내는 방식도 허용됩니다. HTTP에서는 이 오류가 `400` 상태 코드와 함께 옵니다. 이전 방식인 `initialize`로 돌아갈지 판단하는 부분도 주의합니다. stdio에서 `server/discover`를 보냈는데 `-32022`가 오면 그 server는 새 방식을 사용하는 server입니다. 이때는 `initialize`로 돌아가지 않고 지원하는 버전으로 다시 보냅니다.

</details>

### 4. What is the primary source of MCP's interoperability?

A. A single shared reference implementation everyone must use\
B. A standardized protocol that any implementation can follow\
C. A mandatory certification program for servers\
D. All servers being written in TypeScript

**한국어**

MCP 상호운용성은 주로 어디에서 나옵니까?

A. 모두가 써야 하는 공통 레퍼런스 구현 하나\
B. 어떤 구현이든 따를 수 있는 표준 프로토콜\
C. server 대상 의무 인증 프로그램\
D. 모든 server를 TypeScript로 작성하는 것

<details><summary>정답</summary>

B. The protocol is what is standardized, not the implementation — compliant hosts and servers interoperate regardless of language or SDK.

B. 표준으로 정한 것은 구현이 아니라 프로토콜입니다. 스펙을 따르는 host와 server는 언어나 SDK와 상관없이 함께 동작합니다.

</details>

<details><summary>자세한 설명</summary>

MCP에서 서로 다른 구현이 함께 동작할 수 있는 이유를 묻는 문제입니다. MCP가 정하는 것은 메시지 형식과 규칙이라는 점을 알면 풀립니다.

MCP 스펙은 JSON-RPC 2.0 위에서 어떤 메시지를 어떤 형식으로 주고받는지 정합니다. 메시지 형식의 기준은 스펙 저장소의 스키마 파일(`schema.ts`)입니다. 이 형식을 지키면 어떤 언어로 만들었든 서로 통신할 수 있습니다. 그래서 B가 정답입니다.

A는 틀렸습니다. 공식 SDK가 여러 언어로 나와 있지만 반드시 써야 하는 구현은 없습니다. C의 의무 인증 프로그램도 없습니다. D도 틀렸습니다. 스키마 파일이 TypeScript로 적혀 있을 뿐 server를 TypeScript로 만들 필요는 없습니다. 2026-07-28은 TypeScript, Python, Go, C# SDK가 지원합니다.

이 내용은 SDK 등급과도 연결됩니다. SDK는 conformance test(스펙 적합성 시험) 통과율로 등급이 나뉩니다. Tier 1은 100%를 통과해야 하고 Tier 2는 80%를 통과해야 합니다. Tier 3은 실험 단계입니다. 등급은 SDK가 스펙을 얼마나 지키는지 보여 줍니다. 스펙을 따르는 구현은 누구나 만들 수 있습니다.

</details>

### 5. What value does MCP offer a tool/API provider compared to building per-app plugins?

A. It removes the need for any authentication\
B. One MCP server integration reaches every MCP-enabled application\
C. It guarantees a listing in every app marketplace\
D. It makes the API free to operate

**한국어**

앱마다 플러그인을 만드는 것과 비교하면 MCP는 tool/API 제공자에게 어떤 가치를 줍니까?

A. 인증이 전혀 필요 없어집니다\
B. MCP server 통합 하나를 MCP를 지원하는 모든 애플리케이션에서 쓸 수 있습니다\
C. 모든 앱 마켓플레이스 등재를 보장합니다\
D. API를 무료로 운영할 수 있게 됩니다

<details><summary>정답</summary>

B. Build once, integrate everywhere: a single compliant server works with any compliant host, replacing N per-app integrations.

B. 한 번 만들면 어디서나 통합됩니다. 스펙을 따르는 server 하나가 스펙을 따르는 어떤 host와도 동작하므로 앱별 통합 N개를 대신합니다.

</details>

<details><summary>자세한 설명</summary>

MCP가 tool이나 API를 제공하는 쪽에 주는 이점을 묻는 문제입니다. 통합을 한 번만 만들면 된다는 점을 알면 풀립니다.

MCP가 없으면 제공자는 AI 애플리케이션마다 플러그인을 따로 만들어야 합니다. 애플리케이션이 N개면 통합도 N개가 필요합니다. MCP server를 하나 만들면 MCP를 지원하는 host 어디에서나 그 server를 쓸 수 있습니다. 그래서 B가 정답입니다.

A는 틀렸습니다. MCP를 써도 인증은 필요합니다. HTTP transport는 OAuth 2.1 기반 인가를 따르는 것이 권장(SHOULD)됩니다. stdio server는 환경 변수에서 자격 증명을 읽습니다. C와 D는 MCP가 정하는 범위 밖입니다. 마켓플레이스 등재나 운영 비용은 프로토콜과 상관이 없습니다.

같은 이점은 host 쪽에도 있습니다. host도 MCP를 한 번 지원하면 스펙을 따르는 여러 server를 붙일 수 있습니다. MCP Registry를 등재 보장으로 착각하기도 쉽습니다. Registry는 server 메타데이터(`server.json`)만 담는 미리보기(preview) 서비스입니다. 공개 server만 받습니다.

</details>

## Schemas & Structured Data

원본: `architecture/schemas-and-structured-data.mdx`

### 1. What field distinguishes a JSON-RPC request from a notification?

A. The method field\
B. The id field\
C. The params field\
D. The jsonrpc field

**한국어**

JSON-RPC 요청과 notification을 구분하는 필드는 무엇입니까?

A. `method` 필드\
B. `id` 필드\
C. `params` 필드\
D. `jsonrpc` 필드

<details><summary>정답</summary>

B. Requests carry an id and expect a correlated response; notifications have no id and never receive a response.

B. 요청에는 `id`가 있습니다. 요청을 보낸 쪽은 같은 `id`가 붙은 응답을 기다립니다. notification에는 `id`가 없으며 응답도 오지 않습니다.

</details>

<details><summary>자세한 설명</summary>

JSON-RPC 메시지 종류를 구분하는 방법을 묻는 문제입니다. `id` 필드가 있는지 보면 풀립니다.

MCP 메시지는 JSON-RPC 2.0을 따릅니다. 요청에는 반드시 `id`가 있습니다. `id`는 문자열이나 정수이며 `null`이면 안 됩니다. 응답은 같은 `id`를 달고 돌아오므로 어느 요청의 응답인지 알 수 있습니다. notification에는 `id`를 넣으면 안 됩니다(MUST NOT). 받는 쪽도 notification에 응답하면 안 됩니다. 그래서 B가 정답입니다.

A의 `method`는 요청과 notification 모두에 있습니다. C의 `params`도 둘 다 가질 수 있습니다. D의 `jsonrpc`는 모든 메시지에 `"2.0"`으로 들어갑니다. 셋 다 둘을 구분하는 기준이 아니므로 답이 아닙니다.

`id`에는 규칙이 더 있습니다. 아직 응답을 기다리는 요청의 `id`를 다시 쓰면 안 됩니다. MRTR로 다시 보내는 요청도 새 `id`를 써야 합니다. JSON-RPC batching(여러 메시지를 배열 하나로 묶어 보내는 방식)은 2025-06-18에 제거되었습니다. 그래서 Streamable HTTP의 POST 본문 하나에는 요청이나 notification이 정확히 하나만 들어갑니다.

</details>

### 2. Which tool definition field is required and describes valid arguments?

A. outputSchema\
B. annotations\
C. inputSchema\
D. icons

**한국어**

tool 정의에서 필수이며 유효한 인자를 기술하는 필드는 무엇입니까?

A. `outputSchema`\
B. `annotations`\
C. `inputSchema`\
D. `icons`

<details><summary>정답</summary>

C. Every tool must declare an inputSchema (a JSON Schema object); outputSchema, annotations, and icons are optional.

C. 모든 tool은 `inputSchema`(JSON Schema 객체)를 선언해야 합니다. `outputSchema`, `annotations`, `icons`는 선택 사항입니다.

</details>

<details><summary>자세한 설명</summary>

tool 정의에서 꼭 있어야 하는 필드를 묻는 문제입니다. 인자를 설명하는 스키마가 `inputSchema`라는 것을 알면 풀립니다.

`inputSchema`는 tool이 받는 인자를 JSON Schema 객체로 기술합니다. 이 값은 `null`이면 안 됩니다. 인자가 없는 tool도 `{"type":"object","additionalProperties":false}`처럼 빈 객체 스키마를 넣습니다. 그래서 C가 정답입니다.

A의 `outputSchema`는 tool 결과의 형식을 정합니다. 이 필드가 있으면 결과의 `structuredContent`가 이 스키마를 따라야 합니다. 다만 `outputSchema` 자체는 넣지 않아도 됩니다. B의 `annotations`는 tool의 성격을 알려 주는 힌트이며 선택 필드입니다. D의 `icons`도 표시용 선택 필드입니다.

`inputSchema`를 어긴 인자는 프로토콜 오류 `-32602`가 아닙니다. `isError: true`가 붙은 tool 실행 오류로 돌려줍니다. 모델이 이 메시지를 보고 인자를 고쳐 다시 시도할 수 있기 때문입니다. 반면 존재하지 않는 tool 이름은 `-32602`입니다.

</details>

### 3. What JSON Schema dialect do MCP tool schemas use by default?

A. Draft-04\
B. Draft-07\
C. 2019-09\
D. 2020-12

**한국어**

MCP tool 스키마는 기본으로 어떤 JSON Schema 방언(dialect)을 씁니까?

A. Draft-04\
B. Draft-07\
C. 2019-09\
D. 2020-12

<details><summary>정답</summary>

D. MCP tool schemas default to the JSON Schema 2020-12 dialect.

D. MCP tool 스키마의 기본 방언은 JSON Schema 2020-12입니다.

</details>

<details><summary>자세한 설명</summary>

tool 스키마를 해석할 때 기준이 되는 JSON Schema 판을 묻는 문제입니다. 2025-11-25 개정에서 기본 방언이 2020-12로 정해졌다는 것을 알면 풀립니다.

JSON Schema에는 Draft-04, Draft-07, 2019-09, 2020-12 같은 여러 판이 있습니다. 판마다 쓸 수 있는 키워드와 해석이 조금씩 다릅니다. 이 판을 방언이라고 부릅니다. MCP는 2025-11-25 개정부터 tool 스키마의 기본 방언을 2020-12로 정했습니다. 그래서 D가 정답입니다.

Draft-04, Draft-07, 2019-09는 모두 실제로 있는 판입니다. 하지만 MCP의 기본값은 아닙니다. 그래서 A, B, C는 답이 아닙니다.

JSON Schema 보안도 함께 봅니다. 네트워크 주소를 가리키는 `$ref`를 자동으로 따라가면 안 됩니다. 따라가려면 따로 켜야 하며 허용한 호스트만 받아야 합니다. 조합 키워드에는 한도를 두어 서비스 거부 공격을 막습니다.

</details>

### 4. How are MCP resources uniquely identified?

A. By a numeric index\
B. By a URI (RFC 3986)\
C. By their mimeType\
D. By a UUID assigned by the client

**한국어**

MCP resource는 무엇으로 고유하게 식별됩니까?

A. 숫자 인덱스\
B. URI(RFC 3986)\
C. `mimeType`\
D. client가 부여하는 UUID

<details><summary>정답</summary>

B. Each resource has a unique URI following RFC 3986; custom schemes are allowed.

B. 각 resource에는 RFC 3986을 따르는 고유 URI가 있습니다. 사용자 정의 스킴도 쓸 수 있습니다.

</details>

<details><summary>자세한 설명</summary>

resource를 가리키는 식별자가 무엇인지 묻는 문제입니다. resource는 URI로 식별된다는 것만 알면 풀립니다.

server는 각 resource에 URI를 붙입니다. client는 `resources/read`를 보낼 때 이 URI를 넣어 원하는 resource를 지정합니다. URI 형식은 RFC 3986을 따릅니다. 그래서 B가 정답입니다.

A의 숫자 인덱스는 목록 순서가 바뀌면 다른 resource를 가리키게 됩니다. C의 `mimeType`은 내용의 형식을 알려 주는 값일 뿐입니다. 같은 `mimeType`을 가진 resource가 여럿 있을 수 있습니다. D도 틀렸습니다. URI는 server가 정하며 client가 부여하지 않습니다.

URI 스킴도 알아 둡니다. `https://`는 client가 그 주소를 직접 가져올 수 있을 때만 씁니다. `file://`은 실제 파일 시스템이 아니어도 됩니다. 다만 경로 조작 공격에 대비해 경로를 검사해야 합니다. `git://`도 쓸 수 있고 RFC 3986을 따르는 사용자 정의 스킴도 쓸 수 있습니다. 여러 resource를 한 형식으로 표현할 때는 RFC 6570 URI 템플릿을 `resources/templates/list`로 제공합니다. 없는 URI를 읽으면 2026-07-28에서는 `-32602`가 옵니다. 옛 코드 `-32002`와 헷갈리지 않아야 합니다.

</details>

### 5. How should a client treat tool annotations like readOnlyHint?

A. As guaranteed by the protocol\
B. As untrusted unless the server is trusted\
C. As enforced by the model\
D. As mandatory validation rules

**한국어**

client는 `readOnlyHint` 같은 tool annotation을 어떻게 다뤄야 합니까?

A. 프로토콜이 보장하는 값으로\
B. server를 신뢰하지 않는 한 신뢰할 수 없는 값으로\
C. 모델이 적용하는 값으로\
D. 필수 검증 규칙으로

<details><summary>정답</summary>

B. Annotations are hints; clients must treat them as untrusted unless they come from a trusted server.

B. annotation은 힌트입니다. 신뢰하는 server에서 온 것이 아니면 client는 annotation을 신뢰할 수 없는 값으로 다뤄야 합니다.

</details>

<details><summary>자세한 설명</summary>

tool annotation을 얼마나 믿어도 되는지 묻는 문제입니다. annotation은 보안 통제가 아니라 힌트라는 것을 알면 풀립니다.

annotation은 server가 tool의 성격을 스스로 적어 둔 값입니다. `readOnlyHint`는 읽기만 하는 tool인지를 알립니다. `destructiveHint`는 파괴적인 변경을 할 수 있는지를 알립니다. 스펙은 신뢰하는 server가 아니면 이 값을 신뢰하지 말라고(MUST) 정합니다. 악의적인 server는 삭제 tool에 `readOnlyHint: true`를 붙일 수도 있습니다. 그래서 B가 정답입니다.

A는 틀렸습니다. 프로토콜은 annotation 값이 사실인지 검사하지 않습니다. C도 틀렸습니다. 모델은 annotation을 적용하지 않습니다. tool 호출을 막거나 허용하는 일은 host가 합니다. D도 틀렸습니다. annotation은 넣지 않아도 되는 선택 필드입니다.

annotation의 기본값도 정리해 둡니다. `readOnlyHint`의 기본값은 false입니다. `destructiveHint`의 기본값은 true이며 읽기 전용이 아닐 때만 의미가 있습니다. `idempotentHint`의 기본값은 false입니다. `openWorldHint`의 기본값은 true입니다. 값을 적지 않으면 조심스러운 쪽으로 가정하도록 기본값이 잡혀 있습니다.

</details>

## Hosts, Clients & Servers

원본: `architecture/hosts-clients-servers.mdx`

### 1. Which participant enforces user consent and security policy?

A. The server\
B. The client\
C. The host\
D. The transport layer

**한국어**

사용자 동의와 보안 정책을 적용하는 참여자는 누구입니까?

A. server\
B. client\
C. host\
D. transport 계층

<details><summary>정답</summary>

C. The host is the AI application that enforces security policies, permissions, and user consent. Servers never present consent UIs.

C. host는 보안 정책, 권한, 사용자 동의를 적용하는 AI 애플리케이션입니다. server는 동의 UI를 띄우지 않습니다.

</details>

<details><summary>자세한 설명</summary>

MCP 참여자 가운데 사용자 동의를 책임지는 쪽을 묻는 문제입니다. host, client, server의 역할 차이를 알면 풀립니다.

host는 사용자가 직접 쓰는 AI 애플리케이션입니다. host는 사용자와 모델 사이에서 어떤 tool을 언제 호출할지 결정합니다. 그래서 동의를 받고 보안 정책을 적용하는 일도 host가 담당합니다. C가 정답입니다.

client는 host 안에서 server 하나와의 연결을 담당하는 구성 요소입니다. 메시지를 주고받을 뿐 정책을 결정하지 않으므로 B는 답이 아닙니다. server는 tool, resource, prompt를 제공하는 쪽입니다. server는 사용자 화면에 직접 접근하지 않으므로 A도 답이 아닙니다. D의 transport 계층은 메시지를 나르기만 합니다.

스펙 본문 기준으로 보충할 점이 있습니다. URL 모드 elicitation을 쓰면 server가 OAuth 동의 화면 같은 외부 페이지로 사용자를 안내할 수 있습니다. 원 해설의 "server는 동의 UI를 띄우지 않는다"는 tool 호출 동의를 두고 한 말로 읽으면 됩니다. "server가 tool 호출 승인 창을 띄운다"는 설명은 틀립니다.

</details>

### 2. What is the relationship between a client and a server?

A. One client can connect to many servers\
B. Each client has a 1:1 connection with exactly one server\
C. Servers connect to multiple clients within one host\
D. Clients and servers share a single global connection

**한국어**

client와 server는 어떤 관계입니까?

A. client 하나가 여러 server에 연결할 수 있습니다\
B. 각 client는 정확히 server 하나와 1:1로 연결됩니다\
C. server가 host 하나 안의 여러 client에 연결합니다\
D. client와 server가 전역 연결 하나를 공유합니다

<details><summary>정답</summary>

B. Each client maintains a dedicated 1:1 relationship with one server; the host creates one client per server it connects to.

B. 각 client는 server 하나와 전용 1:1 관계를 유지합니다. host는 연결하는 server마다 client를 하나씩 만듭니다.

</details>

<details><summary>자세한 설명</summary>

MCP 구조에서 host, client, server의 개수 관계를 묻는 문제입니다. client와 server가 1:1로 짝을 이룬다는 것을 알면 풀립니다.

host는 server 여러 개에 연결할 수 있습니다. 다만 그 연결마다 client를 하나씩 따로 만듭니다. 그래서 client 하나는 언제나 server 하나만 상대합니다. B가 정답입니다.

A는 host에 맞는 설명을 client에 붙인 함정입니다. 여러 server에 연결하는 것은 client가 아니라 host입니다. C는 방향이 뒤집혀 있습니다. server가 client를 찾아 연결하지 않습니다. D처럼 전역 연결 하나를 공유하면 server끼리 격리할 수 없습니다.

예를 들어 server 셋에 연결한 host 안에는 client도 셋 있습니다.

</details>

### 3. Which transport would a locally installed filesystem server typically use?

A. Streamable HTTP\
B. WebSockets\
C. stdio\
D. gRPC

**한국어**

로컬에 설치한 파일 시스템 server는 보통 어떤 transport를 씁니까?

A. Streamable HTTP\
B. WebSockets\
C. stdio\
D. gRPC

<details><summary>정답</summary>

C. Local servers run as subprocesses of the client and exchange newline-delimited JSON-RPC messages over stdin/stdout.

C. 로컬 server는 client의 하위 프로세스로 실행됩니다. stdin과 stdout으로 줄바꿈으로 구분한 JSON-RPC 메시지를 주고받습니다.

</details>

<details><summary>자세한 설명</summary>

로컬 server에 맞는 transport를 묻는 문제입니다. 같은 컴퓨터에서는 stdio를 쓴다는 것을 알면 풀립니다.

stdio transport에서는 client가 server를 하위 프로세스로 띄웁니다. client는 server의 표준 입력(stdin)에 메시지를 씁니다. server는 표준 출력(stdout)에 메시지를 씁니다. 메시지 하나가 한 줄이며 메시지 안에 줄바꿈이 들어가면 안 됩니다. 로컬 파일 시스템 server는 이 방식에 잘 맞습니다. 그래서 C가 정답입니다.

A의 Streamable HTTP는 주로 원격 server에 씁니다. 로컬에서 쓸 때는 `127.0.0.1`에만 열어 두라고 스펙이 안내합니다. 하지만 이 문제가 묻는 일반적인 경우는 아닙니다. B의 WebSockets와 D의 gRPC는 MCP 표준 transport가 아닙니다.

stdio의 종료 절차와 인가도 알아 둡니다. client는 server를 끝낼 때 stdin을 닫고 기다립니다. 그래도 끝나지 않으면 SIGTERM을 보내고 그다음 SIGKILL을 보냅니다. stdio server는 OAuth 흐름을 쓰지 않아야 합니다(SHOULD NOT). 대신 환경 변수에서 자격 증명을 읽습니다. 2026-07-28에서는 stdio 프로세스 하나가 세션이 아니라는 점도 주의합니다.

</details>

### 4. On the stdio transport, where should a server write its log output?

A. stdout\
B. stderr\
C. A log file required by the spec\
D. It must not log at all

**한국어**

stdio transport에서 server는 로그 출력을 어디에 써야 합니까?

A. stdout\
B. stderr\
C. 스펙이 요구하는 로그 파일\
D. 로그를 아예 남기면 안 됩니다

<details><summary>정답</summary>

B. stdout is reserved exclusively for valid MCP messages; the server may write UTF-8 log output to stderr.

B. stdout은 유효한 MCP 메시지만 쓰는 곳입니다. server는 UTF-8 로그 출력을 stderr에 쓸 수 있습니다.

</details>

<details><summary>자세한 설명</summary>

stdio에서 표준 출력과 표준 오류를 어떻게 나눠 쓰는지 묻는 문제입니다. stdout은 MCP 메시지 전용이라는 것을 알면 풀립니다.

stdio에서 client는 server의 stdout을 읽어 JSON-RPC 메시지로 해석합니다. 여기에 로그 문장이 섞이면 client가 메시지를 해석하지 못합니다. 그래서 stdout에는 MCP 메시지만 써야 합니다. 로그는 stderr에 씁니다. stderr에 쓴 내용이 꼭 오류를 뜻하지는 않습니다. 그래서 B가 정답입니다.

A는 위 이유로 틀렸습니다. C처럼 스펙이 정한 로그 파일은 없습니다. D도 틀렸습니다. 스펙은 로그를 남기는 것 자체를 막지 않습니다.

이 내용은 Logging 기능의 폐기와도 이어집니다. 2026-07-28에서 Logging 기능은 폐기 예정(deprecated)이 되었습니다. `logging/setLevel` 메서드는 제거되었습니다. 스펙은 대신 stdio에서는 stderr에 쓰고 그 밖에서는 OpenTelemetry를 쓰라고 안내합니다. 요청의 `_meta`에 `io.modelcontextprotocol/logLevel`을 넣은 경우에만 server가 그 요청의 응답 스트림으로 `notifications/message`를 보낼 수 있습니다. 로그에는 자격 증명, 비밀 값, 개인 정보를 남기면 안 됩니다.

</details>

### 5. Can a server read the host's full conversation history?

A. Yes, servers receive the whole conversation for context\
B. Only if the server declares a special capability\
C. No — the host isolates servers, which see only what is sent to them\
D. Only over the stdio transport

**한국어**

server가 host의 전체 대화 기록을 읽을 수 있습니까?

A. 예. server는 맥락 파악을 위해 대화 전체를 받습니다\
B. server가 특별한 capability를 선언한 경우에만 가능합니다\
C. 아니요. host가 server를 격리하므로 server는 자신에게 보낸 것만 봅니다\
D. stdio transport에서만 가능합니다

<details><summary>정답</summary>

C. Isolation is a core design principle: servers cannot read the whole conversation or see into other servers; the host enforces this boundary.

C. 격리는 MCP의 기본 설계 원칙입니다. server는 대화 전체를 읽을 수 없고 다른 server의 내용도 볼 수 없습니다. 이 경계는 host가 지킵니다.

</details>

<details><summary>자세한 설명</summary>

server가 볼 수 있는 정보의 범위를 묻는 문제입니다. host가 server를 서로 격리한다는 설계 원칙을 알면 풀립니다.

server는 client가 보낸 요청만 받습니다. 예를 들어 `tools/call`에는 tool 이름과 인자가 들어갈 뿐 사용자와 모델의 대화 전체는 포함되지 않습니다. 어떤 정보를 server에 넘길지는 host가 정합니다. 그래서 C가 정답입니다.

A는 틀렸습니다. 대화 전체를 server마다 보내면 민감한 정보가 퍼집니다. B도 틀렸습니다. 대화 기록을 읽게 해 주는 capability는 없습니다. D처럼 transport에 따라 달라지는 규칙도 아닙니다.

sampling의 `includeContext`도 함께 봅니다. 예전에는 sampling 요청에 `includeContext: "thisServer"`나 `"allServers"`를 넣어 server 쪽 맥락을 포함해 달라고 할 수 있었습니다. 이 두 값은 2025-11-25부터 폐기 예정입니다. 값을 빼거나 `"none"`을 쓰라고 안내합니다. Sampling 기능 자체도 2026-07-28에서 폐기 예정이 되었습니다.

</details>

## Model Interaction Flow

원본: `architecture/model-interaction-flow.mdx`

### 1. In a typical tool-using flow, when does user consent happen?

A. Before discovery, when the server is first listed\
B. After the model requests a tool but before the client calls the server\
C. After the server returns the tool result\
D. Consent is handled by the server during execution

**한국어**

일반적인 tool 사용 흐름에서 사용자 동의는 언제 이루어집니까?

A. 탐색 전, server가 처음 목록에 올라올 때\
B. 모델이 tool을 요청한 뒤, client가 server를 호출하기 전\
C. server가 tool 결과를 돌려준 뒤\
D. 동의는 실행 중에 server가 처리합니다

<details><summary>정답</summary>

B. The host checks policy or asks the user for approval after the model emits a tool-use request and before the client sends tools/call.

B. 모델이 tool 사용 요청을 낸 뒤, client가 `tools/call`을 보내기 전에 host가 정책을 확인하거나 사용자에게 승인을 받습니다.

</details>

<details><summary>자세한 설명</summary>

tool 호출 흐름에서 승인 단계가 어디에 들어가는지 묻는 문제입니다. 흐름의 순서를 알면 풀립니다.

흐름은 이렇습니다. host가 tool 목록을 모델에 넘깁니다. 모델이 어떤 tool을 쓰겠다는 요청을 냅니다. host는 정책을 확인하거나 사용자에게 승인을 묻습니다. 승인이 나면 client가 server에 `tools/call`을 보냅니다. 마지막으로 결과가 모델에 돌아갑니다. 승인은 호출 직전에 있으므로 B가 정답입니다.

A는 너무 이릅니다. 목록에 올라온 시점에는 어떤 인자로 무엇을 할지 아직 모릅니다. C는 너무 늦습니다. 결과가 돌아왔다면 tool은 이미 실행되었습니다. D도 틀렸습니다. 동의는 host가 받으며 server가 처리하지 않습니다.

스펙의 권장 문구도 정확히 알아 둡니다. 사람이 tool 호출을 거부할 수 있어야 합니다(SHOULD). client는 민감한 작업을 확인받고 호출 전에 입력값을 보여 주는 것이 권장(SHOULD)됩니다. OAuth 동의와도 구분해야 합니다. OAuth 동의는 server에 연결해 token을 받을 때 받습니다. 인가 확인은 tool을 호출할 때마다 합니다.

</details>

### 2. Who converts MCP tool definitions into the model's native tool-calling format?

A. The server\
B. The LLM itself\
C. The host\
D. The registry

**한국어**

MCP tool 정의를 모델 고유의 tool 호출 형식으로 바꾸는 것은 누구입니까?

A. server\
B. LLM 자체\
C. host\
D. registry

<details><summary>정답</summary>

C. The host assembles context and translates MCP tool definitions into whatever tool-call format the specific LLM expects.

C. host가 맥락을 조립합니다. 그리고 MCP tool 정의를 해당 LLM이 기대하는 tool 호출 형식으로 바꿉니다.

</details>

<details><summary>자세한 설명</summary>

MCP와 LLM 사이의 형식 변환을 누가 맡는지 묻는 문제입니다. 모델은 MCP를 직접 다루지 않고 host가 중간에서 맞춰 준다는 것을 알면 풀립니다.

LLM마다 tool 호출 형식이 다릅니다. server는 MCP 형식으로 `name`, `description`, `inputSchema`를 줍니다. host는 이 정의를 자기가 쓰는 LLM의 형식으로 바꿔 모델에 넘깁니다. 모델이 tool 호출을 내면 host가 그것을 다시 MCP 요청으로 바꿉니다. 그래서 C가 정답입니다.

A는 틀렸습니다. server는 어느 LLM이 자기 tool을 쓸지 모르므로 LLM별 형식을 맞출 수 없습니다. B도 틀렸습니다. LLM은 받은 정의를 읽을 뿐 변환하지 않습니다. D의 registry는 server 메타데이터를 모아 두는 곳이며 실행 흐름에 끼지 않습니다.

이 변환은 보안 문제와도 이어집니다. tool의 description, annotation, icon, 결과는 모두 모델에 들어가는 신뢰할 수 없는 입력입니다. description에 지시문을 숨기는 prompt injection이나 정의를 몰래 바꾸는 rug pull이 그 예입니다. 그래서 host는 이 정의를 그대로 믿으면 안 됩니다.

</details>

### 3. How does a client discover a server's capabilities in the 2026-07-28 revision?

A. By sending an initialize request and waiting for InitializeResult\
B. By calling server/discover, then listing tools, resources, and prompts\
C. By reading the server's manifest from the MCP Registry\
D. Capabilities are pushed automatically over an SSE stream

**한국어**

2026-07-28 개정에서 client는 server의 capability를 어떻게 알아냅니까?

A. `initialize` 요청을 보내고 `InitializeResult`를 기다려서\
B. `server/discover`를 호출한 뒤 tools, resources, prompts 목록을 조회해서\
C. MCP Registry에서 server의 매니페스트를 읽어서\
D. capability가 SSE 스트림으로 자동 푸시됩니다

<details><summary>정답</summary>

B. The initialize handshake was removed in 2026-07-28; servers advertise versions and capabilities via the server/discover RPC, and clients then call tools/list, resources/list, and prompts/list.

B. 2026-07-28에서 `initialize` handshake가 제거되었습니다. server는 `server/discover` RPC로 버전과 capability를 알립니다. client는 그다음 `tools/list`, `resources/list`, `prompts/list`를 호출합니다.

</details>

<details><summary>자세한 설명</summary>

handshake가 사라진 뒤 capability를 알아내는 방법을 묻는 문제입니다. `server/discover`라는 새 메서드를 알면 풀립니다.

2026-07-28에서 server는 `server/discover`를 반드시 구현해야 합니다(MUST). 결과인 `DiscoverResult`에는 지원 버전 목록 `supportedVersions`와 `capabilities`가 들어갑니다. 모델용 안내문 `instructions`와 server 정보도 들어갈 수 있습니다. capability를 확인한 client는 `tools/list` 같은 목록 메서드로 실제 primitive를 가져옵니다. 그래서 B가 정답입니다.

A는 2025-11-25까지의 방식입니다. C의 MCP Registry는 server를 찾기 위한 메타데이터 저장소입니다. 실행 중인 server의 capability를 알려 주지 않습니다. D도 틀렸습니다. capability를 스트림으로 밀어 주는 방식은 없습니다.

스펙 본문 기준으로는 client가 `server/discover`를 부르는 것이 선택입니다. client는 아무 요청이나 바로 보내도 됩니다. 버전이 맞지 않으면 `-32022` 오류의 `data.supported`를 보고 다시 시도합니다. 그래서 "client는 반드시 `server/discover`부터 불러야 한다"는 설명은 틀립니다. server 쪽 구현은 필수이고 client 쪽 호출은 선택이라는 차이를 기억해 둡니다. `DiscoverResult`가 캐시할 수 있는 결과라서 `ttlMs`와 `cacheScope`를 포함한다는 점도 함께 알아 둡니다.

</details>

### 4. What must a client do in the current spec to receive notifications/tools/list_changed?

A. Nothing — servers can send it at any time on any connection\
B. Open a subscriptions/listen stream and opt into that notification type\
C. Call tools/list with a subscribe flag\
D. Poll server/discover every few seconds

**한국어**

현재 스펙에서 `notifications/tools/list_changed`를 받으려면 client는 무엇을 해야 합니까?

A. 아무것도 하지 않아도 됩니다. server는 어느 연결에서든 언제든 보낼 수 있습니다\
B. `subscriptions/listen` 스트림을 열고 그 notification 유형을 구독(opt in)합니다\
C. subscribe 플래그를 붙여 `tools/list`를 호출합니다\
D. 몇 초마다 `server/discover`를 폴링합니다

<details><summary>정답</summary>

B. In 2026-07-28, change notifications flow over a long-lived subscriptions/listen stream that the client opens and opts into.

B. 2026-07-28에서는 변경 notification이 오래 유지되는 `subscriptions/listen` 스트림으로 옵니다. client가 이 스트림을 열고 받을 유형을 선택해야 합니다.

</details>

<details><summary>자세한 설명</summary>

2026-07-28에서 목록 변경 notification을 받는 방법을 묻는 문제입니다. `subscriptions/listen`이라는 새 메서드를 알면 풀립니다.

client는 `subscriptions/listen` 요청으로 오래 유지되는 스트림을 엽니다. 요청의 `params.notifications`에 받고 싶은 유형을 적습니다. tool 목록 변경을 받으려면 `toolsListChanged`를 true로 둡니다. server는 첫 메시지로 `notifications/subscriptions/acknowledged`를 보냅니다. 이 메시지에는 server가 실제로 보낼 유형이 담깁니다. 그래서 B가 정답입니다.

A는 틀렸습니다. server는 client가 요청하지 않은 유형을 보내면 안 됩니다(MUST NOT). C처럼 `tools/list`에 붙이는 subscribe 플래그는 없습니다. D의 폴링은 notification을 받는 방법이 아닙니다. `server/discover`는 capability를 알려 줄 뿐 목록 변경을 알려 주지 않습니다.

resource 구독도 함께 봅니다. 옛 `resources/subscribe`와 `resources/unsubscribe`는 2026-07-28에서 제거되었습니다. 이제 resource 변경도 같은 스트림에서 받습니다. 이때는 `resourceSubscriptions`에 URI 목록을 넣습니다. 반대로 `notifications/progress`와 `notifications/message`는 이 스트림으로 오지 않습니다. 이 둘은 관련 요청의 응답 스트림으로만 옵니다. 스트림의 모든 메시지에는 `io.modelcontextprotocol/subscriptionId`가 붙습니다. 이 값은 `subscriptions/listen` 요청의 JSON-RPC `id`입니다.

</details>

### 5. Can the LLM call an MCP server directly?

A. Yes, once the tool is approved\
B. Yes, but only over stdio\
C. No — the host/client mediates every call; the model only sees names, descriptions, and schemas\
D. Only for read-only tools

**한국어**

LLM이 MCP server를 직접 호출할 수 있습니까?

A. 예. tool이 승인되면 가능합니다\
B. 예. 다만 stdio에서만 가능합니다\
C. 아니요. 모든 호출은 host/client가 중개합니다. 모델은 이름, 설명, 스키마만 봅니다\
D. 읽기 전용 tool에서만 가능합니다

<details><summary>정답</summary>

C. The model never talks to servers. It emits a tool-use request; the host handles consent and the client sends tools/call.

C. 모델은 server와 직접 통신하지 않습니다. 모델은 tool 사용 요청을 냅니다. 그러면 host가 동의를 처리하고 client가 `tools/call`을 보냅니다.

</details>

<details><summary>자세한 설명</summary>

모델과 server 사이에 누가 있는지 묻는 문제입니다. 모델은 tool 사용 요청을 낼 뿐이라는 것을 알면 풀립니다.

모델은 host가 넘겨 준 tool 이름, 설명, 입력 스키마만 봅니다. 모델이 어떤 tool을 어떤 인자로 쓰겠다는 요청을 내면 host가 그 요청을 받습니다. host는 정책과 동의를 확인합니다. 그다음 해당 server와 연결된 client가 `tools/call`을 보냅니다. 그래서 C가 정답입니다.

A는 승인과 직접 호출을 섞은 함정입니다. 승인이 나도 호출은 client가 보냅니다. B와 D는 transport나 tool 종류에 따라 예외가 있다는 보기입니다. 그런 예외는 없으므로 둘 다 답이 아닙니다.

같은 원리는 다른 상황에도 적용됩니다. 예를 들어 tool 결과 안에 다른 tool을 호출하라는 지시가 숨어 있는 경우입니다. 이 경우에도 다음 호출은 host를 거칩니다. 그래서 host가 정책으로 막을 수 있습니다.

</details>

### 6. How do server-initiated needs like sampling reach the client in 2026-07-28?

A. As server-initiated JSON-RPC requests over the same connection\
B. Inside an InputRequiredResult returned in a reply (the MRTR pattern)\
C. Via a webhook the client registers during discovery\
D. They cannot — servers have no way to request client input

**한국어**

2026-07-28에서 sampling처럼 server 쪽에서 필요한 요청은 client에 어떻게 전달됩니까?

A. 같은 연결 위에서 server가 보내는 JSON-RPC 요청으로\
B. 응답으로 돌려주는 `InputRequiredResult` 안에 담아서(MRTR 패턴)\
C. 탐색 단계에서 client가 등록한 웹훅으로\
D. 전달할 수 없습니다. server는 client 입력을 요청할 방법이 없습니다

<details><summary>정답</summary>

B. Servers no longer initiate requests; sampling/createMessage, elicitation/create, and roots/list are delivered inside InputRequiredResult, and the client retries the original request with the responses. Sampling itself is deprecated in this revision.

B. server는 이제 요청을 먼저 보내지 않습니다. `sampling/createMessage`, `elicitation/create`, `roots/list`는 `InputRequiredResult` 안에 담겨 전달됩니다. client는 그 답을 담아 원래 요청을 다시 보냅니다. 이 개정에서 Sampling 자체는 폐기 예정(deprecated)입니다.

</details>

<details><summary>자세한 설명</summary>

server가 client에게 무언가를 물어야 할 때 2026-07-28에서 어떻게 하는지 묻는 문제입니다. MRTR(Multi Round-Trip Requests, 여러 번 왕복하는 요청) 패턴을 알면 풀립니다.

client가 `tools/call` 같은 요청을 보냅니다. server는 처리 중에 사용자 입력이나 LLM 응답이 필요하면 최종 결과 대신 `resultType: "input_required"`인 결과를 돌려줍니다. 이 결과의 `inputRequests`에는 `sampling/createMessage`, `elicitation/create`, `roots/list` 가운데 필요한 요청이 담깁니다. client는 답을 모은 뒤 원래 요청을 다시 보냅니다. 이때 `params.inputResponses`에 답을 넣습니다. 그래서 B가 정답입니다.

A는 2025-11-25까지의 방식입니다. 2026-07-28에서는 server가 어떤 스트림에서도 요청을 보내면 안 됩니다. C의 웹훅 등록은 스펙에 없습니다. D도 틀렸습니다. server는 MRTR로 입력을 요청할 수 있습니다.

MRTR의 세부 규칙도 정확히 알아 둡니다. 다시 보내는 요청은 새 JSON-RPC `id`를 써야 합니다. server가 `requestState`를 주었다면 client는 그 값을 한 바이트도 바꾸지 않고 돌려보냅니다. server는 client가 capability를 선언하지 않은 요청 유형을 `inputRequests`에 넣으면 안 됩니다. `InputRequiredResult`를 돌려줄 수 있는 메서드는 `tools/call`, `prompts/get`, `resources/read` 셋뿐입니다. Sampling, Roots, Logging은 제거된 것이 아니라 폐기 예정입니다. 2027-07-28 이후에 나오는 개정에서야 제거될 수 있습니다.

</details>

## Interaction Patterns & Response Handling

원본: `interactions/interaction-patterns.mdx`

### 1. In the 2026-07-28 revision, how does a server request LLM sampling or user input from the client?

A. It sends a server-initiated JSON-RPC request like sampling/createMessage\
B. It returns an InputRequiredResult inside its reply, and the client retries with the answers (MRTR)\
C. It opens a WebSocket back to the client\
D. It emits a notifications/input_required notification

**한국어**

2026-07-28 개정에서 server는 client에 LLM sampling이나 사용자 입력을 어떻게 요청합니까?

A. `sampling/createMessage` 같은 server 발신 JSON-RPC 요청을 보냅니다\
B. 응답 안에 `InputRequiredResult`를 돌려줍니다. client는 답을 담아 다시 시도합니다(MRTR)\
C. client 쪽으로 WebSocket을 엽니다\
D. `notifications/input_required` notification을 보냅니다

<details><summary>정답</summary>

B. Server-initiated requests were removed in 2026-07-28; sampling, elicitation, and roots requests now travel inside InputRequiredResult via the Multi Round-Trip Requests pattern.

B. server가 먼저 보내는 요청은 2026-07-28에서 제거되었습니다. 이제 sampling, elicitation, roots 요청은 Multi Round-Trip Requests 패턴에 따라 `InputRequiredResult` 안에 담겨 전달됩니다.

</details>

<details><summary>자세한 설명</summary>

앞 절 6번과 같은 내용을 다른 말로 묻는 문제입니다. server가 먼저 보내던 요청이 MRTR로 바뀌었다는 것을 알면 풀립니다.

2026-07-28 이전에는 server가 client에게 `sampling/createMessage` 같은 요청을 직접 보냈습니다. 2026-07-28에서는 이 방식이 없어졌습니다. 대신 server는 원래 요청의 응답으로 `InputRequiredResult`를 돌려줍니다. client는 답을 담아 원래 요청을 다시 보냅니다. 그래서 B가 정답입니다.

A는 2025-11-25까지의 방식이라 답이 아닙니다. C의 WebSocket은 MCP 표준 transport가 아닙니다. D의 `notifications/input_required`는 스펙에 없습니다. `input_required`는 notification 이름이 아니라 `resultType`의 값입니다.

`resultType` 규칙도 알아 둡니다. 2026-07-28의 모든 결과에는 `resultType`이 들어가야 합니다(MUST). 기본값은 최종 결과를 뜻하는 `"complete"`와 MRTR용 `"input_required"` 두 가지입니다. tasks extension을 쓰면 `"task"`도 나올 수 있습니다. client는 모르는 `resultType`을 잘못된 결과로 다룹니다. `resultType`이 아예 없으면 옛 server로 보고 `"complete"`로 다룹니다.

</details>

### 2. Which sequence describes the legacy (2025-11-25 and earlier) session handshake?

A. server/discover, then normal requests\
B. initialize request, initialize response, notifications/initialized\
C. ping, pong, notifications/ready\
D. subscriptions/listen followed by initialize

**한국어**

레거시(2025-11-25와 그 이전) 세션 handshake 순서로 맞는 것은 무엇입니까?

A. `server/discover` 다음 일반 요청\
B. `initialize` 요청, `initialize` 응답, `notifications/initialized`\
C. `ping`, `pong`, `notifications/ready`\
D. `subscriptions/listen` 다음 `initialize`

<details><summary>정답</summary>

B. Legacy revisions used the initialize request/response exchange followed by the notifications/initialized notification. The 2026-07-28 revision removed the handshake entirely.

B. 레거시 개정은 `initialize` 요청과 응답을 주고받은 뒤 `notifications/initialized` notification을 보냈습니다. 2026-07-28 개정은 handshake를 완전히 없앴습니다.

</details>

<details><summary>자세한 설명</summary>

옛 버전의 연결 시작 절차를 묻는 문제입니다. 2025-11-25까지 쓰던 `initialize` 3단계를 알면 풀립니다.

2025-11-25와 그 이전 개정에서는 연결이 handshake로 시작했습니다. client가 `initialize` 요청을 보냅니다. server는 `initialize` 응답으로 자기 정보를 알립니다. client가 `notifications/initialized`를 보내면 세션이 시작됩니다. 그래서 B가 정답입니다.

A의 `server/discover`는 2026-07-28에서 생긴 메서드라 레거시 절차가 아닙니다. C의 `pong`과 `notifications/ready`는 MCP에 없는 메서드입니다. D의 `subscriptions/listen`도 2026-07-28에서 생겼습니다. 순서도 맞지 않습니다.

이전 방식과 새 방식을 모두 지원하는 구현(dual-era)도 정리해 둡니다. stdio에서 dual-era client는 먼저 `server/discover`를 보내 봅니다. `DiscoverResult`가 오면 새 방식입니다. `-32022` 같은 새 방식 오류가 오면 새 방식이며 버전만 다릅니다. 그 밖의 오류나 시간 초과가 나면 레거시로 보고 `initialize`로 돌아갑니다. 이 판단을 특정 오류 코드 하나에만 기대면 안 됩니다(MUST NOT). 2026-07-28에서는 세션과 `Mcp-Session-Id` 헤더도 함께 제거되었습니다.

</details>

### 3. How does a client learn that a server's tool list changed at runtime?

A. The server closes the connection to force a refresh\
B. The client must poll tools/list on a fixed schedule required by the spec\
C. The server sends notifications/tools/list_changed and the client re-fetches the list\
D. The server pushes the new tool definitions directly in the notification body

**한국어**

client는 실행 중에 server의 tool 목록이 바뀐 것을 어떻게 압니까?

A. server가 연결을 끊어 client가 목록을 새로 받게 합니다\
B. 스펙이 정한 고정 주기로 client가 `tools/list`를 폴링해야 합니다\
C. server가 `notifications/tools/list_changed`를 보내고 client가 목록을 다시 가져옵니다\
D. server가 새 tool 정의를 notification 본문에 직접 담아 보냅니다

<details><summary>정답</summary>

C. Servers with the listChanged capability send notifications/tools/list_changed; the client then re-fetches with tools/list. In 2026-07-28 the client must have opened a subscriptions/listen stream to receive it.

C. `listChanged` capability가 있는 server는 `notifications/tools/list_changed`를 보냅니다. 그러면 client가 `tools/list`로 목록을 다시 가져옵니다. 2026-07-28에서는 이 notification을 받으려면 client가 `subscriptions/listen` 스트림을 열어 두어야 합니다.

</details>

<details><summary>자세한 설명</summary>

tool 목록이 바뀌었을 때 client가 알게 되는 경로를 묻는 문제입니다. 변경 notification을 받고 목록을 다시 조회하는 두 단계를 알면 풀립니다.

server는 `tools` capability에 `listChanged`를 선언해 목록 변경 notification을 지원한다고 알립니다. 목록이 바뀌면 server는 `notifications/tools/list_changed`를 보냅니다. 이 notification에는 새 정의가 들어 있지 않습니다. client는 이것을 받고 `tools/list`를 다시 호출합니다. 그래서 C가 정답입니다.

A처럼 연결을 끊을 필요는 없습니다. B의 고정 폴링 주기는 스펙에 없습니다. D도 틀렸습니다. notification은 바뀌었다는 사실만 알립니다. 새 내용은 client가 다시 가져옵니다.

이 내용은 캐시 규칙과도 이어집니다. 2026-07-28에서 `tools/list` 결과에는 `ttlMs`와 `cacheScope`가 붙습니다. `ttlMs`는 결과를 얼마 동안 새것으로 볼 수 있는지 알려 주는 힌트입니다. 폴링 주기가 아닙니다. 목록 변경 notification이 오면 캐시는 바로 무효가 됩니다. `tools/list` 결과가 연결마다 달라지면 안 된다는 점도 기억해 둡니다. 다만 요청에 실린 인가에 따라서는 달라질 수 있습니다.

</details>

### 4. What must a client do with a nextCursor value returned by a paginated list operation?

A. Parse it to compute the next page number\
B. Treat it as opaque and pass it back unchanged as the cursor parameter\
C. Increment it by the page size\
D. Ignore it — pagination is server-driven only

**한국어**

페이지네이션된 목록 조회가 돌려준 `nextCursor` 값을 client는 어떻게 처리해야 합니까?

A. 파싱해서 다음 페이지 번호를 계산합니다\
B. 불투명한(opaque) 값으로 다루고 `cursor` 파라미터로 그대로 돌려보냅니다\
C. 페이지 크기만큼 더합니다\
D. 무시합니다. 페이지네이션은 server만 주도합니다

<details><summary>정답</summary>

B. Cursors are opaque tokens: clients MUST NOT parse or modify them, only echo them back in params.cursor to fetch the next page.

B. cursor는 불투명한 token입니다. client는 cursor를 파싱하거나 수정하면 안 됩니다(MUST NOT). 다음 페이지를 가져올 때 `params.cursor`에 그대로 돌려보내기만 합니다.

</details>

<details><summary>자세한 설명</summary>

페이지네이션 cursor를 다루는 규칙을 묻는 문제입니다. cursor가 불투명한 값이라는 것을 알면 풀립니다.

불투명하다는 말은 client가 값의 안쪽 구조를 해석하지 않는다는 뜻입니다. server는 목록이 길면 결과에 `nextCursor`를 넣습니다. client는 다음 페이지가 필요할 때 이 값을 `cursor`에 그대로 넣어 같은 목록 메서드를 다시 부릅니다. 그래서 B가 정답입니다.

"그대로 돌려보낸다"는 server가 준 문자열을 한 글자도 바꾸지 않고 다음 요청에 넣는다는 뜻입니다. server가 이렇게 응답했다고 해 보겠습니다.

```json
{ "result": { "tools": [ ... ], "nextCursor": "eyJwYWdlIjoyfQ" } }
```

client는 다음 요청에 이 값을 복사해서 넣기만 합니다.

```json
{ "method": "tools/list", "params": { "cursor": "eyJwYWdlIjoyfQ" } }
```

응답에 `nextCursor`가 없으면 마지막 페이지입니다.

위 문자열은 base64로 보입니다. 풀어 보면 `{"page":2}`가 나옵니다. 이것을 보고 client가 `{"page":3}`을 직접 만들어 보내면 안 됩니다. 문자열을 잘라 내거나 자기 형식으로 다시 만드는 것도 안 됩니다. cursor의 형식은 server만 압니다. server는 그 안에 페이지 번호를 넣을 수도 있고 DB 위치나 시각을 넣을 수도 있습니다. 다음 버전에서 형식을 바꿀 수도 있습니다. client가 내용에 기대지 않으면 server가 형식을 바꿔도 client는 계속 동작합니다.

쿠버네티스의 `continue` 토큰과 같은 방식입니다. `kubectl get pods --chunk-size=500`을 쓰면 API 서버가 `continue` 토큰을 돌려줍니다. 그 안에는 etcd 리비전 같은 내부 정보가 들어 있습니다. 그래도 사용자는 받은 토큰을 다음 요청에 그대로 넣기만 합니다.

A와 C는 cursor 안에 페이지 번호나 위치가 들어 있다고 가정합니다. cursor의 내용은 server만 압니다. client가 해석하거나 계산하면 안 되므로 둘 다 답이 아닙니다. D도 틀렸습니다. 다음 페이지를 요청하는 쪽은 client입니다. cursor를 무시하면 첫 페이지 뒤의 항목을 받을 수 없습니다.

cursor의 경계 사례도 알아 둡니다. 빈 문자열도 유효한 cursor입니다. 잘못된 cursor를 보내면 `-32602` 오류가 옵니다. 페이지 크기는 client가 아니라 server가 정합니다.

</details>

### 5. How does a client cancel an in-flight request over Streamable HTTP in 2026-07-28?

A. It sends notifications/cancelled on a new POST\
B. It sends a DELETE to the MCP endpoint\
C. It closes the SSE response stream for that request\
D. It sends ping with a cancel flag

**한국어**

2026-07-28에서 client는 Streamable HTTP로 진행 중인 요청을 어떻게 취소합니까?

A. 새 POST로 `notifications/cancelled`를 보냅니다\
B. MCP 엔드포인트에 DELETE를 보냅니다\
C. 그 요청의 SSE 응답 스트림을 닫습니다\
D. cancel 플래그를 붙여 `ping`을 보냅니다

<details><summary>정답</summary>

C. On Streamable HTTP, closing the request's SSE response stream is the cancellation signal; notifications/cancelled is used on stdio.

C. Streamable HTTP에서는 요청의 SSE 응답 스트림을 닫는 것이 취소 신호입니다. `notifications/cancelled`는 stdio에서 씁니다.

</details>

<details><summary>자세한 설명</summary>

transport마다 다른 취소 방법을 묻는 문제입니다. HTTP에서는 스트림을 닫고 stdio에서는 notification을 보낸다는 차이를 알면 풀립니다.

Streamable HTTP에서는 요청마다 POST를 따로 보냅니다. 응답이 SSE 스트림으로 오면 그 스트림은 해당 요청 하나에만 쓰입니다. 그래서 client가 그 스트림을 닫으면 server는 요청이 취소된 것으로 봅니다. 별도 메시지는 보내지 않습니다. C가 정답입니다.

연결 전체를 끊는 것과는 다릅니다. 닫히는 것은 요청 하나에 딸린 스트림이므로 다른 요청에는 영향이 없습니다. `kubectl logs -f`를 Ctrl+C로 끊으면 로그 스트림만 끊기고 pod는 그대로 도는 것과 같습니다. stdio에는 요청별 스트림이 없습니다. 프로세스 하나와 연결된 stdin/stdout 하나로 모든 메시지를 주고받습니다. 여기서 연결을 닫으면 server 프로세스 전체가 끝납니다. 그래서 stdio에서는 메시지로 어느 요청을 취소할지 알립니다. 기준은 하나입니다. 취소할 요청 하나만 끊을 수 있으면 끊고 그럴 수 없으면 메시지를 보냅니다.

정리하면 취소 방법은 transport에 따라 다릅니다.

- Streamable HTTP: 그 요청의 응답 스트림을 닫습니다.
- stdio: `notifications/cancelled`를 보내고 `requestId`로 취소할 요청을 지정합니다.

A의 `notifications/cancelled`는 stdio에서 client가 쓰는 방법입니다. 이 notification에는 `requestId`와 선택 항목인 `reason`이 들어갑니다. B의 DELETE는 옛 세션을 끝낼 때 쓰던 방법이며 2026-07-28에서 제거되었습니다. 새 방식만 지원하는 server는 DELETE에 `405`로 답합니다. D의 `ping`도 2026-07-28에서 제거되었습니다.

반대로 stdio에서 프로세스를 닫는 것은 취소 방법이 아닙니다. stdio는 `notifications/cancelled`로 취소합니다. server가 `notifications/cancelled`를 보내는 경우는 `subscriptions/listen` 스트림을 끊을 때뿐입니다. 취소한 요청의 응답이 늦게 도착할 수도 있습니다. client는 그 응답을 무시하면 됩니다. 이런 경합은 정상으로 봅니다.

</details>

### 6. What happened to the ping utility in the 2026-07-28 revision?

A. It became mandatory for servers\
B. It was removed from the protocol\
C. It was renamed to health/check\
D. It now requires a progressToken

**한국어**

2026-07-28 개정에서 `ping` 유틸리티는 어떻게 되었습니까?

A. server의 의무가 되었습니다\
B. 프로토콜에서 제거되었습니다\
C. `health/check`로 이름이 바뀌었습니다\
D. 이제 `progressToken`이 필요합니다

<details><summary>정답</summary>

B. ping (along with logging/setLevel) was removed in 2026-07-28. Through 2025-11-25, either side could send ping and the receiver had to respond with an empty result.

B. `ping`은 `logging/setLevel`과 함께 2026-07-28에서 제거되었습니다. 2025-11-25까지는 어느 쪽이든 `ping`을 보낼 수 있었습니다. 받은 쪽은 빈 결과로 응답해야 했습니다.

</details>

<details><summary>자세한 설명</summary>

2026-07-28에서 없어진 유틸리티를 묻는 문제입니다. 제거 목록에 `ping`이 있다는 것을 알면 풀립니다.

2025-11-25까지 `ping`은 client와 server 어느 쪽이든 보낼 수 있는 요청이었습니다. 받은 쪽은 빈 결과로 답해야 했습니다. 2026-07-28은 이 메서드를 프로토콜에서 뺐습니다. 그래서 B가 정답입니다.

A는 반대입니다. 의무가 된 것이 아니라 없어졌습니다. C의 `health/check`라는 메서드는 없습니다. D의 `progressToken`은 진행 상황 notification을 받겠다고 요청에 붙이는 값입니다. `ping`과는 상관이 없습니다.

제거와 폐기 예정은 서로 다릅니다. `ping`, `logging/setLevel`, `notifications/roots/list_changed`는 제거되었습니다. 반면 Logging, Roots, Sampling 기능은 폐기 예정이며 지금도 동작합니다. 예를 들어 Logging은 요청 `_meta`의 `logLevel`과 `notifications/message`로 남아 있습니다. 그래서 "Logging이 제거되었다"는 설명은 틀리고 "`logging/setLevel`이 제거되었다"는 설명은 맞습니다.

</details>

## Error Handling

원본: `interactions/error-handling.mdx`

### 1. A tool call references a tool name that doesn't exist on the server. How is this reported?

A. A result with isError: true so the model can retry\
B. A JSON-RPC error object (Invalid params, -32602)\
C. HTTP status 404 with no body\
D. A notifications/cancelled notification

**한국어**

tool 호출이 server에 없는 tool 이름을 가리킵니다. 이 상황은 어떻게 보고됩니까?

A. 모델이 다시 시도할 수 있도록 `isError: true`인 결과로\
B. JSON-RPC 오류 객체(Invalid params, `-32602`)로\
C. 본문 없는 HTTP 상태 404로\
D. `notifications/cancelled` notification으로

<details><summary>정답</summary>

B. An unknown tool means the request itself cannot be fulfilled — a protocol error, reported as a JSON-RPC error object with code -32602 (Invalid params).

B. 알 수 없는 tool은 요청 자체를 처리할 수 없다는 뜻이므로 프로토콜 오류입니다. 코드가 `-32602`(Invalid params)인 JSON-RPC 오류 객체로 보고합니다.

</details>

<details><summary>자세한 설명</summary>

없는 tool을 불렀을 때 어느 오류 채널로 보고하는지 묻는 문제입니다. tool 관련 오류는 2가지로 나뉩니다. 하나는 JSON-RPC `error`로 돌려주는 프로토콜 오류입니다. 다른 하나는 정상 결과에 `isError: true`를 붙이는 tool 실행 오류입니다.

server에 그런 이름의 tool이 없으면 실행할 대상이 없습니다. 요청 자체가 잘못됐으므로 프로토콜 오류로 보고합니다. brief의 오류 코드 표도 unknown tool을 `-32602` Invalid params에 넣어 둡니다. 그래서 B가 정답입니다.

A의 `isError: true`는 tool이 실행된 뒤 작업이 실패했을 때 쓰는 방식입니다. 이 문제에서는 tool이 실행되지도 않았으므로 맞지 않습니다. C의 HTTP 404는 Streamable HTTP에서 메서드 자체를 모를 때 `-32601`과 함께 쓰는 상태 코드입니다. `tools/call` 메서드는 존재하므로 404가 아닙니다. D의 `notifications/cancelled`는 stdio에서 진행 중인 요청을 취소할 때 보내는 notification입니다. 오류를 보고하는 수단이 아닙니다.

`-32601`과도 구분해야 합니다. brief 16절은 "unknown tool은 `-32601`을 돌려준다"를 틀린 믿음으로 꼽습니다. `-32601`은 server가 메서드 이름 자체를 모를 때 씁니다. tool 이름을 모르는 것은 params가 잘못된 경우이므로 `-32602`입니다.

</details>

### 2. A weather API inside a tool times out. How should the server report this?

A. Return a JSON-RPC error with code -32603\
B. Close the connection so the client restarts the server\
C. Return a successful JSON-RPC response with isError: true and the error text in content\
D. Send a log notification and no response

**한국어**

tool 안의 날씨 API가 시간 초과됐습니다. server는 이를 어떻게 보고해야 합니까?

A. 코드가 `-32603`인 JSON-RPC 오류를 반환합니다\
B. 연결을 닫아 client가 server를 다시 띄우게 합니다\
C. 성공한 JSON-RPC 응답에 `isError: true`를 넣고 오류 내용은 `content`에 담아 반환합니다\
D. 로그 notification만 보내고 응답하지 않습니다

<details><summary>정답</summary>

C. The tool ran but its operation failed — a tool execution error. It goes in a normal result with isError: true so the model can see it and self-correct.

C. tool은 실행됐지만 그 안의 작업이 실패했으므로 tool 실행 오류입니다. 일반 결과에 `isError: true`를 넣어 반환합니다. 그래야 모델이 오류를 보고 스스로 고칠 수 있습니다.

</details>

<details><summary>자세한 설명</summary>

tool 안에서 외부 API가 실패한 경우를 어느 오류 채널로 보고하는지 묻는 문제입니다. 요청은 정상이었고 실행 단계에서 실패했다는 점을 보면 풀립니다.

server는 `tools/call` 요청을 받아 tool을 실행했습니다. 실패한 것은 tool이 부른 날씨 API입니다. brief는 API 실패를 tool 실행 오류의 예로 듭니다. 이때 server는 정상 `CallToolResult`에 `isError: true`를 넣고 설명을 `content`에 담습니다. 이 결과는 모델에게 전달됩니다. 그래서 모델이 다시 시도하거나 다른 방법을 고를 수 있습니다. C가 정답입니다.

A의 `-32603`은 Internal error입니다. server 자체가 실패했을 때 쓰는 프로토콜 오류입니다. 이 코드로 보내면 오류는 client 코드가 처리하고 모델은 실패 이유를 보지 못합니다. B처럼 연결을 끊을 이유는 없습니다. 외부 API 하나가 응답하지 않는 것은 server가 망가진 것이 아닙니다. D는 요청에 응답하지 않으므로 client가 결과를 받을 수 없습니다. 또 Logging 기능은 2026-07-28에서 폐기 예정(deprecated)이 됐습니다.

다른 상황도 같은 기준으로 나눕니다. 외부 서비스 장애, 잘못된 날짜 형식, 범위를 벗어난 값, 업무 규칙에 따른 거절, 만료된 handle은 모두 tool 실행 오류입니다. 반대로 없는 tool 이름이나 `CallToolRequest` 형식에 맞지 않는 요청은 프로토콜 오류입니다.

</details>

### 3. Why are tool execution errors returned inside a successful JSON-RPC response?

A. Because JSON-RPC has no error object\
B. So the LLM can see the failure details and adjust its next attempt\
C. To avoid HTTP status codes other than 200\
D. Because clients ignore JSON-RPC error objects

**한국어**

tool 실행 오류는 왜 성공한 JSON-RPC 응답 안에 담아 반환합니까?

A. JSON-RPC에는 오류 객체가 없어서\
B. LLM이 실패 내용을 보고 다음 시도를 조정할 수 있게 하려고\
C. 200 이외의 HTTP 상태 코드를 피하려고\
D. client가 JSON-RPC 오류 객체를 무시해서

<details><summary>정답</summary>

B. In-band errors reach the model, which can retry with different arguments or pick another tool. JSON-RPC protocol errors are handled by client code instead.

B. 결과 안에 담긴(in-band) 오류는 모델에게 전달됩니다. 모델은 인자를 바꿔 다시 시도하거나 다른 tool을 고를 수 있습니다. 반면 JSON-RPC 프로토콜 오류는 client 코드가 처리합니다.

</details>

<details><summary>자세한 설명</summary>

tool 실행 오류를 왜 결과 안에 넣는지, 설계 이유를 묻는 문제입니다. 두 오류 채널이 각각 누구에게 가는지 알면 풀립니다.

정상 결과에 담긴 내용은 모델에게 전달됩니다. 그래서 `isError: true`와 함께 `content`에 오류 설명을 넣으면 모델이 그 설명을 읽을 수 있습니다. 모델은 인자를 고쳐 다시 부르거나 다른 tool을 고릅니다. brief도 tool 실행 오류는 모델이 스스로 고칠 수 있도록 모델에게 돌려보낸다고 적습니다. 그래서 B가 정답입니다.

A는 사실이 아닙니다. JSON-RPC 2.0에는 `code`, `message`, `data`를 담는 `error` 객체가 있고 MCP도 이 객체를 씁니다. D도 틀렸습니다. client는 JSON-RPC 오류 객체를 무시하지 않고 client 코드에서 처리합니다. 다만 그 경로로는 모델이 오류를 보지 못합니다. C는 HTTP 상태 코드와 관련이 없습니다. MCP는 Streamable HTTP에서 `400`, `404` 같은 상태 코드를 실제로 씁니다. stdio에는 HTTP 상태 코드가 아예 없습니다.

"누가 이 오류를 보는가"로 나눠 볼 수도 있습니다. 모델이 보아야 하는 오류는 결과 안에 담습니다. client 코드가 처리할 오류는 JSON-RPC `error`로 보냅니다.

</details>

### 4. The model calls a tool with arguments that fail the tool's input schema validation. Per current spec guidance, this is a...

A. Protocol error with code -32700\
B. Protocol error with code -32601\
C. Tool execution error (isError: true) so the model can fix its arguments\
D. Reason to terminate the connection

**한국어**

모델이 tool의 입력 스키마 검증을 통과하지 못하는 인자로 tool을 호출했습니다. 현재 스펙 지침에 따르면 이것은 무엇입니까?

A. 코드가 `-32700`인 프로토콜 오류\
B. 코드가 `-32601`인 프로토콜 오류\
C. 모델이 인자를 고칠 수 있게 하는 tool 실행 오류(`isError: true`)\
D. 연결을 끊을 사유

<details><summary>정답</summary>

C. Since the 2025-11-25 revision, input-validation failures are tool execution errors, keeping the feedback visible to the model.

C. 2025-11-25 개정부터 입력 검증 실패는 tool 실행 오류로 다룹니다. 그래야 피드백이 모델에게 보입니다.

</details>

<details><summary>자세한 설명</summary>

tool의 `inputSchema`에 맞지 않는 인자를 어느 오류로 처리하는지 묻는 문제입니다. 2025-11-25 개정(SEP-1303)에서 바뀐 규칙을 알면 풀립니다.

인자가 스키마에 맞지 않는 것은 모델이 고칠 수 있는 실수입니다. 날짜 형식이 틀렸거나 값이 범위를 벗어난 경우가 그렇습니다. 그래서 스펙은 이 경우를 tool 실행 오류로 돌려주게 합니다. 모델은 `content`의 설명을 보고 인자를 고쳐 다시 부릅니다. 그래서 C가 정답입니다.

A의 `-32700`은 Parse error입니다. JSON 자체를 읽을 수 없을 때 씁니다. 이 문제의 요청은 JSON으로는 멀쩡합니다. B의 `-32601`은 Method not found입니다. `tools/call` 메서드는 존재하므로 맞지 않습니다. D처럼 연결을 끊으면 모델은 무엇이 잘못됐는지 알 수 없습니다.

이 문항의 보기에는 없지만 `-32602`와도 구분해야 합니다. brief 16절은 "스키마에 맞지 않는 인자는 `-32602`를 돌려준다"를 틀린 믿음으로 꼽습니다. `-32602`는 없는 tool 이름처럼 요청 자체를 처리할 수 없을 때 씁니다. 스키마 검증 실패는 `-32602`가 아닙니다.

</details>

### 5. Which JSON-RPC code means "Method not found"?

A. -32700\
B. -32601\
C. -32602\
D. -32603

**한국어**

"Method not found"를 뜻하는 JSON-RPC 코드는 무엇입니까?

A. `-32700`\
B. `-32601`\
C. `-32602`\
D. `-32603`

<details><summary>정답</summary>

B. -32700 is a parse error, -32601 is method not found, -32602 is invalid params, -32603 is an internal error.

B. `-32700`은 파싱 오류, `-32601`은 method not found, `-32602`는 invalid params, `-32603`은 내부 오류입니다.

</details>

<details><summary>자세한 설명</summary>

JSON-RPC 표준 오류 코드를 구분할 수 있는지 묻는 문제입니다.

`-32601`은 요청한 메서드를 server가 모를 때 씁니다. Streamable HTTP에서는 이 오류와 함께 HTTP `404 Not Found`를 돌려줍니다. 그래서 B가 정답입니다.

`-32700` Parse error는 JSON을 읽을 수 없을 때 씁니다. `-32602` Invalid params는 메서드는 있지만 인자가 잘못됐을 때 씁니다. 없는 tool 이름, 빠진 필수 `_meta`, 찾을 수 없는 resource가 여기에 들어갑니다. `-32603` Internal error는 server 내부 실패입니다. 그래서 A, C, D는 답이 아닙니다.

MCP가 따로 정한 코드도 알아 둡니다. 2026-07-28 스펙은 `-32020` HeaderMismatch, `-32021` MissingRequiredClientCapability, `-32022` UnsupportedProtocolVersion을 정의합니다. 예전의 resource not found 코드 `-32002`는 이제 내보내면 안 되고(MUST NOT) `-32602`로 바뀌었습니다. `-32000`부터 `-32019`까지는 legacy 구간입니다. 새 구현은 이 구간을 쓰지 않는 것이 좋습니다(SHOULD NOT).

</details>

### 6. A server does not support the protocol version a request declares. What does the current (2026-07-28) spec say it must return?

A. An isError: true result\
B. UnsupportedProtocolVersionError, JSON-RPC code -32022\
C. HTTP 500 with an empty body\
D. JSON-RPC code -32002

**한국어**

요청이 선언한 프로토콜 버전을 server가 지원하지 않습니다. 현재(2026-07-28) 스펙은 server가 무엇을 반환해야 한다고 합니까?

A. `isError: true`인 결과\
B. `UnsupportedProtocolVersionError`, JSON-RPC 코드 `-32022`\
C. 본문이 빈 HTTP 500\
D. JSON-RPC 코드 `-32002`

<details><summary>정답</summary>

B. The 2026-07-28 spec defines -32022 UnsupportedProtocolVersionError, with data listing the supported versions so the client can retry with one of them.

B. 2026-07-28 스펙은 `-32022` UnsupportedProtocolVersionError를 정의합니다. 이 오류의 `data`에는 지원하는 버전 목록이 들어 있습니다. client는 그중 하나로 다시 시도할 수 있습니다.

</details>

<details><summary>자세한 설명</summary>

버전이 맞지 않을 때 server가 돌려주는 오류를 묻는 문제입니다. 2026-07-28에서 버전 협상 방식이 바뀌었다는 것을 알면 풀립니다.

2026-07-28에는 `initialize` handshake가 없습니다. 대신 client는 모든 요청의 `_meta`에 `io.modelcontextprotocol/protocolVersion`을 넣습니다. server가 그 버전을 지원하지 않으면 `-32022`를 돌려줍니다. 오류의 `data.supported`에는 지원 버전 목록이 들어가고 `data.requested`에는 client가 요청한 버전이 들어갑니다. Streamable HTTP에서는 HTTP `400`과 함께 보냅니다. 그래서 B가 정답입니다.

A의 `isError: true`는 tool 실행 오류에 씁니다. 버전 불일치는 요청 자체를 처리할 수 없는 프로토콜 오류입니다. C의 HTTP 500은 스펙이 이 경우에 정한 상태 코드가 아닙니다. D의 `-32002`는 예전 resource not found 코드입니다. 2026-07-28 구현은 이 코드를 내보내면 안 됩니다(MUST NOT).

client 쪽 동작도 함께 봅니다. client는 `server/discover`를 먼저 부르지 않고 바로 요청을 보내도 됩니다. `-32022`를 받으면 `data.supported`에 있는 버전으로 다시 시도합니다. stdio에서 옛 방식과 새 방식을 모두 지원하는 client는 `-32022`를 받으면 새 방식 server로 판단합니다. 이때 `initialize`로 돌아가지 않습니다.

</details>

## Tool Invocation Lifecycle

원본: `interactions/tool-invocation-lifecycle.mdx`

### 1. In the tool invocation lifecycle, when does user consent/authorization happen?

A. After the server executes the tool but before the result is returned\
B. Before the tools/call request is sent to the server\
C. Only when the tool sets destructiveHint to true\
D. It is enforced by the server during execution

**한국어**

tool 호출 수명 주기에서 사용자 동의나 인가는 언제 일어납니까?

A. server가 tool을 실행한 뒤, 결과를 반환하기 전\
B. `tools/call` 요청을 server로 보내기 전\
C. tool이 `destructiveHint`를 true로 설정한 경우에만\
D. 실행 중에 server가 집행합니다

<details><summary>정답</summary>

B. The host applies policy and obtains consent before invocation — the call never reaches the server without authorization.

B. host가 호출 전에 정책을 적용하고 동의를 받습니다. 인가가 없으면 호출은 server에 닿지 않습니다.

</details>

<details><summary>자세한 설명</summary>

tool 호출 흐름에서 동의 단계가 어디에 있는지 묻는 문제입니다. 동의를 받는 주체가 host라는 점을 알면 풀립니다.

흐름은 이렇습니다. 모델이 tool 호출을 제안합니다. host가 정책을 확인하고 필요하면 사용자에게 묻습니다. 승인된 경우에만 client가 `tools/call`을 server로 보냅니다. brief도 사람이 호출을 거부할 수 있어야 한다(SHOULD)고 적습니다. client는 호출 전에 입력을 보여 주고 민감한 작업을 확인해야 합니다(SHOULD). 그래서 B가 정답입니다.

A는 순서가 틀렸습니다. 실행이 끝난 뒤에 묻는다면 부작용은 이미 일어났습니다. C는 "only"가 틀렸습니다. 동의는 `destructiveHint` 값만 보고 정하지 않습니다. annotations는 server가 스스로 적은 힌트라서 믿을 수 없습니다. D도 틀렸습니다. 동의를 받고 집행하는 쪽은 host입니다.

annotations 기본값도 같이 알아 둡니다. `readOnlyHint` 기본값은 false이고 `destructiveHint` 기본값은 true입니다. 따라서 힌트가 없는 tool은 파괴적일 수 있다고 보는 것이 기본입니다. 다만 어느 값이든 힌트일 뿐 보안 통제는 아닙니다.

</details>

### 2. Which method does a client use to invoke a tool?

A. tools/invoke\
B. tools/run\
C. tools/call\
D. tools/execute

**한국어**

client가 tool을 호출할 때 쓰는 메서드는 무엇입니까?

A. `tools/invoke`\
B. `tools/run`\
C. `tools/call`\
D. `tools/execute`

<details><summary>정답</summary>

C. tools/call takes the tool name and an arguments object; tools/list is used for discovery.

C. `tools/call`은 tool 이름과 `arguments` 객체를 받습니다. `tools/list`는 tool 목록을 찾는 데(discovery) 씁니다.

</details>

<details><summary>자세한 설명</summary>

tool 호출 메서드 이름을 정확히 아는지 묻는 문제입니다.

client는 `tools/list`로 server가 제공하는 tool 목록을 받습니다. 그다음 `tools/call`에 `name`과 `arguments`를 넣어 호출합니다. 그래서 C가 정답입니다. `tools/invoke`, `tools/run`, `tools/execute`는 MCP에 없는 메서드이므로 A, B, D는 답이 아닙니다.

2026-07-28에서는 `tools/call`에 재시도용 파라미터가 붙을 수 있습니다. MRTR(Multi Round-Trip Requests)은 한 요청을 여러 번 왕복해 완성하는 방식입니다. server가 `resultType: "input_required"` 결과를 돌려주면 client는 새 JSON-RPC id로 `tools/call`을 다시 보냅니다. 이때 `inputResponses`를 넣고 `requestState`를 받은 그대로 돌려줍니다. 다른 primitive의 메서드와도 구분해 둡니다. prompt는 `prompts/get`, resource는 `resources/read`로 가져옵니다.

</details>

### 3. How does a client cancel an in-flight tool call over Streamable HTTP (2026-07-28)?

A. It sends notifications/cancelled on a new POST\
B. It closes the SSE response stream\
C. It sends a DELETE request to the MCP endpoint\
D. It waits for the server timeout

**한국어**

Streamable HTTP(2026-07-28)에서 client는 진행 중인 tool 호출을 어떻게 취소합니까?

A. 새 POST로 `notifications/cancelled`를 보냅니다\
B. SSE 응답 스트림을 닫습니다\
C. MCP 엔드포인트에 `DELETE` 요청을 보냅니다\
D. server 타임아웃을 기다립니다

<details><summary>정답</summary>

B. On Streamable HTTP, closing the SSE response stream is the cancellation signal; notifications/cancelled is used on stdio.

B. Streamable HTTP에서는 SSE 응답 스트림을 닫는 것이 취소 신호입니다. `notifications/cancelled`는 stdio에서 씁니다.

</details>

<details><summary>자세한 설명</summary>

transport마다 다른 취소 방법을 묻는 문제입니다. 2026-07-28에서 HTTP의 취소 방식이 바뀌었다는 것을 알면 풀립니다.

Streamable HTTP에서는 요청마다 응답 스트림이 따로 있습니다. client가 그 요청의 SSE 응답 스트림을 닫으면 그것 자체가 취소입니다. 따로 메시지를 보내지 않습니다. 그래서 B가 정답입니다.

A의 `notifications/cancelled`는 stdio에서 쓰는 방법입니다. stdio에서는 client가 `requestId`를 담은 이 notification을 보내 취소합니다. C의 `DELETE`는 예전에 세션을 끝낼 때 쓰던 요청입니다. 2026-07-28에서 세션과 함께 없어졌고 새 방식만 지원하는 server는 `DELETE`에 `405`로 답합니다. D는 취소가 아닙니다. 모든 요청에는 타임아웃을 두는 것이 좋지만(SHOULD) 그것으로 진행 중인 호출을 멈추게 하지는 못합니다.

반대로 "stdio 프로세스를 닫으면 요청이 취소된다"는 설명은 틀립니다. stdio는 `notifications/cancelled`로 취소합니다. server가 `notifications/cancelled`를 보내는 경우는 `subscriptions/listen` 스트림을 끝낼 때뿐입니다. 취소된 요청에 늦게 도착한 응답은 무시합니다.

</details>

### 4. What does structuredContent in a tool result contain?

A. A base64-encoded binary payload\
B. A JSON value conforming to the tool's outputSchema\
C. The JSON-RPC error object\
D. A list of resource URIs

**한국어**

tool 결과의 `structuredContent`에는 무엇이 들어 있습니까?

A. base64로 인코딩한 바이너리 페이로드\
B. tool의 `outputSchema`를 따르는 JSON 값\
C. JSON-RPC 오류 객체\
D. resource URI 목록

<details><summary>정답</summary>

B. If the tool defines an outputSchema, structuredContent must conform to it; clients should validate the result against the schema.

B. tool이 `outputSchema`를 정의하면 `structuredContent`는 그 스키마를 따라야 합니다. client는 결과를 스키마에 맞춰 검증해야 합니다(should).

</details>

<details><summary>자세한 설명</summary>

구조화된 tool 출력이 무엇인지 묻는 문제입니다. 2025-06-18 개정에서 `outputSchema`와 `structuredContent`가 함께 들어왔다는 것을 알면 풀립니다.

`CallToolResult`에는 `content`, `structuredContent`, `isError`가 있습니다. `content`는 텍스트나 이미지 같은 content block의 목록입니다. `structuredContent`는 JSON 값 하나입니다. tool에 `outputSchema`가 있으면 이 값은 그 스키마를 따라야 합니다. 그래서 B가 정답입니다.

A의 base64 데이터는 `content` 안의 `image` block이나 `audio` block에 들어갑니다. C의 JSON-RPC 오류 객체는 결과가 아니라 `error` 응답에 들어갑니다. D의 resource URI는 `content` 안의 `resource_link` block으로 돌려줍니다. 그래서 A, C, D는 답이 아닙니다.

두 필드는 같은 결과를 두 가지 모양으로 줍니다. `content`는 사람이나 모델이 읽는 결과이고 `structuredContent`는 프로그램이 바로 꺼내 쓰는 데이터입니다. 날씨 tool을 예로 들면 이렇습니다.

```json
{
  "content": [
    { "type": "text", "text": "서울은 18도, 맑음입니다." }
  ],
  "structuredContent": {
    "city": "서울",
    "temperature": 18,
    "condition": "맑음"
  }
}
```

`outputSchema`는 tool 정의에 적는 "결과가 이런 모양이다"라는 약속입니다. `inputSchema`가 입력 모양을 정하듯 `outputSchema`는 출력 모양을 정합니다.

함께 알아 둘 점이 있습니다. `structuredContent`는 JSON 값이면 무엇이든 될 수 있습니다. server는 같은 내용을 text block으로도 직렬화해 넣는 것이 좋습니다(SHOULD). "structuredContent는 반드시 객체여야 한다"거나 "outputSchema가 없으면 쓸 수 없다" 같은 설명은 스펙과 맞지 않습니다.

정의를 몰라도 이름으로 풀 수 있는 문제입니다. structured + content는 "구조화된 내용"입니다. 선택지 중에서 구조와 이어지는 단어는 B의 schema뿐입니다. MCP 필드 이름은 `inputSchema`, `outputSchema`, `isError`, `listChanged`처럼 대부분 뜻 그대로 지어져 있습니다. 생소한 필드를 만나면 이름을 쪼개서 뜻을 보고 그 뜻과 이어지는 선택지를 찾습니다.

</details>

### 5. Why can't a host rely on readOnlyHint for a security decision?

A. Because it defaults to true\
B. Because annotations are untrusted hints unless the server itself is trusted\
C. Because readOnlyHint only applies to resources\
D. Because the spec forbids hosts from reading annotations

**한국어**

host가 보안 판단에 `readOnlyHint`를 믿을 수 없는 이유는 무엇입니까?

A. 기본값이 true이기 때문에\
B. server 자체를 신뢰하지 않는 한 annotations는 신뢰할 수 없는 힌트이기 때문에\
C. `readOnlyHint`는 resource에만 적용되기 때문에\
D. 스펙이 host가 annotations를 읽지 못하게 하기 때문에

<details><summary>정답</summary>

B. Annotations are self-reported by the server. A malicious server can claim readOnlyHint: true while performing writes, so hosts must treat them as UX/policy hints only.

B. annotations는 server가 스스로 신고하는 값입니다. 악성 server는 쓰기를 하면서도 `readOnlyHint: true`라고 주장할 수 있습니다. 그래서 host는 이 값을 UX나 정책을 위한 힌트로만 다뤄야 합니다.

</details>

<details><summary>자세한 설명</summary>

tool annotations를 보안 근거로 쓸 수 있는지 묻는 문제입니다. annotations를 누가 적는지 생각하면 풀립니다.

annotations는 server가 tool 정의에 직접 적는 값입니다. host는 server가 실제로 그렇게 동작하는지 확인할 수 없습니다. 스펙은 server를 신뢰하는 경우가 아니면 annotations를 신뢰할 수 없는 값으로 다뤄야 한다(MUST)고 적습니다. 그래서 B가 정답입니다.

A는 사실과 반대입니다. `readOnlyHint` 기본값은 false입니다. C도 틀렸습니다. `readOnlyHint`는 tool에 붙는 annotation입니다. D도 틀렸습니다. 스펙은 host가 annotations를 읽는 것을 막지 않습니다. 힌트로 참고할 수는 있지만 보안 보증으로 믿으면 안 됩니다.

`destructiveHint`, `idempotentHint`, `openWorldHint`도 모두 힌트입니다. 기본값은 `destructiveHint`가 true, `idempotentHint`가 false, `openWorldHint`가 true입니다. tool 설명, icon, tool 결과도 같은 이유로 신뢰할 수 없는 입력으로 다룹니다.

</details>

### 6. A tool execution fails because an external API is down. What should the server return?

A. A JSON-RPC error with code -32603\
B. A normal result with isError: true and the error details in content\
C. An empty content array\
D. An UnsupportedProtocolVersionError

**한국어**

외부 API가 다운되어 tool 실행이 실패했습니다. server는 무엇을 반환해야 합니까?

A. 코드가 `-32603`인 JSON-RPC 오류\
B. `isError: true`와 `content` 안의 오류 세부 내용을 담은 일반 결과\
C. 빈 `content` 배열\
D. `UnsupportedProtocolVersionError`

<details><summary>정답</summary>

B. Execution failures are reported in-band with isError: true so the model can see the error text and self-correct; JSON-RPC errors are reserved for protocol-level problems like an unknown tool.

B. 실행 실패는 결과 안에 `isError: true`를 넣어(in-band) 알립니다. 그래야 모델이 오류 문구를 보고 스스로 고칠 수 있습니다. JSON-RPC 오류는 알 수 없는 tool 같은 프로토콜 수준 문제에만 씁니다.

</details>

<details><summary>자세한 설명</summary>

Error Handling 절 2번과 같은 내용을 다른 말로 묻는 문제입니다. 요청은 정상이었고 tool이 부른 외부 API가 실패했다는 점을 보면 풀립니다.

tool은 실행됐고 그 안의 작업이 실패했습니다. 이것은 tool 실행 오류입니다. server는 정상 결과에 `isError: true`를 넣고 `content`에 실패 이유를 적습니다. 그래서 B가 정답입니다.

A의 `-32603`은 server 자체가 실패했을 때 쓰는 프로토콜 오류입니다. 이 코드로 보내면 모델은 실패 이유를 보지 못합니다. C처럼 빈 `content`를 돌려주면 모델은 성공인지 실패인지 알 수 없습니다. D의 `-32022`는 프로토콜 버전이 맞지 않을 때 쓰는 오류이므로 이 상황과 관련이 없습니다.

오류 채널은 이렇게 선택합니다. 요청을 처리할 수 없으면 JSON-RPC `error`입니다. 처리는 했는데 작업이 실패했으면 `isError: true`입니다. 없는 tool은 `-32602`이고 스키마에 맞지 않는 인자는 `isError: true`라는 점도 같이 기억해 둡니다.

</details>

## Protocol Primitives

원본: `interactions/protocol-primitives.mdx`

### 1. Which primitive lets a server ask the user for missing information mid-operation?

A. Sampling\
B. Roots\
C. Elicitation\
D. Prompts

**한국어**

작업 도중 server가 사용자에게 빠진 정보를 요청할 수 있게 하는 primitive는 무엇입니까?

A. Sampling\
B. Roots\
C. Elicitation\
D. Prompts

<details><summary>정답</summary>

C. Elicitation (elicitation/create) requests input from the user via the client — in form mode or URL mode. The user can accept, decline, or cancel.

C. Elicitation(`elicitation/create`)은 client를 거쳐 사용자에게 입력을 요청합니다. 요청 방식에는 form 모드와 URL 모드가 있습니다. 사용자는 수락(`accept`), 거절(`decline`), 취소(`cancel`) 중 하나로 답합니다.

</details>

<details><summary>자세한 설명</summary>

client가 제공하는 기능 가운데 사용자에게 입력을 받는 것이 무엇인지 묻는 문제입니다. 각 primitive의 용도를 구분하면 풀립니다.

elicitation은 server가 작업에 필요한 정보를 사용자에게 받는 기능입니다. form 모드는 `requestedSchema`로 간단한 입력 양식을 정해 보냅니다. URL 모드는 사용자를 OAuth 동의나 결제 같은 외부 페이지로 보냅니다. 민감한 정보가 client를 거치지 않아야 할 때 URL 모드를 씁니다. 그래서 C가 정답입니다.

A의 sampling은 server가 client 쪽 LLM에게 생성을 요청하는 기능입니다. 사용자에게 정보를 묻는 기능이 아닙니다. B의 roots는 client가 server에게 작업할 디렉터리 범위를 알려 주는 기능입니다. D의 prompts는 server가 제공하는 메시지 템플릿이고 사용자가 골라 씁니다. server가 작업 도중 사용자에게 묻는 수단이 아닙니다.

2026-07-28에서는 elicitation이 전달되는 방식이 바뀌었습니다. server가 client에 요청을 먼저 보내지 않습니다. 대신 `tools/call` 같은 요청에 `resultType: "input_required"` 결과를 돌려주고 `inputRequests`에 `elicitation/create`를 담습니다. client는 사용자 답을 `inputResponses`에 넣어 원래 요청을 새 id로 다시 보냅니다. URL 모드의 `notifications/elicitation/complete`와 `elicitationId`는 2026-07-28에서 없어졌습니다. sampling과 roots는 deprecated됐지만 elicitation은 현행 기능입니다.

</details>

### 2. A client wants to read a document a server exposes. Which methods does it use?

A. tools/list then tools/call\
B. resources/list then resources/read\
C. prompts/list then prompts/get\
D. roots/list then resources/read

**한국어**

client가 server가 노출한 문서를 읽으려고 합니다. 어떤 메서드를 씁니까?

A. `tools/list` 다음 `tools/call`\
B. `resources/list` 다음 `resources/read`\
C. `prompts/list` 다음 `prompts/get`\
D. `roots/list` 다음 `resources/read`

<details><summary>정답</summary>

B. Resources are discovered with resources/list and fetched by URI with resources/read.

B. resource는 `resources/list`로 찾고 `resources/read`에 URI를 지정해 가져옵니다.

</details>

<details><summary>자세한 설명</summary>

server가 내놓은 데이터를 어느 primitive로 가져오는지 묻는 문제입니다. 문서나 파일 내용은 resource로 노출한다는 것을 알면 풀립니다.

client는 `resources/list`로 resource 목록과 각 URI를 받습니다. 그다음 `resources/read`에 URI를 넣어 내용을 가져옵니다. 내용은 `text`나 `blob`으로 옵니다. 그래서 B가 정답입니다.

A의 tool은 모델이 부르는 동작입니다. 문서를 읽는 tool을 만들 수는 있지만 server가 노출한 문서를 읽는 표준 경로는 resource입니다. C의 메서드는 prompt 템플릿을 가져올 때 씁니다. D의 `roots/list`는 server가 client에게 디렉터리 범위를 물을 때 쓰는 메서드입니다. server의 문서를 찾는 데 쓰지 않습니다. roots는 2026-07-28에서 deprecated되기도 했습니다.

없는 URI를 읽으면 `-32602`가 옵니다. 빈 `contents` 배열로 답하지 않습니다. 예전 코드 `-32002`는 2026-07-28 구현이 내보내면 안 됩니다. 다만 client는 옛 server가 보낸 `-32002`를 resource not found로 받아들이는 것이 좋습니다(SHOULD). URI 템플릿 목록은 `resources/templates/list`로 받습니다.

</details>

### 3. Who controls whether a sampling request is fulfilled?

A. The server that sent it\
B. The client/host, with human-in-the-loop review\
C. The model provider\
D. The registry

**한국어**

sampling 요청을 처리할지는 누가 결정합니까?

A. 요청을 보낸 server\
B. 사람의 검토를 거치는(human-in-the-loop) client나 host\
C. 모델 제공자\
D. registry

<details><summary>정답</summary>

B. The client keeps control over model access, selection, and permissions; users should be able to review and approve sampling. Servers never hold the API keys.

B. 모델 접근, 모델 선택, 권한은 client가 통제합니다. 사용자는 sampling을 검토하고 승인할 수 있어야 합니다(should). server는 API 키를 갖지 않습니다.

</details>

<details><summary>자세한 설명</summary>

sampling에서 통제권이 누구에게 있는지 묻는 문제입니다. sampling은 server가 client 쪽 LLM을 빌려 쓰는 기능이라는 것을 알면 풀립니다.

sampling에서 server는 LLM 생성을 요청만 합니다. 어떤 모델을 쓸지와 요청을 처리할지는 client가 정합니다. 스펙도 사람이 sampling 요청을 거부할 수 있어야 한다고 적습니다(SHOULD). 사용자가 보낼 prompt와 생성된 응답을 검토할 수 있는 화면을 두라고 권합니다. 그래서 B가 정답입니다.

A는 틀렸습니다. 요청을 보낸 server가 스스로 승인하면 통제가 되지 않습니다. C의 모델 제공자는 client가 고른 모델을 실행할 뿐입니다. D의 registry는 server 메타데이터를 모아 두는 곳입니다. 실행 중인 요청과는 관련이 없습니다.

2026-07-28 스펙 본문 기준으로는 sampling이 2026-07-28에서 deprecated됐습니다. 아직 동작하고 2027-07-28 이후 처음 나오는 개정부터 제거될 수 있습니다. brief가 적은 대안은 server가 LLM 제공자 API를 직접 부르는 것입니다. 그러면 server가 자기 API 키를 갖게 됩니다. 원 해설의 "server는 API 키를 갖지 않는다"는 sampling을 쓰는 경우에만 맞습니다. 또 2026-07-28에서는 sampling 요청도 MRTR의 `inputRequests`에 `sampling/createMessage`로 담겨 옵니다.

</details>

### 4. Which pairing of primitive and control model is correct?

A. Tools — user-controlled\
B. Resources — model-controlled\
C. Prompts — user-controlled\
D. Prompts — application-controlled

**한국어**

primitive와 통제 모델의 짝으로 옳은 것은 무엇입니까?

A. Tools: 사용자 통제\
B. Resources: 모델 통제\
C. Prompts: 사용자 통제\
D. Prompts: 애플리케이션 통제

<details><summary>정답</summary>

C. Tools are model-controlled, resources are application-driven, and prompts are user-controlled (e.g. slash commands).

C. tool은 모델이 통제하고 resource는 애플리케이션이 주도합니다. prompt는 사용자가 통제합니다. 슬래시 명령이 그 예입니다.

</details>

<details><summary>자세한 설명</summary>

server primitive 세 가지가 각각 누구의 통제를 받는지 묻는 문제입니다. 누가 그것을 쓰기로 정하는지 생각하면 풀립니다.

prompt는 사용자가 직접 골라 씁니다. 채팅 창에서 슬래시 명령으로 템플릿을 고르는 방식이 그 예입니다. 그래서 C가 정답입니다.

A는 틀렸습니다. tool은 모델이 대화 중에 골라 부르므로 모델 통제입니다. B도 틀렸습니다. resource는 애플리케이션이 어떤 내용을 쓸지 정하므로 애플리케이션 주도입니다. D는 prompt에 resource의 짝을 붙인 보기입니다.

tool은 모델, resource는 애플리케이션, prompt는 사용자로 기억해 둡니다. tool이 모델 통제라고 해서 사용자 동의가 빠지지는 않습니다. 호출 전 동의는 여전히 host가 받습니다.

</details>

### 5. In the 2026-07-28 revision, how does per-resource update subscription work?

A. resources/subscribe per URI\
B. A long-lived subscriptions/listen stream the client opts into\
C. Polling resources/list on a timer\
D. notifications/roots/list_changed

**한국어**

2026-07-28 개정판에서 resource별 업데이트 구독은 어떻게 동작합니까?

A. URI마다 `resources/subscribe`를 보냅니다\
B. client가 받을 항목을 골라(opt-in) 여는 장기 `subscriptions/listen` 스트림\
C. 타이머로 `resources/list`를 폴링합니다\
D. `notifications/roots/list_changed`

<details><summary>정답</summary>

B. resources/subscribe and resources/unsubscribe were removed in 2026-07-28; clients open subscriptions/listen and opt into resource updates and list-change notifications.

B. 2026-07-28에서 `resources/subscribe`와 `resources/unsubscribe`는 제거됐습니다. client는 `subscriptions/listen`을 열고 resource 업데이트와 목록 변경 notification 가운데 받을 것을 고릅니다(opt-in).

</details>

<details><summary>자세한 설명</summary>

2026-07-28에서 바뀐 구독 방식을 묻는 문제입니다. 옛 `resources/subscribe`가 없어지고 `subscriptions/listen`이 생겼다는 것을 알면 풀립니다.

client는 `subscriptions/listen` 요청으로 오래 열려 있는 스트림을 엽니다. `params.notifications`에는 받을 종류를 적습니다. 특정 resource의 변경을 받으려면 `resourceSubscriptions`에 URI 목록을 넣습니다. server는 첫 메시지로 `notifications/subscriptions/acknowledged`를 보내 실제로 지원할 범위를 알려 줍니다. 이후 변경이 생기면 `notifications/resources/updated`가 옵니다. 그래서 B가 정답입니다.

A의 `resources/subscribe`는 2026-07-28에서 제거된 메서드입니다. C의 폴링은 스펙이 정한 구독 방식이 아닙니다. 목록 응답의 `ttlMs`도 캐시 유효 시간에 대한 힌트이고 폴링 주기가 아닙니다. D의 `notifications/roots/list_changed`는 roots 목록이 바뀌었을 때 client가 보내던 notification입니다. 이것도 2026-07-28에서 제거됐습니다.

주의할 점이 1가지 더 있습니다. server capability에는 여전히 `resources {listChanged, subscribe}`가 있습니다. capability에 `subscribe`가 남아 있다고 해서 `resources/subscribe` 메서드가 남은 것은 아닙니다. 또 `notifications/progress`와 로그 notification은 listen 스트림으로 오지 않습니다. 해당 요청의 응답 스트림으로만 옵니다.

</details>

### 6. Which client-offered primitives were deprecated in the 2026-07-28 revision?

A. Elicitation and Sampling\
B. Roots and Elicitation\
C. Sampling and Roots\
D. None — all remain current

**한국어**

2026-07-28 개정판에서 deprecated된 client 제공 primitive는 무엇입니까?

A. Elicitation과 Sampling\
B. Roots와 Elicitation\
C. Sampling과 Roots\
D. 없음. 모두 현행입니다

<details><summary>정답</summary>

C. Sampling and Roots were deprecated (SEP-2577); Elicitation remains a current client primitive.

C. Sampling과 Roots가 deprecated됐습니다(SEP-2577). Elicitation은 현행 client primitive로 남아 있습니다.

</details>

<details><summary>자세한 설명</summary>

2026-07-28의 폐기 예정 목록을 묻는 문제입니다. client가 제공하는 기능은 elicitation, sampling, roots이고 그중 elicitation만 현행이라는 것을 알면 풀립니다.

SEP-2577은 Roots, Sampling, Logging을 deprecated로 지정했습니다. deprecated는 폐기 예정이라는 뜻이고 아직 동작합니다. 빨라야 2027-07-28 이후 처음 나오는 개정에서 제거될 수 있습니다. 그래서 C가 정답입니다.

A와 B는 elicitation을 넣었으므로 틀렸습니다. elicitation은 2025-11-25에 URL 모드가 추가됐고 2026-07-28에서도 MRTR을 통해 계속 씁니다. D는 deprecated된 것이 없다고 하므로 틀렸습니다.

removed와 deprecated는 서로 다릅니다. Roots, Sampling, Logging은 제거되지 않았고 deprecated 상태입니다. 반면 `logging/setLevel`과 `notifications/roots/list_changed` 메서드는 2026-07-28에서 제거됐습니다. Dynamic Client Registration도 같은 개정에서 deprecated됐습니다.

</details>

## Trust Boundaries

원본: `security/trust-boundaries.mdx`

### 1. Why are tool descriptions considered untrusted input?

A. They are compressed and may decode incorrectly\
B. They come from third-party server code and can carry injected instructions\
C. The spec requires them to be empty\
D. They are generated by the LLM

**한국어**

tool 설명을 신뢰할 수 없는 입력으로 보는 이유는 무엇입니까?

A. 압축되어 있어 잘못 디코딩될 수 있기 때문에\
B. 제3자 server 코드에서 오며 주입된 지시를 담을 수 있기 때문에\
C. 스펙이 비워 두도록 요구하기 때문에\
D. LLM이 생성하기 때문에

<details><summary>정답</summary>

B. Tool descriptions and annotations are authored by the server. Unless the server is trusted, they must be treated as potentially adversarial content that could manipulate the model.

B. tool 설명과 annotations는 server가 작성합니다. server를 신뢰하지 않는다면 이 내용을 모델을 조작할 수 있는 적대적 콘텐츠로 보고 다뤄야 합니다.

</details>

<details><summary>자세한 설명</summary>

tool 설명이 왜 보안 위험이 되는지 묻는 문제입니다. 설명을 누가 쓰고 누가 읽는지 생각하면 풀립니다.

tool 설명은 server 개발자가 씁니다. 그리고 이 설명은 모델에게 입력으로 들어갑니다. 설명 안에 숨긴 지시가 있으면 모델이 그 지시를 따를 수 있습니다. 이런 공격을 tool poisoning이나 prompt injection이라고 부릅니다. 그래서 B가 정답입니다.

A는 사실이 아닙니다. tool 설명은 일반 문자열이고 압축과 관련이 없습니다. C도 틀렸습니다. 스펙은 설명을 비우라고 하지 않습니다. 모델이 tool을 고르려면 설명이 필요합니다. D는 출처를 잘못 짚었습니다. 설명은 LLM이 만들지 않고 server가 만듭니다.

신뢰할 수 없는 입력은 설명만이 아닙니다. brief는 annotations, icon, tool 결과도 같은 범주에 넣습니다. 처음에는 무해했던 tool 정의를 나중에 바꾸는 rug pull도 같은 위험입니다. `clientInfo`와 `serverInfo`도 스스로 신고한 값이므로 보안 판단에 쓰지 않습니다.

</details>

### 2. Which component is responsible for isolating one server's data from another?

A. The server itself\
B. The LLM\
C. The host\
D. The external API

**한국어**

한 server의 데이터를 다른 server로부터 격리할 책임은 어느 구성 요소에 있습니까?

A. server 자신\
B. LLM\
C. host\
D. 외부 API

<details><summary>정답</summary>

C. The host is the policy enforcement point: it maintains isolation between servers, gates consent, and controls permissions. Servers cannot see into other servers.

C. host가 정책을 집행하는 지점입니다. host는 server 사이의 격리를 유지하고 동의 절차를 관리하며 권한을 통제합니다. server는 다른 server의 내부를 볼 수 없습니다.

</details>

<details><summary>자세한 설명</summary>

MCP 구성 요소 가운데 보안 정책을 집행하는 쪽이 어디인지 묻는 문제입니다. host, client, server가 각각 무슨 일을 하는지 알면 풀립니다.

host는 사용자가 쓰는 AI 애플리케이션이고 여러 server에 연결합니다. 스펙 아키텍처 문서는 host가 보안 정책과 동의 요건을 집행한다고 적습니다. 같은 문서의 설계 원칙에 따르면 server는 대화 전체를 읽을 수 없고 다른 server의 내부도 볼 수 없어야 합니다. 이 격리를 지키는 쪽이 host입니다. C가 정답입니다.

A는 틀렸습니다. server는 자기 바깥을 볼 수 없으므로 다른 server와의 격리를 담당할 위치에 있지 않습니다. 악성 server가 섞여 있다면 그 server에게 격리를 맡길 수도 없습니다. B의 LLM은 tool 호출을 제안할 뿐 정책을 집행하지 않습니다. D의 외부 API는 MCP 구성 요소가 아닙니다.

동의를 받는 주체와 권한을 통제하는 주체는 모두 host입니다. server도 입력 검증, 접근 통제, rate limiting은 해야 합니다(MUST). 하지만 server 사이의 경계와 사용자 동의는 host의 일입니다.

</details>

### 3. How should a tool call proposed by the LLM be treated?

A. As a command to execute immediately\
B. As a request subject to host policy and explicit user consent\
C. As trusted because it came from the model\
D. As a server-side error

**한국어**

LLM이 제안한 tool 호출은 어떻게 다뤄야 합니까?

A. 즉시 실행할 명령으로\
B. host 정책과 명시적인 사용자 동의를 거쳐야 하는 요청으로\
C. 모델에서 왔으므로 신뢰할 수 있는 것으로\
D. server 측 오류로

<details><summary>정답</summary>

B. Model output is data, not commands. The host must obtain explicit user consent before invoking any tool.

B. 모델 출력은 명령이 아니라 데이터입니다. host는 어떤 tool이든 호출하기 전에 사용자에게 명시적인 동의를 받아야 합니다.

</details>

<details><summary>자세한 설명</summary>

모델이 낸 tool 호출 제안을 어떻게 다룰지 묻는 문제입니다. 모델 출력도 그대로 믿을 수 없는 입력이라는 것을 알면 풀립니다.

모델은 tool 설명이나 tool 결과에 섞인 주입 지시의 영향을 받을 수 있습니다. 그래서 모델이 낸 호출 제안은 바로 실행할 명령이 아닙니다. host는 정책을 확인하고 사용자에게 보여 준 뒤 실행합니다. 그래서 B가 정답입니다.

A와 C는 같은 이유로 틀렸습니다. 모델 출력을 그대로 믿고 실행하면 prompt injection이 곧바로 실제 동작으로 이어집니다. D는 상황을 잘못 짚었습니다. 아직 server에 요청을 보내지도 않았으므로 server 오류가 생길 수 없습니다.

스펙 본문의 Security and Trust & Safety 원칙에는 "Hosts must obtain explicit user consent before invoking any tool"이라는 문장이 있습니다. host는 tool을 호출하기 전에 사용자의 명시적 동의를 받아야 한다는 뜻입니다. 다만 소문자 must라서 RFC 키워드로 정한 의무 규정이 아니라 원칙입니다. 스펙도 MCP가 프로토콜 수준에서 이 원칙을 지키게 할 수는 없다고 적습니다. 동의를 host가 받는다는 점과 호출 전에 받는다는 점을 확실히 기억해 둡니다.

</details>

### 4. What does the MCP spec say about authorization on stdio transports?

A. They MUST implement OAuth 2.1\
B. They SHOULD NOT use the OAuth authorization spec and should retrieve credentials from the environment\
C. They must use session cookies\
D. They require Dynamic Client Registration

**한국어**

MCP 스펙은 stdio transport의 인가에 대해 무엇이라고 합니까?

A. OAuth 2.1을 구현해야 합니다(MUST)\
B. OAuth 인가 스펙을 쓰지 않아야 하며(SHOULD NOT) 자격 증명을 환경에서 가져옵니다\
C. 세션 쿠키를 써야 합니다\
D. Dynamic Client Registration이 필요합니다

<details><summary>정답</summary>

B. OAuth 2.1-based authorization applies to HTTP-based transports; stdio servers should instead get credentials from the environment (e.g., environment variables).

B. OAuth 2.1 기반 인가는 HTTP 기반 transport에 적용됩니다. stdio server는 대신 환경 변수 같은 환경에서 자격 증명을 가져와야 합니다.

</details>

<details><summary>자세한 설명</summary>

transport에 따라 인가 방식이 다르다는 것을 아는지 묻는 문제입니다.

MCP의 인가 스펙은 HTTP transport를 위한 것입니다. stdio server는 client가 같은 컴퓨터에서 하위 프로세스로 띄웁니다. 이런 server는 OAuth 흐름을 쓰지 않는 것이 좋습니다(SHOULD NOT). 대신 환경 변수에서 자격 증명을 읽습니다. 그래서 B가 정답입니다.

A는 키워드가 틀렸습니다. 인가는 전체적으로 선택 사항이고 HTTP 구현도 따르는 것이 좋다(SHOULD)는 수준입니다. stdio에 OAuth를 MUST로 요구하지 않습니다. C의 세션 쿠키는 MCP 인가 방식이 아닙니다. 2026-07-28에는 프로토콜 세션 자체가 없습니다. D의 Dynamic Client Registration은 HTTP 인가에서 client를 등록하는 방법입니다. 이 방법도 2026-07-28에서 deprecated됐습니다.

"stdio server도 OAuth로 보호해야 한다"는 설명은 틀립니다. SHOULD NOT을 MUST NOT으로 바꾼 설명도 틀립니다. 원문은 권고이고 금지가 아닙니다.

</details>

### 5. An MCP server receives a bearer token from its client and forwards it to a downstream API. What is this?

A. A recommended pattern for single sign-on\
B. Token passthrough — an explicitly forbidden anti-pattern\
C. Required by RFC 8707\
D. Only allowed for local servers

**한국어**

MCP server가 client에게서 받은 bearer token을 하위(downstream) API로 그대로 넘깁니다. 이것은 무엇입니까?

A. SSO(single sign-on)에 권장되는 패턴\
B. Token passthrough. 명시적으로 금지된 안티패턴\
C. RFC 8707이 요구하는 것\
D. 로컬 server에만 허용되는 것

<details><summary>정답</summary>

B. Token passthrough breaks trust boundaries and audit trails. Servers MUST NOT accept tokens that were not explicitly issued for them; upstream calls need a separate token obtained by the server as its own OAuth client.

B. Token passthrough는 신뢰 경계와 감사 추적을 깨뜨립니다. server는 자신에게 명시적으로 발급되지 않은 token을 받아들이면 안 됩니다(MUST NOT). 상위(upstream) API를 부를 때는 server가 자기 OAuth client 자격으로 따로 받은 token이 필요합니다.

</details>

<details><summary>자세한 설명</summary>

받은 token을 그대로 다른 API에 넘기는 행위가 무엇인지 묻는 문제입니다. token passthrough 금지 규칙을 알면 풀립니다.

client가 MCP server에 보낸 token은 그 MCP server를 위해 발급된 것입니다. server가 이 token을 다른 API에 그대로 쓰면 그 API는 엉뚱한 대상용 token을 받게 됩니다. 스펙은 server가 token의 audience를 검증해야 한다(MUST)고 정합니다. audience는 token이 어느 서비스를 위해 발급됐는지를 나타냅니다. 또 server는 다른 token을 받거나 넘기면 안 됩니다(MUST NOT). 그래서 B가 정답입니다.

A는 틀렸습니다. 한 token을 여러 서비스에 돌려 쓰는 것은 권장 패턴이 아니고 금지된 패턴입니다. C도 틀렸습니다. RFC 8707 Resource Indicators는 token을 특정 server에 묶는 규칙이고 passthrough를 막는 쪽에 있습니다. D도 틀렸습니다. 금지 규칙에 로컬 server 예외는 없습니다.

상위 API를 불러야 한다면 server는 그 API용 token을 따로 받습니다. 이때 server는 그 API에 대해 OAuth client가 됩니다. "token이 유효하면 그대로 써도 된다"는 설명은 틀립니다. token이 유효한지보다 이 server용으로 발급됐는지가 중요합니다.

</details>

## Permissions & Consent

원본: `security/permissions-and-consent.mdx`

### 1. Which participant is responsible for obtaining user consent?

A. The MCP server\
B. The host application\
C. The authorization server\
D. The LLM

**한국어**

사용자 동의를 받을 책임은 어느 참여자에게 있습니까?

A. MCP server\
B. host 애플리케이션\
C. authorization server\
D. LLM

<details><summary>정답</summary>

B. The host presents consent UIs and enforces decisions. Servers never present consent prompts; the protocol itself cannot enforce consent.

B. host가 동의 UI를 보여 주고 결정을 집행합니다. server는 동의 프롬프트를 띄우지 않습니다. 프로토콜만으로는 동의를 보장할 수 없습니다.

</details>

<details><summary>자세한 설명</summary>

동의를 받는 주체가 누구인지 묻는 문제입니다. 사용자 화면을 가진 쪽이 host라는 것을 알면 풀립니다.

사용자가 보는 화면은 host에 있습니다. 그래서 tool 호출 확인 창이나 elicitation 입력 양식을 보여 주는 쪽도 host입니다. 스펙도 MCP가 프로토콜 수준에서 동의 같은 보안 원칙을 지키게 할 수는 없다고 적습니다. 결정을 집행하는 것도 host입니다. 그래서 B가 정답입니다.

A는 틀렸습니다. server는 사용자 화면에 직접 접근하지 못합니다. 정보가 필요하면 elicitation을 요청할 뿐이고 그것을 사용자에게 보여 줄지는 host가 정합니다. C의 authorization server는 OAuth 흐름에서 token을 발급하는 쪽입니다. tool 호출에 대한 동의를 책임지지 않습니다. D의 LLM은 호출을 제안할 뿐입니다.

스펙 본문 기준으로는 "server는 동의 프롬프트를 띄우지 않는다"는 원 해설에 예외가 있습니다. 정적 client id를 쓰는 MCP proxy는 동적으로 등록된 client마다 사용자 동의를 받은 뒤에 제3자 authorization server로 넘겨야 합니다(MUST). 이 규칙은 confused deputy 문제를 막기 위한 것입니다. confused deputy는 권한을 가진 중간자가 속아서 다른 사람 대신 그 권한을 쓰는 문제입니다. 일반 원칙에서는 동의를 받는 쪽이 host입니다.

</details>

### 2. In MCP authorization, which OAuth 2.1 role does the MCP server play?

A. Authorization server\
B. OAuth client\
C. Resource server\
D. Resource owner

**한국어**

MCP 인가에서 MCP server는 OAuth 2.1의 어떤 역할을 맡습니까?

A. Authorization server\
B. OAuth client\
C. Resource server\
D. Resource owner

<details><summary>정답</summary>

C. The MCP server is the resource server; the MCP client is the OAuth client acting on behalf of the resource owner, and a separate (or co-hosted) authorization server issues tokens.

C. MCP server는 resource server입니다. MCP client는 resource owner를 대신해 동작하는 OAuth client입니다. token은 별도의 authorization server가 발급합니다. 이 authorization server는 MCP server와 같은 곳에 둘 수도 있습니다.

</details>

<details><summary>자세한 설명</summary>

MCP 인가에서 각 구성 요소가 OAuth의 어떤 역할에 해당하는지 묻는 문제입니다. 2025-06-18 개정에서 MCP server를 resource server로 분류했다는 것을 알면 풀립니다.

resource server는 보호된 자원을 갖고 있고 token을 확인해 요청을 받아 주는 쪽입니다. MCP server가 여기에 해당합니다. MCP server는 Protected Resource Metadata(RFC 9728)를 제공해야 합니다(MUST). 이 메타데이터의 `authorization_servers`에 어느 authorization server를 쓰는지 적습니다. 그래서 C가 정답입니다.

A의 authorization server는 token을 발급하는 쪽입니다. MCP server와 같은 곳에 둘 수는 있지만 역할은 다릅니다. B의 OAuth client는 MCP client입니다. D의 resource owner는 권한을 위임하는 사용자입니다.

이 역할 구분은 다른 규칙과도 이어집니다. MCP server는 resource server이므로 token의 audience를 검증합니다. OAuth client인 MCP client는 PKCE를 반드시 쓰고 `resource` 파라미터를 보냅니다. token이 없거나 잘못됐으면 server는 `401`을 돌려주고 `WWW-Authenticate` 헤더로 메타데이터 위치를 알려 줍니다. scope가 모자라면 `403`입니다.

</details>

### 3. Why must an MCP server reject a token that was issued for a different service?

A. Tokens for other services are always expired\
B. Accepting foreign tokens enables token passthrough, breaking audit trails and trust boundaries\
C. OAuth 2.1 forbids more than one token per user\
D. The token would be too large for the Authorization header

**한국어**

MCP server가 다른 서비스용으로 발급된 token을 거부해야 하는 이유는 무엇입니까?

A. 다른 서비스용 token은 항상 만료되어 있기 때문에\
B. 다른 서비스의 token을 받으면 token passthrough가 가능해져 감사 추적과 신뢰 경계가 깨지기 때문에\
C. OAuth 2.1이 사용자 한 명에게 token을 두 개 이상 주지 못하게 하기 때문에\
D. token이 Authorization 헤더에 넣기에 너무 크기 때문에

<details><summary>정답</summary>

B. Token passthrough is an explicitly forbidden anti-pattern: it circumvents rate limiting and validation, breaks accountability, and lets one compromised service unlock others. Servers must validate the token audience.

B. Token passthrough는 명시적으로 금지된 안티패턴입니다. rate limiting과 검증을 우회하게 하고 책임 추적을 깨뜨립니다. 또 한 서비스가 뚫리면 다른 서비스까지 열리게 합니다. server는 token의 audience를 검증해야 합니다.

</details>

<details><summary>자세한 설명</summary>

audience 검증을 왜 하는지 묻는 문제입니다. token passthrough가 어떤 피해를 주는지 알면 풀립니다.

server가 audience를 확인하지 않으면 다른 서비스용 token도 받아들이게 됩니다. 그러면 누가 어떤 권한으로 요청했는지 기록이 흐려집니다. 한 서비스에서 새어 나간 token으로 다른 서비스에 들어갈 수도 있습니다. 스펙의 Security Best Practices는 rate limiting이나 요청 검증 같은 보안 통제를 우회하는 위험도 함께 듭니다. 그래서 B가 정답입니다.

A는 "always"가 틀렸습니다. 다른 서비스용 token도 유효 기간 안에 있을 수 있습니다. 그래서 만료 확인만으로는 부족하고 audience를 따로 봐야 합니다. C는 OAuth 2.1에 없는 규칙입니다. D는 token 크기 문제로 문제를 바꾼 보기이고 이 상황과 관련이 없습니다.

"token이 유효하면 받아도 된다"는 설명은 틀립니다. 유효성과 audience는 별개입니다. server는 audience를 검증해야 하고(MUST) 자기용이 아닌 token은 받지도 넘기지도 않습니다(MUST NOT).

</details>

### 4. How does an MCP client ensure a token is audience-bound to a specific server?

A. By including the resource parameter (RFC 8707) with the server's canonical URI in authorization and token requests\
B. By hashing the token with the server name\
C. By registering the token in the MCP Registry\
D. By sending the token in the URL query string

**한국어**

MCP client는 token의 audience가 특정 server로 묶이도록 어떻게 보장합니까?

A. 인가 요청과 token 요청에 server의 canonical URI를 담은 `resource` 파라미터(RFC 8707)를 넣습니다\
B. token을 server 이름과 함께 해시합니다\
C. token을 MCP Registry에 등록합니다\
D. token을 URL 쿼리 문자열에 담아 보냅니다

<details><summary>정답</summary>

A. RFC 8707 Resource Indicators require the client to send a resource parameter identifying the MCP server; the server then validates it is the intended audience. Tokens must never appear in the query string.

A. RFC 8707 Resource Indicators에 따라 client는 MCP server를 가리키는 `resource` 파라미터를 보내야 합니다. 그러면 server는 자신이 의도된 audience인지 검증합니다. token은 절대 쿼리 문자열에 넣으면 안 됩니다.

</details>

<details><summary>자세한 설명</summary>

token을 특정 server에 묶는 방법을 묻는 문제입니다. RFC 8707 Resource Indicators를 알면 풀립니다.

client는 인가 요청과 token 요청 두 곳 모두에 `resource` 파라미터를 넣습니다. 값은 MCP server의 canonical URI입니다. canonical URI는 scheme과 host를 담은 표준 주소이고 fragment는 넣지 않습니다. authorization server가 이 파라미터를 무시하더라도 client는 보냅니다. server는 받은 token이 자신을 위해 발급됐는지 검증합니다. 그래서 A가 정답입니다.

B의 해시는 MCP나 OAuth가 정한 방법이 아닙니다. C의 MCP Registry는 server 메타데이터를 모아 두는 곳이고 token을 다루지 않습니다. D는 금지된 방법입니다. token은 매 HTTP 요청의 `Authorization: Bearer` 헤더에 넣고 쿼리 문자열에는 넣지 않습니다.

`resource` 파라미터는 인가 요청뿐 아니라 token 요청에도 넣어야 합니다. resource indicator는 2025-06-18 개정에서 필수가 됐습니다. PKCE도 필수이고 가능하면 `S256`을 씁니다.

</details>

### 5. How should a stdio-transported MCP server obtain credentials?

A. Via the OAuth 2.1 authorization flow\
B. From the environment — stdio transports SHOULD NOT use the HTTP authorization spec\
C. By eliciting the user's password through form-mode elicitation\
D. By reusing the host's tokens

**한국어**

stdio transport를 쓰는 MCP server는 자격 증명을 어떻게 얻어야 합니까?

A. OAuth 2.1 인가 흐름으로\
B. 환경에서. stdio transport는 HTTP 인가 스펙을 쓰지 않아야 합니다(SHOULD NOT)\
C. form 모드 elicitation으로 사용자 비밀번호를 받아서\
D. host의 token을 재사용해서

<details><summary>정답</summary>

B. The authorization spec targets HTTP transports. Stdio servers retrieve credentials from the environment. Elicitation must never be used to collect passwords or API keys in form mode.

B. 인가 스펙은 HTTP transport를 대상으로 합니다. stdio server는 환경에서 자격 증명을 가져옵니다. form 모드 elicitation으로 비밀번호나 API 키를 받아서는 절대 안 됩니다.

</details>

<details><summary>자세한 설명</summary>

Trust Boundaries 절 4번과 같은 규칙을 server 입장에서 묻는 문제입니다. stdio server는 환경 변수에서 자격 증명을 읽는다는 것을 알면 풀립니다.

stdio server는 같은 컴퓨터에서 하위 프로세스로 실행됩니다. 이런 server는 HTTP용 OAuth 흐름을 쓰지 않는 것이 좋습니다(SHOULD NOT). 대신 환경 변수에서 자격 증명을 읽습니다. 그래서 B가 정답입니다.

A는 HTTP transport에 맞는 방법입니다. C는 금지된 방법입니다. 스펙은 server가 form 모드 elicitation으로 비밀번호, API 키, access token, 결제 정보를 요청하면 안 된다고 적습니다(MUST NOT). 이런 정보는 URL 모드로 받아야 합니다(MUST). D는 token passthrough와 같은 문제입니다. server는 자기용으로 발급되지 않은 token을 쓰면 안 됩니다.

원문은 SHOULD NOT이므로 MUST NOT으로 바꾼 설명은 틀립니다. 인가 스펙이 적용되는 것은 HTTP transport입니다.

</details>

### 6. Which statement about the roots primitive and least privilege is accurate?

A. Roots cryptographically enforce filesystem boundaries\
B. Roots are mandatory for all servers\
C. Roots are informational guidance, not an enforced access control, and are deprecated in the 2026-07-28 revision\
D. Roots replace OAuth scopes for remote servers

**한국어**

roots primitive와 최소 권한에 대한 설명으로 정확한 것은 무엇입니까?

A. roots는 파일 시스템 경계를 암호학적으로 지키게 합니다\
B. roots는 모든 server에 필수입니다\
C. roots는 실제로 적용되는 접근 통제가 아니라 참고용 안내이고 2026-07-28 개정판에서 deprecated됐습니다\
D. roots는 원격 server에서 OAuth scope를 대체합니다

<details><summary>정답</summary>

C. The protocol never enforced that servers stay within roots — they were guidance only — and the 2026-07-28 revision deprecates them in favor of passing paths via tool parameters or server configuration.

C. 프로토콜은 server가 roots 밖으로 나가는 것을 막은 적이 없습니다. roots는 안내일 뿐이었습니다. 2026-07-28 개정판은 roots를 deprecated하고 대신 tool 파라미터나 server 설정으로 경로를 넘기게 합니다.

</details>

<details><summary>자세한 설명</summary>

roots의 성격과 2026-07-28에서의 상태를 함께 묻는 문제입니다. roots가 보안 장치가 아니라는 점과 deprecated됐다는 점을 알면 풀립니다.

roots는 client가 server에게 작업할 디렉터리 범위를 알려 주는 기능입니다. 스펙은 roots가 접근 제어 수단이 아니라 참고 정보라고 적습니다. server가 roots 범위 안에 머무는지 프로토콜이 확인하지 않습니다. 그래서 최소 권한을 roots에만 맡길 수 없습니다. roots는 SEP-2577로 2026-07-28에서 폐기 예정이 됐습니다. 그래서 C가 정답입니다.

A는 틀렸습니다. roots는 안내일 뿐이고 아무것도 막지 않습니다. B도 틀렸습니다. roots는 client가 capability로 선언할 때만 쓰는 기능입니다. D도 틀렸습니다. OAuth scope는 HTTP 인가에서 권한 범위를 정하는 값이고 roots와 관련이 없습니다.

brief가 적은 이전 경로는 tool 파라미터, resource URI, server 설정입니다. deprecated이므로 아직 동작합니다. 2027-07-28 이후 처음 나오는 개정부터 제거될 수 있습니다. `notifications/roots/list_changed`는 이미 제거됐고 `roots/list`는 MRTR의 `inputRequests` 안에서만 남아 있습니다. "roots는 제거됐다"는 설명은 틀립니다.

</details>

## Risk & Safety Controls

원본: `security/risk-and-safety-controls.mdx`

### 1. A server accepts a token that was issued for a different service and forwards it to a downstream API. What is this called?

A. Confused deputy\
B. Token passthrough\
C. Rug pull\
D. Session hijacking

**한국어**

server가 다른 서비스용으로 발급된 token을 받아 하위(downstream) API로 넘깁니다. 이것을 무엇이라고 합니까?

A. Confused deputy\
B. Token passthrough\
C. Rug pull\
D. Session hijacking

<details><summary>정답</summary>

B. Token passthrough is an explicitly forbidden anti-pattern: MCP servers MUST NOT accept any tokens that were not explicitly issued for them.

B. Token passthrough는 명시적으로 금지된 안티패턴입니다. MCP server는 자신에게 명시적으로 발급되지 않은 token을 어떤 것도 받아서는 안 됩니다(MUST NOT).

</details>

<details><summary>자세한 설명</summary>

server가 자기용이 아닌 token을 받아 다른 API로 그대로 넘기는 행위의 이름을 묻는 문제입니다. 인가 규칙의 audience 검증과 token passthrough 금지를 알면 풀립니다.

audience는 token이 어느 서비스를 위해 발급됐는지를 뜻합니다. server는 받은 token의 audience가 자기 자신인지 확인해야 합니다(MUST). 자기용이 아닌 token은 받지도 넘기지도 않습니다. 하위 API를 부를 때는 그 API용으로 따로 받은 token을 씁니다. 받은 token을 그대로 넘기는 것이 token passthrough이므로 B가 정답입니다.

Confused deputy는 정적 client ID를 쓰는 MCP 프록시 server에서 생기는 공격입니다. 이전 동의를 악용해 authorization code를 가로채는 방식이라 A는 답이 아닙니다. Rug pull은 승인된 server가 나중에 tool 정의를 바꾸는 공격이므로 C도 답이 아닙니다. Session hijacking은 세션 ID를 가로채는 공격입니다. 2026-07-28에서는 프로토콜 세션 자체가 없어졌으므로 D도 답이 아닙니다.

"유효한 token이면 받아도 된다"거나 "server가 사용자 token을 상위 API로 넘겨도 된다"는 설명은 둘 다 틀립니다. token이 유효한지보다 이 server를 위해 발급됐는지가 먼저입니다. audience를 지정할 때는 RFC 8707 Resource Indicators의 `resource` 파라미터를 씁니다.

</details>

### 2. Which setup is the target of the confused deputy attack described in the MCP security best practices?

A. A local stdio server with filesystem access\
B. An MCP proxy server using a static client ID with a third-party authorization server\
C. A client that stores refresh tokens in plaintext\
D. A registry hosting unvetted server packages

**한국어**

MCP 보안 모범 사례에서 설명하는 confused deputy 공격은 어떤 구성을 노립니까?

A. 파일 시스템에 접근하는 로컬 stdio server\
B. 제3자 authorization server에 정적 client ID를 쓰는 MCP 프록시 server\
C. refresh token을 평문으로 저장하는 client\
D. 검증되지 않은 server 패키지를 올려 둔 registry

<details><summary>정답</summary>

B. The attack abuses the third-party auth server's consent cookie: after one legitimate consent, a maliciously registered client can get the consent screen skipped and steal the authorization code. Proxies MUST obtain per-client user consent.

B. 이 공격은 제3자 auth server의 동의 쿠키를 악용합니다. 정상 동의가 한 번 이뤄지고 나면 악의적으로 등록한 client가 동의 화면을 건너뛰게 만들 수 있습니다. 그렇게 해서 authorization code를 훔칩니다. 프록시는 client마다 사용자 동의를 받아야 합니다(MUST).

</details>

<details><summary>자세한 설명</summary>

confused deputy 공격이 어떤 구성에서 생기는지 묻는 문제입니다. 프록시 server, 정적 client ID, 제3자 authorization server라는 세 조건을 기억하면 풀립니다.

MCP 프록시 server는 하나의 정적 client ID로 제3자 authorization server에 접속합니다. 그런데 프록시 앞에는 여러 MCP client가 동적으로 등록됩니다. 사용자가 정상 client로 한 번 동의하면 authorization server는 그 동의를 쿠키로 기억합니다. 이후 공격자가 등록한 client가 같은 흐름을 타면 동의 화면이 생략됩니다. 그러면 authorization code가 공격자에게 넘어갑니다.

프록시가 하나의 client ID로 여러 client를 대신하면서 권한을 잘못 빌려주는 구조이므로 B가 정답입니다. 막는 방법은 프록시가 동적으로 등록된 client마다 사용자 동의를 따로 받는 것입니다(MUST).

로컬 stdio server는 OAuth를 쓰지 않고 환경 변수에서 자격 증명을 읽습니다. 그래서 A는 이 공격과 관계가 없습니다. refresh token 평문 저장은 token 보관 문제입니다. 검증되지 않은 registry 패키지는 공급망 문제입니다. 둘 다 confused deputy와 다른 위협이라 C와 D도 답이 아닙니다.

confused deputy와 token passthrough도 구분해 둡니다. 동의 쿠키와 정적 client ID가 얽힌 문제는 confused deputy입니다. 다른 서비스용 token을 받아 넘기는 문제는 token passthrough입니다.

</details>

### 3. A previously approved server silently changes its tool definitions to include malicious instructions. This is best described as:

A. Prompt injection\
B. SSRF\
C. A rug pull\
D. Token theft

**한국어**

이전에 승인된 server가 몰래 tool 정의를 바꿔 악성 지시를 넣었습니다. 이것을 가장 잘 설명하는 것은 무엇입니까?

A. Prompt injection\
B. SSRF\
C. Rug pull\
D. Token 탈취

<details><summary>정답</summary>

C. A rug pull is a post-approval change to tool definitions. The mitigation is re-prompting for consent when definitions change and pinning versions.

C. rug pull은 승인 뒤에 tool 정의를 바꾸는 것입니다. 정의가 바뀌면 동의를 다시 묻고 버전을 고정(pinning)해서 위험을 줄입니다.

</details>

<details><summary>자세한 설명</summary>

승인 뒤에 tool 정의가 바뀌는 공격의 이름을 묻는 문제입니다. 문제의 "이전에 승인된"이라는 조건을 보면 풀립니다.

사용자는 처음 연결할 때 tool 설명을 보고 server를 승인합니다. rug pull은 그 뒤에 server가 정의를 바꿔 악성 지시를 넣는 공격입니다. 이미 승인한 server라서 사용자가 알아채기 어렵습니다. 그래서 C가 정답입니다.

Prompt injection은 모델이 읽는 입력에 악성 지시를 섞는 공격을 넓게 부르는 말입니다. 이 문제처럼 승인 뒤에 정의를 바꾸는 경우는 따로 rug pull이라고 부르므로 A보다 C가 더 정확합니다. SSRF는 내부 주소로 요청을 보내게 만드는 공격이라 B는 답이 아닙니다. token 탈취는 자격 증명을 훔치는 공격이므로 D도 답이 아닙니다.

2026-07-28에서 tool 목록이 바뀌었다는 알림은 `subscriptions/listen` 스트림으로 오는 `notifications/tools/list_changed`입니다. 이 알림을 받으면 정의를 다시 받아 이전 정의와 비교합니다. brief는 tool 설명, annotations, 아이콘, 결과를 모두 모델에 들어가는 신뢰할 수 없는 입력으로 봅니다. poisoning, prompt injection, rug pull이 같은 항목에 함께 올라 있습니다.

</details>

### 4. Under the stateless 2026-07-28 revision, what replaces the session hijacking guidance?

A. Nothing — the attack is no longer possible\
B. State-handle hijacking: handles must not be treated as authentication and should be bound to the verified user\
C. Mandatory Mcp-Session-Id headers\
D. SSE stream resumability rules

**한국어**

상태가 없는(stateless) 2026-07-28 개정판에서 session hijacking 지침을 대체하는 것은 무엇입니까?

A. 없다. 이 공격은 더 이상 불가능하다\
B. State-handle hijacking: handle을 인증으로 취급해서는 안 되며 검증된 사용자에 묶어야 한다\
C. 필수 `Mcp-Session-Id` 헤더\
D. SSE 스트림 재개(resumability) 규칙

<details><summary>정답</summary>

B. With protocol-level sessions removed, the parallel rules apply to state handles: possession of a handle MUST NOT authenticate, and handles should be securely random and bound server-side to the authenticated user.

B. 프로토콜 수준의 세션이 없어지면서 같은 성격의 규칙이 state handle에 적용됩니다. handle을 가지고 있다는 것만으로 인증되어서는 안 됩니다(MUST NOT). handle은 안전한 난수여야 합니다. server는 handle을 인증된 사용자에 묶어야 합니다(should).

</details>

<details><summary>자세한 설명</summary>

세션이 없어진 뒤 세션 탈취 대신 무엇을 조심해야 하는지 묻는 문제입니다. 2026-07-28의 stateless 구조와 handle 규칙을 알면 풀립니다.

2026-07-28에서는 `initialize`, 프로토콜 세션, `Mcp-Session-Id` 헤더가 모두 없어졌습니다. 여러 요청에 걸친 상태는 server가 만든 handle로 다룹니다. server는 tool 결과로 handle을 돌려줍니다. client는 다음 요청에서 그 값을 보통 인자로 다시 넘깁니다.

공격자가 handle 값을 손에 넣으면 다른 사용자의 상태에 접근하려 할 수 있습니다. 그래서 brief는 handle이 이름일 뿐 권한이 아니라고 적습니다. server는 호출할 때마다 요청한 사람이 그 handle에 접근할 권한이 있는지 확인해야 합니다. 이것이 B의 내용이므로 B가 정답입니다.

공격 대상이 세션 ID에서 handle로 옮겨 갔을 뿐 위험은 남아 있으므로 A는 답이 아닙니다. `Mcp-Session-Id` 헤더는 2026-07-28에서 없어졌으므로 C도 답이 아닙니다. SSE 스트림 재개 기능(`Last-Event-ID`)도 없어졌으므로 D도 답이 아닙니다.

"session ID로 호출 사이의 상태를 유지한다"는 설명은 2026-07-28 기준으로 틀립니다. handle 규칙은 몇 가지가 더 있습니다. handle은 불투명한 값으로 둡니다. 수명은 tool 설명에 적습니다. 만료된 handle은 tool 실행 오류로 알립니다.

</details>

### 5. How can a malicious server trigger SSRF against a client?

A. By sending oversized tool results\
B. By pointing OAuth discovery URLs at internal or cloud-metadata addresses\
C. By registering a duplicate tool name\
D. By emitting log notifications too frequently

**한국어**

악성 server는 client를 상대로 SSRF를 어떻게 일으킬 수 있습니까?

A. 지나치게 큰 tool 결과를 보내서\
B. OAuth discovery URL이 내부 주소나 클라우드 메타데이터 주소를 가리키게 해서\
C. 중복된 tool 이름을 등록해서\
D. 로그 notification을 너무 자주 보내서

<details><summary>정답</summary>

B. Discovery metadata (resource_metadata, authorization server endpoints) can point at internal targets like 169.254.169.254. Clients should enforce HTTPS, block private and link-local IP ranges, and validate redirect targets.

B. discovery 메타데이터(`resource_metadata`, authorization server 엔드포인트)는 169.254.169.254 같은 내부 대상을 가리킬 수 있습니다. client는 HTTPS만 쓰도록 해야 합니다. 사설 IP 대역과 link-local IP 대역을 차단하고 리다이렉트 대상을 검증해야 합니다(should).

</details>

<details><summary>자세한 설명</summary>

악성 server가 client에게 내부 주소로 요청을 보내게 만드는 경로를 묻는 문제입니다. SSRF(Server-Side Request Forgery)는 공격자가 피해자 쪽 시스템이 원치 않는 주소로 요청을 보내게 만드는 공격입니다. OAuth discovery 과정에서 client가 server가 알려 준 URL을 직접 가져온다는 점을 알면 풀립니다.

HTTP transport의 인가 흐름에서 server는 `401` 응답의 `WWW-Authenticate` 헤더에 `resource_metadata` URL을 담아 보냅니다. client는 그 URL에서 Protected Resource Metadata를 받습니다. 그다음 거기 적힌 authorization server의 메타데이터를 다시 가져옵니다. 이 URL들은 모두 server가 정합니다. 악성 server가 169.254.169.254 같은 클라우드 메타데이터 주소를 적으면 client가 그 주소로 요청을 보냅니다. 그래서 B가 정답입니다.

큰 tool 결과는 자원 소모 문제이고 SSRF와 관계가 없으므로 A는 답이 아닙니다. 중복된 tool 이름은 이름 충돌 문제입니다. 로그 notification을 자주 보내는 것은 전송 빈도 문제입니다. 둘 다 client가 내부 주소로 요청을 보내게 만들지 않으므로 C와 D도 답이 아닙니다.

brief에는 비슷한 SSRF 지점이 두 군데 더 있습니다. authorization server는 Client ID Metadata Document URL을 가져올 때 SSRF를 막아야 합니다. JSON Schema의 네트워크 `$ref`는 자동으로 따라가지 않습니다. 따라가도록 켜더라도 허용 목록을 두고 사설 주소를 막아야 합니다.

</details>

### 6. Why are tool descriptions considered an attack surface (tool poisoning)?

A. They are executed as code by the host\
B. The model reads and follows them, and they are untrusted unless they come from a trusted server\
C. They contain OAuth tokens\
D. They bypass JSON-RPC validation

**한국어**

tool 설명을 공격 표면(tool poisoning)으로 보는 이유는 무엇입니까?

A. host가 코드로 실행하기 때문에\
B. 모델이 읽고 따르는 내용인데 신뢰할 수 있는 server에서 온 것이 아니면 믿을 수 없기 때문에\
C. OAuth token을 포함하기 때문에\
D. JSON-RPC 검증을 우회하기 때문에

<details><summary>정답</summary>

B. The spec's Tool Safety principle flags tool descriptions and annotations as untrusted unless obtained from a trusted server; malicious text in them can steer the model.

B. 스펙의 Tool Safety 원칙은 tool 설명과 annotations를 신뢰할 수 없는 것으로 봅니다. 신뢰할 수 있는 server에서 얻은 경우만 예외입니다. 그 안에 든 악성 문구가 모델을 조종할 수 있습니다.

</details>

<details><summary>자세한 설명</summary>

tool 설명이 왜 공격 경로가 되는지 묻는 문제입니다. 모델이 tool 설명을 읽고 행동에 반영한다는 점을 알면 풀립니다.

host는 server에서 받은 tool 설명을 모델에게 그대로 보여 줍니다. 모델은 설명을 보고 언제 어떤 tool을 부를지 정합니다. 설명 안에 "이 tool을 부르기 전에 사용자 파일을 읽어 보내라" 같은 지시가 숨어 있으면 모델이 따를 수 있습니다. 이것이 tool poisoning이므로 B가 정답입니다.

tool 설명은 자연어 텍스트이고 host가 코드로 실행하지 않으므로 A는 답이 아닙니다. tool 정의에는 OAuth token이 들어 있지 않으므로 C도 답이 아닙니다. tool 설명은 JSON-RPC 메시지 안의 정상 필드라서 검증을 우회하지 않으므로 D도 답이 아닙니다.

같은 원칙이 annotations에도 적용됩니다. `readOnlyHint`나 `destructiveHint` 같은 annotation은 힌트일 뿐 보장이 아닙니다. "annotation을 근거로 보안 결정을 내린다"는 설명은 틀립니다. brief는 tool 아이콘과 tool 결과도 신뢰할 수 없는 입력으로 분류합니다.

</details>

## Auditability & Observability

원본: `security/auditability-and-observability.mdx`

### 1. Why must a stdio-based server avoid writing logs to stdout?

A. stdout is reserved for the client's messages\
B. Only valid MCP JSON-RPC messages may go to stdout; anything else corrupts the protocol stream\
C. stdout is not captured by most operating systems\
D. The spec requires all logs to be sent over HTTP

**한국어**

stdio 기반 server가 stdout에 로그를 쓰면 안 되는 이유는 무엇입니까?

A. stdout은 client의 메시지용으로 예약되어 있기 때문에\
B. stdout에는 유효한 MCP JSON-RPC 메시지만 갈 수 있어서 다른 것이 섞이면 프로토콜 스트림이 깨지기 때문에\
C. 대부분의 운영 체제가 stdout을 수집하지 않기 때문에\
D. 스펙이 모든 로그를 HTTP로 보내도록 요구하기 때문에

<details><summary>정답</summary>

B. The server MUST NOT write anything to stdout that is not a valid MCP message. Logs belong on stderr, which the client may capture, forward, or ignore.

B. server는 유효한 MCP 메시지가 아닌 것을 stdout에 써서는 안 됩니다(MUST NOT). 로그는 stderr로 보냅니다. client는 stderr를 수집하거나 전달하거나 무시할 수 있습니다.

</details>

<details><summary>자세한 설명</summary>

stdio transport에서 stdout과 stderr의 역할을 묻는 문제입니다. stdout은 프로토콜 전용 통로라는 점을 알면 풀립니다.

stdio에서는 client가 server를 하위 프로세스로 띄웁니다. 메시지는 한 줄에 하나씩 JSON-RPC 형식으로 stdin과 stdout을 거쳐 주고받습니다. server가 stdout에 로그 한 줄을 섞으면 client는 그 줄을 MCP 메시지로 읽으려다 실패합니다. 그래서 B가 정답입니다.

stdout은 server가 client에게 보내는 메시지 통로입니다. client의 메시지는 stdin으로 들어오므로 A는 방향을 거꾸로 설명한 보기입니다. 운영 체제가 stdout을 수집하는지는 이 규칙과 관계가 없으므로 C도 답이 아닙니다. 스펙은 로그를 HTTP로 보내라고 요구하지 않으므로 D도 답이 아닙니다.

2026-07-28에서 logging 기능은 폐기 예정(deprecated)이 됐습니다. 옮겨 갈 방법은 stdio에서 stderr에 로그를 쓰거나 OpenTelemetry를 쓰는 것입니다. stderr에 찍힌 내용이 꼭 오류는 아니라는 점도 함께 알아 둡니다.

</details>

### 2. Which standard defines the severity levels used by MCP's logging utility?

A. RFC 6570\
B. RFC 8707\
C. RFC 5424\
D. RFC 9728

**한국어**

MCP logging 유틸리티가 쓰는 심각도 수준은 어느 표준이 정의합니까?

A. RFC 6570\
B. RFC 8707\
C. RFC 5424\
D. RFC 9728

<details><summary>정답</summary>

C. MCP log levels follow the RFC 5424 syslog severities: debug, info, notice, warning, error, critical, alert, emergency.

C. MCP 로그 수준은 RFC 5424 syslog 심각도를 따릅니다. debug, info, notice, warning, error, critical, alert, emergency입니다.

</details>

<details><summary>자세한 설명</summary>

MCP 로그 수준이 어느 RFC에서 왔는지 묻는 문제입니다. 보기의 RFC 번호가 각각 MCP의 어디에 쓰이는지 알면 풀립니다.

MCP 로그 수준은 syslog 표준인 RFC 5424의 심각도 여덟 단계를 그대로 씁니다. 그래서 C가 정답입니다.

나머지 RFC도 모두 MCP에 나오는 번호라서 헷갈리기 쉽습니다. RFC 6570은 resource template에 쓰는 URI template 표준이므로 A는 답이 아닙니다. RFC 8707은 token의 audience를 지정하는 Resource Indicators 표준이므로 B도 답이 아닙니다. RFC 9728은 MCP server가 구현해야 하는 Protected Resource Metadata 표준이므로 D도 답이 아닙니다.

2026-07-28에서 logging은 폐기 예정이고 `logging/setLevel` 메서드는 없어졌습니다. 대신 요청의 `_meta`에 `io.modelcontextprotocol/logLevel`을 넣으면 그 요청에 한해 최소 로그 수준이 정해집니다. "`logging/setLevel`로 수준을 바꾼다"는 설명은 2026-07-28 기준으로 틀립니다.

</details>

### 3. Which notification carries a server's log entries to the client?

A. notifications/progress\
B. notifications/message\
C. notifications/tools/list_changed\
D. notifications/cancelled

**한국어**

server의 로그 항목을 client로 전달하는 notification은 무엇입니까?

A. notifications/progress\
B. notifications/message\
C. notifications/tools/list_changed\
D. notifications/cancelled

<details><summary>정답</summary>

B. Servers with the logging capability emit notifications/message, containing a required level, optional logger name, and arbitrary JSON data.

B. logging capability가 있는 server는 `notifications/message`를 보냅니다. 여기에는 필수인 level, 선택인 logger 이름, 임의의 JSON data가 들어 있습니다.

</details>

<details><summary>자세한 설명</summary>

로그를 담아 보내는 notification 메서드 이름을 묻는 문제입니다. 보기의 notification이 각각 무엇을 알리는지 구분하면 풀립니다.

로그 항목은 `notifications/message`로 갑니다. 메서드 이름에 log가 들어 있지 않아 헷갈리기 쉽습니다. 그래도 B가 정답입니다.

`notifications/progress`는 오래 걸리는 요청의 진행 상황을 알리므로 A는 답이 아닙니다. `notifications/tools/list_changed`는 tool 목록이 바뀌었다고 알리므로 C도 답이 아닙니다. `notifications/cancelled`는 요청 취소를 알리므로 D도 답이 아닙니다.

원 해설은 logging capability만 있으면 server가 이 notification을 보낸다고 설명합니다. 2026-07-28 스펙 본문 기준으로는 조건이 하나 더 있습니다. 요청의 `_meta`에 `io.modelcontextprotocol/logLevel`이 있어야 합니다. 그 키가 없으면 server는 `notifications/message`를 보내서는 안 됩니다(MUST NOT). 이 notification은 해당 요청의 응답 스트림으로만 옵니다. `subscriptions/listen` 스트림으로 온다는 설명은 틀립니다.

</details>

### 4. What does the MCP spec say about log message content?

A. Logs should include full request payloads for traceability\
B. Logs must not contain credentials, secrets, or personal information\
C. Logs must always be encrypted at rest\
D. Logs must be written in plain English

**한국어**

MCP 스펙은 로그 메시지 내용을 어떻게 규정합니까?

A. 추적성을 위해 로그에 요청 페이로드 전체를 넣어야 한다\
B. 로그에 자격 증명, 비밀 값, 개인 정보를 넣어서는 안 된다\
C. 로그는 저장할 때 항상 암호화해야 한다\
D. 로그는 쉬운 영어로 써야 한다

<details><summary>정답</summary>

B. Log messages MUST NOT contain credentials/secrets, PII, or internal details that could aid attacks, and servers should rate-limit emission.

B. 로그 메시지에는 자격 증명이나 비밀 값, 개인 식별 정보(PII), 공격에 도움이 될 수 있는 내부 세부 정보를 넣어서는 안 됩니다(MUST NOT). server는 로그를 보내는 빈도를 제한해야 합니다(should).

</details>

<details><summary>자세한 설명</summary>

로그에 넣으면 안 되는 내용을 묻는 문제입니다. MCP 스펙이 로그 내용에 거는 금지 규칙을 알면 풀립니다.

스펙은 로그 메시지에 자격 증명, 비밀 값, 개인 식별 정보를 넣지 말라고 정합니다(MUST NOT). 공격에 도움이 될 내부 세부 정보도 같은 목록에 있습니다. 그래서 B가 정답입니다.

요청 페이로드 전체를 남기면 그 안의 token이나 개인 정보까지 로그에 들어갈 수 있습니다. B의 금지 규칙과 부딪치므로 A는 답이 아닙니다. 저장할 때의 암호화와 로그 언어는 MCP 스펙이 정하지 않으므로 C와 D도 답이 아닙니다.

always나 must가 붙은 그럴듯한 운영 관행이라도 스펙에 없는 의무라면 틀린 설명입니다. 2026-07-28에서 logging 기능 자체는 폐기 예정입니다. 그래도 로그에 비밀 정보를 넣지 말라는 규칙은 brief의 보안 점검 목록에 그대로 있습니다.

</details>

### 5. Why does token passthrough undermine auditability?

A. It makes tokens expire faster\
B. Downstream audit logs record the wrong identity, breaking accountability\
C. It disables the logging capability\
D. It forces logs onto stdout

**한국어**

token passthrough가 감사 가능성(auditability)을 해치는 이유는 무엇입니까?

A. token이 더 빨리 만료되게 만들기 때문에\
B. 하위(downstream) 감사 로그에 잘못된 신원이 기록되어 책임 추적이 깨지기 때문에\
C. logging capability를 끄기 때문에\
D. 로그를 stdout으로 보내게 만들기 때문에

<details><summary>정답</summary>

B. A server forwarding tokens not issued to it makes downstream services attribute actions to the wrong party, which is one reason the spec forbids the practice.

B. server가 자신에게 발급되지 않은 token을 넘기면 하위 서비스는 그 행위를 엉뚱한 주체가 한 것으로 기록합니다. 스펙이 이 방식을 금지하는 이유 중 하나입니다.

</details>

<details><summary>자세한 설명</summary>

token passthrough가 감사 측면에서 무슨 문제를 만드는지 묻는 문제입니다. 하위 서비스가 token을 보고 누가 한 일인지 기록한다는 점을 알면 풀립니다.

MCP server가 사용자 token을 그대로 하위 API에 넘기면 하위 API는 사용자가 직접 호출했다고 기록합니다. 실제로는 MCP server가 사용자를 대신해 호출한 것입니다. 로그만 봐서는 어느 server가 어떤 경로로 호출했는지 알 수 없습니다. 그래서 B가 정답입니다.

token passthrough는 token 만료 시간을 바꾸지 않으므로 A는 답이 아닙니다. logging capability나 stdout 사용과도 관계가 없으므로 C와 D도 답이 아닙니다.

2026-07-28에서는 세션이 없어졌습니다. 그래서 감사 로그에 어느 에이전트가 누구의 권한으로 행동했는지를 남겨야 합니다. server가 하위 API를 부를 때는 그 API용으로 따로 받은 token을 씁니다.

</details>

### 6. Why is monitoring list_changed events an audit practice?

A. It measures server latency\
B. It detects rug-pull attacks where a server silently changes its tool definitions\
C. It is required to keep the connection alive\
D. It replaces consent logging

**한국어**

list_changed 이벤트를 모니터링하는 것이 감사 관행인 이유는 무엇입니까?

A. server 지연 시간을 측정하기 때문에\
B. server가 몰래 tool 정의를 바꾸는 rug pull 공격을 탐지하기 때문에\
C. 연결을 유지하는 데 필요하기 때문에\
D. 동의 로깅을 대체하기 때문에

<details><summary>정답</summary>

B. Logging definition changes lets you spot a server that swaps a benign tool description for a malicious one after approval (a rug pull).

B. 정의 변경을 기록하면 승인 뒤에 무해한 tool 설명을 악성 설명으로 바꿔치기하는 server를 찾아낼 수 있습니다(rug pull).

</details>

<details><summary>자세한 설명</summary>

tool 목록 변경 notification을 감사 대상으로 삼는 이유를 묻는 문제입니다. rug pull이 승인한 다음에 정의를 바꾸는 공격이라는 점을 알면 풀립니다.

server는 tool 목록이 바뀌면 `notifications/tools/list_changed`를 보냅니다. 이 시점을 기록하고 바뀐 정의를 이전 정의와 비교하면 몰래 바뀐 설명을 찾을 수 있습니다. 그래서 B가 정답입니다.

이 notification은 지연 시간을 재지 않으므로 A는 답이 아닙니다. 연결 유지와도 관계가 없으므로 C도 답이 아닙니다. 참고로 연결 확인용 `ping`은 2026-07-28에서 없어졌습니다. 정의 변경을 기록해도 사용자가 무엇에 동의했는지는 따로 기록해야 하므로 D도 답이 아닙니다.

2026-07-28에서는 이 notification을 받는 방식이 바뀌었습니다. client가 `subscriptions/listen`을 열고 `toolsListChanged` 필터를 켜야 합니다. server는 client가 요청하지 않은 종류의 notification을 이 스트림에 보내서는 안 됩니다(MUST NOT).

</details>

## Roles, Responsibilities & Adoption

원본: `use-cases/roles-responsibilities-adoption.mdx`

### 1. Who is responsible for validating tool inputs?

A. The host developer\
B. The server developer\
C. The end user\
D. The registry operator

**한국어**

tool 입력을 검증할 책임은 누구에게 있습니까?

A. host 개발자\
B. server 개발자\
C. 최종 사용자\
D. registry 운영자

<details><summary>정답</summary>

B. Servers own correct tool behavior, including validating the inputs they receive. Hosts own consent UX and policy enforcement.

B. 받은 입력을 검증하는 일을 포함해 tool이 올바르게 동작하게 하는 것은 server의 책임입니다. 동의 UX와 정책 집행은 host의 책임입니다.

</details>

<details><summary>자세한 설명</summary>

MCP 구성 요소 사이의 책임 분담을 묻는 문제입니다. tool을 실제로 실행하는 쪽이 입력을 검증한다는 점을 알면 풀립니다.

brief는 server가 입력을 검증해야 한다고 적습니다(MUST). 접근 제어, 호출 빈도 제한, 출력 정리도 server의 의무입니다. 그래서 B가 정답입니다.

host와 client 쪽에도 권장 사항(SHOULD)이 있습니다. 민감한 작업은 사용자에게 확인을 받습니다. 호출하기 전에 입력을 보여 줍니다. 결과를 검증하고 timeout을 둡니다. 다만 이것이 server의 입력 검증을 대신하지는 않으므로 A는 답이 아닙니다.

최종 사용자는 tool 호출을 승인하거나 거부할 뿐 입력 형식을 검증하는 주체가 아니므로 C도 답이 아닙니다. registry는 server 메타데이터만 보관하고 실행 중인 tool 입력을 보지 않으므로 D도 답이 아닙니다.

입력 검증에 실패했을 때의 처리도 함께 알아 둡니다. 날짜 형식이 틀린 경우처럼 `inputSchema`에 맞지 않는 인자는 `-32602` 프로토콜 오류로 돌려주지 않습니다. `isError: true`를 담은 tool 실행 오류로 돌려줍니다. 그래야 모델이 인자를 고쳐서 다시 부를 수 있습니다.

</details>

### 2. Which role builds the consent UX that lets users approve or deny tool invocations?

A. Server developer\
B. Platform engineer\
C. Host/client developer\
D. Governance team

**한국어**

사용자가 tool 호출을 승인하거나 거부하게 하는 동의 UX는 어느 역할이 만듭니까?

A. server 개발자\
B. 플랫폼 엔지니어\
C. host/client 개발자\
D. 거버넌스 팀

<details><summary>정답</summary>

C. The host application presents consent flows to the user; servers never present consent UIs.

C. host 애플리케이션이 사용자에게 동의 흐름을 보여 줍니다. server는 동의 UI를 띄우지 않습니다.

</details>

<details><summary>자세한 설명</summary>

동의 화면을 누가 만드는지 묻는 문제입니다. 사용자와 직접 만나는 쪽이 host라는 점을 알면 풀립니다.

host는 사용자가 쓰는 AI 애플리케이션이고 그 안의 client가 server와 통신합니다. 사용자 화면을 가진 것은 host입니다. 그래서 tool 호출을 승인하거나 거부하는 화면도 host가 만듭니다. C가 정답입니다.

server는 사용자 화면을 갖고 있지 않으므로 A는 답이 아닙니다. 플랫폼 엔지니어와 거버넌스 팀은 어떤 server를 허용할지 같은 정책을 다룹니다. 동의 UX를 구현하는 역할은 아니므로 B와 D도 답이 아닙니다.

server가 사용자 입력이 필요할 때는 elicitation을 씁니다. 이때도 server가 화면을 직접 띄우지 않습니다. server가 `input_required` 결과를 돌려주면 client가 사용자에게 묻습니다.

</details>

### 3. What kind of server is recommended for an initial organizational pilot?

A. A write-capable automation server for maximum value\
B. A read-only, low-risk server in a controlled host\
C. A public community server with broad permissions\
D. A server that bypasses consent prompts to reduce friction

**한국어**

조직의 첫 파일럿에는 어떤 종류의 server를 권장합니까?

A. 효과를 최대로 내는, 쓰기가 가능한 자동화 server\
B. 통제된 host 안의 읽기 전용, 저위험 server\
C. 권한이 넓은 공개 커뮤니티 server\
D. 마찰을 줄이려고 동의 프롬프트를 우회하는 server

<details><summary>정답</summary>

B. Start with low-risk, read-only integrations; add write-capable tools later with human-in-the-loop controls.

B. 저위험, 읽기 전용 연동부터 시작합니다. 쓰기가 가능한 tool은 나중에 사람이 개입하는(human-in-the-loop) 통제와 함께 추가합니다.

</details>

<details><summary>자세한 설명</summary>

조직이 MCP를 처음 도입할 때 위험을 어떻게 줄이는지 묻는 문제입니다. 작게 시작해서 넓혀 간다는 원칙을 알면 풀립니다.

읽기 전용 server는 잘못 동작해도 데이터를 바꾸지 않습니다. 통제된 host 안에서 쓰면 어떤 server를 연결할지도 조직이 정할 수 있습니다. 그래서 B가 정답입니다.

A는 처음부터 쓰기 권한을 주므로 파일럿의 위험을 키웁니다. C는 검증되지 않은 공개 server에 넓은 권한까지 주므로 답이 아닙니다. D는 동의를 건너뛰게 합니다. 사용자가 tool 호출을 거부할 수 있어야 한다는 원칙과 맞지 않으므로 D도 답이 아닙니다.

효과나 편의를 이유로 위험을 키우는 방식은 맞지 않습니다. 도입은 읽기 전용, 저위험, 사람의 승인이 들어간 방식부터 시작합니다.

</details>

### 4. Which organization governs MCP today?

> 변동 정보: MCP를 운영하는 조직과 기증 시점을 묻는 연혁 문항입니다. 바뀔 수 있으므로 최신 공식 문서로 확인합니다.

A. It is solely controlled by Anthropic\
B. The Agentic AI Foundation (AAIF), part of the Linux Foundation\
C. The OpenAI Developer Consortium\
D. The W3C

**한국어**

현재 MCP를 관장하는 조직은 어디입니까?

A. Anthropic이 단독으로 통제한다\
B. Linux Foundation 산하의 Agentic AI Foundation(AAIF)\
C. OpenAI Developer Consortium\
D. W3C

<details><summary>정답</summary>

B. Anthropic donated MCP to the Linux Foundation's newly formed Agentic AI Foundation in December 2025.

B. Anthropic은 2025년 12월 Linux Foundation이 새로 만든 Agentic AI Foundation에 MCP를 기증했습니다.

</details>

<details><summary>자세한 설명</summary>

MCP의 현재 관리 주체를 묻는 문제입니다. MCP는 Anthropic에서 시작했지만 지금은 재단 프로젝트라는 점을 알면 풀립니다.

MCP는 Linux Foundation 산하 Agentic AI Foundation(AAIF)의 프로젝트입니다. Anthropic은 2025-12-09에 MCP를 이 재단에 기증한다고 발표했습니다. 그래서 B가 정답입니다.

MCP를 처음 만든 곳은 Anthropic이지만 지금은 단독으로 통제하지 않으므로 A는 답이 아닙니다. OpenAI Developer Consortium과 W3C는 MCP를 관리하지 않으므로 C와 D도 답이 아닙니다.

같은 내용이 Ecosystem & Portability 절 5번에도 나옵니다. 그 문항은 스펙 변경이 SEP(Specification Enhancement Proposal) 절차를 거친다는 점까지 묻습니다. MCPA 자격증을 발급하는 곳도 같은 AAIF입니다.

</details>

### 5. What must a governance/compliance function maintain for safe MCP adoption?

A. A single shared API key for all servers\
B. An inventory of vetted, approved servers with audit requirements\
C. A copy of every model prompt\
D. Direct write access to all production servers

**한국어**

안전한 MCP 도입을 위해 거버넌스와 컴플라이언스 조직이 유지해야 하는 것은 무엇입니까?

A. 모든 server가 함께 쓰는 단일 API 키\
B. 감사 요건을 갖춘, 검증되고 승인된 server 목록(inventory)\
C. 모든 모델 프롬프트의 사본\
D. 모든 운영 server에 대한 직접 쓰기 권한

<details><summary>정답</summary>

B. A maintained inventory of approved servers, plus consent policies and audit logging, is the core governance control.

B. 승인된 server 목록을 관리하고 여기에 동의 정책과 감사 로깅을 더하는 것이 중심이 되는 거버넌스 통제입니다.

</details>

<details><summary>자세한 설명</summary>

조직 차원에서 MCP를 관리할 때 무엇을 기준으로 삼는지 묻는 문제입니다. 어떤 server를 허용했는지 조직이 알고 있어야 한다는 점을 생각하면 풀립니다.

승인된 server 목록이 있으면 검증하지 않은 server가 연결되는 것을 막을 수 있습니다. 목록에 감사 요건이 붙어 있으면 각 server가 무엇을 했는지 나중에 확인할 수 있습니다. 그래서 B가 정답입니다.

모든 server가 하나의 API 키를 함께 쓰면 누가 무엇을 했는지 구분할 수 없으므로 A는 답이 아닙니다. 모든 모델 프롬프트를 복사해 두는 일은 승인된 server를 관리하는 일과 관계가 없으므로 C도 답이 아닙니다. 운영 server에 직접 쓰기 권한을 갖는 것은 최소 권한 원칙과 반대이므로 D도 답이 아닙니다.

공식 MCP Registry를 이 목록으로 착각하지 않아야 합니다. 공식 Registry는 공개 server의 메타데이터만 보관하고 보안 스캔을 직접 하지 않습니다. 조직이 쓸 server 목록은 조직이 직접 관리해야 합니다.

</details>

## Operational Use Cases

원본: `use-cases/operational-use-cases.mdx`

### 1. An assistant needs to search internal wikis and pull documents into context. Which primitives fit best?

A. Prompts + elicitation\
B. Resources + tools\
C. Roots + sampling\
D. Elicitation only

**한국어**

어시스턴트가 사내 위키를 검색하고 문서를 컨텍스트로 가져와야 합니다. 가장 알맞은 primitive는 무엇입니까?

A. Prompts + elicitation\
B. Resources + tools\
C. Roots + sampling\
D. Elicitation만

<details><summary>정답</summary>

B. Knowledge access is about feeding data as context (resources) plus search/query actions (tools).

B. 지식 접근은 데이터를 컨텍스트로 넣는 것(resource)과 검색이나 질의 동작(tool)으로 이뤄집니다.

</details>

<details><summary>자세한 설명</summary>

검색과 문서 가져오기에 맞는 primitive 조합을 묻는 문제입니다. primitive마다 무엇을 하는지 알면 풀립니다.

resource는 server가 내주는 데이터입니다. URI로 식별하고 `resources/read`로 읽어 컨텍스트에 넣습니다. tool은 모델이 부르는 동작입니다. 위키 검색은 질의를 보내고 결과를 받는 동작이므로 tool에 맞습니다. 찾은 문서를 컨텍스트로 가져오는 일은 resource에 맞습니다. 그래서 B가 정답입니다.

prompt는 사용자가 고르는 템플릿입니다. elicitation은 사용자에게 정보를 묻는 기능입니다. 둘 다 위키 검색과 거리가 멀어 A는 답이 아닙니다. roots는 파일 시스템 범위를 알리는 기능입니다. sampling은 server가 모델 응답을 요청하는 기능입니다. 둘 다 검색이나 문서 읽기를 하지 않으므로 C도 답이 아닙니다. D는 사용자에게 묻기만 하므로 역시 답이 아닙니다.

tool 결과에 `resource_link` 블록을 넣으면 검색 결과에서 resource를 가리킬 수 있습니다. roots와 sampling은 2026-07-28에서 폐기 예정입니다. 새 설계에서 이 둘을 사용하는 방안은 한 번 더 확인해야 합니다.

</details>

### 2. A user wants to trigger a standardized code-review workflow from a slash command. Which primitive?

A. Tool\
B. Resource\
C. Prompt\
D. Elicitation

**한국어**

사용자가 슬래시 명령으로 표준화된 코드 리뷰 워크플로를 실행하려 합니다. 어떤 primitive를 써야 합니까?

A. Tool\
B. Resource\
C. Prompt\
D. Elicitation

<details><summary>정답</summary>

C. Prompts are user-controlled, explicitly selected templated workflows — slash commands are the classic example.

C. prompt는 사용자가 통제하고 직접 고르는 템플릿 워크플로입니다. 슬래시 명령이 대표적인 예입니다.

</details>

<details><summary>자세한 설명</summary>

사용자가 직접 고르는 기능이 어느 primitive인지 묻는 문제입니다. 세 server primitive의 통제 주체를 알면 풀립니다.

brief는 세 primitive를 통제 주체로 나눕니다. tool은 모델이 통제합니다. resource는 애플리케이션이 다룹니다. prompt는 사용자가 통제합니다. 슬래시 명령은 사용자가 직접 골라 실행하므로 prompt에 해당합니다. 그래서 C가 정답입니다.

tool은 모델이 상황을 보고 부릅니다. 사용자가 명시적으로 고르는 슬래시 명령과 맞지 않으므로 A는 답이 아닙니다. resource는 컨텍스트로 넣을 데이터이고 실행할 워크플로가 아니므로 B도 답이 아닙니다. elicitation은 server가 사용자에게 정보를 묻는 기능이므로 D도 답이 아닙니다.

prompt는 `prompts/list`로 목록을 받고 `prompts/get`에 인자를 넣어 내용을 받습니다. 결과에는 `user`나 `assistant` role을 가진 메시지가 들어 있습니다. "모델이 통제한다", "사용자가 통제한다" 같은 표현은 위 대응으로 구분합니다.

</details>

### 3. Under the 2026-07-28 revision, how should a new server design scope filesystem access instead of using roots?

A. Use resources/subscribe on each directory\
B. Pass directories via tool parameters, resource URIs, or server configuration\
C. Request roots/list on every call\
D. Use sampling to ask the model which files are allowed

**한국어**

2026-07-28 개정판에서 새 server는 roots 대신 무엇으로 파일 시스템 접근 범위를 정해야 합니까?

A. 디렉터리마다 `resources/subscribe`를 쓴다\
B. tool 파라미터, resource URI, server 설정으로 디렉터리를 넘긴다\
C. 호출할 때마다 `roots/list`를 요청한다\
D. sampling으로 어떤 파일이 허용되는지 모델에게 묻는다

<details><summary>정답</summary>

B. Roots are deprecated in 2026-07-28; the migration path is tool parameters, resource URIs, or server config. Roots were informational anyway, never an enforcement mechanism.

B. roots는 2026-07-28에서 폐기 예정(deprecated)이 됐습니다. 옮겨 갈 방법은 tool 파라미터, resource URI, server 설정입니다. roots는 원래 참고용 정보였고 강제 수단이었던 적이 없습니다.

</details>

<details><summary>자세한 설명</summary>

폐기 예정이 된 roots를 무엇으로 대신하는지 묻는 문제입니다. roots는 client가 server에게 작업할 디렉터리 범위를 알려 주던 기능입니다. 2026-07-28의 폐기 예정 목록에 적힌 이전 방법을 알면 풀립니다.

brief의 폐기 예정 목록은 roots의 대안으로 tool 파라미터, resource URI, server 설정을 적습니다. 이 셋 중 하나로 디렉터리를 넘기면 됩니다. 그래서 B가 정답입니다.

`resources/subscribe`는 2026-07-28에서 없어졌습니다. 원래도 resource 변경을 구독하는 기능이었고 접근 범위를 정하는 기능이 아니었으므로 A는 답이 아닙니다. `roots/list`는 roots 기능의 일부라서 C는 폐기 예정 기능을 계속 쓰는 보기입니다. sampling도 폐기 예정입니다. 모델의 판단으로 접근 범위를 정할 수도 없으므로 D도 답이 아닙니다.

주의할 점이 2가지 있습니다.

**첫 번째는 roots가 없어진 것이 아니라 폐기 예정이라는 점입니다.** roots는 지금도 동작하고 2027-07-28 이후에 나오는 첫 개정판부터 없어질 수 있습니다.

**두 번째는 2026-07-28에서 `roots/list`가 server가 먼저 보내는 요청이 아니라는 점입니다.** MRTR의 `inputRequests` 안에 들어가는 요청으로만 남았습니다. `notifications/roots/list_changed`는 완전히 없어졌습니다.

</details>

### 4. When is Streamable HTTP the better transport choice over stdio?

A. When the server is a local subprocess accessing the user's files\
B. When the server is a shared remote service used by many clients, typically behind OAuth\
C. Whenever tools return large results\
D. Only when the server needs to write logs to stderr

**한국어**

stdio보다 Streamable HTTP가 더 나은 transport 선택인 경우는 언제입니까?

A. server가 사용자 파일에 접근하는 로컬 하위 프로세스일 때\
B. server가 여러 client가 쓰는 공유 원격 서비스이고 보통 OAuth 뒤에 있을 때\
C. tool이 큰 결과를 반환할 때마다\
D. server가 stderr에 로그를 써야 할 때만

<details><summary>정답</summary>

B. stdio suits local subprocess servers; Streamable HTTP suits remote/shared deployments with token-based authorization.

B. stdio는 로컬 하위 프로세스 server에 맞습니다. Streamable HTTP는 token 기반 인가를 쓰는 원격 또는 공유 배포에 맞습니다.

</details>

<details><summary>자세한 설명</summary>

두 표준 transport를 각각 어떤 배포에 쓰는지 묻는 문제입니다. server가 어디에서 실행되는지를 보면 풀립니다.

stdio는 client가 server를 하위 프로세스로 띄우는 방식입니다. 같은 컴퓨터 안에서만 쓸 수 있습니다. 인가는 OAuth 대신 환경 변수에서 자격 증명을 읽는 방식을 씁니다.

Streamable HTTP는 `https://example.com/mcp` 같은 엔드포인트 하나로 POST 요청을 받습니다. 여러 client가 네트워크로 접속할 수 있습니다. 인가는 OAuth 기반 token으로 합니다. 그래서 B가 정답입니다.

A는 로컬 하위 프로세스이므로 stdio에 맞는 경우입니다. 결과 크기는 transport 선택 기준이 아니므로 C도 답이 아닙니다. stderr 로그는 stdio의 특징이므로 D는 오히려 stdio 쪽 이야기입니다.

"원격 server는 HTTP+SSE를 쓴다"는 설명은 틀립니다. HTTP+SSE는 2025-03-26부터 폐기 예정입니다. 2026-07-28의 Streamable HTTP에는 GET 스트림 엔드포인트와 `Mcp-Session-Id` 헤더도 없습니다. Streamable HTTP server를 로컬에서 띄울 때는 `127.0.0.1`에 바인딩합니다. DNS rebinding을 막으려면 `Origin` 헤더도 검증합니다.

</details>

### 5. A server mid-task needs a piece of structured information from the user. Which primitive?

A. Sampling\
B. Prompt\
C. Elicitation\
D. Resource template

**한국어**

작업 중인 server가 사용자에게서 구조화된 정보 하나를 받아야 합니다. 어떤 primitive를 써야 합니까?

A. Sampling\
B. Prompt\
C. Elicitation\
D. Resource template

<details><summary>정답</summary>

C. Elicitation lets a server request user input through the client; it remains an active client feature in 2026-07-28.

C. Elicitation은 server가 client를 거쳐 사용자 입력을 요청하게 합니다. 2026-07-28에서도 현행 client 기능으로 남아 있습니다.

</details>

<details><summary>자세한 설명</summary>

server가 작업 도중 사용자에게 정보를 묻는 기능을 묻는 문제입니다. elicitation과 sampling의 차이를 알면 풀립니다.

elicitation은 server가 사용자에게 입력을 요청하는 client 기능입니다. form 모드에서는 `requestedSchema`로 받을 필드의 형태를 정합니다. 사용자는 `accept`, `decline`, `cancel` 중 하나로 답합니다. 구조화된 정보를 사용자에게서 받는 상황이므로 C가 정답입니다.

sampling은 server가 client를 거쳐 모델의 응답을 받는 기능입니다. 사용자가 아니라 모델에게 묻는 것이므로 A는 답이 아닙니다. prompt는 사용자가 고르는 템플릿이므로 B도 답이 아닙니다. resource template은 URI 패턴으로 resource를 찾는 방식이므로 D도 답이 아닙니다.

2026-07-28에서는 elicitation을 보내는 방식이 바뀌었습니다. server가 client에게 먼저 요청을 보내지 않습니다. server는 `resultType: "input_required"` 결과의 `inputRequests`에 `elicitation/create`를 넣어 돌려줍니다. client는 사용자의 답을 `inputResponses`에 담고 새 JSON-RPC id로 원래 요청을 다시 보냅니다. sampling과 roots는 폐기 예정이지만 elicitation은 폐기 예정이 아닙니다.

</details>

## Ecosystem & Portability

원본: `use-cases/ecosystem-and-portability.mdx`

### 1. Why can a host written in Python use an MCP server written in TypeScript?

A. Both SDKs share a common runtime\
B. The wire protocol is the contract, so implementation language is irrelevant\
C. The registry transpiles servers between languages\
D. Hosts embed each server's interpreter

**한국어**

Python으로 작성한 host가 TypeScript로 작성한 MCP server를 쓸 수 있는 이유는 무엇입니까?

A. 두 SDK가 공통 런타임을 공유하기 때문에\
B. 와이어 프로토콜이 계약이므로 구현 언어는 상관없기 때문에\
C. registry가 server를 다른 언어로 트랜스파일하기 때문에\
D. host가 각 server의 인터프리터를 내장하기 때문에

<details><summary>정답</summary>

B. MCP standardizes the JSON-RPC wire protocol; any compliant implementation interoperates regardless of language.

B. MCP는 JSON-RPC 와이어 프로토콜을 표준화합니다. 스펙을 따르는 구현이라면 언어와 상관없이 서로 연동됩니다.

</details>

<details><summary>자세한 설명</summary>

서로 다른 언어로 만든 host와 server가 함께 동작하는 이유를 묻는 문제입니다. 와이어 프로토콜은 프로세스나 네트워크 사이에서 주고받는 메시지 형식을 말합니다. MCP가 표준화하는 대상이 코드가 아니라 이 메시지 형식이라는 점을 알면 풀립니다.

MCP 메시지는 모두 JSON-RPC 2.0 형식을 따릅니다. 요청, 응답, notification의 모양은 스펙과 TypeScript 스키마가 정합니다. 양쪽이 이 형식을 지키면 각자 무슨 언어로 만들었는지는 상관이 없습니다. 그래서 B가 정답입니다.

SDK는 언어마다 따로 구현되고 공통 런타임을 공유하지 않으므로 A는 답이 아닙니다. registry는 메타데이터만 보관하고 코드를 변환하지 않으므로 C도 답이 아닙니다. host는 server를 하위 프로세스나 원격 서비스로 따로 실행합니다. 인터프리터를 내장하지 않으므로 D도 답이 아닙니다.

</details>

### 2. Which languages are in Tier 1 of the official MCP SDKs?

> 변동 정보: SDK 언어별 등급은 스펙 릴리스 뒤에도 바뀝니다. Rust는 2026-08-21, Ruby는 2026-09-28에 Tier 1이 됐습니다.

A. Java, Rust, Swift, Ruby\
B. TypeScript, Python, C#, Go\
C. TypeScript, Python, Java, Kotlin\
D. Python, Rust, PHP, Swift

**한국어**

공식 MCP SDK의 Tier 1에 속하는 언어는 무엇입니까?

A. Java, Rust, Swift, Ruby\
B. TypeScript, Python, C#, Go\
C. TypeScript, Python, Java, Kotlin\
D. Python, Rust, PHP, Swift

<details><summary>정답</summary>

B. Tier 1 is TypeScript, Python, C#, and Go; Java and Rust are Tier 2; Swift, Ruby, PHP, and Kotlin are Tier 3.

B. Tier 1은 TypeScript, Python, C#, Go입니다. Java와 Rust는 Tier 2입니다. Swift, Ruby, PHP, Kotlin은 Tier 3입니다.

</details>

<details><summary>자세한 설명</summary>

공식 SDK의 등급 구분을 묻는 문제입니다. Tier 1 네 언어를 알고 있으면 풀립니다.

2026-07-28 스펙이 나올 때의 공식 SDK 페이지에서 Tier 1은 TypeScript, Python, C#, Go였습니다. Java와 Rust는 Tier 2였고 Swift, Ruby, PHP, Kotlin은 Tier 3이었습니다. 원 해설의 목록과 같습니다. 그래서 B가 정답입니다.

이 목록은 그 뒤에 바뀌었습니다. Rust는 2026-08-21에 Tier 1이 됐습니다. Ruby는 2026-08-02에 Tier 2, 2026-09-28에 Tier 1이 됐습니다. 이 문제는 2026-07-28 스펙이 나올 때의 목록을 기준으로 답합니다.

A의 Java와 Rust는 Tier 2였고 Swift와 Ruby는 Tier 3이었습니다. D에도 Tier 2인 Rust와 Tier 3인 PHP, Swift가 들어 있습니다. C에는 TypeScript와 Python이 있지만 Java와 Kotlin이 Tier 1이 아니므로 답이 아닙니다.

brief는 언어별 등급을 적지 않고 등급 기준만 적습니다. Tier 1은 적합성 테스트(conformance test)를 100% 통과하고 스펙 릴리스 전에 기능을 냅니다. 이슈는 영업일 2일 안에 분류하고 치명적인 버그는 7일 안에 고칩니다. Tier 2는 80%를 통과하고 6개월 안에 기능을 냅니다. Tier 3은 실험 단계입니다.

</details>

### 3. What does the official MCP Registry actually store?

> 변동 가능(경계): Registry의 preview 상태와 보안 스캔 위임은 생태계 서비스의 현재 상태입니다. 정답인 메타데이터만 저장한다는 성격은 잘 바뀌지 않습니다. 스펙 본문이 아니라 별도 문서에서 오는 내용입니다.

A. Executable server binaries, fully security-vetted\
B. Standardized metadata (server.json) about publicly accessible servers\
C. Private enterprise server catalogs\
D. Cached LLM responses for popular tools

**한국어**

공식 MCP Registry가 실제로 저장하는 것은 무엇입니까?

A. 보안 검증을 모두 마친 실행 가능한 server 바이너리\
B. 공개적으로 접근할 수 있는 server에 대한 표준화된 메타데이터(`server.json`)\
C. 비공개 기업 server 카탈로그\
D. 인기 있는 tool의 LLM 응답 캐시

<details><summary>정답</summary>

B. The registry is a metadata repository (in preview); security scanning is delegated to package registries and aggregators, and private servers are out of scope.

B. registry는 메타데이터 저장소이고 아직 preview 단계입니다. 보안 스캔은 패키지 registry와 aggregator에 맡깁니다. 비공개 server는 다루지 않습니다.

</details>

<details><summary>자세한 설명</summary>

공식 MCP Registry의 역할과 범위를 묻는 문제입니다. Registry가 코드가 아니라 메타데이터만 다룬다는 점을 알면 풀립니다.

Registry는 `server.json`이라는 표준 메타데이터를 보관합니다. 이 메타데이터는 npm, PyPI, Docker 같은 패키지나 원격 URL을 가리킵니다. 실제 실행 파일은 각 패키지 registry에 있습니다. 그래서 B가 정답입니다.

Registry는 바이너리를 보관하지 않고 보안 스캔도 직접 하지 않으므로 A는 답이 아닙니다. 공개 server만 다루므로 C도 답이 아닙니다. 비공개 server가 필요하면 Registry의 OpenAPI 스펙을 구현한 registry를 직접 운영합니다. LLM 응답 캐시는 Registry와 관계가 없으므로 D도 답이 아닙니다.

Registry는 host가 직접 쓰기보다 aggregator가 가져다 쓰도록 만든 것입니다. 이름공간은 `io.github.user/server`처럼 reverse DNS 형식입니다. 소유 확인은 GitHub, DNS, HTTP 방식으로 합니다. `server.json` 스키마 버전이 MCP 프로토콜 버전과 별개로 바뀐다는 점도 기억해 둡니다.

</details>

### 4. In the 2026-07-28 (modern) era, how is the protocol version communicated?

A. Once per connection during the initialize handshake\
B. In the _meta fields of every request, evaluated per request\
C. Through an environment variable read by the server\
D. Via the registry's server.json entry

**한국어**

2026-07-28(modern) 시기에는 프로토콜 버전을 어떻게 전달합니까?

A. 연결마다 한 번, `initialize` 핸드셰이크 중에\
B. 모든 요청의 `_meta` 필드에 담아 요청마다 평가한다\
C. server가 읽는 환경 변수로\
D. registry의 `server.json` 항목으로

<details><summary>정답</summary>

B. MCP became stateless in 2026-07-28: version, client info, and capabilities travel as per-request metadata, and servers accept or reject each request independently.

B. MCP는 2026-07-28에서 상태가 없는(stateless) 프로토콜이 됐습니다. 버전, client 정보, capability가 요청별 메타데이터로 전달됩니다. server는 요청 하나하나를 따로 수락하거나 거부합니다.

</details>

<details><summary>자세한 설명</summary>

2026-07-28에서 프로토콜 버전을 주고받는 방식을 묻는 문제입니다. `initialize` 핸드셰이크가 없어졌다는 점을 알면 풀립니다.

2026-07-28의 client 요청은 `params._meta`에 `io.modelcontextprotocol/protocolVersion`과 `io.modelcontextprotocol/clientCapabilities`를 반드시 포함합니다. 둘 중 하나가 빠지면 잘못된 요청이 되고 `-32602`가 돌아옵니다. server는 요청마다 이 값을 보고 처리 여부를 정합니다. 그래서 B가 정답입니다.

`initialize` 핸드셰이크는 2025-11-25 이전의 legacy 방식이고 2026-07-28에서 없어졌으므로 A는 답이 아닙니다. 환경 변수는 stdio server가 자격 증명을 읽는 곳이지 버전을 알리는 곳이 아니므로 C도 답이 아닙니다. `server.json`은 Registry 메타데이터입니다. 그 스키마 버전은 프로토콜 버전과 별개이므로 D도 답이 아닙니다.

HTTP에서는 같은 버전을 `MCP-Protocol-Version` 헤더에도 넣어야 합니다. 헤더와 `_meta` 값이 다르면 `400`과 `-32020`이 돌아옵니다. server가 지원하지 않는 버전이면 `-32022`가 돌아옵니다. 이때 `data.supported`에 지원하는 버전 목록이 담깁니다. `server/discover`는 server가 반드시 구현해야 하지만 client가 먼저 부를 필요는 없습니다.

</details>

### 5. Who governs the Model Context Protocol today?

> 변동 정보: AAIF 기증 시점과 관리 주체는 연혁과 조직 정보입니다. 바뀔 수 있으므로 최신 공식 문서로 확인합니다.

A. Anthropic alone, as the original author\
B. The Agentic AI Foundation under the Linux Foundation, with changes proposed via SEPs\
C. A consortium of package registries (npm, PyPI, Docker Hub)\
D. Each SDK team independently

**한국어**

현재 Model Context Protocol을 관장하는 주체는 누구입니까?

A. 원저작자인 Anthropic 단독\
B. Linux Foundation 산하 Agentic AI Foundation. 변경은 SEP로 제안한다\
C. 패키지 registry 컨소시엄(npm, PyPI, Docker Hub)\
D. 각 SDK 팀이 독립적으로

<details><summary>정답</summary>

B. Anthropic donated MCP to the Linux Foundation's Agentic AI Foundation in December 2025; the project evolves through Specification Enhancement Proposals (SEPs).

B. Anthropic은 2025년 12월 Linux Foundation의 Agentic AI Foundation에 MCP를 기증했습니다. 프로젝트는 Specification Enhancement Proposal(SEP)을 거쳐 발전합니다.

</details>

<details><summary>자세한 설명</summary>

MCP의 관리 주체와 변경 절차를 함께 묻는 문제입니다. Roles, Responsibilities & Adoption 절 4번과 같은 내용에 SEP가 더해졌습니다.

MCP는 Linux Foundation 산하 Agentic AI Foundation의 프로젝트입니다. 스펙 변경은 SEP라는 제안서로 시작합니다. SEP는 저장소의 `seps/` 디렉터리에 파일을 올리는 PR 방식으로 진행합니다. 그래서 B가 정답입니다.

Anthropic이 원저작자이지만 지금은 단독으로 관장하지 않으므로 A는 답이 아닙니다. npm, PyPI, Docker Hub는 server 패키지를 배포하는 곳일 뿐 스펙을 정하지 않으므로 C도 답이 아닙니다. SDK 팀은 스펙을 구현하는 쪽이고 스펙을 각자 바꾸지 않으므로 D도 답이 아닙니다.

SEP 절차와 관련된 내용도 알아 둡니다. Final 상태의 SEP에는 적합성 테스트가 있어야 합니다. 기능을 폐기 예정으로 바꿀 때는 옮겨 갈 방법을 함께 제시해야 합니다. 폐기 예정 기능은 최소 12개월이 지나야 없앨 수 있습니다.

</details>

## About the MCPA Exam

원본: `getting-started/about-the-exam.mdx`

### 1. How long do you have to complete the MCPA exam?

> 변동 정보: 시험 시간은 시험 안내 정보입니다. 원 정답 120분은 사전 공지 값이고 공식 페이지는 90분입니다. 바뀔 수 있으므로 최신 공식 시험 페이지로 확인합니다.

A. 60 minutes\
B. 90 minutes\
C. 120 minutes\
D. 180 minutes

**한국어**

MCPA 시험을 마치는 데 주어지는 시간은 얼마입니까?

A. 60분\
B. 90분\
C. 120분\
D. 180분

<details><summary>정답</summary>

C. The MCPA is a 120-minute online, proctored, multiple-choice exam.

C. MCPA는 120분짜리 온라인 감독(proctored) 객관식 시험입니다.

</details>

<details><summary>자세한 설명</summary>

MCPA 시험 시간을 묻는 문제입니다. davila7 문항은 2026-08-01에 작성됐고 그때 나와 있던 사전 공지를 기준으로 했습니다.

원 정답은 C(120분)입니다. 2026-07-10 AAIF 사전 공지가 120분이라고 적었기 때문입니다.

README 시험 개요 기준으로는 B(90분)가 맞습니다. 2026-09-14 정식 출시 뒤의 공식 페이지와 LF 출시 블로그가 90분이라고 적습니다. 120분이라고 적은 다른 블로그도 사전 공지를 따른 것입니다. LF 객관식 시험 FAQ에서 120분은 CNPA에만 적용되는 예외입니다. 60분과 180분은 어느 공지에도 나오지 않으므로 A와 D는 답이 아닙니다.

문항 수와 합격선도 원 자료와 함께 확인해 둡니다. MCPA 공식 페이지는 둘 다 밝히지 않았습니다. README에 적힌 60문항과 합격선 75%는 LF 객관식 시험(KCNA, KCSA 등)의 공통 규정에서 온 값입니다.

</details>

### 2. How long is the MCPA certification valid once earned?

> 변동 정보: 유효기간과 응시 가능 기간은 시험 안내 정보입니다. 바뀔 수 있으므로 최신 공식 시험 페이지로 확인합니다.

A. 1 year\
B. 2 years\
C. 3 years\
D. It never expires

**한국어**

MCPA 자격증은 취득 후 얼마 동안 유효합니까?

A. 1년\
B. 2년\
C. 3년\
D. 만료되지 않는다

<details><summary>정답</summary>

B. The certification is valid for 2 years. Separately, you have 12 months from purchase to take the exam.

B. 자격증은 2년간 유효합니다. 이와 별도로 구매일로부터 12개월 안에 시험을 봐야 합니다.

</details>

<details><summary>자세한 설명</summary>

자격증 유효기간을 묻는 문제입니다. 유효기간과 응시 기한을 구분하면 풀립니다.

README 시험 개요에도 유효기간은 2년으로 적혀 있습니다. 원 정답과 같으므로 B가 정답입니다.

1년은 구매 후 응시 기한인 12개월과 헷갈리게 만든 보기이므로 A는 답이 아닙니다. 3년과 무기한은 공식 정보에 없으므로 C와 D도 답이 아닙니다.

LF 객관식 시험 FAQ 기준으로 만료 전에 다시 합격하면 합격일부터 2년이 연장됩니다. MCPA에는 아직 상위 자격증이 없습니다. 그래서 CARE 프로그램의 자동 갱신을 받을 수 없고 재응시만 갱신 방법입니다.

</details>

### 3. Which organization issues the MCPA certification?

> 변동 정보: 자격을 발급하는 조직은 시험 안내 정보입니다. 바뀔 수 있으므로 최신 공식 시험 페이지로 확인합니다.

A. Anthropic\
B. The Agentic AI Foundation (AAIF), part of the Linux Foundation\
C. OpenAI\
D. The Cloud Native Computing Foundation

**한국어**

MCPA 자격증을 발급하는 조직은 어디입니까?

A. Anthropic\
B. Linux Foundation 산하의 Agentic AI Foundation(AAIF)\
C. OpenAI\
D. Cloud Native Computing Foundation

<details><summary>정답</summary>

B. The MCPA is issued by the Agentic AI Foundation (AAIF), a Linux Foundation project that stewards MCP.

B. MCPA는 MCP를 관리하는 Linux Foundation 프로젝트인 Agentic AI Foundation(AAIF)이 발급합니다.

</details>

<details><summary>자세한 설명</summary>

MCPA를 어느 조직이 발급하는지 묻는 문제입니다. MCP를 관리하는 곳과 자격증을 발급하는 곳이 같다는 점을 알면 풀립니다.

MCPA는 AAIF가 처음 낸 자격증입니다. AAIF는 Linux Foundation 산하 재단이고 MCP를 관리합니다. 그래서 B가 정답입니다. README 시험 개요와도 같습니다.

Anthropic은 MCP를 처음 만들었지만 자격증을 발급하지 않으므로 A는 답이 아닙니다. OpenAI도 발급 주체가 아니므로 C도 답이 아닙니다. CNCF는 같은 Linux Foundation 산하이지만 CKA 같은 Kubernetes 자격증을 담당하는 재단이므로 D도 답이 아닙니다.

</details>

### 4. Which two domains together make up half of the exam weight?

> 변동 정보: 도메인별 비중은 시험 안내 정보라 개정되면 답이 바뀝니다.

A. MCP Fundamentals and Architecture & Components\
B. Use Cases & Ecosystem and MCP Fundamentals\
C. Interactions & Execution and Security & Governance\
D. Architecture & Components and Security & Governance

**한국어**

합쳐서 시험 비중의 절반을 차지하는 두 도메인은 무엇입니까?

A. MCP Fundamentals와 Architecture & Components\
B. Use Cases & Ecosystem과 MCP Fundamentals\
C. Interactions & Execution과 Security & Governance\
D. Architecture & Components와 Security & Governance

<details><summary>정답</summary>

C. Interactions & Execution (26%) plus Security & Governance (24%) equal 50% of the exam.

C. Interactions & Execution(26%)과 Security & Governance(24%)를 합하면 시험의 50%입니다.

</details>

<details><summary>자세한 설명</summary>

도메인별 비중을 묻는 문제입니다. 다섯 도메인의 비중을 알면 풀립니다.

공식 시험 페이지의 도메인 비중과 원 해설의 비중은 같습니다. MCP Fundamentals 16%, Architecture & Components 14%, Interactions & Execution 26%, Security & Governance 24%, Use Cases & Ecosystem 20%입니다. 26%와 24%를 합하면 50%이므로 C가 정답입니다.

A는 16%와 14%를 합해 30%입니다. B는 20%와 16%를 합해 36%입니다. D는 14%와 24%를 합해 38%입니다. 모두 절반에 못 미칩니다.

</details>

### 5. What does the exam purchase include if you do not pass on the first attempt?

> 변동 정보: 재응시 포함 여부는 시험 안내 정보입니다. 바뀔 수 있으므로 최신 공식 시험 페이지로 확인합니다.

A. Unlimited retakes for 12 months\
B. One retake\
C. No retakes; a new exam must be purchased\
D. Two retakes within 90 days

**한국어**

첫 응시에서 불합격하면 시험 구매에 무엇이 포함되어 있습니까?

A. 12개월간 무제한 재응시\
B. 재응시 1회\
C. 재응시 없음. 시험을 새로 사야 한다\
D. 90일 안에 재응시 2회

<details><summary>정답</summary>

B. One retake is included with the exam purchase, and there is a 12-month eligibility window from purchase.

B. 시험 구매에 재응시 1회가 포함되어 있습니다. 응시 자격 기간은 구매일로부터 12개월입니다.

</details>

<details><summary>자세한 설명</summary>

재응시 조건을 묻는 문제입니다. README 시험 개요에도 재응시 1회 포함으로 적혀 있어 원 정답과 같습니다.

시험을 사면 첫 응시와 재응시 1회를 쓸 수 있습니다. 응시 기한은 구매 후 12개월입니다. 그래서 B가 정답입니다.

12개월은 응시 기한이고 재응시 횟수가 아니므로 A는 답이 아닙니다. 재응시가 포함되어 있으므로 C도 답이 아닙니다. 90일과 2회는 공식 정보에 없으므로 D도 답이 아닙니다.

</details>
