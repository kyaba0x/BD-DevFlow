# File Reference

This document describes every file in the system — both the rule files you install and the documentation files that get generated.

---

## Rule Files

These files go in `.cursor/rules/` or `.claude/rules/`.

### 01-init.mdc

**Purpose:** Gather requirements and clarify project scope before planning begins.

**Trigger:** Manual invocation with `@01-init.mdc`

**When to Use:**
- Starting a new project
- Defining a major new feature
- When requirements are unclear

**Output:** `/Docs/Scope.md`

**Key Behaviors:**
- Asks exactly 5 clarifying questions
- Uses lettered options for easy responses
- Produces a Scope Summary
- Requires explicit user confirmation
- Does NOT suggest technologies
- Does NOT create any code

---

### 02-plan.mdc

**Purpose:** Transform confirmed scope into comprehensive project documentation.

**Trigger:** Manual invocation with `@02-plan.mdc`

**Prerequisites:** `/Docs/Scope.md` must exist

**When to Use:**
- After scope is confirmed
- When you need to regenerate documentation
- Before starting implementation

**Outputs:**
- `/Docs/PRD.md`
- `/Docs/Tech.md`
- `/Docs/Structure.md`
- `/Docs/Design.md`
- `/Docs/Implementation.md`
- `/Docs/Issues.md`

**Key Behaviors:**
- Searches the web for technology best practices
- Provides documentation links for all tech choices
- Creates mandatory design system
- Breaks work into numbered stages and subtasks
- Does NOT write any implementation code

---

### 03-execute.mdc

**Purpose:** Implement the project by following documentation and working through tasks.

**Trigger:** Always on (`alwaysApply: true`)

**Prerequisites:** All `/Docs/` files must exist

**When Active:**
- During all implementation work
- Whenever you say "start", "continue", "next", "do [task]"

**Key Behaviors:**
- Follows document priority: Issues → Implementation → Structure → Design → Tech → PRD
- Announces each task before starting
- Checks relevant docs before writing code
- Reports completion with file changes
- Waits for explicit approval before continuing
- Updates Implementation.md when tasks complete
- Logs errors to Issues.md

**Commands:**
| Command | Action |
|---------|--------|
| `start` | Begin first unchecked task |
| `continue` / `next` | Proceed to next task |
| `do 2.3.1` | Start specific task |
| `skip` | Skip current task |
| `status` | Show progress |
| `rollback [X.Y.Z]` | Invoke recovery for rollback |

---

### 04-recover.mdc

**Purpose:** Handle exceptions — errors, rollbacks, handoffs, and reviews.

**Trigger:** Manual invocation or escalation from execute agent

**When to Use:**
- Persistent errors that can't be quickly fixed
- When you want to undo work (rollback)
- When context is getting long (handoff)
- Stage or project completion review

**Outputs:**
- Updates to Issues.md
- `/Docs/Handoff.md` (when doing handoffs)

**Modes:**
1. **Error Recovery** — Deep analysis and fix options
2. **Rollback** — Revert specific task changes
3. **Handoff** — Create session summary for context transfer
4. **Stage Review** — Validate stage completion
5. **Project Review** — Final deliverables check

**Commands:**
| Command | Action |
|---------|--------|
| `rollback 2.3` | Undo task 2.3 changes |
| `handoff` | Create handoff summary |
| `status` | Current state overview |
| `stuck` | Analyze why progress stopped |
| `review` | Force stage review |

---

## Generated Documentation

These files are created by `02-plan.mdc` and live in `/Docs/`.

### Scope.md

**Created By:** `01-init.mdc`

**Purpose:** Capture confirmed project scope before planning.

**Contents:**
- Project name and type
- Core requirements (3-5 items)
- Explicit exclusions
- Tech constraints
- Target users
- Design direction

**Used By:**
- `02-plan.mdc` — As input for all other docs
- Reference for scope questions during execution

---

### PRD.md

**Created By:** `02-plan.mdc`

**Purpose:** Define what we're building and why.

**Contents:**
- Overview and goals
- User stories by user type
- Functional requirements table (ID, feature, description, acceptance criteria)
- Non-functional requirements (performance, security, accessibility)
- Out of scope items
- Success metrics

**Used By:**
- `03-execute.mdc` — Reference for requirements context
- Stakeholder communication
- Feature validation

**Structure:**
```markdown
# [Project] — Product Requirements Document

## Overview
## Goals
## User Stories
## Functional Requirements
## Non-Functional Requirements
## Out of Scope
## Success Metrics
```

---

### Tech.md

**Created By:** `02-plan.mdc`

**Purpose:** Document technology choices with rationale.

**Contents:**
- Technology choices by category (Frontend, Backend, Database, Infrastructure)
- Rationale for each choice
- Links to official documentation
- Decision log for major choices

**Used By:**
- `03-execute.mdc` — Reference for implementation patterns
- Onboarding new team members
- Architecture decisions

**Structure:**
```markdown
# Technology Stack & Decisions

## Overview
## Frontend
## Backend
## Database
## Infrastructure
## Decision Log
```

---

### Structure.md

**Created By:** `02-plan.mdc`

**Purpose:** Define project organization and conventions.

**Contents:**
- Directory layout (full tree)
- File naming conventions
- Module boundaries
- Import order rules
- Configuration file locations

**Used By:**
- `03-execute.mdc` — **Always checked** before creating files
- Code review reference
- Onboarding

**Structure:**
```markdown
# Project Structure

## Directory Layout
## File Naming Conventions
## Module Boundaries
## Import Order
```

**Critical Rule:** The execution agent MUST check this file before creating any new file to ensure correct location and naming.

---

### Design.md

**Created By:** `02-plan.mdc`

**Purpose:** Define the visual design system. **Mandatory for all projects.**

**Contents:**
- Design principles
- Color palette (primary, neutral, semantic)
- Typography scale
- Spacing system
- Component specifications (buttons, inputs, cards)
- Layout system (breakpoints, grid, container)
- Accessibility requirements
- Motion/animation standards
- Icon specifications

**Used By:**
- `03-execute.mdc` — **Always checked** for any UI work
- Component development
- Design consistency validation

**Structure:**
```markdown
# Design System

## Design Principles
## Color Palette
## Typography
## Spacing
## Components
## Layout
## Accessibility
## Motion
## Icons
```

**Critical Rule:** The execution agent MUST use values from this file when implementing any UI. No hard-coded colors, sizes, or spacing.

---

### Implementation.md

**Created By:** `02-plan.mdc`

**Purpose:** Break project into actionable tasks with acceptance criteria.

**Contents:**
- Staged task breakdown
- Numbered subtasks (X.Y.Z format)
- Checkboxes for tracking completion
- Duration estimates
- Dependencies between stages
- Acceptance criteria per task group
- Progress tracking table

**Used By:**
- `03-execute.mdc` — **Primary reference** for what to work on
- Progress tracking
- Sprint planning

**Structure:**
```markdown
# Implementation Plan

## Overview
## Stage 1: [Name]
### 1.1 [Task]
- [ ] 1.1.1 [Subtask]
- [ ] 1.1.2 [Subtask]
## Stage 2: [Name]
...
## Progress Tracking
```

**Task Numbering:**
- `X` = Stage number
- `Y` = Task number within stage
- `Z` = Subtask number within task

Example: `2.3.1` = Stage 2, Task 3, Subtask 1

**Status Markers:**
- `[ ]` = Not started
- `[x]` = Complete
- `⏭️` = Skipped
- `❌` = Blocked

---

### Issues.md

**Created By:** `02-plan.mdc` (template), updated throughout project

**Purpose:** Track bugs, decisions, blockers, and technical debt.

**Contents:**
- Active issues table
- Resolved issues table
- Issue template
- Open questions
- Decisions made log

**Used By:**
- `03-execute.mdc` — **Checked first** when encountering problems
- `04-recover.mdc` — Reference for recovery options
- Post-mortem analysis

**Structure:**
```markdown
# Issues & Decisions Log

## Active Issues
## Resolved Issues
## Issue Template
## Open Questions
## Decisions Made
```

**Issue Fields:**
| Field | Description |
|-------|-------------|
| ID | ISS-XXX format |
| Type | Bug, Blocker, Question, Decision, Technical Debt |
| Status | Open, In Progress, Resolved, Deferred |
| Priority | Critical, High, Medium, Low |
| Task | Related task (e.g., 2.3.1) |
| Created | Date |
| Resolved | Date or - |
| Description | What happened |
| Context | What was being worked on |
| Root Cause | Why it happened |
| Resolution | How it was fixed |
| Prevention | How to avoid in future |

---

### Handoff.md

**Created By:** `04-recover.mdc` (on demand)

**Purpose:** Capture session state for context transfer.

**Contents:**
- Current date and session focus
- Implementation progress (stage status table)
- Current task and status
- Active issues
- Recent changes
- Key decisions from session
- Known problems
- Next steps
- Resume instructions

**Used By:**
- Starting new conversations after context overflow
- Pausing work for later
- Sharing status with others

**Structure:**
```markdown
# Session Handoff

**Date:** [Date]
**Session Focus:** [Description]

## Current State
## Active Issues
## Recent Changes
## Key Decisions Made This Session
## Known Problems
## Next Steps
## To Resume
```

**Resume Pattern:**
```
Read /Docs/Handoff.md and continue from task [X.Y.Z]
```

---

## File Relationships

```
Scope.md ─────────────────────────────────────────────┐
    │                                                 │
    └──▶ PRD.md ──────────┐                          │
    └──▶ Tech.md ─────────┤                          │
    └──▶ Structure.md ────┼──▶ Implementation.md ────┼──▶ Execution
    └──▶ Design.md ───────┤         │                │
                          │         ▼                │
                          └──▶ Issues.md ◀───────────┘
                                    │
                                    ▼
                              Handoff.md
```

**Flow:**
1. Scope.md informs all planning documents
2. Planning documents feed into Implementation.md
3. Implementation.md drives execution
4. Issues.md tracks problems across all phases
5. Handoff.md captures state for context transfer
