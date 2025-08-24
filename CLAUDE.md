# CLAUDE.md

This file provides guidance to Claude Code (claude.ai/code) when working with code in this repository.

## Essential Commands

### Development
- **Start server**: `./run.sh` or `cd backend && uv run uvicorn app:app --reload --port 8000`
- **Install dependencies**: `uv sync`
- **Python environment**: Uses `uv` package manager with Python 3.13+

### Environment Setup
- Create `.env` file with `ANTHROPIC_API_KEY=your_key_here`
- Application runs on `http://localhost:8000` (web UI and API)
- API docs available at `http://localhost:8000/docs`

## Architecture Overview

This is a **Retrieval-Augmented Generation (RAG) system** for course materials with the following key components:

### Backend Structure (`backend/`)
- **`app.py`**: FastAPI application with CORS, serving both API endpoints and static frontend
- **`rag_system.py`**: Main orchestrator that coordinates all components
- **`config.py`**: Configuration management using environment variables and dataclasses
- **`vector_store.py`**: ChromaDB integration for vector storage and semantic search
- **`ai_generator.py`**: Anthropic Claude API integration for response generation
- **`document_processor.py`**: Text chunking and course document parsing
- **`session_manager.py`**: Conversation history management
- **`search_tools.py`**: Tool-based search functionality for AI function calling
- **`models.py`**: Pydantic models for Course, Lesson, and CourseChunk

### Frontend Structure (`frontend/`)
- **`index.html`**: Single-page web interface
- **`script.js`**: Frontend JavaScript for API communication
- **`style.css`**: UI styling

### Data Flow
1. Course documents (PDF/DOCX/TXT) are processed into chunks via `DocumentProcessor`
2. Chunks are embedded and stored in ChromaDB via `VectorStore`
3. User queries trigger semantic search through `CourseSearchTool`
4. Claude AI generates responses using retrieved context via `AIGenerator`
5. Conversation history is maintained per session via `SessionManager`

### Key Design Patterns
- **Tool-based AI**: Uses function calling with `CourseSearchTool` for contextual retrieval
- **Session Management**: Maintains conversation context with configurable history limits
- **Modular Components**: Each major functionality is separated into dedicated classes
- **Vector Storage**: ChromaDB with sentence-transformers for embeddings
- **Configuration**: Centralized config with environment variable support

### API Endpoints
- **POST `/api/query`**: Main query endpoint for RAG responses
- **GET `/api/courses`**: Course analytics and statistics
- **Static files**: Frontend served from root path

### Document Processing
- Supports PDF, DOCX, TXT formats in `docs/` folder
- Configurable chunk size (800 chars) and overlap (100 chars)
- Automatic course title extraction and deduplication
- ChromaDB storage path: `./chroma_db`

### Dependencies
- **FastAPI**: Web framework and API
- **ChromaDB**: Vector database
- **Anthropic**: Claude AI integration
- **sentence-transformers**: Text embeddings
- **uvicorn**: ASGI server