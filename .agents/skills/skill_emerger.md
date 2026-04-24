# Skill: Pattern Recognition (@emerger)

*   **목표**: 파편화된 자료 더미 깊숙이 묻혀 있는 거시적 통찰 선제 도출.
*   **지침서 (Prompt)**:
    1. 개별 문서 수준에서 논의하지 말고, 누적된 최근 문서들의 집합을 멀리서 내려다보십시오.
    2. [무작위 패턴 교류] 동질적인 카테고리 내에서만 놀지 마십시오. Librarian이 제시한 '전혀 엉뚱한(Ambiguous)' 타겟 카테고리 데이터셋을 억지로 가져와 대조하면서, 두 극단 사이의 숨은 갈망이나 거시적 패턴을 분석하십시오.
    3. 단순 스크랩 폴더들을 통째로 묶어줄 수 있는 상위 비전(Entity, Project)을 사용자에게 제안하십시오.

## 🔍 50개마다 패턴 마이닝 (Cross-Domain Serendipity Mining)

처리 카운터가 50의 배수에 도달할 때마다 다음을 실행하고 결과를 `.agents/logs/emerger_report_N.md`에 저장한다.

### [분석 1] 카테고리 교차 빈도 스캔
1. `library/wiki/` 전체 wiki 파일의 YAML category 필드를 수집
2. 카테고리 쌍(pair)별 동시 출현 빈도를 집계
3. **5회 이상 등장한 쌍** → "확인된 패턴"으로 분류하여 `workflows/wiki_template.md`의 `@connector` 섹션 힌트로 추가
4. 출력 예시:
   ```
   🔗 확인된 패턴: 피드백 × 1on1 (8회), 리더십 × 심리적안전감 (6회)
   → wiki_template.md @connector 섹션에 힌트 추가됨
   ```

### [분석 2] 미발굴 연결 탐색
1. `env.md`의 ALLOWED_CATEGORIES 전체 목록에서 한 번도 함께 등장한 적 없는 쌍을 추출
2. 이를 `.agents/logs/undiscovered_links.md`에 기록
3. 다음 50개 처리 시 @connector가 이 미발굴 쌍을 **의도적으로 연결 시도**
4. 출력 예시:
   ```
   🔎 미발굴 연결 기회: 명상 × ai전략기획, 글쓰기 × 팀성과
   → @connector 다음 배치 우선 연결 대상으로 등록
   ```

