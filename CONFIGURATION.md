# Configuration Guide

How to install, configure, and customize the Agentic Dev System.

---

## Installation

### For Cursor

```bash
# Clone or download the rules
git clone https://github.com/your-username/agentic-dev-system.git

# Copy rules to your project
mkdir -p your-project/.cursor/rules
cp agentic-dev-system/rules/*.mdc your-project/.cursor/rules/

# Or copy to global Cursor rules
cp agentic-dev-system/rules/*.mdc ~/.cursor/rules/
```

**Verify Installation:**
1. Open your project in Cursor
2. Open Agent chat
3. Type `@01-init.mdc` — it should autocomplete

### For Claude Code

```bash
# Copy rules to your project
mkdir -p your-project/.claude/rules
cp agentic-dev-system/rules/*.mdc your-project/.claude/rules/

# Or add to CLAUDE.md
cat >> your-project/CLAUDE.md << 'EOF'

## Agentic Workflow
Use these rules for structured development:
- @01-init.mdc - Start new project/feature
- @02-plan.mdc - Generate documentation
- 03-execute.mdc is always active
- @04-recover.mdc - Handle errors/rollbacks
EOF
```

### For Other Tools

Copy the contents of each `.mdc` file into your tool's system prompt or rules configuration. The key parts are:

1. `03-execute.mdc` should be always active
2. Other rules should be invokable on demand

---

## Configuration Options

### Changing Default Project Type

By default, projects are assumed to be **Full-stack**. To change this, edit `01-init.mdc`:

```markdown
# Line 27 - Change default:
Default assumption: **Frontend-only** unless explicitly stated otherwise.
```

### Adjusting Clarifying Questions

To ask more or fewer questions, edit `01-init.mdc`:

```markdown
# Line 32 - Change count:
Ask exactly **3 questions** to fill gaps in understanding.
```

To change question categories, modify the **Required question categories** section.

### Customizing Tech Stack

To set default technology preferences, create `/Docs/Preferences.md`:

```markdown
# Tech Preferences

## Always Use
- TypeScript (strict mode)
- React 18+ with hooks
- Tailwind CSS
- PostgreSQL

## Never Use
- Class components
- CSS-in-JS (styled-components, emotion)
- MongoDB

## Prefer
- pnpm over npm
- Prisma over TypeORM
- Vitest over Jest
```

Then modify `02-plan.mdc` to check this file before researching:

```markdown
### Phase 2: Research Tech Stack
1. Check if `/Docs/Preferences.md` exists
2. If yes, use specified technologies without research
3. If no, search the web for current best practices
```

### Changing Task Granularity

To make tasks larger (faster progress, less control):

Edit `02-plan.mdc`, Phase 3.5:

```markdown
### Task Granularity Guidelines
- Each subtask should represent 2-4 hours of work
- Group related changes into single subtasks
- Focus on deliverable milestones
```

To make tasks smaller (more control, slower progress):

```markdown
### Task Granularity Guidelines
- Each subtask should represent 30-60 minutes of work
- Separate distinct concerns into individual subtasks
- Err on the side of more granular tasks
```

### Modifying Design System Template

To customize the default design system structure, edit the Design.md template in `02-plan.mdc`, section 3.4.

**Adding a section:**
```markdown
## Animation Library
- Library: Framer Motion
- Default duration: 200ms
- Easing: ease-out
```

**Removing a section:**
Delete the relevant section from the template. Note: Color, Typography, and Spacing should always be present.

### Adjusting Approval Gates

To require approval only at stage boundaries (not every subtask):

Edit `03-execute.mdc`:

```markdown
### After Completing a Subtask

# Change this:
**Ready to proceed to [X.Y.Z+1]: [Next Task Name]?**
Reply 'yes' to continue, or provide feedback.

# To this:
[Proceed automatically to next subtask]

### After Completing a Stage
# Keep approval here
**Stage [N] complete. Ready for Stage [N+1]?**
```

To remove approval gates entirely (not recommended):

```markdown
### After Completing a Subtask
- Update Implementation.md
- Proceed immediately to next subtask
- [No approval wait]
```

---

## Project-Specific Customization

### Adding Domain Rules

For specialized domains, create additional rule files:

**Example: E-commerce Rules**

`.cursor/rules/domain-ecommerce.mdc`:

```markdown
---
alwaysApply: true
description: "E-commerce specific patterns and requirements"
globs: ["**/shop/**", "**/cart/**", "**/checkout/**"]
---

# E-commerce Development Rules

## Required Patterns
- All prices must use decimal/money types, never floats
- Cart state must be persisted to database, not just session
- Checkout must handle payment failures gracefully

## Security Requirements
- PCI compliance for payment handling
- No credit card data in logs
- HTTPS required for all checkout pages

## Performance Requirements
- Product pages must load in <2 seconds
- Cart updates must be optimistic with rollback
```

**Example: Healthcare Rules**

`.cursor/rules/domain-healthcare.mdc`:

```markdown
---
alwaysApply: true
description: "Healthcare/HIPAA compliance requirements"
globs: ["**/patient/**", "**/medical/**", "**/health/**"]
---

# Healthcare Development Rules

## HIPAA Compliance
- All PHI must be encrypted at rest and in transit
- Audit logging required for all data access
- Minimum necessary access principle

## Data Handling
- Never log patient identifiers
- Use referential IDs, not SSN/MRN in URLs
- Session timeout: 15 minutes maximum
```

### Creating Project Templates

For common project types, create pre-filled Scope.md files:

**SaaS Template:**

`templates/scope-saas.md`:

```markdown
## Scope Summary

**Project Name:** [NAME]
**Type:** Full-stack

### What We're Building
A SaaS application with multi-tenant architecture, subscription billing,
and user management.

### Core Requirements
1. Multi-tenant data isolation
2. Subscription management (Stripe)
3. User authentication with roles
4. Admin dashboard
5. Usage analytics

### Standard Exclusions
- Mobile apps (web-responsive only)
- White-labeling
- Custom integrations

### Tech Constraints
- Must support Stripe for payments
- Must handle team/organization hierarchy
```

---

## Integration with CI/CD

### Pre-commit Validation

Add a hook to validate documentation exists:

`.husky/pre-commit`:

```bash
#!/bin/bash

# Ensure documentation exists before commit
REQUIRED_DOCS=(
  "Docs/PRD.md"
  "Docs/Tech.md"
  "Docs/Structure.md"
  "Docs/Design.md"
  "Docs/Implementation.md"
)

for doc in "${REQUIRED_DOCS[@]}"; do
  if [ ! -f "$doc" ]; then
    echo "Error: Missing required documentation: $doc"
    echo "Run @02-plan.mdc to generate documentation first."
    exit 1
  fi
done
```

### GitHub Actions Integration

`.github/workflows/docs-check.yml`:

```yaml
name: Documentation Check

on: [push, pull_request]

jobs:
  check-docs:
    runs-on: ubuntu-latest
    steps:
      - uses: actions/checkout@v4
      
      - name: Check required documentation
        run: |
          for doc in PRD Tech Structure Design Implementation; do
            if [ ! -f "Docs/${doc}.md" ]; then
              echo "::error::Missing Docs/${doc}.md"
              exit 1
            fi
          done
          
      - name: Check Implementation progress
        run: |
          # Count incomplete tasks
          incomplete=$(grep -c '\- \[ \]' Docs/Implementation.md || true)
          echo "Incomplete tasks: $incomplete"
          
          # Fail if too many incomplete in main branch
          if [ "${{ github.ref }}" = "refs/heads/main" ] && [ "$incomplete" -gt 0 ]; then
            echo "::warning::$incomplete tasks still incomplete"
          fi
```

---

## Troubleshooting

### Rule Not Loading

**Symptom:** Agent doesn't recognize `@01-init.mdc`

**Solutions:**
1. Check file is in correct location (`.cursor/rules/` or `.claude/rules/`)
2. Verify file extension is `.mdc` not `.md`
3. Restart IDE
4. Check frontmatter is valid YAML

### Always-On Rule Not Active

**Symptom:** Execution agent doesn't follow documentation

**Solutions:**
1. Verify `alwaysApply: true` in `03-execute.mdc` frontmatter
2. Check `globs` pattern matches current files
3. Ensure documentation files exist in `/Docs/`

### Web Search Not Working

**Symptom:** Planning agent doesn't research technologies

**Solutions:**
1. Verify your tool supports web search
2. Check network/firewall settings
3. Add explicit instruction: "Search the web for [topic] best practices"

### Design System Being Ignored

**Symptom:** UI code uses hard-coded values instead of design tokens

**Solutions:**
1. Verify `/Docs/Design.md` exists and is complete
2. Add reminder in `03-execute.mdc`: "Check Design.md before ANY UI implementation"
3. Review the task — it may not be flagged as UI work

### Context Getting Lost

**Symptom:** Agent forgets earlier conversation, makes inconsistent choices

**Solutions:**
1. Use `handoff` command to save state
2. Start new conversation with: "Read /Docs/Handoff.md and continue"
3. Consider using `/compact` if available
4. Break work into smaller sessions

---

## Upgrading

When updating the rule files:

1. **Backup current rules**
   ```bash
   cp -r .cursor/rules .cursor/rules.backup
   ```

2. **Copy new rules**
   ```bash
   cp agentic-dev-system/rules/*.mdc .cursor/rules/
   ```

3. **Preserve customizations**
   - Diff old and new files
   - Re-apply any custom changes
   
4. **Verify documentation compatibility**
   - Existing `/Docs/` files should still work
   - Run through one task to verify

---

## Disabling the System

To temporarily disable:

1. Rename `03-execute.mdc` to `03-execute.mdc.disabled`
2. Agent will behave normally without structured workflow

To permanently remove:

```bash
rm -rf .cursor/rules/
rm -rf Docs/
```

Note: Removing `/Docs/` deletes all project documentation. Backup first.
