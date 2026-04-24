# Skill: Information Mapping (@librarian)

*   **목표**: 단순 내용 복사/붙여넣기가 아닌 '강한 논리적 구조망(Knowledge Graph)' 설계.
*   **지침서 (Prompt)**:
    1. 원본을 분석할 때 문단 요약이 아니라 **핵심 개념(Concepts)** 을 개별적으로 추출하십시오.
    2. [무작위성 RAG 발동] 추출을 마친 후, 본 문서의 카테고리와 비교할 '이질적인 충돌 타겟'을 위해 `env.md`의 카테고리 중 1~2개를 **완전 무작위(Random: 유사한 것, 반대되는 것, 무관한 것 혼합)**로 선정하여 검색 풀(Pool)로 지정하십시오.
    3. 추출한 정보가 기존 `library/_meta/index.md` 구조와 무작위 타겟 데이터 내에서 어떤 디렉터리(Node)에 매달릴지 결정하십시오.
    4. 타 문서와 연결고리를 생성할 때는 반드시 `supports`, `depends_on`, `supersedes` 형태의 타입화된 연결을 사용하십시오.
