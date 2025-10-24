---
description: Review code against spec acceptance criteria with reflector subagent
agent: meta-agent
model: ollama/kimi-k2:1t-cloud
---

Review implementation of `.specify/specs/$ARGUMENTS/` against its acceptance criteria.

**Your Role (Meta-Agent):**

Invoke `@reflector` subagent to perform comprehensive code review:

1. **Spec Compliance**: Check all acceptance criteria from spec.md are met
2. **NO BULLSHIT CODE**: Enforce quality standards
   - All imports at top of file
   - No try/except import blocks
   - No version checks (if version >= X)
   - No HAS_* availability flags
3. **Testing**: Run test suite (pytest/bun test)
4. **Package Manager**: Verify uv (not pip) or bun (not npm) used
5. **Type Safety**: Check Pydantic models and type hints

**Files to review:**
!`git diff --name-only HEAD~1`

**Spec acceptance criteria:**
@.specify/specs/$ARGUMENTS/spec.md

Invoke @reflector now for detailed critique.
