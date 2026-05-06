# 테이블오더 서비스 - Unit of Work 정의

## 유닛 구성 요약

| 유닛 | 이름 | 기술 스택 | 역할 |
|------|------|-----------|------|
| Unit 1 | Backend API | Go + SQLite | REST API, SSE, 인증, DB 관리 |
| Unit 2 | Customer App | Vue.js (TypeScript) | 고객 주문 화면 (태블릿) |
| Unit 3 | Admin App | Vue.js (TypeScript) | 관리자 모니터링/관리 화면 |

---

## 모노레포 디렉토리 구조

```
table-order/
├── backend/                    # Unit 1: Go Backend API
│   ├── cmd/
│   │   └── server/
│   │       └── main.go        # 엔트리포인트
│   ├── internal/
│   │   ├── auth/              # 인증 모듈 (JWT, bcrypt)
│   │   ├── handler/           # HTTP 핸들러 (라우트)
│   │   ├── middleware/        # 미들웨어 (CORS, 보안 헤더, 레이트리밋)
│   │   ├── model/             # 데이터 모델
│   │   ├── repository/        # DB 접근 계층
│   │   ├── service/           # 비즈니스 로직
│   │   ├── sse/               # SSE 이벤트 관리
│   │   └── seed/              # 초기 데이터 (메뉴 Seed)
│   ├── db/
│   │   └── migrations/        # DB 마이그레이션
│   ├── go.mod
│   ├── go.sum
│   └── Dockerfile
├── frontend/
│   ├── customer/              # Unit 2: Customer App
│   │   ├── src/
│   │   │   ├── components/    # Vue 컴포넌트
│   │   │   ├── views/         # 페이지 뷰
│   │   │   ├── stores/        # Pinia 상태 관리
│   │   │   ├── services/      # API 호출, SSE 클라이언트
│   │   │   ├── types/         # TypeScript 타입 정의
│   │   │   ├── router/        # Vue Router
│   │   │   └── utils/         # 유틸리티
│   │   ├── package.json
│   │   ├── vite.config.ts
│   │   ├── tsconfig.json
│   │   └── Dockerfile
│   └── admin/                 # Unit 3: Admin App
│       ├── src/
│       │   ├── components/    # Vue 컴포넌트
│       │   ├── views/         # 페이지 뷰
│       │   ├── stores/        # Pinia 상태 관리
│       │   ├── services/      # API 호출, SSE 클라이언트
│       │   ├── types/         # TypeScript 타입 정의
│       │   ├── router/        # Vue Router
│       │   └── utils/         # 유틸리티
│       ├── package.json
│       ├── vite.config.ts
│       ├── tsconfig.json
│       └── Dockerfile
├── docker-compose.yml          # 전체 서비스 오케스트레이션
└── README.md
```

---

## Unit 1: Backend API

### 책임
- REST API 엔드포인트 제공 (고객 + 관리자)
- SSE 이벤트 스트림 관리 (주문 상태 변경, 신규 주문 알림)
- 인증/인가 (테이블 토큰, 관리자 JWT)
- SQLite 데이터베이스 관리
- 비즈니스 로직 (주문 생성, 상태 변경, 세션 관리)
- 메뉴 Seed 데이터 초기화
- 보안 (입력값 검증, 레이트 리미팅, 보안 헤더)

### 기술 스택
| 항목 | 선택 |
|------|------|
| 언어 | Go |
| HTTP 프레임워크 | 표준 라이브러리 (net/http) 또는 Chi/Echo |
| DB | SQLite (go-sqlite3 또는 modernc.org/sqlite) |
| 인증 | JWT (golang-jwt) |
| 해싱 | bcrypt (golang.org/x/crypto/bcrypt) |
| 검증 | go-playground/validator |
| 로깅 | slog (표준 라이브러리) |
| 테스트 | testing + rapid (PBT) |

### 주요 API 엔드포인트 (예상)
| Method | Path | 설명 | 인증 |
|--------|------|------|------|
| POST | /api/table/auth | 테이블 태블릿 인증 | 없음 |
| POST | /api/admin/login | 관리자 로그인 | 없음 |
| GET | /api/menu | 메뉴 목록 조회 | 테이블 토큰 |
| GET | /api/menu/:categoryId | 카테고리별 메뉴 | 테이블 토큰 |
| POST | /api/orders | 주문 생성 | 테이블 토큰 |
| GET | /api/orders | 현재 세션 주문 조회 | 테이블 토큰 |
| GET | /api/admin/orders | 전체 주문 조회 | 관리자 JWT |
| PATCH | /api/admin/orders/:id/status | 주문 상태 변경 | 관리자 JWT |
| DELETE | /api/admin/orders/:id | 주문 삭제 | 관리자 JWT |
| POST | /api/admin/tables/:id/complete | 테이블 이용 완료 | 관리자 JWT |
| GET | /api/admin/tables/:id/history | 과거 주문 내역 | 관리자 JWT |
| GET | /api/sse/customer/:tableId | 고객 SSE 스트림 | 테이블 토큰 |
| GET | /api/sse/admin | 관리자 SSE 스트림 | 관리자 JWT |

---

## Unit 2: Customer App

### 책임
- 태블릿 자동 로그인 및 세션 관리
- 메뉴 조회 및 탐색 UI
- 장바구니 관리 (localStorage)
- 주문 생성 및 확인
- 주문 내역 조회 (현재 세션)
- SSE를 통한 주문 상태 실시간 업데이트

### 기술 스택
| 항목 | 선택 |
|------|------|
| 프레임워크 | Vue.js 3 (Composition API) |
| 언어 | TypeScript |
| 빌드 도구 | Vite |
| 상태 관리 | Pinia |
| 라우터 | Vue Router |
| HTTP 클라이언트 | fetch API (네이티브) |
| 테스트 | Vitest + fast-check (PBT) |
| 스타일 | CSS (또는 Tailwind CSS) |

### 주요 화면
| 화면 | 경로 | 설명 |
|------|------|------|
| 초기 설정 | /setup | 관리자가 태블릿 설정 |
| 메뉴 | / | 카테고리별 메뉴 (기본 화면) |
| 장바구니 | /cart | 장바구니 관리 |
| 주문 확인 | /order/confirm | 주문 최종 확인 |
| 주문 완료 | /order/success | 주문 성공 (5초 후 리다이렉트) |
| 주문 내역 | /orders | 현재 세션 주문 목록 |

---

## Unit 3: Admin App

### 책임
- 관리자 로그인/로그아웃
- 실시간 주문 모니터링 대시보드 (SSE)
- 주문 상태 변경 (대기중/준비중/완료)
- 테이블 관리 (초기 설정, 주문 삭제, 이용 완료)
- 과거 주문 내역 조회

### 기술 스택
| 항목 | 선택 |
|------|------|
| 프레임워크 | Vue.js 3 (Composition API) |
| 언어 | TypeScript |
| 빌드 도구 | Vite |
| 상태 관리 | Pinia |
| 라우터 | Vue Router |
| HTTP 클라이언트 | fetch API (네이티브) |
| 테스트 | Vitest + fast-check (PBT) |
| 스타일 | CSS (또는 Tailwind CSS) |

### 주요 화면
| 화면 | 경로 | 설명 |
|------|------|------|
| 로그인 | /login | 관리자 인증 |
| 대시보드 | / | 테이블별 그리드 (실시간 주문) |
| 테이블 상세 | /tables/:id | 테이블 주문 상세 |
| 과거 내역 | /tables/:id/history | 과거 주문 조회 |
| 테이블 설정 | /tables/setup | 태블릿 초기 설정 |

---

## 유닛 간 통신 방식

```
+------------------+          HTTP (REST)          +------------------+
|  Customer App    | ──────────────────────────────>|   Backend API    |
|  (Unit 2)        |<──────────────────────────────|   (Unit 1)       |
+------------------+          SSE (실시간)          +------------------+
                                                          ^
+------------------+          HTTP (REST)                  |
|  Admin App       | ─────────────────────────────────────+
|  (Unit 3)        |<────────────────────────────────────-+
+------------------+          SSE (실시간)
```

- **Customer App → Backend**: REST API 호출 (메뉴 조회, 주문 생성, 주문 내역)
- **Backend → Customer App**: SSE (주문 상태 변경, 세션 종료 이벤트)
- **Admin App → Backend**: REST API 호출 (로그인, 주문 관리, 테이블 관리)
- **Backend → Admin App**: SSE (신규 주문, 주문 상태 변경)

---

## 개발 전략

**동시 개발 (API 스펙 우선 합의)**:
1. API 스펙(엔드포인트, 요청/응답 형식) 먼저 정의
2. 3개 유닛 병렬 개발
3. Backend는 실제 로직 구현, Frontend는 Mock 데이터로 UI 개발
4. 통합 테스트로 연동 검증

**공유 코드**: 없음 — Customer App과 Admin App은 완전히 독립적으로 개발
