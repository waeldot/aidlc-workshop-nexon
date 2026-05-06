# 테이블오더 서비스 - Unit of Work 의존성 매트릭스

## 의존성 관계

```
Unit 2 (Customer App) ───depends on───> Unit 1 (Backend API)
Unit 3 (Admin App)    ───depends on───> Unit 1 (Backend API)
Unit 2 (Customer App) ───independent───> Unit 3 (Admin App)
```

## 의존성 매트릭스

| 유닛 | Unit 1 (Backend) | Unit 2 (Customer) | Unit 3 (Admin) |
|------|:-:|:-:|:-:|
| **Unit 1 (Backend)** | - | 없음 | 없음 |
| **Unit 2 (Customer)** | **의존** | - | 없음 |
| **Unit 3 (Admin)** | **의존** | 없음 | - |

### 의존성 설명

| 관계 | 유형 | 설명 |
|------|------|------|
| Unit 2 → Unit 1 | Runtime | Customer App은 Backend API에 HTTP/SSE 요청 |
| Unit 3 → Unit 1 | Runtime | Admin App은 Backend API에 HTTP/SSE 요청 |
| Unit 2 ↔ Unit 3 | 없음 | 두 프론트엔드 앱은 서로 독립적 |

---

## 공유 리소스

| 리소스 | 소유 유닛 | 사용 유닛 | 설명 |
|--------|-----------|-----------|------|
| SQLite DB | Unit 1 | Unit 1 only | DB는 Backend만 직접 접근 |
| API 스펙 | Unit 1 | Unit 2, 3 | API 엔드포인트 계약 |
| SSE 이벤트 스펙 | Unit 1 | Unit 2, 3 | SSE 이벤트 타입 및 페이로드 |
| Docker Network | 공유 | 전체 | docker-compose 네트워크 |

---

## 개발 순서 (동시 개발 전략)

### Phase 0: API 스펙 합의 (선행 조건)
- REST API 엔드포인트 정의 (요청/응답 스키마)
- SSE 이벤트 타입 및 페이로드 정의
- 인증 토큰 형식 정의

### Phase 1: 병렬 개발
| 유닛 | 작업 | 비고 |
|------|------|------|
| Unit 1 (Backend) | API 구현, DB 스키마, 비즈니스 로직 | 실제 로직 |
| Unit 2 (Customer) | UI 구현, 상태 관리, Mock API | Mock 데이터 사용 |
| Unit 3 (Admin) | UI 구현, 상태 관리, Mock API | Mock 데이터 사용 |

### Phase 2: 통합
- Frontend Mock → 실제 Backend API 연결
- E2E 테스트
- Docker Compose로 전체 서비스 실행 검증

---

## 빌드 및 배포 의존성

```
docker-compose.yml
├── backend (port 8080)
│   └── SQLite DB (volume mount)
├── customer (port 3000)
│   └── Nginx serving built Vue app
└── admin (port 3001)
    └── Nginx serving built Vue app
```

| 유닛 | 빌드 도구 | 빌드 산출물 | 배포 방식 |
|------|-----------|-------------|-----------|
| Unit 1 | `go build` | 단일 바이너리 | Docker (Go binary) |
| Unit 2 | `npm run build` (Vite) | 정적 파일 (dist/) | Docker (Nginx) |
| Unit 3 | `npm run build` (Vite) | 정적 파일 (dist/) | Docker (Nginx) |

---

## 리스크 및 고려사항

| 리스크 | 영향 | 완화 방안 |
|--------|------|-----------|
| API 스펙 변경 | Unit 2, 3 수정 필요 | 스펙 먼저 확정, 변경 시 즉시 공유 |
| SSE 연결 관리 | 동시 연결 수 제한 | 최대 10 테이블 + 1 관리자 (소규모) |
| SQLite 동시 쓰기 | 쓰기 잠금 | WAL 모드 활성화, 소규모 트래픽 |
| CORS 설정 | 프론트엔드 접근 차단 | Backend에서 허용 오리진 명시 |
