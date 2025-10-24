# Technical Plan: Suno MCP Server

## Tech Stack

### Core Framework
- **Python 3.11+** (modern async/await support)
- **mcp** (official MCP SDK from Anthropic) - NOT FastMCP
- **httpx** (async HTTP client for Suno API)
- **pydantic** 2.0+ (data validation and settings)
- **pydantic-settings** (environment variable management)

### Package Manager
- **uv** for all package operations (per constitution)

### Development Tools
- **pytest** (testing framework)
- **pytest-asyncio** (async test support)
- **pytest-httpx** (HTTP mocking)

## Architecture

### Component Structure
```
suno-mcp/
├── src/
│   └── suno_mcp/
│       ├── __init__.py
│       ├── server.py           # MCP server setup
│       ├── client.py           # Suno API client
│       ├── tools.py            # MCP tool definitions
│       └── models.py           # Pydantic models
├── tests/
│   ├── test_tools.py
│   └── test_client.py
├── pyproject.toml
├── .env.example
└── README.md
```

### Data Flow
```
AI Agent (Claude)
    ↓ MCP Request
MCP Server (server.py)
    ↓ Parse tool call
Tool Handler (tools.py)
    ↓ Validate params (models.py)
Suno API Client (client.py)
    ↓ HTTP Request (httpx)
Suno API (https://api.sunoapi.org)
    ↓ Response
Tool Handler
    ↓ Format response
MCP Server
    ↓ MCP Response
AI Agent
```

### MCP Tools Mapping

| MCP Tool Name | Suno API Endpoint | Purpose |
|---------------|-------------------|---------|
| `generate_music` | POST /api/generate | Create music from text |
| `generate_lyrics` | POST /api/generate_lyrics | Create lyrics only |
| `extend_audio` | POST /api/extend_audio | Continue existing track |
| `get_clip` | GET /api/get | Retrieve generation details |
| `get_credits` | GET /api/get_limit | Check account credits |
| `separate_stems` | POST /api/stem_separation | Extract vocal/instrumental |

## Implementation Details

### Files to Create

**1. `src/suno_mcp/server.py`**
- Initialize MCP server using official `mcp` SDK
- Register all tools
- Handle server lifecycle (startup/shutdown)
- Configure stdio transport

**2. `src/suno_mcp/client.py`**
- Async httpx client for Suno API
- Base URL configuration from env
- Bearer token authentication
- Rate limit handling with exponential backoff
- Error response parsing

**3. `src/suno_mcp/tools.py`**
- Tool definitions with @mcp.tool decorator
- Parameter validation using Pydantic models
- Async tool handlers calling client.py
- Response formatting

**4. `src/suno_mcp/models.py`**
- Pydantic models for all request/response schemas
- Settings model for environment variables
- Type-safe data structures

**5. `tests/test_*.py`**
- Mocked HTTP responses using pytest-httpx
- Tool invocation tests
- Error handling tests

### Files to Modify

**1. `pyproject.toml`**
- Add dependencies: mcp, httpx, pydantic, pydantic-settings
- Add dev dependencies: pytest, pytest-asyncio, pytest-httpx
- Configure project metadata

**2. `README.md`**
- Installation instructions (uv sync)
- Configuration guide (.env setup)
- Usage examples for each tool
- MCP client configuration

**3. `.gitignore`**
- Add .env (already present)

## Dependencies

### Production Dependencies
```toml
dependencies = [
    "mcp>=1.18.0",           # Official MCP SDK
    "httpx>=0.27.0",         # Async HTTP client
    "pydantic>=2.0.0",       # Data validation
    "pydantic-settings>=2.0.0",  # Settings management
]
```

### Development Dependencies
```toml
[tool.uv]
dev-dependencies = [
    "pytest>=8.0.0",
    "pytest-asyncio>=0.23.0",
    "pytest-httpx>=0.30.0",
]
```

## Environment Variables

Required:
- `SUNO_API_KEY` - Suno API authentication token

Optional:
- `SUNO_API_BASE_URL` - Override API base (default: https://api.sunoapi.org)
- `SUNO_TIMEOUT` - Request timeout in seconds (default: 300)

## Error Handling Strategy

### API Errors
- HTTP 401: Invalid API key → Clear error message
- HTTP 429: Rate limit → Exponential backoff (1s, 2s, 4s, 8s)
- HTTP 500: Server error → Retry once, then fail with message
- Network errors: Timeout after configured duration

### Tool Errors
- Invalid parameters → Pydantic validation error with field details
- Missing API key → Configuration error with setup instructions
- Task not found → Clear "Task ID not found" message

## Testing Strategy

### Unit Tests
- Tool parameter validation (Pydantic models)
- Response formatting logic
- Error message generation

### Integration Tests (Mocked)
- Full tool invocation with mocked HTTP responses
- Rate limit handling simulation
- Error response handling

### Manual Integration Test
- Real API call using test credits
- Verify end-to-end flow
- Document in README

## Performance Considerations

- Async I/O throughout (httpx + async tools)
- Connection pooling via httpx client reuse
- Rate limit respecting (prevent 429 errors)
- Timeout configuration for long-running generations

## Security Measures

- API key from environment only (never hardcoded)
- .env in .gitignore
- Error messages sanitized (no key exposure)
- HTTPS only for API calls
- Input validation via Pydantic (prevent injection)
