# 테이블오더 서비스 - Unit of Work 스토리 매핑

## 매핑 요약

| 유닛 | 스토리 수 | Feature 영역 |
|------|-----------|-------------|
| Unit 1 (Backend API) | 38 (전체) | 모든 Feature의 서버 로직 |
| Unit 2 (Customer App) | 20 | Feature 1~5, 9.2~9.3, 10 |
| Unit 3 (Admin App) | 18 | Feature 1.1, 6~8, 9.1 |

---

## Unit 1: Backend API — 스토리 매핑

Backend는 모든 스토리의 서버 측 로직을 담당합니다.

### Feature 1: 태블릿 자동 로그인
| 스토리 | Backend 역할 |
|--------|-------------|
| US-1.1 | 테이블 인증 API (POST /api/table/auth) |
| US-1.2 | 토큰 발급 로직 |
| US-1.3 | 토큰 검증 API |
| US-1.4 | 만료 토큰 거부 (401) |
| US-1.5 | 인증 실패 응답 (401) |

### Feature 2: 메뉴 조회
| 스토리 | Backend 역할 |
|--------|-------------|
| US-2.1 | 카테고리 목록 API (GET /api/menu) |
| US-2.2 | 카테고리별 메뉴 API (GET /api/menu/:categoryId) |
| US-2.3 | 메뉴 상세 데이터 포함 |
| US-2.4 | (프론트엔드 전용 — Backend 관여 없음) |
| US-2.5 | 서버 에러 시 적절한 에러 응답 |

### Feature 3: 장바구니
| 스토리 | Backend 역할 |
|--------|-------------|
| US-3.1~3.7 | (프론트엔드 전용 — localStorage 관리) |

### Feature 4: 주문 생성
| 스토리 | Backend 역할 |
|--------|-------------|
| US-4.1 | (프론트엔드 전용) |
| US-4.2 | 주문 생성 API (POST /api/orders) — 검증, 저장, 세션 관리 |
| US-4.3 | 주문 번호 생성 및 반환, SSE 이벤트 발행 |
| US-4.4 | 주문 실패 시 에러 응답 |
| US-4.5 | (프론트엔드 전용) |

### Feature 5: 주문 내역 조회
| 스토리 | Backend 역할 |
|--------|-------------|
| US-5.1 | 현재 세션 주문 조회 API (GET /api/orders) |
| US-5.2 | SSE로 주문 상태 변경 이벤트 전달 |
| US-5.3 | 에러 응답 |
| US-5.4 | SSE 연결 관리 (재연결 지원) |

### Feature 6: 관리자 인증
| 스토리 | Backend 역할 |
|--------|-------------|
| US-6.1 | 관리자 로그인 API (POST /api/admin/login) — JWT 발급 |
| US-6.2 | JWT 검증 미들웨어 |
| US-6.3 | 토큰 만료 처리 (16시간) |
| US-6.4 | 로그인 실패 응답, 브루트포스 방지 |

### Feature 7: 실시간 주문 모니터링
| 스토리 | Backend 역할 |
|--------|-------------|
| US-7.1 | 테이블별 주문 현황 API |
| US-7.2 | SSE로 신규 주문 이벤트 전달 (2초 이내) |
| US-7.3 | 테이블별 주문 상세 API |
| US-7.4 | 주문 상태 변경 API (PATCH /api/admin/orders/:id/status) |
| US-7.5 | 주문 상태 변경 + SSE 이벤트 발행 |
| US-7.6 | (프론트엔드 필터링 — Backend 관여 없음) |
| US-7.7 | SSE 연결 관리 |

### Feature 8: 테이블 관리
| 스토리 | Backend 역할 |
|--------|-------------|
| US-8.1 | 테이블 설정 API |
| US-8.2~8.3 | 주문 삭제 API (DELETE /api/admin/orders/:id) |
| US-8.4~8.5 | 이용 완료 API (POST /api/admin/tables/:id/complete) — 세션 종료, OrderHistory 이동 |
| US-8.6~8.7 | 과거 주문 내역 API (GET /api/admin/tables/:id/history) — 날짜 필터 |

### Feature 9: 세션 엣지 케이스
| 스토리 | Backend 역할 |
|--------|-------------|
| US-9.1 | 이용 완료 트랜잭션 처리, 동시 주문 거부 |
| US-9.2 | 동시 주문 순차 처리 (SQLite WAL) |
| US-9.3 | 세션 종료 SSE 이벤트 발행 |

### Feature 10: 메뉴 Seed
| 스토리 | Backend 역할 |
|--------|-------------|
| US-10.1 | DB 초기화 시 Seed 데이터 삽입 (멱등) |

---

## Unit 2: Customer App — 스토리 매핑

| Feature | 스토리 | 화면 |
|---------|--------|------|
| Feature 1 | US-1.1 (설정 UI), US-1.2 (저장), US-1.3 (자동 로그인), US-1.4 (실패 이동), US-1.5 (에러 표시) | /setup |
| Feature 2 | US-2.1 (카테고리), US-2.2 (메뉴 목록), US-2.3 (상세), US-2.4 (이동), US-2.5 (에러) | / |
| Feature 3 | US-3.1~3.7 (장바구니 전체) | /cart |
| Feature 4 | US-4.1 (확인), US-4.2 (전송), US-4.3 (성공), US-4.4 (실패), US-4.5 (빈 장바구니) | /order/* |
| Feature 5 | US-5.1 (목록), US-5.2 (실시간), US-5.3 (에러), US-5.4 (재연결) | /orders |
| Feature 9 | US-9.2 (동시 주문 — 클라이언트 측), US-9.3 (세션 종료 수신) | 전체 |

---

## Unit 3: Admin App — 스토리 매핑

| Feature | 스토리 | 화면 |
|---------|--------|------|
| Feature 1 | US-1.1 (태블릿 설정 — 관리자 수행) | /tables/setup |
| Feature 6 | US-6.1 (로그인), US-6.2 (세션 유지), US-6.3 (자동 로그아웃), US-6.4 (실패) | /login |
| Feature 7 | US-7.1 (대시보드), US-7.2 (실시간), US-7.3 (상세), US-7.4 (상태→준비중), US-7.5 (상태→완료), US-7.6 (필터), US-7.7 (재연결) | / |
| Feature 8 | US-8.1 (설정), US-8.2~8.3 (삭제), US-8.4~8.5 (이용 완료), US-8.6~8.7 (과거 내역) | /tables/* |
| Feature 9 | US-9.1 (이용 완료 중 주문 — 관리자 측) | / |

---

## 크로스-유닛 스토리

아래 스토리는 여러 유닛에 걸쳐 구현이 필요합니다:

| 스토리 | 관련 유닛 | 설명 |
|--------|-----------|------|
| US-1.1 | Unit 1 + Unit 2 | Backend 인증 API + Customer 설정 UI |
| US-4.2~4.3 | Unit 1 + Unit 2 + Unit 3 | 주문 생성 → Backend 저장 → SSE로 Admin에 전달 |
| US-7.4~7.5 | Unit 1 + Unit 2 + Unit 3 | Admin 상태 변경 → Backend 저장 → SSE로 Customer에 전달 |
| US-8.4~8.5 | Unit 1 + Unit 2 + Unit 3 | Admin 이용 완료 → Backend 처리 → SSE로 Customer 초기화 |
| US-9.1 | Unit 1 + Unit 2 + Unit 3 | 동시성 처리 (Backend 트랜잭션 + 양쪽 UI 반영) |
| US-9.3 | Unit 1 + Unit 2 | Backend 세션 종료 이벤트 → Customer 화면 초기화 |

---

## 검증 결과

- ✅ 모든 38개 스토리가 최소 1개 유닛에 할당됨
- ✅ 크로스-유닛 스토리 6개 식별 (통합 테스트 대상)
- ✅ 각 유닛이 독립적으로 개발 가능 (Mock API 활용)
- ✅ 의존성 방향 단방향 (Frontend → Backend)
