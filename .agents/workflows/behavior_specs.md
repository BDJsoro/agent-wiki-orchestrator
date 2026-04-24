# Behavior Specs — BDD 처리 명세

이 파일은 $doit 명령 시 에이전트가 취해야 할 행동을 **Given-When-Then** 형식으로 명세한다.
모든 처리는 이 명세 내에서 이루어진다.

---

## Scenario 1: 정상 처리 흐름

```gherkin
Given  세션이 시작됨
And    workflows/session_bootstrap.md Pre-flight가 완료됨
And    기준선 wiki가 로드됨
And    inbox/ 폴더에 미처리 파일이 존재함

When   $doit 명령이 입력됨

Then   inbox/ 첫 번째 파일을 읽는다
And    workflows/wiki_template.md를 로드한다
And    7개 위원회 페르소나를 순서대로 실행한다
And    workflows/quality_gates.md의 4개 Gate를 통과해야 한다
And    workflows/quality_review_checklist.md로 기준선과 비교한다
And    통과 시 sources/에 저장, wiki/에 저장, index.md 업데이트, inbox/ 원본 삭제
And    처리 위원회 로그와 baseline_score/delta를 wiki 하단에 기록한다
And    다음 파일로 이동하여 반복한다
And    10번째 파일마다 Kaizen 루프를 실행한다
And    50번째 파일마다 @emerger 패턴 마이닝을 실행한다
```

## Scenario 2: Quality Gate 실패

```gherkin
Given  wiki 초안이 생성됨

When   workflows/quality_gates.md Stage 2 (Generation Gate) 실패
       — 필수 섹션이 비어있음

Then   해당 섹션을 Self-RAG 루프로 보완 시도한다 (최대 3회)
And    3회 후에도 실패 시 wiki 하단에 "⚠️ 미완성 섹션: [섹션명]" 표시
And    confidence를 0.1 낮추고 처리 완료로 간주
And    다음 파일로 이동한다
```

## Scenario 3: Kaizen 루프 (10번째마다)

```gherkin
Given  처리 카운터가 10의 배수에 도달함

When   Kaizen 루프가 트리거됨

Then   직전 10개 wiki를 스캔한다
And    반복 등장한 우수 패턴을 추출한다
And    wiki_template.md 해당 섹션에 패턴 힌트를 추가한다
And    반복 실패 항목을 skill_linter.md Hard Rules에 추가한다
And    .agents/logs/kaizen_log.md에 결과를 기록한다
And    10개 처리를 재개한다
```

## Scenario 4: @emerger 마이닝 (50번째마다)

```gherkin
Given  처리 카운터가 50의 배수에 도달함

When   @emerger 마이닝이 트리거됨

Then   library/wiki/ 전체를 스캔하여 카테고리 교차 빈도를 집계한다
And    5회 이상 등장한 쌍을 wiki_template.md @connector 힌트에 추가한다
And    미발굴 쌍을 .agents/logs/undiscovered_links.md에 기록한다
And    .agents/logs/emerger_report_N.md을 생성한다
And    50개 처리를 재개한다
```
