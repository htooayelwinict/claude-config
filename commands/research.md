---
description: Research a topic using Gemini CLI/MCP and save to plan's research folder
argument-hint: <topic> [--plan <plan-folder>]
allowed-tools: Read, Bash, WebFetch, mcp_gemini-bridge, mcp_context7, mcp_web-search-prime, mcp_web-reader, mcp_zread
---

# Research Mode

**Routes to:** `researcher` agent
**Skill:** `research-and-synthesis`

# Research Command

Research the following topic: **$ARGUMENTS**

## Purpose

Use Gemini CLI to gather information about the requested topic and save findings to the appropriate research folder.

## Usage Patterns

```bash
# Research for a new plan (creates temp research)
/research user authentication best practices

# Research for existing plan
/research user authentication --plan plan/user-auth-20241214-153042

# Research specific technology
/research "Laravel Sanctum API tokens"

# Research security concerns
/research "OWASP authentication guidelines"
```

## Step 1: Determine Research Location

If `--plan` flag provided:
- Save to `[plan-folder]/research/`

If no plan specified:
- Save to `plan/research-[topic]-[timestamp]/`

## Step 2: Use MCP Helpers for Research

### 🧠 Gemini-Bridge (Brain) — Deep Analysis
```
mcp_gemini-bridge_consult_gemini(
  query="Research the following topic for a Laravel 12 + React 19 + Inertia.js OR Python/FastAPI/LangChain project:

  Topic: $ARGUMENTS

  Provide:
  1. Overview and key concepts
  2. Best practices and patterns
  3. Security considerations
  4. Implementation approaches
  5. Code examples (Laravel/React/Python where applicable)
  6. Common pitfalls to avoid
  7. Recommended packages/libraries
  8. Official documentation links

  Format as structured markdown.",
  directory="."
)
```

### 📚 Context7 (Memory) — Up-to-Date Docs
```
# First resolve the library ID
mcp_context7_resolve-library-id(libraryName="[library-name]", query="$ARGUMENTS")

# Then query the docs
mcp_context7_query-docs(libraryId="/[resolved-id]", query="$ARGUMENTS")
```

### Supported Stacks
- **PHP/Laravel**: Laravel 12, Pest, Inertia.js
- **JavaScript/React**: React 19, TypeScript, Tailwind, shadcn/ui
- **Python**: FastAPI, LangChain, LangGraph, pytest, Pydantic

## Step 3: Save Research Output

Create research file with timestamp:

```markdown
# Research: [Topic]

**Date:** [YYYY-MM-DD HH:MM]
**Query:** [Original query]

## Summary
[Key findings in 2-3 sentences]

## Key Concepts
- Concept 1: explanation
- Concept 2: explanation

## Best Practices
1. Practice 1
2. Practice 2

## Security Considerations
- ⚠️ Security point 1
- ⚠️ Security point 2

## Implementation Approaches

### Approach 1: [Name]
**Pros:** ...
**Cons:** ...
```php
// Example code
```

### Approach 2: [Name]
**Pros:** ...
**Cons:** ...
```tsx
// Example code
```

## Recommended Stack
| Purpose | Package | Why |
|---------|---------|-----|
| ... | ... | ... |

## Common Pitfalls
1. ❌ Pitfall 1 — How to avoid
2. ❌ Pitfall 2 — How to avoid

## References
- [Official Docs](url)
- [Tutorial](url)
- [Best Practices Guide](url)

## Raw Gemini Response
<details>
<summary>Full response</summary>

[Full Gemini CLI output]

</details>
```

## Step 4: Additional Research Sources

After MCP research, supplement with:

1. **Codebase Analysis**
   ```bash
   # Find similar implementations (PHP/Laravel)
   grep -r "related_pattern" --include="*.php" app/
   grep -r "related_pattern" --include="*.tsx" resources/js/
   
   # Find similar implementations (Python)
   grep -r "related_pattern" --include="*.py" src/
   ```

2. **Package Documentation**
   - PHP: Check `composer.json`, review docs on Packagist/GitHub
   - Node: Check `package.json`, review npm/GitHub
   - Python: Check `pyproject.toml` or `requirements.txt`, review PyPI/GitHub

3. **Official Docs**
   **PHP/Laravel Stack:**
   - Laravel: https://laravel.com/docs
   - React: https://react.dev
   - Inertia: https://inertiajs.com
   
   **Python Stack:**
   - FastAPI: https://fastapi.tiangolo.com
   - LangChain: https://python.langchain.com
   - LangGraph: https://langchain-ai.github.io/langgraph

## Step 5: Output Summary

```markdown
## ✅ Research Complete

**Topic:** [Topic]
**Saved to:** `[research-folder]/[filename].md`

### Key Findings
1. [Finding 1]
2. [Finding 2]
3. [Finding 3]

### Recommended Approach
[Brief recommendation based on research]

### Next Steps
- Review full research at `[path]`
- Create plan with `/plan [feature]`
- Or add to existing plan research folder
```

## Research Templates

### Feature Research (PHP/Laravel)
```
mcp_gemini-bridge_consult_gemini(
  query="How to implement [feature] in Laravel 12 with React frontend using Inertia.js? Include security considerations and testing approach.",
  directory="."
)
```

### Feature Research (Python)
```
mcp_gemini-bridge_consult_gemini(
  query="How to implement [feature] in FastAPI/LangChain? Include Pydantic models, async patterns, and pytest testing approach.",
  directory="."
)
```

### Library Docs Lookup
```
mcp_context7_resolve-library-id(libraryName="langchain", query="[feature] implementation")
mcp_context7_query-docs(libraryId="/langchain-ai/langchain", query="[specific topic]")
```

### Package Evaluation
```
mcp_gemini-bridge_consult_gemini(
  query="Compare [package1] vs [package2] for [use-case]. Include pros, cons, and recommendation.",
  directory="."
)
```

### Security Research
```
mcp_gemini-bridge_consult_gemini(
  query="Security best practices for [feature] in web applications. Focus on [Laravel/FastAPI] backend and [React/Vue] frontend.",
  directory="."
)
```

### Performance Research
```
mcp_gemini-bridge_consult_gemini(
  query="Performance optimization for [feature]. Include caching strategies, async patterns, and database optimization.",
  directory="."
)
```

### Security Research
```bash
gemini "Security best practices for [feature] in web applications. Focus on Laravel/PHP backend and React frontend."
```

### Performance Research
```bash
gemini "Performance optimization for [feature] in Laravel with React/Inertia. Include caching strategies and database optimization."
```
