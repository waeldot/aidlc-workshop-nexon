# 테이블오더 서비스 - 스토리 생성 계획

## 계획 개요

이 문서는 테이블오더 서비스의 User Stories 생성을 위한 계획입니다.
아래 질문에 답변 후, 승인하시면 스토리 생성을 진행합니다.

---

## 스토리 생성 실행 계획

### Phase 1: 페르소나 정의
- [x] 고객 페르소나 정의 (테이블 주문 고객)
- [x] 관리자 페르소나 정의 (매장 운영자)
- [x] 페르소나별 목표, 동기, 불편사항 정리

### Phase 2: 사용자 스토리 작성
- [x] 고객 스토리 작성 (자동 로그인, 메뉴 조회, 장바구니, 주문, 주문 내역)
- [x] 관리자 스토리 작성 (인증, 실시간 모니터링, 주문 상태 변경, 테이블 관리)
- [x] 시스템 스토리 작성 (메뉴 Seed 데이터, SSE 이벤트 전달)

### Phase 3: 수용 기준 및 검증
- [x] 각 스토리에 Acceptance Criteria 추가
- [x] INVEST 기준 검증
- [x] 페르소나-스토리 매핑 완성

---

## 명확화 질문

아래 질문에 답변해 주세요. 각 `[Answer]:` 태그 뒤에 선택한 알파벳을 입력해 주세요.

---

## Question 1
스토리 분류(breakdown) 방식으로 어떤 것을 선호하시나요?

A) User Journey 기반 — 사용자 워크플로우 순서대로 스토리 구성 (예: 앱 진입 → 메뉴 탐색 → 장바구니 → 주문 → 확인)
B) Feature 기반 — 시스템 기능 단위로 스토리 구성 (예: 인증, 메뉴, 장바구니, 주문, 모니터링)
C) Persona 기반 — 사용자 유형별로 스토리 그룹화 (고객 스토리 묶음 / 관리자 스토리 묶음)
D) Other (please describe after [Answer]: tag below)

[Answer]: B

---

## Question 2
스토리의 세분화 수준(granularity)은 어느 정도가 적절하다고 생각하시나요?

A) 큰 단위 (Epic 수준) — 기능 영역당 1~2개 스토리 (예: "고객으로서 메뉴를 보고 주문할 수 있다")
B) 중간 단위 — 기능별 1개 스토리 (예: "고객으로서 장바구니에 메뉴를 추가할 수 있다")
C) 작은 단위 — 세부 동작별 스토리 (예: "고객으로서 장바구니에서 수량을 1 증가시킬 수 있다")
D) Other (please describe after [Answer]: tag below)

[Answer]: C

---

## Question 3
Acceptance Criteria(수용 기준)의 형식은 어떤 것을 선호하시나요?

A) Given-When-Then (BDD 스타일) — 예: "Given 장바구니에 메뉴가 있을 때, When 주문 확정을 누르면, Then 주문이 생성된다"
B) 체크리스트 형식 — 예: "✓ 주문 번호가 표시된다 ✓ 장바구니가 비워진다"
C) 혼합 (핵심 시나리오는 Given-When-Then, 부가 조건은 체크리스트)
D) Other (please describe after [Answer]: tag below)

[Answer]: C

---

## Question 4
스토리 우선순위를 어떻게 표현하시겠습니까?

A) MoSCoW (Must/Should/Could/Won't)
B) 숫자 우선순위 (P1, P2, P3)
C) 우선순위 표시 불필요 — MVP 범위 내 모든 스토리가 필수
D) Other (please describe after [Answer]: tag below)

[Answer]: C

---

## Question 5
에러/예외 시나리오를 별도 스토리로 분리하시겠습니까?

A) Yes — 에러 시나리오를 별도 스토리로 작성 (예: "고객으로서 주문 실패 시 에러 메시지를 볼 수 있다")
B) No — 에러 시나리오는 해당 기능 스토리의 Acceptance Criteria에 포함
C) Other (please describe after [Answer]: tag below)

[Answer]: A

---

## Question 6
테이블 세션 라이프사이클에서 특별히 강조하고 싶은 시나리오가 있나요?

A) 세션 시작/종료의 기본 플로우만 충분
B) 엣지 케이스 포함 — 예: 세션 종료 중 새 주문 도착, 동시 주문 처리 등
C) 세션 전환 시나리오 포함 — 예: 이전 고객 세션 종료 후 새 고객 시작 전환 과정
D) Other (please describe after [Answer]: tag below)

[Answer]: B

---

모든 질문에 답변하신 후 "완료"라고 알려주세요.
