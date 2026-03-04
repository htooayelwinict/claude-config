---
description: |
  Create a structured implementation plan with research and phases.
  Examples: /plan user-authentication, /plan "add comments feature", /plan dashboard-redesign
argument-hint: <feature-name>
allowed-tools: Read, Edit, Bash, Write, mcp_codex-bridge, mcp_gemini-bridge, mcp_context7, mcp_playwright, mcp_zread, mcp_web-search-prime, mcp_web-reader, mcp_zai-mcp-server, mcp_open-bridge
---
# Plan Command

**Routes to:** `planner` agent
**Skill:** `project-planning`

> Create plan.md (consolidated) AND individual phase files in phases/ folder for detailed tracking.

Create a comprehensive implementation plan for: **$ARGUMENTS**

## Folder Structure

Create the following structure:
```
plan/
└── $1-YYYYMMDD-HHMMSS/
    ├── README.md           # Plan overview and summary
    ├── plan.md             # ⭐ CONSOLIDATED: All phases + context in one file
    ├── research/
    │   ├── requirements.md # User requirements analysis
    │   ├── existing-code.md # Current codebase analysis
    │   └── references.md   # External docs and best practices
    └── phases/
        ├── phase-1-*.md    # First phase details
        ├── phase-2-*.md    # Second phase details
        └── ...             # Additional phases as needed
```

## Step 1: Create Plan Folder

Generate folder name using format: `[feature-name]-[YYYYMMDD]-[HHMMSS]`

Example: `user-authentication-20241214-153042`

```bash
# Get current timestamp
TIMESTAMP=$(date +%Y%m%d-%H%M%S)
PLAN_NAME="$1-${TIMESTAMP}"
mkdir -p "plan/${PLAN_NAME}/research" "plan/${PLAN_NAME}/phases"
```

## MCP Helpers (Brain + Memory)

### 🧠 Gemini-Bridge (Brain) — Architecture Analysis
```
mcp_gemini-bridge_consult_gemini(
  query="Analyze architecture for [feature]: requirements, patterns, risks, phases...",
  directory="."
)
```

### 🌉 Open-Bridge — Alternative Architecture Analysis
```
mcp_open-bridge_consult_gemini(
  query="Analyze architecture for [feature]: requirements, patterns, risks, phases...",
  directory="."
)
```

### 📚 Context7 (Memory) — Documentation Lookup
```
mcp_context7_resolve-library-id(libraryName="laravel", query="[feature] implementation")
mcp_context7_query-docs(libraryId="/laravel/docs", query="[specific topic]")
```

## Step 2: Research Phase

Before planning, gather context:

1. **Analyze Requirements** → `research/requirements.md`
   - What exactly is being requested?
   - What are the acceptance criteria?
   - What are the constraints?

2. **Analyze Existing Code** → `research/existing-code.md`
   - Find similar patterns in codebase
   - Identify files that need modification
   - Note dependencies and relationships

3. **External References** → `research/references.md`
   - Laravel documentation references
   - React/Inertia patterns
   - Best practices for this type of feature

## Step 3: Create Plan Overview

Create `README.md` with:

```markdown
# Plan: [Feature Name]

**Created:** [Date/Time]
**Status:** Planning

## Summary
[One paragraph description]

## Goals
- [ ] Primary goal
- [ ] Secondary goals

## Scope
### In Scope
- ...

### Out of Scope
- ...

## Risk Assessment
| Risk | Impact | Mitigation |
|------|--------|------------|
| ... | High/Med/Low | ... |

## Phases Overview
| Phase | Description | Est. Effort |
|-------|-------------|-------------|
| 1 | ... | ... |
| 2 | ... | ... |

## Files to Modify
- `path/to/file.php` — Description of change
- `path/to/component.tsx` — Description of change
```

## Step 4: Create Phase Files

For each phase, create `phases/phase-N-[name].md`:

```markdown
# Phase N: [Phase Name]

## Objective
[What this phase accomplishes]

## Prerequisites
- Phase N-1 completed (if applicable)
- [Other prerequisites]

## Tasks
- [ ] Task 1
  - Implementation details
  - Files: `path/to/file`
- [ ] Task 2
  - Implementation details
  - Files: `path/to/file`

## Verification
```bash
# Commands to verify this phase
composer test --filter=TestName
npm run types
```

## Deliverables
- [ ] Code changes
- [ ] Tests
- [ ] Documentation updates

## Notes
[Any additional context]
```

## Step 5: Output Summary

After creating the plan, output:

```markdown
## ✅ Plan Created

**Location:** `plan/[folder-name]/`

### Structure
- 📄 `plan.md` — ⭐ All phases + context (single source of truth)
- 📄 `README.md` — Plan overview
- 📁 `research/` — Background research and analysis
- 📁 `phases/` — Detailed implementation phases

### Phases
1. [Phase 1 name] — [brief description]
2. [Phase 2 name] — [brief description]
...

### Next Steps
1. Review the consolidated plan in `plan/[folder-name]/plan.md`
2. Run `/research [topic]` if more research needed
3. Run `/code plan/[folder-name]` to start implementation
```

## Planning Guidelines

- **Break large features into 3-5 phases**
- **Each phase should be independently testable**
- **First phase handles data layer (migrations, models)**
- **Last phase handles polish (UI, docs, tests)**
- **Include rollback strategy for risky changes**
