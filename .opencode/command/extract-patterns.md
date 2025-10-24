---
description: Extract learnings to playbooks with curator subagent
agent: meta-agent
model: ollama/kimi-k2:1t-cloud
---

Extract learnings from recent implementation to playbooks.

**Your Role (Meta-Agent):**

Invoke `@curator` subagent to analyze and document:

1. **Successful Patterns**: What worked well in this implementation?
   - Update `~/.config/opencode/playbooks/python-patterns.md` or similar

2. **Anti-Patterns Detected**: Any violations of NO BULLSHIT CODE?
   - Document in playbooks for future reference

3. **Model Performance**: How did generator model perform?
   - Update `~/.config/opencode/playbooks/model-quirks.md`

4. **Review Insights**: Key findings from reflector critique
   - Update `~/.config/opencode/playbooks/code-review-checklist.md`

**Recent changes:**
!`git log --oneline -3`

**Recent diff:**
!`git diff HEAD~3..HEAD --stat`

Invoke @curator to extract patterns and update playbooks now.
