# 테이블오더 서비스 - Unit of Work 생성 계획

## 계획 개요

사용자 요청에 따라 시스템을 **3개 유닛**으로 분리합니다.
아래 질문에 답변 후, 승인하시면 유닛 생성을 진행합니다.

---

## 유닛 생성 실행 계획

### Phase 1: 유닛 정의
- [x] 3개 유닛의 경계 및 책임 정의
- [x] 각 유닛의 기술 스택 및 디렉토리 구조 정의
- [x] 유닛 간 통신 방식 정의

### Phase 2: 의존성 매트릭스
- [x] 유닛 간 의존성 관계 정의
- [x] 공유 리소스 식별
- [x] 개발 순서 결정

### Phase 3: 스토리 매핑
- [x] 각 스토리를 유닛에 할당
- [x] 크로스-유닛 스토리 식별
- [x] 매핑 검증

---

## 제안하는 3개 유닛 구성

프로젝트 특성(모노레포, Go 백엔드, Vue.js 프론트엔드 2개)을 고려한 자연스러운 분할:

| 유닛 | 이름 | 설명 |
|------|------|------|
| Unit 1 | **Backend API** | Go 백엔드 서버 (REST API + SSE + DB) |
| Unit 2 | **Customer App** | Vue.js 고객용 프론트엔드 |
| Unit 3 | **Admin App** | Vue.js 관리자용 프론트엔드 |

---

## 명확화 질문

아래 질문에 답변해 주세요. 각 `[Answer]:` 태그 뒤에 선택한 알파벳을 입력해 주세요.

---

## Question 1
위에서 제안한 3개 유닛 구성(Backend API / Customer App / Admin App)에 동의하시나요?

A) Yes — 제안대로 진행
B) No — 다른 분할 방식 선호 (아래에 설명)
C) Other (please describe after [Answer]: tag below)

[Answer]: A

---

## Question 2
모노레포 내 디렉토리 구조를 어떻게 구성하시겠습니까?

A) 루트에 3개 폴더 분리:
```
/backend/     (Go API 서버)
/customer/    (Vue.js 고객 앱)
/admin/       (Vue.js 관리자 앱)
```

B) 프론트엔드를 하나의 폴더 아래에:
```
/backend/         (Go API 서버)
/frontend/customer/  (Vue.js 고객 앱)
/frontend/admin/     (Vue.js 관리자 앱)
```

C) 서버가 프론트엔드를 서빙하는 구조:
```
/server/          (Go API + 정적 파일 서빙)
/client/customer/ (Vue.js 고객 앱 → 빌드 후 server에 배포)
/client/admin/    (Vue.js 관리자 앱 → 빌드 후 server에 배포)
```

D) Other (please describe after [Answer]: tag below)

[Answer]: B

---

## Question 3
유닛 간 개발 순서를 어떻게 하시겠습니까?

A) Backend API 먼저 → Customer App → Admin App (순차적)
B) Backend API 먼저 → Customer App + Admin App 동시 (백엔드 우선)
C) 3개 유닛 동시 개발 (API 스펙 먼저 합의 후 병렬)
D) Other (please describe after [Answer]: tag below)

[Answer]: C

---

## Question 4
프론트엔드 두 앱(Customer/Admin) 간 공유 코드가 있을 수 있습니다. 어떻게 처리하시겠습니까?

A) 공유 코드 없음 — 각 앱이 완전히 독립적
B) 공유 패키지 생성 — 공통 타입, API 클라이언트, 유틸리티를 별도 패키지로 분리
C) 필요 시 코드 복사 — 초기에는 복사하고 나중에 리팩토링
D) Other (please describe after [Answer]: tag below)

[Answer]: A

---

모든 질문에 답변하신 후 "완료"라고 알려주세요.
