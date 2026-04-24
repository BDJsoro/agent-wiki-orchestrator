# Quality Review Checklist — 기존 wiki 구조·품질 비교 검토

이 절차는 Stage 3 (Quality Gate) 내에서 실행된다.
새 wiki를 기준선 wiki와 항목별로 비교하여 gap을 탐지한다.

---

## 실행 시점
wiki 초안 완성 직후, index.md 업데이트 전

## Step 1: 기준선 wiki 확인
- session_bootstrap.md에서 이미 로드한 기준선 wiki를 참조
- 기준선의 다음 항목을 측정값으로 기록:
  - claim 문장 존재 여부 및 선언형 강도
  - @challenger 섹션 존재 및 줄 수
  - @architect 아이디어 개수
  - 연결 문서([[링크]]) 개수
  - 카테고리 형식 (단일 문자열 vs 배열)
  - 처리 위원회 로그 존재 여부

## Step 2: 신규 wiki 동일 항목 측정

## Step 3: 비교표 출력 (wiki 하단 @linter 섹션에 삽입)
```markdown
| 항목 | 기준선 | 이번 wiki | 판정 |
|---|---|---|---|
| claim 선언형 | ✅ | [✅/❌] | [통과/경고] |
| @challenger 작성 | ✅ ([N]줄) | [✅/❌] ([N]줄) | [통과/경고] |
| @architect 아이디어 | [N]개 | [N]개 | [통과/경고] |
| 연결 문서 수 | [N]개 | [N]개 | [통과/경고] |
| 카테고리 배열 형식 | [✅/❌] | [✅/❌] | [통과/경고] |
| 처리 위원회 로그 | ✅ | [✅/❌] | [통과/경고] |

baseline_score: [통과항목]/6
delta: [+N / 0 / -N]
```

## Step 4: Gap 처리 규칙

| 상황 | 처리 |
|---|---|
| Gap 없음 (delta ≥ 0) | 처리 완료, Stage 4로 |
| Gap 있음 (delta < 0) | self_rag_loop.md 실행 → 보완 시도 |
| 보완 후에도 gap | ⚠️ 표시 후 Stage 4 진행 |
| Hard Rule 위반 | Stage 2로 되돌아가 보완 |

## Step 5: 기준선 자동 갱신 여부 판단
- 신규 wiki의 confidence ≥ 현재 기준선 wiki의 confidence
- AND delta ≥ +2
- → 이 wiki를 새 기준선 후보로 기록 (.agents/logs/baseline_candidates.md)
- → 다음 세션 Pre-flight 시 최고 confidence wiki가 자동으로 기준선이 됨
