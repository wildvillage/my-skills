---
name: skill-creator
description: Guide for creating effective skills. This skill should be used when users want to create a new skill (or update an existing skill) that extends Agent's capabilities with specialized knowledge, workflows, or tool integrations.
---

# Skill Creator

## Overview

This Skill documents a repeatable, token-efficient process for turning repeatable procedures, documentation, codebases, or SOPs into Claude Skills. It combines high-level design principles (concise context, progressive disclosure, degrees of freedom) with a concrete generator workflow that supports common input types (GitHub repo, article, SOP, idea). Use this Skill whenever you want to create or update a Skill so Claude can reliably handle a domain-specific task.

## Purpose

Provide an actionable, developer-friendly template and workflow to:

* Extract purpose, triggers, workflows, examples, and resources from source material.
* Produce a validated `SKILL.md` and optional `scripts/`, `references/`, and `assets/` that follow best practices.
* Write the skill to dual locations for local development (`.agent/skills/`) and runtime metadata (`.claude/skills/`).
* Package, test, and iterate on the Skill.

## Core principles (design constraints)

1. **Concise is key.** Only add context Claude cannot already infer. Keep SKILL.md focused; move bulk references to `references/`.
2. **Progressive disclosure.** Three levels of loading:

   * Metadata (frontmatter `name` + `description`) — always present and primary trigger.
   * SKILL.md body — loaded after trigger (<5k words recommended).
   * Bundled resources (`scripts/`, `references/`, `assets/`) — loaded/executed only when needed.
3. **Degrees of freedom.** Choose high/medium/low freedom forms:

   * High freedom: textual heuristics and options.
   * Medium freedom: pseudocode or parameterized scripts.
   * Low freedom: deterministic scripts for fragile operations.
4. **Avoid duplication.** Put persistent or large reference content in `references/`. Keep SKILL.md as the operational guide.
5. **Imperative voice.** Write instructions as commands (use, run, create, verify).

## When to use this Skill (frontmatter is the trigger)

* Convert a GitHub repo, article, SOP, or idea into a Skill.
* Standardize repeated Claude tasks into reusable Skills.
* Provide Claude with deterministic scripts or company-specific references.
* Package skills for distribution and local runtime consumption.

# Anatomy of a Skill (what to include)

```
skill-name/
├── SKILL.md             # required — full instructions and workflows
├── scripts/             # optional — deterministic executable code
├── references/          # optional — domain docs and schemas
└── assets/              # optional — templates, icons, PPTs, fonts
```

### SKILL.md (required)

* YAML frontmatter with only:

  * `name`: skill name (lowercase letters, numbers, hyphens).
  * `description`: what it does + triggers/contexts (this is the primary trigger Claude uses).
* Body: concise workflows, quick start, examples, best practices, and references to bundled files.

### Scripts

* Store code for repetitive, error-prone, or deterministic tasks (e.g., PDF rotate, file transforms, packaging).
* Prefer parameterized scripts with minimal assumptions.
* Test representative scripts locally.

### References

* Store long or sensitive documentation (schemas, API docs, policies).
* For files >100 lines, include a table of contents at top.
* Reference files from SKILL.md with clear "when to load" cues.

### Assets

* Include templates and files used directly in outputs (PPT templates, logos, starter projects).
* Don't load into Claude context unless necessary.

# Skill creation workflow (merged & adapted)

Follow these steps to create a high-quality Skill from source material.

## Step 1 — Analyze the input (determine strategy)

Determine input type and choose extraction approach:

|        Input Type | Action                                                                                                          |
| ----------------: | --------------------------------------------------------------------------------------------------------------- |
|        GitHub URL | Inspect README, docs, package manifests, and code structure. Extract install/setup, common workflows, key APIs. |
|  Article/URL/Text | Extract thesis, stepwise methods, mental models, and recommended tools.                                         |
| SOP / Process doc | Break into discrete steps, decision points, checks, edge cases.                                                 |
|    Idea / Concept | Clarify missing requirements, propose structure, and present a minimal viable skill.                            |

**Action:** Identify concrete example requests that should trigger the Skill.

## Step 2 — Extract core information (what becomes the SKILL.md)

From the source material, extract:

1. **Purpose** — problem solved.
2. **Trigger conditions** — specific user utterances and contexts.
3. **Key workflows** — step-by-step operational guidance.
4. **Best practices** — guardrails and anti-patterns.
5. **Examples** — realistic examples and example inputs/outputs.
6. **Resources** — scripts, reference files, and assets to bundle.

Document the above concisely; create a SKILL.md skeleton from them.

## Step 3 — Generate SKILL.md structure (template)

Use this canonical SKILL.md layout:

```
---
name: skill-name
description: [One-line: what it does + when to use it]
---

# Skill Name

## Overview
[Short: 2-4 sentences]

## Quick start
[Commands or fastest way to use the skill]

## Workflows
### Workflow 1: [Name]
1. Step one
2. Step two
...

## Best practices
- Bullet points

## Examples
### Example 1: [Scenario]
[Concrete example with expected output]

## Resources
- references/REFERENCE.md
- scripts/script.py
- assets/template.pptx

## Validation checklist
- name validity
- description completeness
- examples present
- etc.
```

## Step 4 — Validate content before writing

Verify the following before writing files:

* [ ] `name` uses only lowercase letters, numbers, hyphens; max 64 chars.
* [ ] `description` contains purpose and trigger conditions; keep ≤1024 chars.
* [ ] SKILL.md body is concise, actionable, and imperative.
* [ ] Examples are concrete and realistic.
* [ ] No XML tags or reserved words in frontmatter.
* [ ] References and scripts are referenced, not duplicated in SKILL.md.

## Step 5 — Dual-location write pattern (development vs runtime)

Write the Skill to two locations for local editing and runtime metadata:

1. Full content (developer copy):

```
.agent/skills/{skill-name}/SKILL.md
```

2. Runtime metadata (lightweight pointer):

```
.claude/skills/{skill-name}/SKILL.md
---
name: skill-name
description: [Same as .agent version]
---

# {skill-name}

This skill's full content is located at:
`.agent/skills/{skill-name}/SKILL.md`
```

**Purpose:** Keep autocomplete/trigger information lightweight in runtime contexts while preserving the full authoring copy for developers.

## Step 6 — Update project README (catalog)

When adding a skill to a codebase, update the skills table:

1. Read `README.md`.
2. Locate the `## 已有 Skills` section.
3. Add a row using the canonical format:

```
| [skill-name](.agent/skills/skill-name/SKILL.md) | [Short Chinese description / summary] |
```

4. Preserve footer notes about auto-update.

## Step 7 — Package and validate (optional)

Run packaging and validation tools:

```bash
# initialize a skeleton
scripts/init_skill.py <skill-name> --path <output-directory>

# package (validates and creates a .skill zip)
scripts/package_skill.py <path/to/skill-folder> ./dist
```

Packaging checks:

* Valid YAML frontmatter
* Proper directory structure
* Naming conventions
* References exist and are reachable

# Command reference (examples to automate common tasks)

Use these shell snippets as templates when writing skills programmatically.

```bash
# Create skill directories
mkdir -p .agent/skills/{skill-name}
mkdir -p .claude/skills/{skill-name}

# Write the .agent version (full SKILL.md)
cat > .agent/skills/{skill-name}/SKILL.md << 'EOF'
[full content goes here]
EOF

# Write the .claude version (metadata + pointer)
cat > .claude/skills/{skill-name}/SKILL.md << 'EOF'
---
name: {skill-name}
description: [One-line description]
---

# {skill-name}

This skill's full content is located at:
`.agent/skills/{skill-name}/SKILL.md`
EOF
```

# Input-specific processing guidelines (detailed)

## GitHub repository analysis

When given a GitHub URL:

1. Fetch repo metadata.
2. Read `README.md`, docs/, and CHANGELOG if present.
3. Inspect manifests (`package.json`, `pyproject.toml`, `requirements.txt`, `go.mod`) for dependencies and runtime assumptions.
4. Map top-level directories to components/modules.
5. Extract:

   * Purpose and main features.
   * Setup and install steps.
   * Common developer workflows (build/test/run).
   * Public API endpoints or exported functions.
6. Produce workflows that help Claude assist users with:

   * Setting up the project.
   * Running common tasks.
   * Locating the right modules for a given problem.

**Deliverable:** A SKILL.md with Quick Start, Workflows for setup, debugging, and contribution patterns, and references to scripts that automate common tasks.

## Article/document analysis

When given an article or doc:

1. Extract main thesis and actionable claims.
2. Identify unique frameworks, decision heuristics, or checklists.
3. Convert methods into workflows with stepwise instructions.
4. Provide short examples demonstrating application of the method.

**Deliverable:** A SKILL.md capturing the method, when to use it, and 2–3 short examples.

## SOP / Process conversion

When converting SOPs:

1. Break operations into discrete steps and decision branches.
2. Add validation checkpoints and error-handling guidance.
3. Include edge cases and examples for each major path.
4. For fragile operations, provide deterministic scripts in `scripts/`.

**Deliverable:** A SKILL.md with an explicit Workflow section and a `references/SOP.md` for long policy text.

## Idea / Concept development

When given a vague idea:

1. Clarify the problem and intended users.
2. Propose 2–3 concrete trigger phrases and a minimal feature set.
3. Draft a skeleton SKILL.md and a list of small scripts/assets that would increase usefulness.
4. If clarifications are needed, list the specific questions (limit to the top 3).

**Deliverable:** A draft SKILL.md plus a short implementation plan.

# Validation checklist (final review before committing)

* [ ] Frontmatter: `name` and `description` only, description contains purpose + triggers.
* [ ] `name` conforms to conventions (lowercase, hyphens, ≤64 chars).
* [ ] Body: concise quick start + workflows + examples.
* [ ] References split out of SKILL.md for large content (>500 lines).
* [ ] Scripts present for fragile or repetitive operations and tested.
* [ ] README index updated if appropriate.
* [ ] Packaging script/run passes validation.

# Examples (concrete, copyable)

## Example: code-review (Skeletal)

```markdown
---
name: code-review
description: Systematic code review covering functionality, tests, security, and style. Use when user requests code review or PR review.
---

# Code Review

## Quick start
1. Ask for PR link and test run logs.
2. Run tests locally or via CI artifact.
3. Apply checklist below.

## Workflow
1. Functionality: verify requirements and edge cases.
2. Tests: ensure tests added & coverage updated.
3. Security: check sanitization and secrets.
4. Style: run formatter & linters.

## Examples
- Input: "Please review PR #123 for SQL injection risks"
- Output: Stepwise review notes and suggested fixes.
```

## Example: skill-generator (integrated rules)

Use when you want to auto-generate a skill from a repo, article, or SOP. Follow the "GitHub repository analysis" and "Dual-location write pattern" steps above to produce `.agent/skills/{skill}/SKILL.md` and `.claude/skills/{skill}/SKILL.md`.

# Error handling (policy)

| Error                  | Resolution                                                                     |
| ---------------------- | ------------------------------------------------------------------------------ |
| Invalid skill name     | Return a suggested valid name (lowercase, hyphens).                            |
| Missing directories    | Create `.agent/skills/` and `.claude/skills/` or instruct user to create them. |
| Source inaccessible    | Return specific error and request alternate source or credentials.             |
| Ambiguous requirements | Present a concise list of the top 3 clarifying questions.                      |

# Limitations & security notes

* Cannot access private repositories without credentials. If required, request a token and explain scope.
* Network operations are network-dependent.
* Large repos may require targeted module analysis — prefer scoping to a subdir.
* Do not embed secrets in SKILL.md or bundled resources. Store credentials out of band.

# Iteration & maintenance

1. Use Skill in real tasks; capture failure cases.
2. Update SKILL.md with missing flows or clearer examples.
3. Add or refine scripts when repetitive manual steps arise.
4. Repackage and bump version if distributed.

# Quick templates & utilities

* **SKILL.md skeleton** (copy/paste block provided in Step 3).
* **README update snippet** (copy/paste in Command reference).
* **Validation script pseudocode** (low-freedom script recommended for CI):

```python
# pseudocode: validate_skill.py
# 1. parse frontmatter: ensure name & description present
# 2. validate name regex: ^[a-z0-9-]{1,64}$
# 3. check description length <= 1024
# 4. ensure SKILL.md body length < 5000 lines (warning if >)
# 5. ensure referenced scripts/references/assets exist
```

# Final notes (practical guidance)

* Treat SKILL.md as an operational SOP for Claude, not as user documentation. Keep it action-focused.
* Prefer concrete examples and quick start blocks — they reduce repeated clarifying questions.
* Move long schemas, API docs, and sensitive policy text to `references/` and reference them explicitly.
* When in doubt about a design choice, default to **smaller SKILL.md + more references**: it preserves the context window and improves runtime performance.

---

If you want, I will:

1. Generate a filled SKILL.md for a concrete source (give me a GitHub repo URL, article, or SOP).
2. Produce the `.agent/` and `.claude/` file pairs (content only).
3. Create the skeleton `scripts/init_skill.py` and `scripts/package_skill.py` templates for local automation.

Tell me which one to do next and provide the source (or say "draft from scratch" for a hypothetical example).
