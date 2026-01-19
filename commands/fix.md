---
description: |
  Debug and fix bugs, errors, or failing tests with minimal, targeted changes.
  Examples: /fix "500 error on login", /fix PostTest, /fix "TypeError in dashboard"
argument-hint: [error message or bug description]
allowed-tools: Read, Grep, Glob, Edit, Write, Bash, mcp_gemini-bridge, mcp_open-bridge, mcp_codex-bridge, mcp_context7
---
# Fix Mode

**Routes to:** `debugger` agent
**Skill:** `bugfix-and-debug`

> Fix root cause only. Every fix MUST include a regression test.

## Problem
**$ARGUMENTS**

## MCP Helpers (Brain + Memory)

### 🧠 Gemini-Bridge (Brain) — Deep Error Analysis
```
mcp_gemini-bridge_consult_gemini(query="Analyze this error and suggest root cause: [error message]. Stack trace: [trace]", directory=".")
```

### 🌉 Open-Bridge — Alternative Error Analysis
```
mcp_open-bridge_consult_gemini(query="Analyze this error and suggest root cause: [error message]. Stack trace: [trace]", directory=".")
```

### 📚 Context7 (Memory) — Framework Error Patterns
```
mcp_context7_resolve-library-id(libraryName="[framework]", query="[error type]")
mcp_context7_query-docs(libraryId="/[resolved-id]", query="[specific error]")
```

## Debugging Process

### 1. Reproduce
- Get the exact error message / stack trace
- Find the minimal reproduction steps
- Identify the entry point (route, command, test)

### 2. Investigate
```bash
# Search for error message
grep -r "error text" --include="*.php" --include="*.tsx"

# Check recent changes
git log --oneline -10
git diff HEAD~3

# Run specific failing test
php artisan test --filter=TestName
```

### 3. Hypothesize
List 1-3 possible causes:
1. [Most likely cause]
2. [Second possibility]
3. [Third possibility]

### 4. Verify Hypothesis
- Read the relevant code
- Add temporary logging if needed
- Confirm which hypothesis is correct

### 5. Fix
- Make the **smallest** change that fixes the root cause
- Don't fix symptoms — fix the underlying issue
- Consider edge cases

### 6. Prevent Regression
- Add or update test to cover this case
- Ensure the test fails without the fix, passes with it

### 7. Verify
```bash
composer test                    # All tests pass
npm run types                    # No type errors  
npm run lint                     # No lint errors
```

## Required Output Format

```markdown
## 🐛 Bug
[One sentence description]

## 🔍 Root Cause
[What was actually wrong and why]

## 🔧 Fix
| File | Change |
|------|--------|
| ... | ... |

## 🧪 Regression Test
[Test name and what it covers]

## ✅ Verification
$ [command]
[output showing fix works]

## 💡 Prevention
[How to prevent similar bugs, if applicable]
```

## Rules
- Don't change unrelated code
- Fix root cause, not symptoms
- Always add a test
- Keep the fix minimal
