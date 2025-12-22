# Claude Config

Multi-agent development orchestration system for Claude Code. Defines agents, commands, skills, and workflows for structured software development.

## Overview

This config provides a coordinated agent system for development tasks. It routes work to specialized agents (planner, fullstack-developer, debugger, etc.) through slash commands, ensuring proper architecture, verification, and documentation.

## Quick Start

1. **Copy to your project**
   ```bash
   cp -r /path/to/claude-config/* .claude/
   ```

2. **Use slash commands**
   ```bash
   /plan user-authentication    # Create implementation plan
   /code plan/user-auth-*       # Execute the plan
   /research "Laravel policies" # Research topic
   ```

## Commands

| Command | Purpose | Routes To |
|---------|---------|-----------|
| `/plan <feature>` | Create implementation plan with research + phases | planner |
| `/code <plan-folder>` | Execute plan phase-by-phase with verification | fullstack-developer |
| `/research <topic> [--plan <folder>]` | Research via Codex, save to plan/research/ | researcher |
| `/docs [init\|update]` | Create/update project documentation | project-manager |
| `/fix <issue>` | Diagnose and fix bugs | debugger |
| `/review` | Code review | reviewer |
| `/test` | Testing/TDD | testing-expert |
| `/security` | Security audit | security-expert |
| `/brainstorm` | Technical advising (options, pros/cons) | brainstormer |
| `/deploy` | Infrastructure/deployment | devops-engineer |
| `/create-agent` | Scaffold new DeepAgent | - |

## Agents

| Agent | Model | Specialty | Trigger Phrases |
|-------|-------|-----------|-----------------|
| **planner** | Opus | Architecture & Planning | plan, design, architect, how should we, break down, scope |
| **project-manager** | Sonnet | Coordination & Docs | coordinate, scope, update docs, status, what's next |
| **fullstack-developer** | Sonnet | Implementation | implement, build, create, add feature, write code |
| **database-admin** | Sonnet | Data Layer | migration, schema, index |
| **ui-ux-designer** | Sonnet | Interface | UI, component, accessibility |
| **researcher** | Opus | External Knowledge | research, compare, best practice |
| **debugger** | Sonnet | Bug Fixing | bug, error, fix, debug |
| **reviewer** | Sonnet | Code Review | review, PR, check code |
| **testing-expert** | Sonnet | Testing & TDD | test, TDD, coverage |
| **security-expert** | Sonnet | Security Audits | security, vulnerability, OWASP |
| **devops-engineer** | Sonnet | Infrastructure | deploy, docker, CI/CD |
| **brainstormer** | Sonnet | Technical Advising | brainstorm, options, pros/cons |

## File Structure

```
claude-config/
├── agents/              # Agent definitions (frontmatter + instructions)
│   ├── planner.md
│   ├── fullstack-developer.md
│   ├── project-manager.md
│   └── ...
├── commands/            # User-invocable slash commands
│   ├── plan.md
│   ├── code.md
│   ├── research.md
│   └── ...
├── skills/              # Reusable skill packages
│   ├── project-planning/
│   │   ├── SKILL.md
│   │   └── reference.md
│   ├── fullstack-implementation/
│   ├── bugfix-and-debug/
│   └── ...
├── workflows/           # Workflow protocols
│   ├── primary-workflow.md
│   ├── orchestration-protocol.md
│   ├── project-development-rules.md
│   └── documentation-management.md
├── settings.local.json  # Local permissions config
└── .gitignore
```

## How It Works

### 1. Planning Phase

```bash
/plan user-authentication
```

Creates a structured plan:
```
plan/
└── user-authentication-20241214-153042/
    ├── README.md              # Plan overview
    ├── plan.md                # CONSOLIDATED: all phases
    ├── research/
    │   ├── requirements.md
    │   ├── existing-code.md
    │   └── references.md
    └── phases/
        ├── phase-1-database.md
        ├── phase-2-backend.md
        ├── phase-3-frontend.md
        └── phase-4-tests.md
```

### 2. Implementation Phase

```bash
/code plan/user-authentication-20241214-153042
```

Executes each phase with:
- Backend-first order (migration → model → controller → route)
- Frontend second (types → page → components)
- Verification after each phase

### 3. Agent Coordination

```
User Request
    │
    ├─ Planning/Architecture? ───────► planner
    │
    ├─ Database/Schema change? ───────► database-admin
    │
    ├─ Bug/Error/Fix? ────────────────► debugger
    │
    ├─ UI/UX/Component? ───────────────► ui-ux-designer
    │
    ├─ External research? ─────────────► researcher
    │
    ├─ Multi-domain/Coordination? ────► project-manager
    │
    └─ Code implementation? ───────────► fullstack-developer
```

## Workflows

### Primary Workflow

Default 7-phase process for all development:

1. **Understand** — Clarify requirements
2. **Investigate** — Find existing patterns
3. **Plan** — Design minimal change set
4. **Implement** — Write code following patterns
5. **Verify** — Run tests, types, lint
6. **Document** — Update docs
7. **Report** — Summarize changes

### Orchestration Protocol

Coordinates work between agents for multi-domain tasks:

| Pattern | Flow |
|---------|------|
| Feature Development | planner → database-admin → fullstack-developer → project-manager |
| Bug Fix | debugger → database-admin (if DB) → fullstack-developer |
| UI Enhancement | ui-ux-designer → fullstack-developer → ui-ux-designer (review) |
| Pre-Deployment | reviewer → testing-expert → security-expert → devops-engineer |

### Development Rules

Non-negotiable rules:
- Minimal changes (one concern per commit)
- Follow existing patterns exactly
- Fix root causes with tests
- Never commit secrets
- All verification must pass

## Verification Commands

```bash
# PHP/Laravel
composer test                # All tests
php artisan test --filter=Name
./vendor/bin/pint --test     # PHP style

# TypeScript/React
npm run types                # TypeScript check
npm run lint                 # ESLint

# Python
python -m pytest             # Tests
ruff check .                 # Lint
mypy .                       # Type check
```

## Skills

Reusable skill packages for agents:

| Skill | Purpose |
|-------|---------|
| project-planning | Architecture design and implementation planning |
| project-orchestration | Multi-agent coordination |
| fullstack-implementation | Laravel + React + Inertia code execution |
| database-change-management | Migration design and schema changes |
| bugfix-and-debug | Troubleshooting and fixing |
| testing | TDD and test coverage |
| security-review | Security audits |
| ui-ux-design | Interface and component design |
| research-and-synthesis | External knowledge gathering |
| documentation-management | Keeping docs in sync |
| devops-infrastructure | Deployment and infrastructure |
| brainstorming | Technical advising (options/pros/cons) |
| langchain | LangChain framework guidance |
| langgraph | LangGraph agent building |
| deepagent | DeepAgents framework |

## Agent Format

Each agent file has YAML frontmatter:

```yaml
---
name: agent-name
description: |
  What this agent does. Trigger phrases listed here.
tools: [Read, Edit, Bash, etc.]
model: opus|sonnet|haiku
permissionMode: default|acceptEdits
skills: [skill names]
---
# Agent Name

Instructions for the agent...
```

## Command Format

Each command file has YAML frontmatter:

```yaml
---
description: What this command does
argument-hint: <args> format
allowed-tools: [list of tools]
---
# Command Name

Instructions for executing this command...
```

## Creating New Components

### Add Agent

Create `agents/new-agent.md` with frontmatter specifying tools, model, skills.

### Add Command

Create `commands/new-command.md` with description and allowed-tools.

### Add Skill

Create `skills/new-skill/SKILL.md` and `reference.md`.

## Settings

`settings.local.json` configures allowed bash commands:

```json
{
  "permissions": {
    "allow": [
      "Bash(./vendor/bin/pint:*)",
      "Bash(composer test:*)",
      "Bash(npm run types:*)",
      ...
    ]
  }
}
```

## Tech Stack Target

Designed for but not limited to:
- **Backend**: Laravel 12, PHP 8.3+
- **Frontend**: React 19, Inertia.js, TypeScript 5.x, Tailwind CSS 4
- **Testing**: Pest (PHP), pytest (Python)
- **Python**: FastAPI, LangChain, LangGraph

## License

MIT
