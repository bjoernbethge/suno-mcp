# Tasks: Suno MCP Server

## Phase 1: Project Setup

- [ ] Update pyproject.toml with MCP SDK dependencies (mcp, httpx, pydantic, pydantic-settings)
- [ ] Add dev dependencies (pytest, pytest-asyncio, pytest-httpx)
- [ ] Run `uv sync` to install dependencies
- [ ] Create src/suno_mcp/ directory structure
- [ ] Create tests/ directory
- [ ] Verify .env in .gitignore
- [ ] Update .env.example with SUNO_API_KEY placeholder

## Phase 2: Core Infrastructure

- [ ] Create src/suno_mcp/models.py with Pydantic models:
  - Settings model (API key, base URL from env)
  - GenerateMusicRequest/Response
  - GenerateLyricsRequest/Response
  - ExtendAudioRequest/Response
  - GetClipResponse
  - GetCreditsResponse
  - SeparateStemsRequest/Response
- [ ] Create src/suno_mcp/client.py with async Suno API client:
  - Initialize httpx.AsyncClient with base URL and auth header
  - Implement rate limit handling with exponential backoff
  - Add timeout configuration
  - Error response parsing and formatting
- [ ] Create tests/test_client.py with mocked HTTP tests

## Phase 3: MCP Server Setup

- [ ] Create src/suno_mcp/server.py:
  - Import official mcp SDK
  - Initialize MCP server instance
  - Configure stdio transport
  - Add server lifecycle handlers (startup/shutdown)
- [ ] Create src/suno_mcp/__init__.py with version
- [ ] Create main entry point for running server

## Phase 4: MCP Tools Implementation

- [ ] Create src/suno_mcp/tools.py
- [ ] Implement `generate_music` tool:
  - @mcp.tool decorator with description
  - Parameters: prompt, instrumental (bool), custom_lyrics (optional)
  - Call client.generate_music()
  - Return formatted response
- [ ] Implement `generate_lyrics` tool:
  - Parameters: prompt, style (optional)
  - Call client.generate_lyrics()
- [ ] Implement `extend_audio` tool:
  - Parameters: audio_id, continue_at (timestamp)
  - Call client.extend_audio()
- [ ] Implement `get_clip` tool:
  - Parameters: clip_id
  - Call client.get_clip()
- [ ] Implement `get_credits` tool:
  - No parameters
  - Call client.get_credits()
- [ ] Implement `separate_stems` tool:
  - Parameters: clip_id
  - Call client.separate_stems()

## Phase 5: Testing

- [ ] Create tests/test_tools.py:
  - Test each tool with mocked client responses
  - Test parameter validation (invalid inputs)
  - Test error handling (API failures)
- [ ] Create tests/test_models.py:
  - Test Pydantic model validation
  - Test Settings loading from environment
- [ ] Run tests: `uv run pytest`
- [ ] Fix any failing tests
- [ ] Verify test coverage >80%

## Phase 6: Documentation

- [ ] Update README.md:
  - Project description
  - Installation instructions (uv sync)
  - Configuration (.env setup with API key)
  - Running the server
  - Example tool calls for each major feature
  - Troubleshooting section
- [ ] Add docstrings to all public functions
- [ ] Add type hints to all functions

## Phase 7: Integration & Validation

- [ ] Manual integration test with real Suno API:
  - Generate a simple music track
  - Retrieve clip details
  - Check credits
  - Document results in README
- [ ] Test MCP server with Claude Desktop:
  - Add server to claude_desktop_config.json
  - Restart Claude Desktop
  - Test generate_music tool
  - Test get_credits tool
- [ ] Verify all acceptance criteria from spec.md

## Phase 8: Git & Documentation

- [ ] git add .specify/
- [ ] git add src/ tests/ pyproject.toml README.md
- [ ] git commit with spec reference:
  "Implement Suno MCP Server
  
  Implements: .specify/specs/suno-mcp-server/spec.md
  - MCP tools for music generation, lyrics, audio extension
  - Official MCP SDK integration
  - Async Suno API client with rate limiting
  - Pydantic models for type safety
  - Comprehensive test coverage"
- [ ] Create git tag: v0.1.0
