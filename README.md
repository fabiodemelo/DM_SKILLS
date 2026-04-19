# DM_SKILLS

A collection of reusable Claude AI skills for Demelos LLC projects. Each skill is a self-contained module that extends Claude's capabilities for specific workflows, writing styles, and domain expertise.

## What's Inside

- **human-writing** — A comprehensive writing style guide that produces clear, direct, human-sounding content without corporate jargon or AI-speak. Applies universally across all text generation tasks.

More skills coming soon.

## Installation

### Claude Code

Copy a skill folder into your Claude Code skills directory:

```bash
cp -r skills/human-writing ~/.claude/skills/human-writing
```

Or clone the entire library:

```bash
git clone https://github.com/fabiodemelo/DM_SKILLS.git
cp -r DM_SKILLS/skills/* ~/.claude/skills/
```

### Cowork

```bash
cp -r skills/human-writing ~/.cowork/skills/human-writing
```

Or with the full library:

```bash
git clone https://github.com/fabiodemelo/DM_SKILLS.git
cp -r DM_SKILLS/skills/* ~/.cowork/skills/
```

### Claude.ai

Skills in this library are designed for Claude Code and Cowork. To use them in Claude.ai, reference the skill documentation directly or apply the rules manually.

## How Skills Work

Each skill is a folder containing:

- `SKILL.md` — The skill definition with triggering rules, instructions, and guidelines
- `README.md` — Documentation specific to that skill, including examples and use cases

When a skill is installed in `~/.claude/skills/` or `~/.cowork/skills/`, Claude automatically loads it and uses it based on the triggering description.

## Using a Skill

Once installed, skills are activated automatically based on their description. For example, the `human-writing` skill triggers on all text generation tasks and applies the writing rules without you having to ask.

You can also explicitly reference a skill:

```
Use the human-writing skill to rewrite this paragraph.
```

## Contributing

To add a new skill to this library:

1. Create a new folder under `skills/`
2. Write your `SKILL.md` with proper frontmatter (name, description)
3. Add a `README.md` with examples and detailed documentation
4. Test it in Claude Code or Cowork
5. Submit a pull request or commit directly

See `CONTRIBUTING.md` for detailed guidelines.

## Author

Fabio DeMelo  
demelos.com - AI experts  
https://demelos.com

---

Last updated: April 2026
