# Feature: Suno MCP Server

## Description

An MCP (Model Context Protocol) server that enables AI music agents to generate and manipulate music using the Suno AI API. The server exposes Suno's music generation capabilities as MCP tools, allowing Claude and other AI assistants to create music from text descriptions, generate lyrics, extend audio, and manage music tasks.

## Why This Feature

Enable AI agents to become music production assistants by:
- Converting natural language music requests into API calls
- Managing music generation workflows programmatically
- Providing structured access to professional music generation
- Abstracting complex API interactions behind simple tool calls

## Requirements

### Core Music Generation
- [ ] Generate music from text description (style, mood, theme)
- [ ] Generate instrumental music (no vocals)
- [ ] Create music with custom lyrics
- [ ] Generate lyrics only (without music)
- [ ] Extend existing music tracks (continue in same style)

### Audio Processing
- [ ] Get vocal stem from track (voice only)
- [ ] Get instrumental stem from track (music only)
- [ ] Convert generated music to WAV format
- [ ] Upload and extend user-provided audio

### Task Management
- [ ] Check generation task status (pending/complete/failed)
- [ ] Retrieve generated music details (URL, metadata)
- [ ] Get account credits remaining
- [ ] List recent generation tasks

### MCP Tool Design
- [ ] Each tool has clear description and parameter schema
- [ ] Tools return structured JSON responses
- [ ] Error handling with actionable messages
- [ ] Support for long-running operations (async status checks)

### Configuration
- [ ] API key via environment variable (SUNO_API_KEY)
- [ ] Optional base URL override (for testing/proxies)
- [ ] Rate limit handling with backoff

## User Stories

**As a music agent,** I want to generate music from text descriptions so that I can create custom soundtracks based on user requests.

**As a music agent,** I want to extend existing tracks so that I can create longer versions or variations of generated music.

**As a music agent,** I want to check task status so that I can track long-running music generation operations.

**As a music agent,** I want to extract vocal/instrumental stems so that I can remix or analyze tracks.

**As a developer,** I want clear error messages when API calls fail so that I can debug issues quickly.

**As a user,** I want secure API key management so that my credentials are never exposed in logs or code.

## Acceptance Criteria

### Functionality
- [ ] Generate music tool successfully creates tracks from text prompts
- [ ] Lyrics generation tool returns coherent song text
- [ ] Extend audio tool continues tracks in same style
- [ ] Stem separation tools return isolated vocal/instrumental tracks
- [ ] Task status tool reports accurate generation progress

### Quality
- [ ] All tools have Pydantic models for request/response validation
- [ ] Error responses include error type and remediation steps
- [ ] API key never appears in logs or error messages
- [ ] Rate limit errors trigger exponential backoff

### Testing
- [ ] Unit tests for all MCP tools
- [ ] Mocked Suno API responses for reliable testing
- [ ] Integration test with real API (manual, using test credits)
- [ ] Error handling tests (invalid params, API failures, network errors)

### Documentation
- [ ] README with setup instructions
- [ ] Tool descriptions visible in MCP tool list
- [ ] Example usage for each major tool
- [ ] Environment variable configuration documented

### Security
- [ ] .env file in .gitignore
- [ ] .env.example provided with placeholder key
- [ ] No hardcoded secrets in code
- [ ] Sanitized error messages (no key leakage)

## Out of Scope (for v1.0)

- Music video generation
- Batch processing multiple generations
- Caching generated music locally
- Custom model fine-tuning
- Real-time streaming generation updates
