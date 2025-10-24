# Suno MCP Server Constitution

## Core Principles

### I. NO BULLSHIT CODE (NON-NEGOTIABLE)
**Import Rules - ZERO TOLERANCE:**
- ALL imports at top of file
- NO try/except import blocks
- NO conditional imports based on availability
- NO HAS_* flags for optional dependencies
- Fail fast if dependencies are missing

**Refactoring Standards:**
- Use modern API directly, no version checks
- NO compatibility wrapper functions
- NO backward compatibility shims
- Clean slate approach - pretend old API never existed

### II. Package Management (CRITICAL)
- Python: Use `uv` for ALL operations (NEVER `pip`)
- Commands: `uv add`, `uv remove`, `uv run`, `uv sync`
- If dependency required: Declare explicitly in pyproject.toml

### III. Type Safety & Quality
- Full Pydantic models for all data structures
- Complete type hints on all functions
- Explicit error handling - no silent failures
- Meaningful error messages

### IV. MCP Server Standards
- FastMCP framework for MCP implementation
- Tools must be well-documented with clear parameters
- Error responses must include actionable information
- Environment variables for all secrets (NO hardcoded keys)

### V. Testing Requirements
- Unit tests for core logic
- Integration tests for API interactions
- Tests run via `uv run pytest`
- Mocks for external API calls (Suno API)

## Security Requirements

### API Key Management
- Environment variables ONLY (`.env` file)
- `.env` in `.gitignore` (ALWAYS)
- `.env.example` template provided
- NO keys in code, logs, or error messages

### Error Handling
- Sanitize error messages (no API keys in logs)
- Graceful degradation on API failures
- Rate limit handling with exponential backoff

## Development Workflow

### File Management
- Modify existing files ONLY unless explicitly required
- Ask before creating new files
- Minimal project footprint

### Git Workflow
- Atomic commits with clear messages
- Branch naming: `feature/*`, `bugfix/*`
- Commit after significant changes
- Run tests before committing

## Governance
- This constitution supersedes all practices
- All code must verify compliance with NO BULLSHIT CODE
- Complexity must be justified
- Quality over speed

**Version**: 1.0.0 | **Ratified**: 2025-10-24 | **Last Amended**: 2025-10-24
