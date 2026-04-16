# Local LLM Chatbot Engine

Real-time AI chatbot engine built with FastAPI, WebSocket, and Ollama.
Supports character-based conversations with streaming responses using local LLM models.

## Architecture

```
Client (Browser / WebSocket)
    |
    v
[FastAPI Backend]
    |--- [PostgreSQL] : Conversations, Characters, Messages
    |--- [Redis]      : Session cache, Chat history (Context Window)
    |--- [Ollama API] : Local LLM (Gemma 4 / Llama 3.1)
```

## Tech Stack

- **Backend**: Python 3.11+, FastAPI (async)
- **Database**: PostgreSQL 16, SQLAlchemy 2.0 (async)
- **Cache**: Redis 7
- **LLM**: Ollama (local inference)
- **Real-time**: WebSocket streaming
- **Container**: Docker, docker-compose

## Quick Start

```bash
# 1. Clone
git clone https://github.com/kbs8584/local-llm-chatbot.git
cd local-llm-chatbot

# 2. Environment
cp .env.example .env
# Edit .env with your Ollama/DB/Redis settings

# 3. Run with Docker
docker-compose up -d

# 4. Or run locally
python -m venv .venv
source .venv/bin/activate
pip install -r requirements.txt
uvicorn app.main:app --reload
```

## API Endpoints

| Method | Path | Description |
|--------|------|-------------|
| GET | /api/health | Health check |
| POST | /api/characters | Create character persona |
| GET | /api/characters | List characters |
| POST | /api/chat | Send message (REST) |
| WS | /api/chat/ws | Stream chat (WebSocket) |

## Features

- Character persona management (bio, scenario, first message)
- REST and WebSocket dual communication
- Token-by-token streaming responses
- Redis-based context window management (LRU)
- Async database operations with connection pooling

## License

MIT
