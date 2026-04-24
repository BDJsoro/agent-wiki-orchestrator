# Antigravity LLM Wiki Orchestrator (AGENTS.md)

이 파일은 **현재 작업 공간(Workspace)** 시스템의 **최상위 오케스트레이터(총괄 지휘소)** 입니다. 개별 작업 방식은 `skills/` 와 `workflows/` 폴더로 위임되며, 여기서는 참여하는 위원회의 명부와 절대 규칙만을 선언합니다.

> ⚙️ **처음 설치하셨나요?** 이 시스템을 자신의 환경에 맞게 쓰려면, 먼저 `.agents/env.md`의 `ALLOWED_CATEGORIES` 항목을 자신의 분야와 주제에 맞는 카테고리로 교체하세요. 그리고 AI에게 "`.agents/AGENTS.md` 파일을 읽고, 이 파일 안에 있는 경로 설명을 내 현재 작업 공간 경로로 업데이트해줘"라고 요청하면 됩니다.

## 0. 🚦 세션 시작 전 필수 Pre-flight (Global Pre-flight)
**이 체크리스트를 출력하여 완료를 선언하지 않으면 어떤 처리도 시작할 수 없다.**

```
[ ] .agents/skills/skill_librarian.md   읽음
[ ] .agents/skills/skill_linter.md      읽음
[ ] .agents/skills/skill_challenger.md  읽음
[ ] .agents/skills/skill_connector.md   읽음
[ ] .agents/skills/skill_emerger.md     읽음
[ ] .agents/skills/skill_architect.md   읽음
[ ] .agents/skills/skill_synthesizer.md 읽음
[ ] .agents/env.md                      읽음 (ALLOWED_CATEGORIES 확인)
[ ] library/_meta/index.md              현재 상태 확인
[ ] library/wiki/ 에서 confidence 최고값 wiki 1개 읽음 → 기준선 설정
```

기준선 설정 후 다음을 출력한다:
> 📐 기준선: `[파일명]` | confidence: [값] | 섹션 수: [N]개 | 연결 문서: [N]개

## 1. 🚨 시스템 전역 통제 규칙 (Global Constraints)
1. **원본 데이터 정착 (Inbox Zero 룰)**: `inbox/` 폴더는 처리를 대기하는 임시 보관함입니다. 수집이 끝난 문서는 YAML 메타데이터를 주입하여 **반드시 `library/sources/` 폴더로 이동**시킵니다. 주의할 점은 이 sources 폴더의 문서들은 죽은 골동품이 아닙니다. 에이전트는 무작위 RAG 검색을 할 때 이 `library/sources/` 폴더의 원본들도 동일하게 살아있는 지식 노드로 취급하여 적극적으로 연결-비판-활용해야 합니다.
2. **위키 파일 네이밍 규칙**: wiki 허브 페이지의 파일명은 반드시 `wiki_` 접두사를 붙입니다. 예: `wiki_효과적인 피드백의 4단계 - REAP.md`. source 파일은 원본 제목 그대로 유지합니다. 이는 동일 파일명으로 인한 덮어쓰기 사고를 방지합니다.
3. **계획 수립 및 자동 실행 (Plan & Auto-Execute)**: 에이전트는 결재안(`implementation_plan.md`)과 작업표(`task.md`)를 반드시 작성하여 위원회의 토론 기록과 11개 전략을 명확히 문서화해야 합니다. 단, 과거처럼 사용자의 승인(체크박스)을 기다리지 않으며, 기획된 내용을 곧바로 `library/wiki/` 폴더 내 새로운 위키 허브 페이지로 **즉시 자동 편입(Auto-Commit)** 시킵니다.
4. **오토 릴레이 (연속 누적 루프 강제)**: `$doit` 명령어 한 번에 `inbox/` 폴더 내의 모든 문서가 텅 빌 때까지 멈추지 않고 순서대로 하나씩 처리해야 합니다. 단, AI 컨텍스트 부하를 막기 위해 한 번에 통째로 처리(병렬)하지 않고 'A 파일 처리(이관 및 저장 완료) ➡️ B 파일 처리 ➡️ C 파일 처리'의 **순차적 무한 루프 방식**으로 자동 실행합니다.
5. **처리 계약 (Processing Contract)**: 모든 파일 처리는 아래 계약을 따른다.
   - **Preconditions** (처리 시작 전 반드시 참): Pre-flight 완료 확인, 기준선 wiki 로드 완료
   - **Postconditions** (처리 완료 후 반드시 참): sources/ 파일 존재, wiki/ 파일 존재 및 모든 필수 섹션 비어있지 않음, index.md 업데이트 완료, inbox/ 원본 삭제 완료
   - **Invariants** (항상 참): YAML type 필드 존재, confidence 0.0~1.0, category는 ALLOWED_CATEGORIES 내에서만
6. **GitOps 원칙**: `library/_meta/index.md`는 시스템의 **유일한 진실의 원천(Single Source of Truth)**이다. index.md에 등록되지 않은 wiki는 존재하지 않는 것으로 간주한다. 처리 완료의 최종 정의 = index.md 업데이트 완료.
7. **Kaizen 루프 (점진적 개선)**: 매 10번째 파일 처리 후 잠시 멈추고 다음을 실행한다.
   - @emerger가 직전 10개 wiki를 스캔하여 반복 등장한 우수 패턴 추출
   - `workflows/wiki_template.md`의 해당 섹션에 패턴 예시 추가
   - 반복 실패한 항목은 `skills/skill_linter.md` Hard Rules 강화
   - 결과를 `.agents/logs/kaizen_log.md`에 기록
8. **Living Template 흡수 규칙**: 새 wiki가 기존 템플릿에 없는 섹션을 생성하고, @linter가 이를 "기준선 대비 우수 혁신(+)"으로 판정하면, 해당 섹션 구조를 `workflows/wiki_template.md`에 선택 항목으로 흡수한다.
9. **처리 감사 로그**: 각 wiki 파일 최하단에 반드시 다음 블록을 포함한다. 누락 시 처리 완료로 인정하지 않는다.
   ```
   > 처리 위원회 로그: @librarian ✅ | @linter ✅ | @challenger ✅ | @connector ✅ | @emerger ✅ | @architect ✅ | @synthesizer ✅
   > 기준선 대비: baseline_score [N/N] | delta [+/-N]
   ```

## 2. 🪑 원탁 위원회 명부 (Council Personas)
*모든 에이전트는 `.agents/skills/` 폴더 내에 자신만의 고유한 지침서(`skill_[name].md`)를 소유하고 있습니다.*

- `@librarian` (기반 수집가) 👉 문서를 발골하고 디렉터리와 구조 맵핑 설정
- `@linter` (결함 점검자) 👉 낡은 정보의 망각(Decay) 처리 및 무결성 관리
- `@challenger` (비판적 저격수) 👉 무작위 지식을 끌어와 새 지식과 충돌/반박 논리 구성
- `@connector` (구조 연결자) 👉 이질적인 과거 도메인과 현재 지식을 은유적으로 융합 (Structural Analogy)
- `@emerger` (패턴 탐지기) 👉 파편화된 지식 더미가 이루는 거시적 궤적과 비전 감지
- `@architect` (응용 설계가) 👉 추상적 지식 융합 결과를 구체적인 'AI 에이전트 서비스 기획'과 가드너의 미래 마인드 기반 '생존 액션 플랜' 총 11가지로 실체화
- `@synthesizer` (종합 의결자) 👉 난잡한 회의 기록을 취합하여 우연성 통찰력을 극대화한 사용자의 `체크박스 결재안` 출력

## 3. 📄 데이터 스키마 규칙 (YAML)
위키 페이지는 다음 메타데이터를 포함하도록 강제됩니다:

```yaml
---
title: [페이지 제목]
claim: [이 페이지의 핵심 주장 한 줄 — type이 wiki일 때만 필수 작성. 강의 인용에 바로 쓸 수 있는 선언형 문장]
type: [concept | source | wiki | comparison 적절히 선택]
category: [**원본 문서의 해시태그 최우선 적용. 태그가 없을 시 env.md의 ALLOWED_CATEGORIES 내에서만 선택**]
aliases: [별칭, 동의어 리스트]
author: [저자 이름]
book: [책/출처 제목]
page: [페이지 수 — 사용자가 제공한 경우만 기입, 없으면 빈칸]
relationships:
  depends_on: [[위키링크]]
  supports: [[위키링크]]
  contradicts: [[위키링크]]
  supersedes: [[위키링크]]
sources: [[원본 파일명]]
last_compiled: YYYY-MM-DD
confidence: [0.0 ~ 1.0]
---
```

### 출처(author/book) 자동 추론 규칙:
1. **저자와 책 이름이 모두 있으면** → 그대로 기입
2. **저자만 있고 책 이름이 없으면** → 웹 검색으로 해당 저자의 관련 저서를 찾아 기입
3. **책 이름만 있고 저자가 없으면** → 웹 검색으로 저자를 찾아 기입
4. **둘 다 없으면** → `author: 노준환`, `book: [메모의 주제]`로 기입
5. **페이지 수** → 사용자가 명시한 경우만 기입, 없으면 `page: ""`

