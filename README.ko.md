# Decision Case Pooling

**AI 보조 소프트웨어 개발 과정에서 내려진 엔지니어링 의사결정을, 근거와 함께 기록하기 위한 에이전트 스킬입니다.**

AI는 설계를 제안하고, 코드를 작성하고, 테스트를 실행하고, 실패를 진단할 수 있습니다. 그래서 어떤 판단이 실제로 사람 엔지니어의 것이었는지, 어떤 선택지가 AI에서 나왔는지, 어떤 결과가 실제로 검증되었는지를 나중에 재구성하기가 더 어려워집니다. Decision Case Pooling은 그 구분들을 검색 가능한 Markdown/YAML 케이스 풀에 모읍니다.

이것은 자동 생산성 점수도, AI 의존도 지표도, 보기 좋은 포트폴리오 스토리 생성기도 **아닙니다**.

## 설치

Skills CLI 사용:

```sh
npx skills add aassaapp/decision-case-pooling --skill decision-case-pooling
```

또는 Codex 스킬 설치기에 `https://github.com/aassaapp/decision-case-pooling` 루트에서 스킬을 설치하도록 요청하세요.

## 프롬프트 예시

```text
$decision-case-pooling 이번 세션에서 가장 의미 있었던 엔지니어링 의사결정을 찾아줘.
$decision-case-pooling 이 의사결정을 기록해줘. 누가 문제를 정의하고, 테스트하고, 선택했는지 포함해서.
$decision-case-pooling 이 케이스를 최신 검증 근거로 업데이트해줘.
$decision-case-pooling 케이스 풀 전체에서 사람의 독립적 판단과 AI 주도 작업을 비교 분석해줘.
$decision-case-pooling 빠진 기여 내용을 추측하지 말고 이 v2 케이스들을 v3로 마이그레이션해줘.
```

이 스킬은 실제로 접근할 수 있는 대화, 코드, 기록만 사용합니다. 과거 대화를 자동으로 수집하거나, 백그라운드에서 실행되거나, MCP 서버나 Notion 연결을 요구하지 않습니다.

## 케이스에 기록되는 내용

각 케이스는 YAML 프론트매터와 근거 기반 서술로 이루어진 하나의 Markdown 파일입니다.

세 가지 귀속(attribution) 축은 의도적으로 분리되어 있습니다.

1. **의사결정 주체 (`decided_by`):** 최종 접근법을 누가 선택했는가?
2. **검토 상태 (`user_review`):** 사람이 그 특정 의사결정을 실제로 살펴보았는가?
3. **단계별 기여 (`contributions`, v3 신규):** 누가 문제를 정의하고, 가설을 세우고, 테스트를 설계하고, 변경을 구현하고, 결과를 해석하고, 결론을 선택했는가? 특정 단계에서 사람의 독립적 판단에 대한 직접적인 근거가 있는가?

예를 들어, 사람이 결과를 읽은 뒤 AI가 제안한 설계를 선택할 수 있습니다. 이는 **사람이 소유한 의사결정**이지만, **사람이 그 설계를 독립적으로 고안했다는 근거는 아닙니다**. 반대로, 사람이 결함 있는 전제를 독립적으로 발견하고 이후 코드 변경은 전부 AI가 수행할 수도 있습니다.

### v3 기여 기록 예시

```yaml
schema_version: 3
decided_by: user
user_review: reviewed
contributions:
  problem_framing:
    lead: user
    human_actions: [originated, challenged]
    independent_judgment: demonstrated
    evidence: [E1]
  implementation:
    lead: ai
    human_actions: [delegated]
    independent_judgment: not_observed
    evidence: [E2]
  final_decision:
    lead: user
    human_actions: [selected, reviewed]
    independent_judgment: not_observed
    evidence: [E3]
```

여기서 `not_observed`는 **해당 단계에서 사람의 독립적 판단에 대한 근거가 없다**는 뜻입니다. 그 사람에게 역량이 없다는 뜻이 아닙니다. 누락된 단계는 분류되지 않은 것이며, AI 소유로 집계해서는 안 됩니다.

## 저장소 및 케이스 풀 구조

이 공개 저장소는 스킬을 배포하는 곳이지, 당신의 비공개 작업 이력을 담는 곳이 아닙니다.

```text
<central-case-pool>/
├── index.md
└── <project>/
    └── 2026-10-05-topic.md
```

선택적으로, 프로젝트 저장소에 그 중앙 위치를 가리키는 `.casepool` 포인터를 둘 수 있습니다. 스킬은 이 포인터를 만들기 전에 사용자에게 물어야 합니다. 케이스 기록은 소스 저장소 여기저기에 흩어지지 않고, 지정된 중앙 위치에 남습니다.

템플릿에는 상황, 의사결정 계기, 실제 대안, 기여 연대기, 검증과 한계, 교훈, 근거, 열린 질문이 포함됩니다.

## 거짓된 정밀도 없는 분석

여러 케이스에 걸쳐, 스킬은 의사결정 주체, 검증된 검토, 관찰된 단계 주도자, 입증된 사람의 독립적 판단을 집계할 수 있습니다. 이때 분모, 미상 항목, 서로 연관된 케이스, 근거의 한계를 반드시 함께 보고해야 합니다.

**이 수치를 전체 인간 기여도 퍼센트, 엔지니어링 역량 점수, 채용 적합도 예측으로 변환해서는 안 됩니다.** 케이스는 흥미로운 의사결정을 의도적으로 골라낸 것이므로, 전체 작업 시간을 대표하지 않습니다.

학습 목적이라면, AI가 주도한 선택을 도움 없이 설명하거나 재현해보는 후속 활동을 선택적으로 해볼 수 있습니다. 그것은 현재 역량에 대한 새로운 근거이지, 원래 의사결정에 대한 소급 주장이 아닙니다.

## 버전 호환성

**v3는 기존 v2 필드를 보존하면서 단계별 기여를 추가합니다.** 기존 v2 기록은 그대로 유효하고 읽을 수 있습니다. 자동으로 재작성되지 않으며, `contributions`가 없다는 것은 *AI 주도*가 아니라 *미분류*를 뜻합니다.

케이스 업그레이드는 원본 대화, 관련 코드, 테스트 근거를 확보한 상태에서만 하세요. 안정적인 UUID와 원래 날짜를 유지하고, 근거가 있는 단계만 추가하며, 사람이 작성한 내용은 보존하세요. 자세한 규칙은 [SKILL.md](SKILL.md)와 [MIGRATION.md](MIGRATION.md)에 있습니다.

## 파일

- [SKILL.md](SKILL.md) — 운영 규칙, 출처 판단 기준, 귀속, 분석, 저장.
- [references/case-template.md](references/case-template.md) — v3 Markdown/YAML 케이스 템플릿.
- [MIGRATION.md](MIGRATION.md) — 안전한 v2→v3 마이그레이션 예시와 검증 체크리스트.
- [agents/openai.yaml](agents/openai.yaml) — Codex 표시용 메타데이터.

## 프라이버시와 신뢰성

AI가 생성한 회고는 인용된 기록으로 뒷받침되는 해석일 뿐, 역량에 대한 독립적인 증명이 아닙니다. 공개하기 전에 비밀정보, 내부 문서, 개인 식별 정보, 기타 기밀 정보를 제거하세요. 로컬에 저장하는 것이 공개나 푸시를 허가하는 것은 아닙니다.
