# Contributing to DM_SKILLS

This is a living skill library. New skills are added frequently. Here's how to add one.

## Skill Structure

Each skill must have this structure:

```
skills/skill-name/
├── SKILL.md          (required)
├── README.md         (required)
└── examples/         (optional)
    ├── example1.txt
    └── example2.txt
```

## Creating a New Skill

### 1. Create the Skill Folder

```bash
mkdir -p skills/your-skill-name
```

### 2. Write SKILL.md

This is the core skill definition. It must have YAML frontmatter:

```yaml
---
name: your-skill-name
description: What this skill does and when it triggers. Be explicit about triggering contexts.
---

# Skill Title

Your skill content here. Include:
- Clear instructions
- Rules and guidelines
- Examples if helpful
- Any special notes

```

**Important**: Make the description "pushy" — include explicit trigger contexts so Claude actually uses the skill when it should.

### 3. Write README.md

A skill-specific README. Include:

- What the skill does
- When to use it
- Examples of input and output
- Installation notes if special setup is needed
- Author info

Template:

```markdown
# Your Skill Name

Brief description of what it does.

## What It Does

Detailed explanation.

## When to Use

Specific contexts and examples.

## Examples

Show input and output.

## Installation

```bash
cp -r skills/your-skill-name ~/.claude/skills/your-skill-name
```

## Author

Your name and link.
```

### 4. Test It

Install locally and test in Claude Code or Cowork:

```bash
cp -r skills/your-skill-name ~/.claude/skills/your-skill-name
```

Run a few test cases to make sure it actually triggers and works.

### 5. Add Examples (Optional)

If your skill benefits from concrete examples, add them to `skills/your-skill-name/examples/`. Each file can be a test case or sample input/output.

### 6. Commit and Push

```bash
cd DM_SKILLS
git add skills/your-skill-name/
git commit -m "Add your-skill-name skill"
git push origin main
```

## Naming Conventions

- Skill folder names: lowercase with hyphens (e.g., `my-skill-name`)
- SKILL.md frontmatter `name` field: matches folder name
- Be descriptive. `api-design-guide` is better than `api-guide`.

## Skill Quality Checklist

- [ ] SKILL.md has proper YAML frontmatter
- [ ] Description clearly states when the skill triggers
- [ ] README.md exists and is informative
- [ ] Skill is tested in Claude Code or Cowork
- [ ] No hardcoded paths or system-specific details
- [ ] Author info is included in SKILL.md (comment at top)

## Questions?

If you're unsure about something, look at existing skills in the library for examples.

---

DM_SKILLS Library
Fabio DeMelo
demelos.com - AI experts
https://www.demelos.com
