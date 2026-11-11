# refine-prompt

An [Agent Skill](https://agentskills.io) that analyzes your draft prompt, asks only the questions that matter, and rewrites it **before** the model executes it — so the request arrives in its best form.

```
/refine-prompt add a pricing page to the site, make it quick
```

The skill:

1. Checks the draft for goal, context, constraints and trade-offs, success criteria, output contract, out-of-scope items, missing tools, and prompt hygiene.
2. Asks at most 3 questions — only the ones that change the outcome; reasonable defaults are stated as assumptions instead.
3. Shows a rewritten prompt (XML-structured when it has several parts) with a short "what changed" note.
4. Iterates on your feedback (up to 3 rounds), then **Run**, **Edit** or **Save** to `prompts/<slug>.md`.

Nothing is executed until you approve. Drafts stay in the language you wrote them in.

## Install

**Claude Code (plugin marketplace)**

```
/plugin marketplace add a-erta/refine-prompt
/plugin install refine-prompt@refine-prompt
```

**Any Agent Skills–compatible tool (Claude Code, Codex, Cursor, …)**

```
npx skills add a-erta/refine-prompt
```

**Manual**

Copy `skills/refine-prompt/` into `.claude/skills/` (per project) or `~/.claude/skills/` (all projects).

## Background

The checklist follows the principles in Anthropic's talk [*The prompting playbook*](https://www.youtube.com/watch?v=G2B0YWuJUgI): structure with XML, give both sides of every trade-off, replace "simulated" capabilities with real tools, define an output contract, and remove leftover patches.

## License

MIT
