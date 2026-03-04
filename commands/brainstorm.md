---
description: |
  Brainstorm ideas, features, architecture decisions, or creative solutions. Advises only, does NOT implement.
  Examples: /brainstorm "new dashboard features", /brainstorm "auth architecture", /brainstorm "SaaS pricing"
argument-hint: [topic, problem, or "new project"]
allowed-tools: Read, Grep, Glob, Bash, WebFetch, Write, mcp_codex-bridge, mcp_gemini-bridge, mcp_context7, mcp_playwright, mcp_zread, mcp_web-search-prime, mcp_web-reader, mcp_zai-mcp-server, mcp_open-bridge
---
# Brainstorm Mode

**Routes to:** `brainstormer` agent
**Skill:** `brainstorming`

> Advise and explore options, but do NOT implement. Create summary markdown when consensus is reached.

## Topic
$ARGUMENTS

## MCP Helpers (Brain + Memory)

### 🧠 Gemini-Bridge (Brain) — Deep Reasoning
Use for complex analysis, architecture decisions, and creative problem-solving:
```
mcp_gemini-bridge_consult_gemini(query="Analyze approaches for [topic] considering trade-offs...", directory=".")
```

### 🌉 Open-Bridge — Alternative Deep Reasoning
```
mcp_open-bridge_consult_gemini(query="Analyze approaches for [topic] considering trade-offs...", directory=".")
```

### 📚 Context7 (Memory) — Documentation Lookup
Use for up-to-date library docs and best practices:
```
mcp_context7_resolve-library-id(libraryName="react", query="state management patterns")
mcp_context7_query-docs(libraryId="/vercel/next.js", query="app router best practices")
```

## Brainstorming Process

### 1. Clarify Context
- What problem are we solving?
- Who is the target user?
- What constraints exist?

### 2. Diverge (Generate Many Ideas)
Use SCAMPER technique:
- **S**ubstitute — What can be replaced?
- **C**ombine — What can be merged?
- **A**dapt — What can be borrowed?
- **M**odify — What can be changed?
- **P**ut to other uses — New applications?
- **E**liminate — What can be removed?
- **R**everse — What if we did the opposite?

### 3. Organize by Theme
Group related ideas together.

### 4. Evaluate
| Idea | Feasibility | Impact | Effort |
|------|-------------|--------|--------|
| ... | Low/Med/High | Low/Med/High | Low/Med/High |

### 5. Recommend Top 3
Highlight the most promising ideas with rationale.

## Output Format

```markdown
## 🧠 Brainstorm: [Topic]

### 💡 Quick Wins
1. **[Idea]** — [Why it's good]

### 🚀 High Impact
1. **[Idea]** — [Why it's worth the investment]

### 🌟 Moonshots
1. **[Idea]** — [The bold vision]

### Next Steps
1. [Action to explore further]
```

## Quick Prompts
- `/brainstorm new SaaS ideas`
- `/brainstorm features for dashboard`
- `/brainstorm ways to improve UX`
