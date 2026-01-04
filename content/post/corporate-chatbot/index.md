---
title: Corporate Chatbot
description: A corporate chatbot using LangGraph and Graph-based Retrieval Augmented Generation. This chatbot can answer questions about the company's policies, procedures, and other relevant information.
slug: corporate-chatbot
date: 2025-08-31 00:00:00+0000
image: cover.jpg
categories:
    - Company Projects
tags:
    - AI
    - LLM
    - RAG
weight: 1       # You can add weight to some posts to override the default sorting (date descending)
---

**Github Repository:** Private

# Sun Assistant

<div align="center">

![Version](https://img.shields.io/badge/version-4.1.0-blue.svg)
![Python](https://img.shields.io/badge/python-3.11+-green.svg)
![License](https://img.shields.io/badge/license-MIT-yellow.svg)

**Always ready, always listening!**

An enterprise-grade AI assistant platform with intelligent document management, conversational AI, and multi-source knowledge retrieval.

[Features](#-features) • [Architecture](#-architecture) • [Quick Start](#-quick-start) • [Documentation](#-documentation)

</div>

---

## Table of Contents

- [Overview](#-overview)
- [Features](#-features)
- [Architecture](#-architecture)
- [Services](#-services)
- [Libraries](#-libraries)
- [Quick Start](#-quick-start)
- [Development](#-development)
- [Deployment](#-deployment)
- [Testing](#-testing)
- [Database Migrations](#-database-migrations)

---

## Overview

**Sun Assistant** is a comprehensive AI-powered assistant platform designed for enterprise environments. It combines advanced natural language processing, intelligent document management, and multi-modal data retrieval to provide accurate, context-aware responses to user queries.

### Key Capabilities

- **Intelligent Chatbot** - AI-powered question answering with LangGraph state management
- **Document Management** - Automated document processing and indexing
- **Multi-Source Search** - OpenSearch, PostgreSQL, and Neo4j integration
- **Slack Integration** - Seamless workplace communication
- **Graph Knowledge Base** - Neo4j-powered relationship mapping
- **Agentic Architecture** - Autonomous task planning with MCP (Model Context Protocol)
- **Multi-Language Support** - Vietnamese, Japanese, and English

---

## Features

### Conversational AI
- Multi-turn conversation with context awareness
- Intelligent question decomposition
- Dynamic task planning and orchestration
- Human-in-the-loop intervention for complex queries
- Checkpoint-based state persistence

### Document Intelligence
- Automated document ingestion and processing
- Azure Document Intelligence integration
- Semantic embedding generation
- Multi-format support (PDF, DOCX, images)
- Vector and graph-based indexing

### Knowledge Retrieval
- Semantic search with OpenSearch
- Graph-based relationship traversal with Neo4j
- Hybrid search combining vector and keyword matching
- Context filtering and ranking
- Answer aggregation from multiple sources

### Integration & Deployment
- Docker-based microservices architecture
- RESTful APIs with FastAPI
- Prometheus metrics and health checks
- Slack bot integration
- Horizontal scalability

---

## Architecture

Sun Assistant follows a **microservices architecture** with clear separation of concerns:

```
┌─────────────────────────────────────────────────────────────┐
│                        User Interface                        │
│                      (Slack / REST API)                      │
└────────────────────┬────────────────────────────────────────┘
                     │
     ┌───────────────┼───────────────┐
     │               │               │
┌────▼────┐    ┌────▼────┐    ┌────▼────────┐
│  Slack  │    │ Chatbot │    │    Doc      │
│ Service │    │ Service │    │  Manager    │
└────┬────┘    └────┬────┘    └────┬────────┘
     │              │              │
     └──────────────┼──────────────┘
                    │
     ┌──────────────┼──────────────┐
     │              │              │
┌────▼─────┐  ┌────▼────┐  ┌─────▼─────┐
│OpenSearch│  │  Neo4j  │  │PostgreSQL │
│  (Vector)│  │ (Graph) │  │  (Relat.) │
└──────────┘  └─────────┘  └───────────┘
```

### Technology Stack

- **Backend Framework**: Python 3.11+, FastAPI, LangGraph
- **AI/ML**: LiteLLM (multi-provider LLM), Azure OpenAI, Claude
- **Search Engines**: OpenSearch (vector search), Azure AI Search
- **Databases**: PostgreSQL 17, Neo4j 5.22 (graph)
- **Message Queue**: Slack SDK
- **Infrastructure**: Docker, Docker Compose
- **Package Manager**: UV (ultra-fast Python package manager)

---

## Services

### 1. **Chatbot Service**
AI-powered conversational interface with intelligent question answering.

**Key Features:**
- Question processing and sub-question generation
- Context retrieval from multiple sources
- Answer generation using multiple LLM providers
- Conversation management and history
- Prometheus metrics

**Endpoints:**
- `GET /v1/healthz` - Health check
- `POST /v1/chat` - Chat interface
- `GET /v1/metrics` - Prometheus metrics

[Learn more →](services/chatbot/CHATBOT_SERVICE_ANALYSIS.md)

### 2. **Document Manager Service**
Handles document upload, processing, and indexing.

**Features:**
- Document upload and validation
- Azure Document Intelligence processing
- Embedding generation and storage
- Multi-index management
- Document lifecycle management

**Operations:**
- Add/Update/Delete documents
- Get document status
- Batch processing

### 3. **Slack Service**
Seamless integration with Slack workspace.

**Features:**
- Direct message handling
- Channel monitoring
- Feedback collection (1-5 star ratings)
- Health monitoring
- Error handling and maintenance mode

**Health Checks:**
- Neo4j connectivity
- RAG service availability
- Bot responsiveness

### 4. **MCP Server**
Model Context Protocol server for advanced task orchestration.

**Capabilities:**
- Dynamic tool discovery
- Task planning and execution
- Context management
- Multi-step reasoning

### 5. **Text2GraphQL Service**
Natural language to GraphQL query translation for Neo4j queries.

---

## Libraries

The project includes reusable libraries in the `libs/` directory:

| Library | Purpose |
|---------|---------|
| `aws_claude` | AWS Bedrock Claude integration |
| `azure_ai_search` | Azure AI Search wrapper |
| `azure_document_intelligent` | Azure Document Intelligence client |
| `base` | Common utilities and base classes |
| `litellm` | Multi-provider LLM abstraction |
| `logger` | Centralized logging |
| `neograph` | Neo4j graph database client |
| `open_search` | OpenSearch integration |
| `pg` | PostgreSQL database client |
| `slack_client` | Slack SDK wrapper |
| `storage` | Cloud storage abstraction |

---

## Quick Start

### Prerequisites

- Docker & Docker Compose
- Python 3.11+
- UV package manager
- Git

### 1. Clone the Repository

```bash
git clone https://github.com/framgia/sun-assistant.git
cd sun-assistant
```

### 2. Environment Configuration

Create a `.env` file with required configuration:

```bash
# Azure Search
AZURE_SEARCH__INDEX_NAME=documents_production

# PostgreSQL
POSTGRES__USERNAME=your_username
POSTGRES__PASSWORD=your_password
POSTGRES__DB=sun_assistant
POSTGRES__HOST=postgres

# Neo4j
NEO4J__HOST=neo4j
NEO4J__PORT=7687
NEO4J__USERNAME=neo4j
NEO4J__PASSWORD=your_neo4j_password

# Slack
SLACK__SIGNING_SECRET=your_signing_secret
SLACK__BOT_TOKEN=xoxb-your-bot-token
SLACK__APP_TOKEN=xapp-your-app-token
SLACK__BOT_ID=your_bot_id

# LLM Services
LITELLM_SERVICE_URL=http://litellm:4000
CHATBOT_SERVICE_URL=http://chatbot_service:8000
```

### 3. Build and Start Services

```bash
bash ./scripts/get_started.sh
```

Or manually with Docker Compose:

```bash
docker-compose up -d
```

### 4. Verify Installation

Check service health:

```bash
# Chatbot service
curl http://localhost:8000/v1/healthz

# Neo4j browser
open http://localhost:17474

# PostgreSQL
psql -h localhost -p 15432 -U your_username -d sun_assistant
```

---

## Development

### Set Up Development Environment

1. **Install UV Package Manager**

```bash
curl -LsSf https://astral.sh/uv/install.sh | sh
```

2. **Install Dependencies**

```bash
uv sync
```

3. **Set Up Pre-commit Hooks**

```bash
# Install pre-commit
pip install pre-commit

# Add to git hooks
pre-commit install

# Run on all files
pre-commit run --all-files
```

### Local Development Workflow

```bash
# Work on a specific service
cd services/chatbot
uv sync
uv run chatbot

# Run with auto-reload
uv run uvicorn src.chatbot.main:app --reload
```

### Code Style

This project uses:
- **Black** for code formatting
- **isort** for import sorting
- **flake8** for linting
- **mypy** for type checking

Pre-commit hooks automatically format code before commits.

---

## Testing

### Unit Tests

Test files must be in the `tests/` directory of each service.

```bash
# Run all tests in a service
cd services/chatbot
python -m unittest discover

# Run specific test file
python -m unittest tests.test_module_name

# Run with pytest
uv run pytest
```

### Test Conventions

1. Test files start with `test_` (e.g., `test_chatbot.py`)
2. Tests organized by feature/module
3. Use descriptive test names
4. Include docstrings for complex tests

### Integration Tests

```bash
# Run integration tests with Docker
docker-compose run --rm chatbot_service uv run pytest tests/integration/
```

---

## Deployment

### Docker Deployment

```bash
# Production build
docker-compose -f docker-compose.yml up -d

# Check logs
docker-compose logs -f chatbot_service

# Scale services
docker-compose up -d --scale chatbot_service=3
```

### Deployment Scripts

```bash
# Deploy to staging
bash ./scripts/deploy/staging.sh

# Deploy to production
bash ./scripts/deploy/host_prod.sh
```

### Health Monitoring

All services expose health check endpoints:

- Chatbot: `http://localhost:8000/v1/healthz`
- Slack: Health check via tracker.py (every 6h)
- Document Manager: `http://localhost:8001/healthz`

---

## Database Migrations

### Neo4j Migrations

1. **Installation**

```bash
curl -LO https://github.com/michael-simons/neo4j-migrations/releases/download/2.0.3/neo4j-migrations-2.0.3-linux-x86_64.zip
unzip neo4j-migrations-2.0.3-linux-x86_64.zip
echo "NEO4J__MIGRATION_PATH=/path/to/neo4j-migrations-2.0.3-linux-x86_64/bin" >> .env
```

2. **Usage**

```bash
# Check migration status
python migrations.py info

# Apply pending migrations
python migrations.py migrate

# Run specific migration
python migrations.py run V001__migration_name.cypher

# Clean migrations history
python migrations.py clean false
```

3. **Migration File Convention**

```
migrations/V<version>__<description>.cypher

Examples:
- V000__Add_Communities_Label.cypher
- V1_0_2__AddChunkLabel.cypher
```

### Data Migration

```bash
# Stop Neo4j service
docker-compose stop neo4j

# Dump data
bash ./scripts/utils/dump_data.sh

# Restore data
bash ./scripts/utils/restore_data.sh
```

---

<div align="center">

**Built by Sun Asterisk**

[Back to Top](#sun-assistant)

</div>