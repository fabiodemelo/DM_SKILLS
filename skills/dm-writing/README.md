# dm-writing

The demelos writing system. One skill for voice, banned words, rhythm, what never to publish about how our work is done, and consistency across surfaces. Replaces `human-writing`.

## What's inside

- `SKILL.md`: the always-on core. Voice, word bans, punctuation, principles, mechanical checks, workflow, cheat sheet.
- `references/channels.md`: website, articles, LinkedIn, replies, email, app UI strings, case studies, decks.
- `references/protection.md`: the boundary test (what never to publish) and the locked-facts consistency rules.
- `references/project-template.md`: copy into a project as `WRITING.md` to lock names, prices, counts, protected elements and retired terms.

The references load only when the task needs them, so a quick email doesn't pull in the whole system.

## When it triggers

On any text generation: articles, emails, website copy, LinkedIn, docs, UI strings, decks, case studies, code comments.

## Examples

### Before (AI-speak)

"In the world of software development, it's important to note that the paradigm of microservices has revolutionized how we architect systems."

### After

"Microservices changed how we build software. Instead of one giant system, you break things into independent pieces that talk to each other."

### Before (gives away the method)

"Our agent scores each caller on a 0-100 intent model, then routes anyone above 70 to the booking flow."

### After (outcome only)

"Callers who are ready to book get booked. Everyone else reaches a person."

## Installation

### Claude Code

```bash
cp -r skills/dm-writing ~/.claude/skills/dm-writing
```

### Cowork

```bash
cp -r skills/dm-writing ~/.cowork/skills/dm-writing
```

## Author

Fabio DeMelo, demelos.com
