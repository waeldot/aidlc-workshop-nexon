# 테이블오더 서비스 - 요구사항 명확화 질문

요구사항 문서를 검토했습니다. 아래 질문들에 답변해 주세요.  
각 질문의 `[Answer]:` 태그 뒤에 선택한 알파벳을 입력해 주세요.  
제공된 옵션이 맞지 않는 경우, 마지막 옵션(Other)을 선택하고 설명을 추가해 주세요.

---

## 기술 스택

## Question 1
백엔드 기술 스택으로 어떤 것을 선호하시나요?

A) Node.js (Express / Fastify)
B) Python (FastAPI / Django)
C) Java (Spring Boot)
D) Go
E) Other (please describe after [Answer]: tag below)

[Answer]: D

---

## Question 2
프론트엔드 기술 스택으로 어떤 것을 선호하시나요?

A) React (TypeScript)
B) Vue.js (TypeScript)
C) Next.js (React 기반 SSR)
D) Vanilla JavaScript (프레임워크 없음)
E) Other (please describe after [Answer]: tag below)

[Answer]: B

---

## Question 3
데이터베이스로 어떤 것을 선호하시나요?

A) PostgreSQL (관계형 DB)
B) MySQL (관계형 DB)
C) MongoDB (NoSQL 문서형)
D) SQLite (경량 관계형 DB, 개발/소규모용)
E) Other (please describe after [Answer]: tag below)

[Answer]: D

---

## Question 4
애플리케이션 배포 환경은 어디인가요?

A) AWS (클라우드)
B) 로컬 서버 / 온프레미스
C) Docker 컨테이너 (환경 무관)
D) 아직 결정하지 않음 (개발 우선)
E) Other (please describe after [Answer]: tag below)

[Answer]: C

---

## 아키텍처

## Question 5
백엔드와 프론트엔드의 구성 방식은 어떻게 하시겠습니까?

A) 단일 레포지토리 (모노레포) - 백엔드 + 프론트엔드 함께
B) 분리된 레포지토리 - 백엔드와 프론트엔드 별도
C) 백엔드 API 서버 + 프론트엔드 정적 파일 서빙 (단일 서버)
D) Other (please describe after [Answer]: tag below)

[Answer]: A

---

## Question 6
고객용 UI와 관리자용 UI를 어떻게 구성하시겠습니까?

A) 단일 프론트엔드 앱 (라우팅으로 분리)
B) 별도의 두 개 프론트엔드 앱
C) Other (please describe after [Answer]: tag below)

[Answer]: B

---

## 메뉴 관리

## Question 7
메뉴 이미지는 어떻게 관리하시겠습니까?

A) 이미지 URL만 저장 (외부 이미지 링크)
B) 서버에 이미지 파일 업로드 및 저장
C) 클라우드 스토리지 (S3 등) 연동
D) Other (please describe after [Answer]: tag below)

[Answer]: A

---

## 테이블 및 매장 설정

## Question 8
초기 MVP에서 지원할 매장 수는 어느 정도인가요?

A) 단일 매장 (1개 매장만 지원)
B) 소규모 멀티 매장 (2~10개)
C) 확장 가능한 멀티 매장 (제한 없음)
D) Other (please describe after [Answer]: tag below)

[Answer]: A

---

## Question 9
테이블 수는 매장당 최대 몇 개를 지원해야 하나요?

A) 소규모 (최대 10개)
B) 중규모 (최대 30개)
C) 대규모 (최대 100개 이상)
D) Other (please describe after [Answer]: tag below)

[Answer]: A

---

## 실시간 통신

## Question 10
관리자 화면의 실시간 주문 업데이트에 SSE(Server-Sent Events)를 사용하기로 명시되어 있습니다.  
고객 화면의 주문 상태 업데이트(선택사항으로 명시됨)는 어떻게 처리하시겠습니까?

A) SSE 사용 (관리자와 동일 방식)
B) 주기적 폴링 (예: 10초마다 새로고침)
C) MVP에서는 실시간 업데이트 미구현 (수동 새로고침)
D) Other (please describe after [Answer]: tag below)

[Answer]: A

---

## 보안 및 인증

## Question 11
테이블 태블릿의 자동 로그인 토큰은 어떻게 저장하시겠습니까?

A) localStorage (브라우저 로컬 스토리지)
B) sessionStorage (세션 스토리지)
C) 쿠키 (HttpOnly 쿠키)
D) Other (please describe after [Answer]: tag below)

[Answer]: A

---

## Question 12 (Security Extension Opt-In)
이 프로젝트에 보안 확장 규칙을 적용하시겠습니까?

A) Yes — 모든 보안 규칙을 필수 제약으로 적용 (프로덕션 수준 애플리케이션에 권장)
B) No — 보안 규칙 생략 (PoC, 프로토타입, 실험적 프로젝트에 적합)
C) Other (please describe after [Answer]: tag below)

[Answer]: A

---

## Question 13 (Property-Based Testing Extension Opt-In)
이 프로젝트에 속성 기반 테스팅(PBT) 규칙을 적용하시겠습니까?

A) Yes — 모든 PBT 규칙을 필수 제약으로 적용 (비즈니스 로직, 데이터 변환, 직렬화, 상태 컴포넌트가 있는 프로젝트에 권장)
B) Partial — 순수 함수와 직렬화 라운드트립에만 PBT 규칙 적용
C) No — PBT 규칙 생략 (단순 CRUD, UI 전용, 비즈니스 로직이 없는 통합 레이어에 적합)
D) Other (please describe after [Answer]: tag below)

[Answer]: A

---

## 추가 요구사항

## Question 14
메뉴 관리 기능(MVP 범위에 명시되지 않음)을 MVP에 포함하시겠습니까?

A) Yes — 메뉴 CRUD 기능을 MVP에 포함
B) No — MVP에서는 메뉴를 DB 초기 데이터(seed)로만 관리
C) Other (please describe after [Answer]: tag below)

[Answer]: B

---

## Question 15
주문 상태 변경 기능(대기중/준비중/완료)을 MVP에 포함하시겠습니까?

A) Yes — 관리자가 주문 상태를 변경할 수 있어야 함
B) No — MVP에서는 주문 상태 변경 불필요
C) Other (please describe after [Answer]: tag below)

[Answer]: A

---

모든 질문에 답변하신 후 "완료"라고 알려주세요.
