---
name: skill-generator
description: Generate new Agent Skills from various sources (GitHub projects, articles, SOPs, workflows). Use when user requests to create, design, or generate a skill based on existing content, documentation, or process descriptions.
---

# Skill Generator

## Overview

Generate Agent Skills (SKILL.md files) by analyzing source content and extracting structured instructions, workflows, and best practices. This skill creates skills in dual locations:
- `.agent/skills/{skill-name}/SKILL.md` - Full content with instructions
- `.claude/skills/{skill-name}/SKILL.md` - Metadata only, referencing the .agent version

## Prerequisites

- Project root must have `.agent/skills/` and `.claude/skills/` directories
- Output skill name must follow naming convention: lowercase letters, numbers, hyphens only

## Workflow

### Step 1: Analyze Input

Determine the input type and apply appropriate extraction strategy:

| Input Type | Action |
|------------|--------|
| **GitHub URL** | Clone or fetch repo, analyze README, docs, code structure |
| **Article URL/Text** | Extract key concepts, workflows, actionable guidelines |
| **SOP/Process** | Convert steps into structured instructions with examples |
| **Idea/Concept** | Clarify requirements, ask missing details, generate structure |

### Step 2: Extract Core Information

From the source, identify and extract:

1. **Purpose** - What problem does this skill solve?
2. **Trigger Conditions** - When should Claude use this skill?
3. **Key Workflows** - Step-by-step procedures
4. **Best Practices** - Guidelines and patterns
5. **Examples** - Concrete usage scenarios
6. **Resources** - Related files, scripts, references needed

### Step 3: Generate SKILL.md Structure

```markdown
---
name: skill-name
description: [What it does + when to use it]
---

# Skill Name

## Overview
[Brief introduction]

## Quick Start
[Fastest way to use this skill]

## Workflows
### Workflow 1: [Name]
1. Step one
2. Step two
...

### Workflow 2: [Name]
...

## Best Practices
- Practice one
- Practice two

## Examples
### Example 1: [Scenario]
[Concrete example]

## Resources
- [Optional] FORMS.md - Forms and templates
- [Optional] REFERENCE.md - Detailed reference
- [Optional] scripts/ - Utility scripts
```

### Step 4: Validate Skill Content

Before writing, verify:

- [ ] `name` follows convention (lowercase, hyphens, max 64 chars)
- [ ] `description` includes purpose + trigger conditions (max 1024 chars)
- [ ] Instructions are clear and actionable
- [ ] Examples are concrete and realistic
- [ ] No XML tags in metadata fields
- [ ] No reserved words ("anthropic", "claude") in name

### Step 5: Write to Dual Locations

1. **Create .agent/skills/{skill-name}/SKILL.md** with full content
2. **Create .claude/skills/{skill-name}/SKILL.md** with metadata + reference:

```markdown
---
name: skill-name
description: [Same as .agent version]
---

# {skill-name}

This skill's full content is located at:
`.agent/skills/{skill-name}/SKILL.md`

To view or edit the complete instructions, refer to the file above.
```

### Step 6: Update README.md

Add the new skill to the project README.md skills table:

1. Read the current README.md
2. Locate the `## 已有 Skills` table
3. Add a new row following the table format:

```markdown
| [skill-name](.agent/skills/skill-name/) | [中文说明] |
```

4. Preserve the footer note about auto-update

## Command Reference

### Available Bash Commands

```bash
# Create new skill directory structure
mkdir -p .agent/skills/{skill-name}
mkdir -p .claude/skills/{skill-name}

# Write .agent version (full content)
cat > .agent/skills/{skill-name}/SKILL.md << 'EOF'
[full content]
EOF

# Write .claude version (metadata + reference)
cat > .claude/skills/{skill-name}/SKILL.md << 'EOF'
[metadata + reference]
EOF
```

## Input Processing Guidelines

### GitHub Repository Analysis

When given a GitHub URL:

1. Fetch repository information
2. Read README.md and documentation files
3. Analyze package.json, requirements.txt, or similar for dependencies
4. Examine main source code structure
5. Extract:
   - Project purpose and goals
   - Setup/installation procedures
   - Common workflows
   - API endpoints or key functions
   - Testing procedures

Generate a skill that helps Claude work with this codebase effectively.

### Article/Document Analysis

When given an article or document:

1. Extract main thesis or key concepts
2. Identify actionable steps or methodologies
3. Capture frameworks or mental models presented
4. Note any tools, techniques, or best practices
5. Convert to procedural guidance Claude can follow

### SOP/Process Conversion

When given a standard operating procedure:

1. Break down into discrete steps
2. Identify decision points and branches
3. Add validation checkpoints
4. Include edge cases and error handling
5. Add examples for each major step

### Idea/Concept Development

When given a vague idea or concept:

1. **Ask clarifying questions**:
   - What problem does this solve?
   - Who will use this skill?
   - What are the key scenarios?
   - Are there existing examples to reference?
2. Propose a structure for feedback
3. Iterate based on user input

## Examples

### Example 1: GitHub Repository

**Input**: "Create a skill for https://github.com/anthropics/anthropic-sdk-typescript"

**Output Actions**:
1. Fetch repo contents
2. Analyze SDK structure (clients, models, streaming)
3. Extract common patterns (message creation, tool use, streaming responses)
4. Generate skill with workflows for:
   - Setting up the SDK
   - Creating messages
   - Using tools
   - Handling streaming
5. Write to `.agent/skills/anthropic-typescript-sdk/` and `.claude/skills/anthropic-typescript-sdk/`

### Example 2: Process Description

**Input**: "Create a skill for code review process: 1) Check functionality, 2) Verify tests, 3) Review security, 4) Check style"

**Output**:
```markdown
---
name: code-review
description: Systematic code review covering functionality, tests, security, and style. Use when user requests code review or PR review.
---

# Code Review

## Workflow
1. **Functionality Check**
   - Verify requirements are met
   - Check edge cases
2. **Test Verification**
   - Ensure new tests added
   - Check test coverage
3. **Security Review**
   - Check for injection vulnerabilities
   - Validate input sanitization
4. **Style Check**
   - Verify code follows project conventions
   - Check naming and formatting
...
```

## Error Handling

| Error | Resolution |
|-------|------------|
| Invalid skill name | Prompt user for valid name (lowercase, hyphens) |
| Missing directories | Create `.agent/skills/` and `.claude/skills/` first |
| Source inaccessible | Inform user and request alternative source |
| Unclear requirements | Ask specific clarifying questions |

## Limitations

- Cannot access private GitHub repos without authentication
- Network-dependent for remote sources
- Generated quality depends on source clarity
- Large repositories may require focused analysis (e.g., specific modules)

## Notes

- Always preserve user's intent when converting to skill format
- If source contains ambiguities, make them explicit in the generated skill
- Generated skills should be reviewed and refined by users
- Consider creating companion resources (FORMS.md, scripts/) for complex skills
