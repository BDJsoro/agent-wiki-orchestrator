# Quality Gates — CI/CD 4단계 처리 게이트

각 Gate를 통과하지 못하면 다음 Stage로 진입할 수 없다.

---

## Stage 1: Pre-flight Gate
**통과 조건**: session_bootstrap.md 절차 완료 선언
```
✅ skill 파일 7개 읽음
✅ env.md 읽음
✅ index.md 현재 상태 파악
✅ 기준선 wiki 로드 및 출력
```
**실패 시**: 처리 시작 불가. Pre-flight 재실행.

---

## Stage 2: Generation Gate
**통과 조건**: wiki_template.md 모든 필수 섹션 비어있지 않음
```
✅ YAML 필드 전부 존재 (title, claim, type, category, confidence)
✅ @challenger 섹션 1줄 이상
✅ @connector 섹션 1줄 이상
✅ @architect 섹션 아이디어 1개 이상
✅ 가드너 5대 마인드 표 채워짐
✅ 처리 위원회 로그 블록 존재
```
**실패 시**: self_rag_loop.md 실행 → 최대 3회 보완 시도 → 실패 시 ⚠️ 표시 후 confidence -0.1

---

## Stage 3: Quality Gate (기준선 비교)
**통과 조건**: quality_review_checklist.md 실행 후 baseline_score ≥ 5/6
```
✅ skill_linter.md Hard Rules 전부 통과
✅ 기준선 대비 delta ≥ 0
```
**실패 시**: Soft Rules 위반 항목에 ⚠️ 표시, delta에 -N 반영, 처리는 계속 진행

---

## Stage 4: Commit Gate
**통과 조건**: 모든 파일이 올바른 위치에 저장됨
```
✅ library/sources/[원본파일명].md 저장 완료
✅ library/wiki/wiki_[제목].md 저장 완료
✅ library/_meta/index.md 업데이트 완료
✅ inbox/[원본파일명].md 삭제 완료
✅ session_log 업데이트 완료
```
**실패 시**: 실패한 항목만 재시도. 전체 롤백은 하지 않음.

---

## Stage 게이트 통과 출력 형식
```
⏭️ [파일명] 처리 완료
   Stage 1 ✅ | Stage 2 ✅ | Stage 3 ✅ | Stage 4 ✅
   baseline_score: 6/6 | delta: +2
   누적 처리: [N]개
```
