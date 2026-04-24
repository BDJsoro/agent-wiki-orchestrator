# Session Bootstrap — 세션 시작 Pre-flight 절차

$doit 명령어를 받은 즉시 이 파일의 절차를 순서대로 실행한다.
이 체크리스트를 완료하여 출력하지 않으면 어떤 inbox 처리도 시작할 수 없다.

---

## Step 1: Skill 파일 읽기 (순서대로)
```
[ ] .agents/skills/skill_librarian.md
[ ] .agents/skills/skill_linter.md
[ ] .agents/skills/skill_challenger.md
[ ] .agents/skills/skill_connector.md
[ ] .agents/skills/skill_emerger.md
[ ] .agents/skills/skill_architect.md
[ ] .agents/skills/skill_synthesizer.md
```

## Step 2: 환경 설정 확인
```
[ ] .agents/env.md 읽음
    → ALLOWED_CATEGORIES 목록 메모리에 로드
    → BASELINE_PERCENTILE, BASELINE_MIN_SAMPLE 확인
[ ] library/_meta/index.md 읽음
    → 현재 등록된 카테고리 및 wiki 목록 파악
```

## Step 3: 기준선 설정
```
[ ] library/wiki/ 폴더 내 전체 파일 목록 확인
[ ] YAML confidence 필드 기준으로 최고값 wiki 1개 선택
[ ] 해당 wiki 파일 내용 읽기
[ ] 기준선 출력:
```

> 📐 기준선: `[파일명]` | confidence: [값] | 섹션 수: [N]개 | 연결 문서: [N]개

## Step 4: 처리 카운터 확인
```
[ ] .agents/logs/ 폴더에서 가장 최근 session_log 파일 확인
    → 이전 세션까지 총 처리된 파일 수 파악
    → 다음 Kaizen 루프까지 남은 파일 수 계산 (10개마다)
    → 다음 @emerger 마이닝까지 남은 파일 수 계산 (50개마다)
[ ] 출력:
```
> 📊 누적 처리: [N]개 | 다음 Kaizen까지: [N]개 | 다음 Emerger 마이닝까지: [N]개

## Step 5: Pre-flight 완료 선언
모든 위 항목이 완료되면 다음을 출력하고 inbox 처리를 시작한다:

```
✅ Pre-flight 완료. 처리를 시작합니다.
📐 기준선: [파일명] (confidence: [값])
📊 누적 처리: [N]개
🚦 workflows/behavior_specs.md 명세에 따라 처리를 진행합니다.
```
