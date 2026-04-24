# Self-RAG Loop — 자기비평 후 보완 루프

Generation Gate (Stage 2) 또는 Quality Gate (Stage 3) 실패 시 실행한다.
최대 3회까지 자동 보완을 시도한다.

---

## 트리거 조건
- Stage 2: wiki_template.md 필수 섹션이 비어있음
- Stage 3: baseline_score < 5/6 또는 Hard Rule 위반

## 실행 절차

### Round 1: 자기 진단
1. 비어있거나 실패한 섹션을 정확히 파악한다
2. 원본 inbox 파일을 다시 읽고 누락된 정보를 찾는다
3. 해당 섹션을 보완한다
4. @linter Hard Rules 재검토

### Round 2: 확장 검색 (Round 1 실패 시)
1. library/sources/ 에서 관련 파일 1개를 추가로 읽는다
2. 해당 정보로 실패 섹션을 보완한다
3. @linter Hard Rules 재검토

### Round 3: 최소 기준으로 마무리 (Round 2 실패 시)
1. 채울 수 있는 최소한의 내용으로 섹션을 채운다
2. confidence를 원래 값의 -0.1로 조정한다
3. 해당 섹션 하단에 다음을 표시:
   ```
   > ⚠️ 미완성 섹션: [섹션명] — 원본 정보 부족으로 추후 보완 필요
   ```

### Round 3 종료 후
- Stage 4 (Commit Gate)로 진행
- ⚠️ 항목은 차후 @linter Decay 검사 시 우선 검토 대상이 됨

---

## 보완 성공 기준
- Hard Rules 전부 통과
- @linter 비교표에서 baseline_score ≥ 4/6

## 로그 출력
```
🔄 Self-RAG 루프 실행: [파일명]
   Round 1: [성공/실패]
   Round 2: [성공/실패 / 미실행]
   Round 3: [성공/실패 / 미실행]
   결과: baseline_score [N]/6 | delta [+N / 0 / -N]
```
