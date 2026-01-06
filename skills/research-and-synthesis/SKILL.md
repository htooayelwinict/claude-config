---
name: research-and-synthesis
description: Fetch and summarize authoritative external sources into actionable guidance using MCP helpers. EXCLUSIVE to researcher agent.
allowed-tools: Read, Grep, Glob, Bash, WebFetch, mcp_codex-bridge, mcp_context7
---
# Research and Synthesis

**Exclusive to:** `researcher` agent

## MCP Helpers (Brain + Memory)

### 🧠 Codex-Bridge (Brain) — Deep Analysis
```
mcp_codex-bridge_consult_codex(
  query="Research [topic] with pros/cons, security considerations, and code examples",
  directory="."
)
```

### 📚 Context7 (Memory) — Up-to-Date Docs
```
# Resolve library first
mcp_context7_resolve-library-id(libraryName="fastapi", query="dependency injection")

# Then query
mcp_context7_query-docs(libraryId="/tiangolo/fastapi", query="Depends pattern")
```

## Instructions

1. Understand project stack from `docs/project-overview-pdr.md`
2. Define the research question clearly
3. Use **Codex-Bridge** for deep analysis and reasoning
4. Use **Context7** for up-to-date library documentation
5. Verify with multiple sources
6. Summarize actionable findings

## Supported Stacks

**PHP/Laravel:**
- Laravel 12, Inertia.js, Pest

**JavaScript/React:**
- React 19, TypeScript, Tailwind, shadcn/ui

**Python:**
- FastAPI, LangChain, LangGraph, pytest, Pydantic

## Source Evaluation

| Criteria | ✅ Good | ❌ Bad |
|----------|---------|--------|
| Recency | < 1 year | > 2 years |
| Authority | Official docs | Random blogs |
| Relevance | Same stack | Different framework |

## Research Process

1. Define the question clearly
2. Fetch 1-3 authoritative sources
3. Verify with multiple sources
4. Summarize actionable findings

## Package Evaluation

| Factor | Check |
|--------|-------|
| Maintenance | Last commit < 6 months |
| Compatibility | Works with Laravel 12 / React 19 |
| Documentation | Clear docs, examples |
| Security | No known vulnerabilities |

## Comparison Matrix

```markdown
| Criteria | Option A | Option B |
|----------|----------|----------|
| Implementation | ⭐⭐⭐ | ⭐⭐ |
| Performance | ⭐⭐ | ⭐⭐⭐ |
| Security | ⭐⭐⭐ | ⭐⭐ |
```

## Output Template

```markdown
## Summary
[2-3 sentences]

## Key Findings
- Finding 1
- Finding 2

## Recommendation
[Which option and why]

## References
- [Source](url)
```

## Examples
- "Confirm best practice for Laravel validation"
- "Compare package A vs package B"
