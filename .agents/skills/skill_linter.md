# Skill: Decay & Integrity Check (@linter)

*   **목표**: 시스템 내 지식의 부패(Decay) 및 무결성 파괴 방어.
*   **지침서 (Prompt)**:
    1. **고립 페이지 포착**: 어떤 외부 문장과도 연결되지 못하는 페이지(Orphans)가 생기지 않도록 경고하십시오.
    2. **가변적 망각 지수 알고리즘 (Dynamic Decay Math)**: 과거 지식과 마주했을 때, 하드코딩된 값을 쓰지 말고 **반드시 루트 디렉터리의 `.agents/env.md` 파일을 먼저 읽으십시오.** 그곳에 명시된 `DECAY_PERIOD`와 `DECAY_SCORE` 값을 수식에 대입하여 점수를 삭감하십시오.
    3. [무작위 렌즈 검증] 점수를 깎기 전에, 위키 내의 **아주 엉뚱하거나 반대되는 랜덤 노드**의 시각을 빌려와 질문하십시오. "이 오래된 지식이 완전히 전혀 다른 OOO의 관점에서 봐도 여전히 참인가?"
    4. 점수가 `env.md`의 `DECAY_TRIGGER_MIN_SCORE` 이하로 떨어지거나 새 팩트를 통해 무작위 렌즈 검증에서 파괴된 경우, 과감하게 `superseded_by` (대체됨) 속성을 부여할 것을 제안하십시오.

## 📏 품질 검증 규칙 (Quality Rules)

### Hard Rules — 필수 통과 (위반 시 처리 완료 불인정)
- [ ] YAML `title` 필드 존재하고 비어있지 않음
- [ ] YAML `claim` 필드 존재하고 비어있지 않음 (type: wiki인 경우)
- [ ] YAML `category` 가 ALLOWED_CATEGORIES 내에서 선택됨
- [ ] YAML `confidence` 가 0.0~1.0 범위
- [ ] `@challenger` 섹션 존재하고 1줄 이상
- [ ] `@architect` 섹션 존재하고 아이디어 1개 이상
- [ ] 처리 위원회 로그 블록 존재
- [ ] sources/ 와 wiki/ 파일이 동시에 생성됨
- [ ] index.md에 등록됨

### Soft Rules — 권고 (위반 시 delta -1, ⚠️ 표시)
- [ ] category가 배열 형식 `[]`로 2개 이상
- [ ] 연결 문서 3개 이상
- [ ] `@architect` 아이디어 2개 이상
- [ ] confidence 0.8 이상 (출처 명시 근거 있을 때)
- [ ] `@emerger` 섹션 존재

### 기준선 vs 신규 wiki 비교 출력 형식
```
| 항목              | 기준선 | 이번 wiki | 판정 |
|------------------|--------|----------|------|
| claim 선언형      | ✅     | ?        | ✅/❌ |
| @challenger 작성  | ✅     | ?        | ✅/❌ |
| @architect 아이디어| 3개    | ?        | ✅/❌ |
| 연결 문서 수       | N개    | ?        | ✅/❌ |
| 카테고리 배열 형식 | ✅     | ?        | ✅/❌ |
| 처리 위원회 로그   | ✅     | ?        | ✅/❌ |

baseline_score: [통과항목/전체항목]
delta: [+N / -N / 0]
```

