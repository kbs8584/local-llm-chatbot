# Local LLM Chatbot Engine

## 목적

모다이브(Moit) 백엔드 포지션 포트폴리오용 프로젝트.
Java 11년차 백엔드 엔지니어가 Python/FastAPI로 AI 챗봇 엔진을 설계/구현할 수 있음을 증명.

## 아키텍처

```
Client (WebSocket/REST)
  |
FastAPI (맥북 개발)
  |--- PostgreSQL (NAS)
  |--- Redis (NAS, 기존)
  |--- Ollama API (윈도우 PC, RTX 3070)
```

## 기술 스택

- Python 3.11+, FastAPI (async)
- PostgreSQL 16 + SQLAlchemy 2.0 (async_sessionmaker)
- Redis (세션, 대화 히스토리 캐시)
- Ollama (Gemma 4 / Llama 3.1 8B)
- WebSocket (실시간 스트리밍)
- Docker, docker-compose

## 프로젝트 구조

```
app/
  api/          -- 라우터 (REST + WebSocket 엔드포인트)
  core/         -- 설정, DB, Redis 연결
  models/       -- SQLAlchemy ORM 모델
  schemas/      -- Pydantic DTO
  services/     -- 비즈니스 로직 (LLM 호출, 대화 관리)
  repositories/ -- DB 접근 계층
tests/          -- pytest
alembic/        -- DB 마이그레이션
```

## 패턴 (Spring Boot 대응)

| Spring Boot | FastAPI |
|-------------|---------|
| @RestController | APIRouter |
| @Service | services/ 모듈 |
| @Repository + JPA | repositories/ + SQLAlchemy |
| DTO (Record/Class) | Pydantic BaseModel |
| @Transactional | async with session + yield DI |
| application.yml | .env + pydantic-settings |

## 규칙

- 세션 관리: yield 기반 DI (미들웨어 방식 금지)
- 금액/수치: float 금지, Decimal 사용
- 에러: 명시적 처리, 삼키지 않기
- 테스트: pytest-asyncio, 80%+ 커버리지 목표
- 커밋: conventional commits (feat/fix/refactor/test/docs/chore)

## 외부 서비스 연결

- Ollama: `OLLAMA_BASE_URL` 환경변수
- PostgreSQL: `DATABASE_URL` 환경변수
- Redis: `REDIS_URL` 환경변수
- 실제 접속 정보는 `.claude/settings.local.json` 또는 `.env` 참조 (git 미추적)

## 핵심 기능 (MVP)

1. 캐릭터(페르소나) CRUD
2. REST 기반 대화 API
3. WebSocket 스트리밍 대화
4. Redis 기반 대화 히스토리 + Context Window 관리
5. Ollama 비동기 스트리밍 호출
