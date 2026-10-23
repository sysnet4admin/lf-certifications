# 참고 자료

예제 시험(exam-01~08)의 범위와 해설 근거가 되는 자료다. 내용이 서로 다르면 MCP 공식 스펙(2026-07-28)이 우선이다.
주소는 2026-10-22에 모두 열리는 것을 확인했다.

## MCP 공식 스펙 2026-07-28

- [Specification 개요](https://modelcontextprotocol.io/specification/2026-07-28)
- [Key Changes](https://modelcontextprotocol.io/specification/2026-07-28/changelog): 2025-11-25 개정과 비교해 바뀐 것
- [Deprecated Features](https://modelcontextprotocol.io/specification/2026-07-28/deprecated)
- [Architecture](https://modelcontextprotocol.io/specification/2026-07-28/architecture)
- 기본 프로토콜: [Overview](https://modelcontextprotocol.io/specification/2026-07-28/basic), [Versioning and Compatibility](https://modelcontextprotocol.io/specification/2026-07-28/basic/versioning)
- 패턴: [Overview](https://modelcontextprotocol.io/specification/2026-07-28/basic/patterns), [Multi Round-Trip Requests](https://modelcontextprotocol.io/specification/2026-07-28/basic/patterns/mrtr), [Subscriptions](https://modelcontextprotocol.io/specification/2026-07-28/basic/patterns/subscriptions), [Cancellation](https://modelcontextprotocol.io/specification/2026-07-28/basic/patterns/cancellation), [Progress](https://modelcontextprotocol.io/specification/2026-07-28/basic/patterns/progress)
- Transport: [Overview](https://modelcontextprotocol.io/specification/2026-07-28/basic/transports), [stdio](https://modelcontextprotocol.io/specification/2026-07-28/basic/transports/stdio), [Streamable HTTP](https://modelcontextprotocol.io/specification/2026-07-28/basic/transports/streamable-http)
- 인가: [Authorization](https://modelcontextprotocol.io/specification/2026-07-28/basic/authorization), [Authorization Server Discovery](https://modelcontextprotocol.io/specification/2026-07-28/basic/authorization/authorization-server-discovery), [Client Registration](https://modelcontextprotocol.io/specification/2026-07-28/basic/authorization/client-registration), [Authorization Security Considerations](https://modelcontextprotocol.io/specification/2026-07-28/basic/authorization/security-considerations)
- Client 기능: [Roots](https://modelcontextprotocol.io/specification/2026-07-28/client/roots), [Sampling](https://modelcontextprotocol.io/specification/2026-07-28/client/sampling), [Elicitation](https://modelcontextprotocol.io/specification/2026-07-28/client/elicitation)
- Server 기능: [Overview](https://modelcontextprotocol.io/specification/2026-07-28/server), [Discovery](https://modelcontextprotocol.io/specification/2026-07-28/server/discover), [Prompts](https://modelcontextprotocol.io/specification/2026-07-28/server/prompts), [Resources](https://modelcontextprotocol.io/specification/2026-07-28/server/resources), [Tools](https://modelcontextprotocol.io/specification/2026-07-28/server/tools)
- Server 유틸리티: [Caching](https://modelcontextprotocol.io/specification/2026-07-28/server/utilities/caching), [Completion](https://modelcontextprotocol.io/specification/2026-07-28/server/utilities/completion), [Logging](https://modelcontextprotocol.io/specification/2026-07-28/server/utilities/logging), [Pagination](https://modelcontextprotocol.io/specification/2026-07-28/server/utilities/pagination)
- 스키마: [Schema Reference](https://modelcontextprotocol.io/specification/2026-07-28/schema), [TypeScript 스키마 원본](https://github.com/modelcontextprotocol/modelcontextprotocol/blob/main/schema/2026-07-28/schema.ts)
- 이전 개정과 비교: [Specification 2025-11-25](https://modelcontextprotocol.io/specification/2025-11-25)

## MCP 공식 문서

- 개념: [Architecture overview](https://modelcontextprotocol.io/docs/2026-07-28/learn/architecture), [Understanding MCP servers](https://modelcontextprotocol.io/docs/2026-07-28/learn/server-concepts), [Understanding MCP clients](https://modelcontextprotocol.io/docs/2026-07-28/learn/client-concepts), [Versioning](https://modelcontextprotocol.io/docs/2026-07-28/learn/versioning)
- Client 구현: [Client Best Practices](https://modelcontextprotocol.io/docs/2026-07-28/develop/clients/client-best-practices)
- 보안: [Understanding Authorization in MCP](https://modelcontextprotocol.io/docs/2026-07-28/tutorials/security/authorization), [Security Best Practices](https://modelcontextprotocol.io/docs/2026-07-28/tutorials/security/security_best_practices), [Local Server Security](https://modelcontextprotocol.io/docs/2026-07-28/tutorials/security/local-server-security)
- SDK: [SDKs](https://modelcontextprotocol.io/docs/2026-07-28/sdk), [SDK Tiering System](https://modelcontextprotocol.io/community/sdk-tiers)
- 확장: [Extensions Overview](https://modelcontextprotocol.io/extensions/overview), [MCP Apps](https://modelcontextprotocol.io/extensions/apps/overview), [Tasks](https://modelcontextprotocol.io/extensions/tasks/overview), [OAuth Client Credentials](https://modelcontextprotocol.io/extensions/auth/oauth-client-credentials)
- 생태계: [The MCP Registry](https://modelcontextprotocol.io/registry/about)
- 거버넌스: [Governance and Stewardship](https://modelcontextprotocol.io/community/governance), [Design Principles](https://modelcontextprotocol.io/community/design-principles), [Feature Lifecycle and Deprecation Policy](https://modelcontextprotocol.io/community/feature-lifecycle), [Working and Interest Groups](https://modelcontextprotocol.io/community/working-interest-groups)

## SEP(Specification Enhancement Proposal)

- [SEP 목록](https://modelcontextprotocol.io/seps)
- [SEP-414: OpenTelemetry Trace Context Propagation Conventions](https://modelcontextprotocol.io/seps/414-request-meta)
- [SEP-1303: Input Validation Errors as Tool Execution Errors](https://modelcontextprotocol.io/seps/1303-input-validation-errors-as-tool-execution-errors)
- [SEP-1686: Tasks](https://modelcontextprotocol.io/seps/1686-tasks)
- [SEP-1730: SDKs Tiering System](https://modelcontextprotocol.io/seps/1730-sdks-tiering-system)
- [SEP-1865: MCP Apps](https://modelcontextprotocol.io/seps/1865-mcp-apps-interactive-user-interfaces-for-mcp)
- [SEP-2133: Extensions](https://modelcontextprotocol.io/seps/2133-extensions)
- [SEP-2322: Multi Round-Trip Requests](https://modelcontextprotocol.io/seps/2322-MRTR)
- [SEP-2549: TTL for List Results](https://modelcontextprotocol.io/seps/2549-TTL-for-list-results)
- [SEP-2567: Sessionless MCP via Explicit State Handles](https://modelcontextprotocol.io/seps/2567-sessionless-mcp)
- [SEP-2577: Deprecate Roots, Sampling and Logging](https://modelcontextprotocol.io/seps/2577-deprecate-roots-sampling-and-logging)
- [SEP-2596: Spec Feature Lifecycle and Deprecation](https://modelcontextprotocol.io/seps/2596-spec-feature-lifecycle-and-deprecation)

## 릴리스 발표

- [MCP 블로그: 2026-07-28 정식 릴리스](https://blog.modelcontextprotocol.io/posts/2026-07-28/)
- [MCP 블로그: 2026-07-28 릴리스 후보](https://blog.modelcontextprotocol.io/posts/2026-07-28-release-candidate/)
- [AAIF 블로그: 2026-07-28에서 바뀌는 것과 이전 방법](https://aaif.io/blog/mcp-2026-07-28-whats-changing-and-how-to-migrate)

## 표준과 관련 프로토콜

- [JSON-RPC 2.0 Specification](https://www.jsonrpc.org/specification)
- [The OAuth 2.1 Authorization Framework (IETF draft)](https://datatracker.ietf.org/doc/draft-ietf-oauth-v2-1/)
- [RFC 9728: OAuth 2.0 Protected Resource Metadata](https://www.rfc-editor.org/rfc/rfc9728)
- [RFC 8414: OAuth 2.0 Authorization Server Metadata](https://www.rfc-editor.org/rfc/rfc8414)
- [RFC 8707: Resource Indicators for OAuth 2.0](https://www.rfc-editor.org/rfc/rfc8707)
- [RFC 7636: Proof Key for Code Exchange (PKCE)](https://www.rfc-editor.org/rfc/rfc7636)
- [RFC 9207: OAuth 2.0 Authorization Server Issuer Identification](https://www.rfc-editor.org/rfc/rfc9207)
- [RFC 7591: OAuth 2.0 Dynamic Client Registration Protocol](https://www.rfc-editor.org/rfc/rfc7591)
- [OAuth Client ID Metadata Document (IETF draft)](https://datatracker.ietf.org/doc/draft-ietf-oauth-client-id-metadata-document/)
- [RFC 6570: URI Template](https://www.rfc-editor.org/rfc/rfc6570)
- [RFC 3986: URI Generic Syntax](https://www.rfc-editor.org/rfc/rfc3986)
- [JSON Schema 2020-12](https://json-schema.org/draft/2020-12)
- [W3C Trace Context](https://www.w3.org/TR/trace-context/): `_meta`의 `traceparent`
- [A2A (Agent2Agent) Protocol](https://a2a-protocol.org/)
- [ReAct: Synergizing Reasoning and Acting in Language Models](https://arxiv.org/abs/2210.03629)
- [Retrieval-Augmented Generation for Knowledge-Intensive NLP Tasks](https://arxiv.org/abs/2005.11401)

## MCPA 공개 정보

- [MCPA 공식 페이지(Linux Foundation Training)](https://training.linuxfoundation.org/certification/model-context-protocol-associate-mcpa/)
- [Linux Foundation 출시 블로그(2026-09-14)](https://training.linuxfoundation.org/blog/just-launched-model-context-protocol-associate-mcpa-certification/)
- [Linux Foundation 보도자료(2026-09-14)](https://www.linuxfoundation.org/press/agentic-ai-foundation-launches-mcpa-certification-to-validate-mcp-expertise)
- [AAIF 사전 공지(2026-07-10)](https://aaif.io/blog/introducing-the-mcpa-the-first-official-certification-for-the-model-context-protocol)
- [Linux Foundation 객관식 시험 FAQ](https://docs.linuxfoundation.org/tc-docs/certification/faq-mc): 합격선 75%
- [Linux Foundation 객관식 시험 안내(Important Instructions)](https://docs.linuxfoundation.org/tc-docs/certification/important-instructions-mc): 60문항, 90분

## 커뮤니티 자료

- [davila7/model_context_protocol_associate_exam_prep](https://github.com/davila7/model_context_protocol_associate_exam_prep): 연습 문제 119문항, MIT
- [rohitg00/ai-engineering-from-scratch](https://github.com/rohitg00/ai-engineering-from-scratch): [MCPA 자료](https://github.com/rohitg00/ai-engineering-from-scratch/blob/main/certifications/mcpa/README.md), 모의고사 3세트, MIT
- [rohitg00 MCP 2026-07-28 brief 한국어 병기본](../others-exam/rohitg00/brief.ko.md): 스펙 원문, 스키마, changelog를 정리한 요약, MIT
- [WorkOS 블로그: MCPA 보안과 거버넌스 도메인](https://workos.com/blog/mcpa-certification-security-and-governance)
- [spec-2026-07-28.md](../spec-2026-07-28.md): 이 저장소의 2026-07-28 변경 요약
