# Workflow Guide

<img width="1677" height="934" alt="image" src="https://github.com/user-attachments/assets/6856fd10-5b97-4553-9675-3ac00057054b" />

This guide walks through the complete workflow from idea to deployed project.

## Phase 1: Initialization

### Starting a New Project

Invoke the init agent with your idea:

```
@01-init.mdc

I want to build a carbon accounting platform for Turkish businesses.
It should calculate emissions, generate ISO 14064 compliant reports,
and integrate with common ERP systems.
```

### What the Init Agent Does

1. **Categorizes** your project (Full-stack, Frontend-only, Backend-only, CLI)
2. **Asks 5 clarifying questions** covering:
   - Users & access patterns
   - Core functionality
   - Data & integrations
   - Design requirements
   - Technical constraints

### Example Questions

```
## Clarifying Questions

1. **Users & Access**
   Who will use this platform?
   a) Internal company employees only
   b) External business clients
   c) Both internal and external users
   
   What authentication is needed?
   a) None
   b) Simple email/password
   c) SSO/OAuth (Google, Microsoft)
   d) Role-based with admin/user levels

2. **Core Functionality**
   What is the ONE thing this must do perfectly in v1?
   
   What's explicitly out of scope for the first version?

3. **Data & State**
   What data sources will you integrate with?
   a) Manual data entry only
   b) ERP systems (which ones?)
   c) Accounting software
   d) Other: ___

4. **Design & UX**
   Do you have existing brand guidelines?
   a) No - create new design system
   b) Yes - I'll provide brand colors/fonts
   c) Match an existing app: ___
   
   Primary device target?
   a) Desktop-first (enterprise dashboard)
   b) Mobile-first
   c) Both equally important

5. **Constraints**
   Any hard technology requirements or restrictions?
   Any deadline or milestone dates?
```

### Confirming Scope

After you answer, the agent produces a **Scope Summary**:

```markdown
## Scope Summary

**Project Name:** BotanikaUS Carbon Accounting
**Type:** Full-stack

### What We're Building
A carbon accounting SaaS platform for Turkish businesses that calculates
emissions based on ERP data, generates ISO 14064 compliant reports, and
provides dashboard visualizations for tracking progress toward reduction goals.

### Core Requirements
1. ERP data integration (SAP, Logo, Netsis)
2. Emission calculation engine following GHG Protocol
3. ISO 14064 compliant PDF report generation
4. Dashboard with data visualization

### Explicitly Out of Scope (v1)
- Supply chain (Scope 3) emissions
- Mobile app
- Multi-language support
- AI-powered recommendations

### Tech Constraints
- Must handle Turkish character encoding
- PDF generation required for official reports

### Target Users
Sustainability managers and CFOs at medium-to-large Turkish companies

### Design Direction
New design system - professional, data-focused, accessible
```

You confirm or request changes. On confirmation:
- Scope is saved to `/Docs/Scope.md`
- You're prompted to run the planning agent

---

## Phase 2: Planning

### Generating Documentation

```
@02-plan.mdc
```

### What the Planning Agent Does

1. **Reads** `/Docs/Scope.md`
2. **Researches** technology choices via web search
3. **Generates** six documentation files:
   - PRD.md
   - Tech.md
   - Structure.md
   - Design.md
   - Implementation.md
   - Issues.md

### Tech Stack Research

The agent searches for current best practices:

```
Searching: "React vs Vue 2024 enterprise dashboard"
Searching: "TypeScript Node.js backend best practices"
Searching: "PostgreSQL vs MySQL Turkish character support"
Searching: "PDF generation Node.js ISO compliant reports"
```

Then recommends with rationale:

```markdown
## Frontend
| Category | Choice | Rationale |
|----------|--------|-----------|
| Framework | Next.js 14 | SSR for SEO, App Router, strong enterprise adoption |
| Styling | Tailwind CSS | Rapid development, design system friendly |
| Charts | Recharts | Good React integration, accessible |

**Documentation:**
- https://nextjs.org/docs
- https://tailwindcss.com/docs
```

### Design System Generation

`Design.md` is created with:

```markdown
## Color Palette

### Primary Colors
| Name | Value | Usage |
|------|-------|-------|
| primary | #0F766E | Main actions, key metrics |
| primary-hover | #0D9488 | Hover states |
| primary-light | #CCFBF1 | Backgrounds, highlights |

### Semantic Colors
| Name | Value | Usage |
|------|-------|-------|
| success | #16A34A | Positive trends, completions |
| warning | #CA8A04 | Alerts, thresholds |
| error | #DC2626 | Errors, negative trends |

## Typography

### Scale
| Name | Size | Weight | Usage |
|------|------|--------|-------|
| h1 | 2.25rem | 700 | Page titles |
| h2 | 1.5rem | 600 | Section headers |
| metric | 2rem | 700 | Dashboard numbers |
| body | 1rem | 400 | Body text |
```

### Implementation Plan

Tasks are broken into numbered stages and subtasks:

```markdown
## Stage 1: Project Setup
**Duration:** 2-3 days

### 1.1 Initialize Monorepo
- [ ] 1.1.1 Create directory structure
- [ ] 1.1.2 Configure TypeScript
- [ ] 1.1.3 Set up linting

### 1.2 Database Setup
- [ ] 1.2.1 Configure PostgreSQL
- [ ] 1.2.2 Create initial schema
- [ ] 1.2.3 Set up Prisma

## Stage 2: Core Backend
**Duration:** 1 week

### 2.1 API Foundation
- [ ] 2.1.1 Express server setup
- [ ] 2.1.2 Authentication endpoints
...
```

### Reviewing Documentation

The agent presents all generated files:

```
## Documentation Generated

✅ /Docs/PRD.md — 847 words
✅ /Docs/Tech.md — 412 words
✅ /Docs/Structure.md — 523 words
✅ /Docs/Design.md — 689 words
✅ /Docs/Implementation.md — 1,247 words (23 tasks)
✅ /Docs/Issues.md — Template ready

Review each document. Request changes if needed.
When ready, say "start" to begin implementation.
```

---

## Phase 3: Execution

### Starting Work

The execution agent (`03-execute.mdc`) is always on. Say:

```
start
```

Or specify a task:

```
do 2.1.1
```

### Task Execution Flow

For each task, the agent:

1. **Announces** what it's working on:
```
## Starting Task 1.1.1: Create directory structure

**From Stage:** 1 - Project Setup
**Acceptance Criteria:** Project runs npm install without errors

Checking relevant documentation...
- Structure.md: Monorepo layout confirmed
- Tech.md: Using npm workspaces
```

2. **Checks documentation** before coding:
   - Structure.md for file locations
   - Design.md for any UI work
   - Tech.md for patterns
   - Issues.md for known problems

3. **Implements** the task

4. **Reports** completion:
```
## Completed: 1.1.1 Create directory structure

### Changes Made
- `frontend/` — Initialized Next.js app
- `backend/` — Created Express structure
- `shared/` — Shared types directory
- `package.json` — Root workspace config

### Verification
- [x] npm install runs without errors
- [x] Matches Structure.md layout
- [x] TypeScript configured

Ready to proceed to 1.1.2: Configure TypeScript?
```

5. **Waits** for your approval

### Approval Responses

| Say | Effect |
|-----|--------|
| `yes` / `continue` / `next` | Proceed to next task |
| `skip` | Skip current task, continue |
| `wait` | Pause, you'll manually continue later |
| `[feedback]` | Agent addresses feedback first |

### UI Implementation

For any UI task, the agent **must** check Design.md:

```
## Starting Task 3.2.3: Create Button component

Checking Design.md...
- Primary button: #0F766E, hover #0D9488
- Sizes: sm (32px), md (40px), lg (48px)
- Border radius: 6px
- Font weight: 500

Implementing with design tokens...
```

The generated code uses specified values:

```tsx
// Button.tsx
const variants = {
  primary: 'bg-primary hover:bg-primary-hover text-white',
  secondary: 'bg-gray-100 hover:bg-gray-200 text-gray-900',
  ghost: 'hover:bg-gray-100 text-gray-700',
};

const sizes = {
  sm: 'h-8 px-3 text-sm',
  md: 'h-10 px-4',
  lg: 'h-12 px-6 text-lg',
};
```

### Progress Tracking

At any point, say `status` to see progress:

```
## Current Progress

**Stage 1: Project Setup** ✅ Complete
- [x] 1.1 Initialize Monorepo
- [x] 1.2 Database Setup
- [x] 1.3 Development Environment

**Stage 2: Core Backend** 🟡 In Progress
- [x] 2.1 API Foundation
- [ ] 2.2 Authentication ← Current
- [ ] 2.3 Emission Calculations

**Stage 3: Core Frontend** ⬜ Not Started
**Stage 4: Integration** ⬜ Not Started

**Current Task:** 2.2.1 Create user model
**Issues:** 1 open (ISS-003: Database connection timeout)
```

---

## Phase 4: Error Handling

### When Errors Occur

The execution agent attempts to handle errors inline:

```
## Error in Task 2.2.3

**Error:**
TypeError: Cannot read property 'id' of undefined
  at createSession (auth.ts:47)

**Analysis:**
User object not being returned from database query.

**Attempting fix...**
[Agent tries a focused fix]
```

If the fix works, it continues. If not:

```
## Blocked: 2.2.3 Session creation

**Error:** User lookup returns undefined
**Attempted:** Added null check, verified query
**Logged:** Issues.md (ISS-004)

I need your input:
a) Debug database connection
b) Check user seeding script
c) Skip and continue
d) Other guidance
```

### Recovery Mode

For persistent issues, invoke:

```
@04-recover.mdc
```

The recovery agent provides deeper analysis and options:

```
## Recovery Analysis: ISS-004

**Root Cause:**
The users table exists but has no data. The seed script
ran against a different database instance.

**Options:**

### A: Re-run seed script
- Connect to correct database
- Run: npx prisma db seed
- Effort: Low

### B: Create user manually
- Insert test user via Prisma Studio
- Effort: Low

### C: Fix environment configuration
- Update .env with correct DATABASE_URL
- Re-run migrations
- Effort: Medium

Which option?
```

---

## Phase 5: Completion

### Stage Completion

When a stage finishes:

```
## Stage 2 Complete! 🎉

### Completed Tasks
- [x] 2.1 API Foundation
- [x] 2.2 Authentication
- [x] 2.3 Emission Calculations
- [x] 2.4 Report Generation

### Acceptance Check
| Criteria | Status |
|----------|--------|
| All endpoints return correct data | ✅ |
| Authentication flow works | ✅ |
| Reports generate as PDF | ✅ |

### Issues Resolved
- ISS-004: Database seeding (fixed)
- ISS-005: PDF encoding (fixed)

Ready for Stage 3: Core Frontend?
```

### Project Completion

When all stages are done:

```
## 🎉 Project Complete: BotanikaUS

### Final Status
All 4 stages complete. 23/23 tasks done.

### Deliverables
- [x] ERP data integration working
- [x] Emission calculations accurate
- [x] ISO 14064 reports generating
- [x] Dashboard visualizations complete

### Open Items
- ISS-012: Performance optimization (deferred to v1.1)

### Deployment Ready
- [x] Environment variables documented
- [x] Build scripts tested
- [x] CI/CD configured
```

---

## Context Management

### Handoff Protocol

When conversations get long (or you need to pause):

```
handoff
```

The recovery agent creates `/Docs/Handoff.md`:

```markdown
# Session Handoff

**Date:** 2025-01-11
**Last Task:** 3.2.4 Dashboard layout

## Current State
- Stage 1: ✅ Complete
- Stage 2: ✅ Complete  
- Stage 3: 🟡 In Progress (at 3.2.4)
- Stage 4: ⬜ Not Started

## Active Issues
- ISS-008: Chart responsiveness (in progress)

## To Resume
"Read /Docs/Handoff.md and continue from 3.2.4"
```

### Resuming Work

In a new conversation:

```
Read /Docs/Handoff.md and continue from 3.2.4
```

The execution agent picks up where you left off, using Implementation.md to track progress.

---

## Tips for Best Results

### 1. Don't Skip Initialization
The clarifying questions catch scope issues early. A few minutes here saves hours later.

### 2. Review Generated Docs
Before starting execution, read through PRD.md and Implementation.md. Catch misunderstandings while they're cheap to fix.

### 3. One Task at a Time
The approval gates exist for a reason. Reviewing each task catches issues before they compound.

### 4. Use the Design System
When tempted to hard-code a color or spacing value, check Design.md first. Consistency compounds.

### 5. Log Everything
Even quick fixes should go in Issues.md. Your future self will thank you.

### 6. Handoff Before Context Overflow
If responses are getting slow or the model seems confused, create a handoff. Fresh context is better than degraded context.
