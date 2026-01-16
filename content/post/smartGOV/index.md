---
title: "SmartGOV - Chatbot for Administrative Procedures"
description: "SmartGOV is an intelligent chatbot system designed to assist citizens in administrative procedures using advanced AI technologies, Knowledge Graph, and Retrieval-Augmented Generation (RAG)."
slug: smartgov
date: 2025-08-17 00:00:00+0000
image: cover.jpg
categories:
    - Codecamp Projects
tags:
    - AI
    - LLM
    - RAG
    - Graph
    - Multi-Agent System
weight: 1       # You can add weight to some posts to override the default sorting (date descending)
---

**Github Repository:** [SmartGOV](https://github.com/vuniem131104/UET-Codecamp-2025.git)

# SmartGov - Hệ thống Chatbot hỗ trợ Thủ tục Hành chính

[![Python](https://img.shields.io/badge/Python-3.13-blue.svg)](https://python.org)
[![React](https://img.shields.io/badge/React-18.2.0-blue.svg)](https://reactjs.org)
[![TypeScript](https://img.shields.io/badge/TypeScript-5.3.3-blue.svg)](https://typescriptlang.org)
[![FastAPI](https://img.shields.io/badge/FastAPI-Latest-green.svg)](https://fastapi.tiangolo.com)
[![Neo4j](https://img.shields.io/badge/Neo4j-5.22.0-orange.svg)](https://neo4j.com)

SmartGov là một hệ thống chatbot thông minh được thiết kế để hỗ trợ công dân trong việc thực hiện các thủ tục hành chính. Hệ thống sử dụng công nghệ AI hiện đại, Knowledge Graph và Retrieval-Augmented Generation (RAG) để cung cấp thông tin chính xác và hữu ích về các dịch vụ công.

![SmartGov Architecture](architecture.png)

![SmartGov Assistant](chatbot.png)

## Tính năng chính

### Chatbot thông minh
- **Multi-Agent Architecture**: Hệ thống agent đa tầng với routing thông minh
- **Profile Agent**: Tự động điền thông tin cá nhân vào biểu mẫu
- **GeoAdmin Agent**: Trả lời các câu hỏi về địa danh và đơn vị hành chính
- **RAG Agent**: Tìm kiếm và trả lời dựa trên knowledge base
- **Speech-to-Text**: Hỗ trợ nhập liệu bằng giọng nói

### Xử lý tài liệu
- **Document Parsing**: Xử lý file DOCX, PDF và chuyển đổi sang văn bản
- **Intelligent Chunking**: Chia nhỏ tài liệu thành các đoạn có ý nghĩa
- **Knowledge Graph Building**: Tạo đồ thị tri thức từ nội dung tài liệu
- **Embedding**: Vector hóa nội dung để tìm kiếm ngữ nghĩa

### Giao diện người dùng
- **Modern React UI**: Giao diện đẹp mắt với Tailwind CSS
- **Real-time Chat**: Trò chuyện thời gian thực với hiệu ứng typing
- **Document Management**: Quản lý và đánh chỉ mục tài liệu
- **Responsive Design**: Tương thích mọi thiết bị

## Kiến trúc hệ thống

### Microservices Architecture

```
┌─────────────────┐    ┌─────────────────┐    ┌─────────────────┐
│   Frontend      │    │   Chatbot       │    │   Indexing      │
│   (React)       │◄──►│   Service       │◄──►│   Service       │
│   Port: 5173    │    │   Port: 3010    │    │   Port: 3006    │
└─────────────────┘    └─────────────────┘    └─────────────────┘
         │                       │                       │
         │                       │                       │
         ▼                       ▼                       ▼
┌─────────────────┐    ┌─────────────────┐    ┌─────────────────┐
│   RAG Service   │    │   STT Service   │    │   LiteLL M      │
│   Port: 8001    │    │   Port: 8000    │    │   Port: 9510    │
└─────────────────┘    └─────────────────┘    └─────────────────┘
         │                       │                       │
         └───────────────────────┼───────────────────────┘
                                 │
         ┌───────────────────────▼───────────────────────┐
         │              Data Layer                       │
         │                                               │
         │  ┌──────────┐ ┌──────────┐ ┌──────────┐      │
         │  │PostgreSQL│ │  Neo4j   │ │  MinIO   │      │
         │  │Port:15432│ │Port:17474│ │Port: 9000│      │
         │  └──────────┘ └──────────┘ └──────────┘      │
         └───────────────────────────────────────────────┘
```

### Services Overview

#### Core Services

1. **Chatbot Service** (`services/chatbot/`)
   - Orchestrates conversation flow
   - Implements multi-agent routing
   - Handles user interactions and responses

2. **Indexing Service** (`services/indexing/`)
   - Processes and indexes documents
   - Builds knowledge graphs from content
   - Manages document embeddings

3. **RAG Service** (`services/rag/`)
   - Implements Retrieval-Augmented Generation
   - Semantic search in knowledge base
   - Context-aware response generation

4. **STT Service** (`services/stt/`)
   - Speech-to-Text conversion
   - Voice input processing
   - Audio transcription

#### Library Modules

1. **Base** (`libs/base/`)
   - Common base classes and utilities
   - Shared Pydantic models
   - Application foundations

2. **LiteLLM** (`libs/lite_llm/`)
   - LLM integration wrapper
   - Multiple AI model support (Gemini, OpenAI, etc.)
   - Unified API interface

3. **Graph DB** (`libs/graph_db/`)
   - Neo4j database interface
   - Knowledge graph operations
   - Cypher query abstractions

4. **Storage** (`libs/storage/`)
   - MinIO object storage integration
   - File upload/download management
   - Binary data handling

5. **Logger** (`libs/logger/`)
   - Structured logging system
   - Centralized log management
   - Debug and monitoring support

6. **PG** (`libs/pg/`)
   - PostgreSQL database interface
   - SQL operations and migrations
   - Data persistence layer

## Cài đặt và triển khai

### Yêu cầu hệ thống

- **Python**: >= 3.13
- **Node.js**: >= 18.0.0
- **Docker & Docker Compose**: Latest version
- **Git**: Latest version

### 1. Clone Repository

```bash
git clone https://github.com/vuniem131104/UET-Codecamp-2025.git
cd UET-Codecamp-2025
```

### 2. Cấu hình Environment Variables

Tạo file `.env` từ template:

```bash
cp .env.example .env
```

Cấu hình các biến môi trường:

```env
# Database Configuration
POSTGRES__USER=admin
POSTGRES__PASSWORD=secure_password
POSTGRES__DB=smartgov_db
POSTGRES__HOST=localhost
POSTGRES__PORT=15432

# Neo4j Configuration
NEO4J__USERNAME=neo4j
NEO4J__PASSWORD=secure_neo4j_password

# MinIO Configuration
MINIO__ACCESS_KEY=minioadmin
MINIO__SECRET_KEY=minioadmin123

# AI API Keys
GEMINI_API_KEY1=your_gemini_api_key_1
GEMINI_API_KEY2=your_gemini_api_key_2
# ... add more API keys as needed
```

### 3. Khởi động Infrastructure

```bash
# Khởi động databases và dependencies
docker-compose up -d postgres neo4j minio redis litellm_service

# Kiểm tra trạng thái services
docker-compose ps
```

### 4. Cài đặt Python Dependencies

```bash
# Sử dụng uv để quản lý dependencies
pip install uv

# Cài đặt workspace dependencies
uv sync
```

### 5. Khởi động Backend Services

```bash
# Terminal 1: Chatbot Service
cd services/chatbot
uv run python -m chatbot

# Terminal 2: Indexing Service  
cd services/indexing
uv run python -m indexing

# Terminal 3: RAG Service
cd services/rag
uv run python -m rag

# Terminal 4: STT Service
cd services/stt
uv run python -m stt
```

### 6. Khởi động Frontend

```bash
cd frontend
npm install
npm run dev
```

## Hướng dẫn sử dụng

### 1. Truy cập ứng dụng

Mở trình duyệt và truy cập: `http://localhost:5173`

### 2. Upload và đánh chỉ mục tài liệu

1. Click vào tab "Indexing" trong sidebar
2. Upload file DOCX chứa thông tin thủ tục hành chính
3. Click "Index" để xử lý tài liệu
4. Chờ quá trình đánh chỉ mục hoàn thành

### 3. Sử dụng Chatbot

1. Quay lại tab "Chatbot"
2. Nhập câu hỏi về thủ tục hành chính
3. Hoặc sử dụng microphone để nói
4. Nhận câu trả lời từ AI agent

### 4. Các loại câu hỏi được hỗ trợ

#### Câu hỏi chung về thủ tục
```
"Thủ tục cấp căn cước công dân cần những giấy tờ gì?"
"Thời gian xử lý hồ sơ đăng ký xe máy là bao lâu?"
```

#### Câu hỏi về địa danh, cơ quan
```
"Ủy ban nhân dân phường Cầu Giấy ở đâu?"
"Thành phố Hà Nội có bao nhiều quận?"
```

#### Yêu cầu điền form tự động
```
"Giúp tôi điền đơn xin cấp căn cước công dân"
"Tạo đơn đăng ký tạm trú cho tôi"
```

## Phát triển

### Cấu trúc dự án

```
UET-CodeCamp-2025/
├── services/              # Microservices
│   ├── chatbot/          # Main chatbot logic
│   ├── indexing/         # Document processing
│   ├── rag/              # RAG implementation  
│   └── stt/              # Speech-to-text
├── libs/                 # Shared libraries
│   ├── base/             # Base classes
│   ├── lite_llm/         # LLM integration
│   ├── graph_db/         # Neo4j interface
│   ├── storage/          # MinIO interface
│   ├── logger/           # Logging system
│   └── pg/               # PostgreSQL interface
├── frontend/             # React frontend
├── docker/               # Docker configurations
├── data/                 # Database data
├── logs/                 # Application logs
└── config/               # Configuration files
```

### Thêm tính năng mới

#### 1. Tạo Agent mới

```python
# services/chatbot/src/chatbot/domain/new_agent/service.py
from base import BaseService
from chatbot.shared.state.chatbot_state import ChatbotState

class NewAgentService(BaseService):
    def process(self, state: ChatbotState) -> dict:
        # Implement your agent logic here
        return {
            "answer": "Response from new agent",
            "references": []
        }
```

#### 2. Đăng ký Agent trong Router

```python
# services/chatbot/src/chatbot/application/chatbot.py
def route_agent(self, state: ChatbotState) -> Literal["new_agent", ...]:
    # Add routing logic for new agent
    if "new_feature" in state.get('raw_question', '').lower():
        return "new_agent"
    # ... existing routing logic
```

#### 3. Thêm vào Graph

```python
# services/chatbot/src/chatbot/application/chatbot.py
@property
def nodes(self) -> dict[str, Any]:
    return {
        # ... existing nodes
        "new_agent": self.new_agent.process,
    }
```

### Testing

```bash
# Run unit tests
uv run pytest

# Run integration tests
uv run pytest tests/integration/

# Test specific service
cd services/chatbot
uv run pytest tests/
```

### Code Quality

```bash
# Format code
uv run black .
uv run isort .

# Lint code
uv run flake8
uv run mypy .
```

## Monitoring và Logs

### Log Locations

- **Application Logs**: `logs/`
- **Neo4j Logs**: `logs/neo4j/`
- **Container Logs**: `docker-compose logs [service]`

### Health Checks

```bash
# Check service health
curl http://localhost:3010/health  # Chatbot
curl http://localhost:3006/health  # Indexing
curl http://localhost:8001/health  # RAG
curl http://localhost:8000/health  # STT
```

### Database Access

```bash
# PostgreSQL
psql -h localhost -p 15432 -U admin -d smartgov_db

# Neo4j Browser
open http://localhost:17474

# MinIO Console
open http://localhost:9001
```

## Troubleshooting

### Các lỗi thường gặp

#### 1. Service không khởi động được
```bash
# Kiểm tra logs
docker-compose logs [service-name]

# Restart service
docker-compose restart [service-name]
```

#### 2. Database connection failed
```bash
# Kiểm tra database status
docker-compose ps

# Reset database
docker-compose down
docker-compose up -d postgres neo4j
```

#### 3. Frontend build errors
```bash
# Clear cache và reinstall
cd frontend
rm -rf node_modules package-lock.json
npm install
npm run dev
```

#### 4. Python import errors
```bash
# Reinstall dependencies
uv sync --force

# Check Python path
uv run python -c "import sys; print(sys.path)"
```

## Đóng góp

### Quy trình đóng góp

1. Fork repository
2. Tạo feature branch: `git checkout -b feature/amazing-feature`
3. Commit changes: `git commit -m 'Add amazing feature'`
4. Push to branch: `git push origin feature/amazing-feature`
5. Tạo Pull Request

### Coding Standards

- **Python**: Tuân thủ PEP 8, sử dụng type hints
- **TypeScript**: Tuân thủ ESLint config
- **Documentation**: Viết docstring cho functions/classes
- **Testing**: Đảm bảo coverage >= 80%

### Commit Convention

```
type(scope): subject

body

footer
```

Ví dụ:
```
feat(chatbot): add new profile agent

- Implement automatic form filling
- Add user profile detection
- Update routing logic

Closes #123
```

## License

Dự án này được phát hành dưới MIT License. Xem file [LICENSE](LICENSE) để biết thêm chi tiết.

## Tác giả

- **Lê Hoàng Vũ** - *Lead Developer* - [@vulh-1357](https://github.com/vulh-1357)
- **UET-VNU Team** - *Contributors*

## Acknowledgments

- Đại học Công nghệ - ĐHQGHN (UET-VNU)
- Google Gemini AI Platform
- Neo4j Graph Database
- FastAPI Framework
- React & TypeScript Community

## Liên hệ

- **Email**: le.hoang.vu@sun-asterisk.com
- **GitHub**: https://github.com/vulh-1357/UET-Codecamp-2025
- **Website**: https://uet.vnu.edu.vn

---

**SmartGov** - Democratizing access to government services through AI