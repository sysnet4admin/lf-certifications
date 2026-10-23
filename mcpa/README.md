# MCPA (Model Context Protocol Associate)

AAIF(Agentic AI Foundation)의 첫 자격증입니다. MCP(Model Context Protocol) 지식을 검증합니다.

> 개인적으로 정리한 연습용 자료입니다. 실제 시험 문항이 아니며 출제 내용을 보장하지 않습니다.
> 리눅스 재단(Linux Foundation)과 AAIF의 공식 자료가 아닙니다.

## 시험 개요

| 항목 | 내용 |
|---|---|
| 형식 | 온라인 감독, 객관식 |
| 시간 | 90분 |
| 문항 수 | 60문항 ([리눅스 재단 객관식 시험 공통 안내](https://docs.linuxfoundation.org/tc-docs/certification/important-instructions-mc), MCPA 공식 페이지에는 따로 적혀 있지 않음) |
| 합격선 | 75% ([리눅스 재단 객관식 시험 FAQ](https://docs.linuxfoundation.org/tc-docs/certification/faq-mc), MCPA 공식 페이지에는 따로 적혀 있지 않음) |
| 기준 스펙 | MCP 2026-07-28 |

가격과 할인 코드는 https://promo.kuberneteslab.dev/ 에서 확인합니다.

## 도메인과 비중

| 도메인 | 비중 | 세부 항목 |
|---|---|---|
| MCP Fundamentals | 16% | MCP의 목적과 범위, 핵심 개념, 상호운용성과 가치 |
| Architecture & Components | 14% | 스키마와 구조화된 데이터, Host, Client 그리고 Server, 모델 상호작용 흐름 |
| Interactions & Execution | 26% | 상호작용 패턴과 응답 처리, 오류 처리, 도구 호출 생애주기, 프로토콜 기본 요소(primitive) |
| Security & Governance | 24% | 신뢰 경계, 권한과 동의, 위험과 안전 통제, 감사와 관측 |
| Use Cases & Ecosystem | 20% | 역할, 책임 그리고 도입, 운영 사례, 생태계와 portability |

## 가장 먼저 알아 둘 것: 스펙 버전

MCPA 공식 페이지는 MCP 2026-07-28 스펙으로 준비하라고 안내합니다. 이 개정에서 `initialize` handshake, 세션(`Mcp-Session-Id`),
`ping`, `resources/subscribe` 등이 없어졌습니다. 2025년에 나온 블로그나 튜토리얼은 없어진 기능을 그대로
설명합니다. [spec-2026-07-28.md](spec-2026-07-28.md)의 "한눈에 보기" 표를 먼저 봅니다.

## 자료 구성

출처별로 폴더를 나눴습니다. 연습 문제 폴더는 만든 사람의 GitHub 아이디를 앞에 붙였습니다. 이 저장소에서 만든 자료는
`cheatsheet/`와 `sysnet4admin-exam/`에 있습니다. 다른 사람이 만든 자료는 `davila7-exam/`과 `rohitg00-exam/`에
옮겼고 폴더마다 원작자의 LICENSE가 있습니다. 원문은 `.en.md`입니다. 영어 원문 아래에
한국어 번역을 붙인 파일은 `.ko.md`입니다.

| 경로 | 내용 |
|---|---|
| [mcpa-guide-ko.pdf](mcpa-guide-ko.pdf) | 시험 안내 슬라이드(26장). 시험 형식, 범위, 공부 방법을 짧게 소개합니다 |
| [spec-2026-07-28.md](spec-2026-07-28.md) | 2026-07-28 스펙에서 바뀐 것 요약 |
| [cheatsheet/](cheatsheet/) | 용어 치트시트(한국어, 영어 PDF) |
| [sysnet4admin-exam/](sysnet4admin-exam/) | sysnet4admin(조훈)이 AI를 사용해 만들고 검증한 예제 시험 8세트(`exam-01.ko.md`~`exam-08.ko.md`)와 참고 자료 목록(`references.md`). 세트마다 60문항이고 영어 문제 아래에 한국어 번역과 해설이 있습니다 |
| [davila7-exam/](davila7-exam/) | davila7 연습 문제 119문항(`questions.en.md`, `questions.ko.md`) |
| [rohitg00-exam/](rohitg00-exam/) | rohitg00 모의고사 3세트 180문항(`mock-01`~`mock-03`)과 모의고사의 근거가 되는 스펙 요약 문서(`brief`) |

## 출처와 라이선스

- 이 저장소에서 만든 자료는 저장소 루트의 [LICENSE](../LICENSE)(MIT)를 따릅니다.
- davila7: [davila7/model_context_protocol_associate_exam_prep](https://github.com/davila7/model_context_protocol_associate_exam_prep)의 커밋 `8e8e7da`(2026-08-01)에서 옮겼습니다. 사이트 페이지마다 흩어진 퀴즈를 한 파일로 모았고 문제와 해설은 원문 그대로입니다. MIT 라이선스이고 원문은 [davila7-exam/LICENSE](davila7-exam/LICENSE)에 있습니다.
- rohitg00: [rohitg00/ai-engineering-from-scratch](https://github.com/rohitg00/ai-engineering-from-scratch)에서 옮겼습니다. MIT 라이선스이고 원문은 [rohitg00-exam/LICENSE](rohitg00-exam/LICENSE)에 있습니다.
  - 모의고사: `certifications/mcpa/assessments/mcpa-f/`의 커밋 `1bafaa8`(2026-09-29). JSON을 마크다운으로 바꿨을 뿐 문제와 해설은 원문 그대로입니다.
  - 스펙 요약 문서(`brief`): `certifications/mcpa/research/mcp-2026-07-28-brief.md`의 커밋 `9f06b4b`(2026-09-25).
- 한국어 번역(`.ko.md`)과 덧붙인 설명은 AI로 작성했습니다.

## 예제 시험

[exam-01](sysnet4admin-exam/exam-01.ko.md), [exam-02](sysnet4admin-exam/exam-02.ko.md), [exam-03](sysnet4admin-exam/exam-03.ko.md), [exam-04](sysnet4admin-exam/exam-04.ko.md),
[exam-05](sysnet4admin-exam/exam-05.ko.md), [exam-06](sysnet4admin-exam/exam-06.ko.md), [exam-07](sysnet4admin-exam/exam-07.ko.md), [exam-08](sysnet4admin-exam/exam-08.ko.md)

MCPA 공식 범위와 관련 문서를 참고해 재조합해서 만들었습니다. 세트마다 도메인 배분을 공식 비중에 맞췄습니다
(Fundamentals 10, Architecture 8, Interactions 16, Security 14, Use Cases 12). exam-04와 exam-05는
davila7과 rohitg00 문항에서 주제를 선택해 다시 작성했습니다.

예제 시험은 AI를 사용해 만들었고 공개 전에 아래처럼 검증했습니다.

- 정답: 정답과 해설을 지운 영어 문제지를 AI 모델이 따로 풀어 정답표와 비교했습니다. 답이 다르게 나온 문항은
  공식 스펙과 대조해 정답을 다시 확인했습니다.
- 해설: exam-01~05는 작성에 참여하지 않은 AI 에이전트가 해설의 사실 문장을 공식 문서와 대조했습니다. 이때 나온
  지적으로 exam-04와 exam-05에서 모두 11문항을 다른 문항으로 바꾸고 해설 9곳을 수정했습니다.

시험마다 정답표 바로 앞에 검증 내용과 참고 자료를 적었습니다.

| 시험 | 정답 교차 확인 | 해설 사실 검증(공식 문서와 대조) |
|---|---|---|
| exam-01~03 | Claude Opus 5.5, Claude Fable 5.1, gpt-6.1-sol(Azure OpenAI) | 했음 |
| exam-04, 05 | Claude Opus 5.5, Claude Fable 5.1 | 했음 |
| exam-06~08 | Claude Opus 5.5(2회) | 하지 않음 |

## 추천 순서

1. [spec-2026-07-28.md](spec-2026-07-28.md)의 "한눈에 보기" 표를 읽습니다.
2. [davila7 연습 문제](davila7-exam/questions.ko.md)를 풉니다. 공식 비중이 가장 큰 Interactions(26%)와 Security(24%) 섹션부터 풉니다.
3. [exam-01](sysnet4admin-exam/exam-01.ko.md), [exam-03](sysnet4admin-exam/exam-03.ko.md), [exam-04](sysnet4admin-exam/exam-04.ko.md), [exam-05](sysnet4admin-exam/exam-05.ko.md)를 90분 시간을 정해 두고 풉니다.
4. [exam-06](sysnet4admin-exam/exam-06.ko.md), [exam-07](sysnet4admin-exam/exam-07.ko.md), [exam-08](sysnet4admin-exam/exam-08.ko.md)을 같은 방식으로 풉니다.
5. 여유가 있으면 [exam-02](sysnet4admin-exam/exam-02.ko.md)와 rohitg00 모의고사([01](rohitg00-exam/mock-01.ko.md), [02](rohitg00-exam/mock-02.ko.md), [03](rohitg00-exam/mock-03.ko.md))를 풉니다. 둘 다 다른 세트보다 세부 동작을 많이 묻습니다.

## 주의

- 공개 자료와 공식 스펙으로 만든 연습용 자료입니다. 실제 시험 문항이 아니며 출제 내용을 보장하지 않습니다.
- 리눅스 재단(Linux Foundation)과 AAIF의 공식 자료가 아닙니다.
- 예제 시험은 AI를 사용해 만들고 위처럼 검증했습니다. 그래도 틀린 곳이 있을 수 있으니 이상한 부분은 이슈로 알려 주세요.
- 스펙이 다시 바뀌면 일부 내용이 맞지 않을 수 있습니다. 공식 스펙이 우선입니다: https://modelcontextprotocol.io/specification/2026-07-28
