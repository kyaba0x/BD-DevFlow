# BD DevFlow System
![banner](https://github.com/user-attachments/assets/b231151c-9384-4c77-b69d-52bf271e7113)<svg xmlns="http://www.w3.org/2000/svg" viewBox="0 0 1200 400">
  <defs>
    <linearGradient id="heroGrad" x1="0%" y1="0%" x2="100%" y2="100%">
      <stop offset="0%" style="stop-color:#1a1b26;stop-opacity:1" />
      <stop offset="100%" style="stop-color:#0D1117;stop-opacity:1" />
    </linearGradient>
    <linearGradient id="accentGrad" x1="0%" y1="0%" x2="100%" y2="0%">
      <stop offset="0%" style="stop-color:#818CF8;stop-opacity:1" />
      <stop offset="33%" style="stop-color:#34D399;stop-opacity:1" />
      <stop offset="66%" style="stop-color:#60A5FA;stop-opacity:1" />
      <stop offset="100%" style="stop-color:#F472B6;stop-opacity:1" />
    </linearGradient>
    <filter id="glow2" x="-50%" y="-50%" width="200%" height="200%">
      <feGaussianBlur stdDeviation="20" result="coloredBlur"/>
      <feMerge>
        <feMergeNode in="coloredBlur"/>
        <feMergeNode in="SourceGraphic"/>
      </feMerge>
    </filter>
  </defs>
  
  <!-- Background -->
  <rect width="1200" height="400" fill="url(#heroGrad)"/>
  
  <!-- Accent line top -->
  <rect x="0" y="0" width="1200" height="4" fill="url(#accentGrad)"/>
  
  <!-- Decorative circles -->
  <circle cx="100" cy="100" r="150" fill="#818CF8" opacity="0.03"/>
  <circle cx="1100" cy="300" r="200" fill="#34D399" opacity="0.03"/>
  <circle cx="600" cy="400" r="180" fill="#60A5FA" opacity="0.02"/>
  
  <!-- Main Title -->
  <text x="600" y="130" font-family="system-ui, -apple-system, BlinkMacSystemFont, sans-serif" font-size="64" font-weight="800" fill="#F0F6FC" text-anchor="middle">BD DevFlow</text>
  
  <!-- Subtitle -->
  <text x="600" y="175" font-family="system-ui, sans-serif" font-size="22" font-weight="400" fill="#8B949E" text-anchor="middle">4-Agent AI Development Workflow</text>
  
  <!-- Agent badges -->
  <g transform="translate(290, 210)">
    <rect x="0" y="0" width="100" height="36" rx="18" fill="#818CF8"/>
    <text x="50" y="23" font-family="system-ui, sans-serif" font-size="13" font-weight="600" fill="white" text-anchor="middle">01 Init</text>
  </g>
  <g transform="translate(410, 210)">
    <rect x="0" y="0" width="100" height="36" rx="18" fill="#34D399"/>
    <text x="50" y="23" font-family="system-ui, sans-serif" font-size="13" font-weight="600" fill="white" text-anchor="middle">02 Plan</text>
  </g>
  <g transform="translate(530, 210)">
    <rect x="0" y="0" width="110" height="36" rx="18" fill="#60A5FA"/>
    <text x="55" y="23" font-family="system-ui, sans-serif" font-size="13" font-weight="600" fill="white" text-anchor="middle">03 Execute</text>
  </g>
  <g transform="translate(660, 210)">
    <rect x="0" y="0" width="115" height="36" rx="18" fill="#F472B6"/>
    <text x="57" y="23" font-family="system-ui, sans-serif" font-size="13" font-weight="600" fill="white" text-anchor="middle">04 Recover</text>
  </g>
  
  <!-- Feature highlights -->
  <g transform="translate(200, 290)">
    <text x="0" y="0" font-family="system-ui, sans-serif" font-size="14" fill="#7EE787">✓</text>
    <text x="20" y="0" font-family="system-ui, sans-serif" font-size="14" fill="#C9D1D9">Mandatory design system</text>
  </g>
  <g transform="translate(420, 290)">
    <text x="0" y="0" font-family="system-ui, sans-serif" font-size="14" fill="#7EE787">✓</text>
    <text x="20" y="0" font-family="system-ui, sans-serif" font-size="14" fill="#C9D1D9">Approval gates</text>
  </g>
  <g transform="translate(600, 290)">
    <text x="0" y="0" font-family="system-ui, sans-serif" font-size="14" fill="#7EE787">✓</text>
    <text x="20" y="0" font-family="system-ui, sans-serif" font-size="14" fill="#C9D1D9">Documentation-driven</text>
  </g>
  <g transform="translate(800, 290)">
    <text x="0" y="0" font-family="system-ui, sans-serif" font-size="14" fill="#7EE787">✓</text>
    <text x="20" y="0" font-family="system-ui, sans-serif" font-size="14" fill="#C9D1D9">Error recovery</text>
  </g>
  
  <!-- Compatible with -->
  <text x="600" y="345" font-family="system-ui, sans-serif" font-size="12" fill="#484F58" text-anchor="middle">Works with Cursor • Claude Code • Windsurf</text>
  
  <!-- BDIGITALIST -->
  <text x="600" y="380" font-family="system-ui, sans-serif" font-size="14" font-weight="600" fill="#6E7681" text-anchor="middle">BDIGITALIST</text>
</svg>


A structured workflow for AI-assisted development that separates planning from execution and enforces documentation-driven development.

## Philosophy

Most AI coding workflows fail because they:
1. Mix planning and execution in one context
2. Let the agent make decisions without documentation
3. Have no error memory or learning loop
4. Skip design systems, leading to inconsistent UI

This system fixes that with:
- **Four specialized agents** with distinct responsibilities
- **Documentation as state** — the `/Docs` folder is the source of truth
- **Mandatory design system** — no UI work without specifications
- **Approval gates** — human confirms each subtask before continuing
- **Error tracking** — issues are logged, solutions are preserved

## Quick Start

### 1. Install
Copy the `rules/` folder to your project:

```bash
cp -r rules/ your-project/.cursor/rules/
# or for Claude Code
cp -r rules/ your-project/.claude/rules/
```

### 2. Initialize a Project
```
@01-init.mdc

I want to build a task management app with user authentication,
team workspaces, and real-time collaboration.
```

### 3. Generate Documentation
After confirming scope:
```
@02-plan.mdc
```

### 4. Start Building
```
Start task 1.1
```

The execution agent (`03-execute.mdc`) is always on and will guide you through each task.

## The Four Agents

| Agent | File | Trigger | Purpose |
|-------|------|---------|---------|
| **Init** | `01-init.mdc` | Manual | Gather requirements, clarify scope |
| **Plan** | `02-plan.mdc` | Manual | Generate all project documentation |
| **Execute** | `03-execute.mdc` | Always On | Implement tasks, follow documentation |
| **Recover** | `04-recover.mdc` | Manual | Handle errors, rollbacks, handoffs |

### Agent Flow

```
User: "Build me a..."
         │
         ▼
┌─────────────────┐
│   01-init.mdc   │  Asks 5 clarifying questions
│   (Scope)       │  Outputs: /Docs/Scope.md
└────────┬────────┘
         │ User confirms scope
         ▼
┌─────────────────┐
│   02-plan.mdc   │  Researches tech, creates docs
│   (Plan)        │  Outputs: PRD, Tech, Structure, Design, Implementation, Issues
└────────┬────────┘
         │ User approves docs
         ▼
┌─────────────────┐
│  03-execute.mdc │  Works through tasks one by one
│  (Build)        │  Waits for approval after each
└────────┬────────┘
         │ On error or completion
         ▼
┌─────────────────┐
│  04-recover.mdc │  Handles problems, reviews progress
│  (Recover)      │  Manages rollbacks and handoffs
└─────────────────┘
```

## Generated Documentation

The planning agent creates six files in `/Docs/`:

| File | Purpose |
|------|---------|
| `Scope.md` | Confirmed project scope from init |
| `PRD.md` | Product requirements, user stories, acceptance criteria |
| `Tech.md` | Technology choices with rationale and doc links |
| `Structure.md` | Folder structure, naming conventions, module boundaries |
| `Design.md` | **Mandatory** design system: colors, typography, spacing, components |
| `Implementation.md` | Staged task breakdown with numbered subtasks |
| `Issues.md` | Bug tracking, decisions, blockers |

### Why Design.md is Mandatory

Without a design system:
- Every component looks different
- Colors and spacing are inconsistent
- Accessibility is an afterthought
- UI debt accumulates fast

With Design.md:
- Agent uses defined tokens
- Components follow specifications
- Consistency is enforced
- Accessibility requirements are explicit

## The Execution Loop

The `03-execute.mdc` agent follows this loop for every task:

```
1. READ    → Check Implementation.md for current task
2. CHECK   → Consult relevant docs (Structure, Design, Tech, Issues)
3. BUILD   → Implement the subtask
4. VERIFY  → Ensure it meets acceptance criteria
5. REPORT  → Show what was done, what changed
6. WAIT    → Get user approval before continuing
7. UPDATE  → Mark task complete in Implementation.md
8. NEXT    → Proceed to next subtask
```

### Approval Gates

After each subtask, the agent reports:

```
## Completed: 2.3.1 Create user registration endpoint

### Changes Made
- `backend/src/routes/auth.ts` — Added /register route
- `backend/src/controllers/auth.ts` — Registration logic
- `backend/src/models/user.ts` — User model

### Verification
- [x] Code runs without errors
- [x] Meets acceptance criteria
- [x] Follows project structure

Ready to proceed to 2.3.2: Create login endpoint?
Reply 'yes' to continue, or provide feedback.
```

## Document Reference Priority

When the agent needs to make a decision, it checks docs in this order:

1. **Issues.md** — Known problems, past decisions
2. **Implementation.md** — Current task requirements
3. **Structure.md** — Where to put files
4. **Design.md** — How UI should look
5. **Tech.md** — Technology patterns
6. **PRD.md** — Original requirements

## Error Handling

When errors occur:

1. Agent logs issue to `Issues.md`
2. Checks for similar past issues
3. Attempts one focused fix
4. If blocked, escalates to user with options

Example escalation:
```
## Blocked: 2.3.4 JWT token generation

**Error:** jsonwebtoken module not found
**Attempted:** npm install jsonwebtoken
**Issue:** Package.json in wrong directory

I need your input:
a) Move package.json to correct location
b) Initialize new package.json
c) Skip and continue
d) Other guidance
```

## Recovery Features

Invoke `@04-recover.mdc` for:

| Situation | Command |
|-----------|---------|
| Persistent error | "Help me fix this error" |
| Undo changes | "Rollback task 2.3" |
| Context full | "Create handoff summary" |
| Stage review | "Review stage 2" |
| Project complete | "Final review" |

### Context Handoff

When conversations get long, the recovery agent creates `/Docs/Handoff.md`:

```markdown
# Session Handoff

**Current Task:** 2.3.4 JWT implementation
**Status:** Blocked on dependency issue

## To Resume
Start new conversation:
"Read /Docs/Handoff.md and continue from task 2.3.4"
```

## Best Practices

### Do
- Let the init agent ask its questions — don't skip scope clarification
- Review generated docs before starting execution
- Approve tasks one at a time — this catches issues early
- Log issues even if you fix them quickly
- Use handoffs for long projects

### Don't
- Skip directly to coding without documentation
- Approve multiple tasks at once
- Ignore the design system for "quick" UI work
- Let errors accumulate without logging

## Customization

### Adjusting Task Granularity
Edit `02-plan.mdc` to change how detailed tasks are:
- For faster progress: Larger subtasks (hours of work each)
- For more control: Smaller subtasks (30-60 min each)

### Adding Tech Stack Preferences
Add a `/Docs/Preferences.md` file:
```markdown
# Tech Preferences
- Frontend: Always use React + TypeScript
- Styling: Tailwind CSS only
- Backend: Node.js + Express
- Database: PostgreSQL with Prisma
```
Reference it in `02-plan.mdc` to skip research for known preferences.

### Disabling Design System for Backend-Only
In `02-plan.mdc`, Design.md is generated for all projects. For pure backend/CLI projects, it becomes API documentation patterns instead of visual design tokens.

## Tool Compatibility

This system works with:
- **Cursor** — Place rules in `.cursor/rules/`
- **Claude Code** — Place rules in `.claude/rules/` or project root
- **Windsurf** — Place rules in project root
- **Other AI IDEs** — Copy rule content into system prompts

## File Structure

```
your-project/
├── .cursor/rules/           # or .claude/rules/
│   ├── 01-init.mdc
│   ├── 02-plan.mdc
│   ├── 03-execute.mdc
│   └── 04-recover.mdc
│
├── Docs/                    # Generated by agents
│   ├── Scope.md
│   ├── PRD.md
│   ├── Tech.md
│   ├── Structure.md
│   ├── Design.md
│   ├── Implementation.md
│   ├── Issues.md
│   └── Handoff.md          # Created during handoffs
│
├── frontend/                # Your code (structure from Structure.md)
├── backend/
└── shared/
```

## Comparison to Alternatives

| Feature | This System | ai-dev-tasks | Cursor-PM |
|---------|-------------|--------------|-----------|
| Agent separation | 4 specialized | 1 general | 1 general |
| Design system | Mandatory | Optional section | None |
| Tech research | Web search + docs | None | None |
| Error tracking | Dedicated Issues.md | None | None |
| Approval gates | Every subtask | Every subtask | None |
| Recovery/Rollback | Dedicated agent | None | None |
| Context handoff | Explicit protocol | None | None |
| Complexity | Medium | Low | Low |

## Contributing

This system is designed to be forked and customized. Key extension points:

- Add new document types to `02-plan.mdc`
- Customize task granularity
- Add domain-specific rules (e.g., mobile, ML)
- Create industry templates (SaaS, e-commerce)

## License

MIT — Use freely, attribution appreciated.

---

## Quick Reference

### Commands
| Say | Does |
|-----|------|
| `@01-init.mdc [description]` | Start new project |
| `@02-plan.mdc` | Generate documentation |
| `start` / `continue` / `next` | Begin/continue tasks |
| `do [X.Y.Z]` | Start specific task |
| `skip` | Skip current task |
| `status` | Show progress |
| `@04-recover.mdc` | Enter recovery mode |
| `rollback [X.Y.Z]` | Undo task |
| `handoff` | Create handoff summary |

### Document Priority
1. Issues.md → 2. Implementation.md → 3. Structure.md → 4. Design.md → 5. Tech.md → 6. PRD.md
