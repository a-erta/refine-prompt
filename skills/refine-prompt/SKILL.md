---
name: refine-prompt
description: Analyze and iteratively rewrite the user's draft prompt before executing it. Use with /refine-prompt followed by a draft, or when the user asks to improve a request before it is processed.
---

# refine-prompt

Goal: bring the user's draft into its best form BEFORE running it. Do not execute the task until the user approves the rewritten prompt.

## Flow

1. **Read the draft** (from the arguments or the message). If you are in a repository, read `CLAUDE.md`, `AGENTS.md` or a similar project file for context, so you don't ask about things already documented. Reading is fine; making changes is not.

2. **Diagnose** — check the draft against this checklist and flag only what is missing or weak:
   - **Goal**: what concrete deliverable must exist at the end?
   - **Context**: why, for whom, what already exists (files, repos, systems, decisions already made)
   - **Constraints and trade-offs**: for every one-sided directive ("short", "fast", "don't ask"), state the opposite cost too
   - **Success criteria**: how to verify it's done (tests, checks, an example of the expected output)
   - **Output contract**: format, length, where it gets saved or delivered
   - **Out of scope**: what must not be touched
   - **Capabilities**: if the draft asks the model to "know" or "compute" something, name the tool or source to use instead of relying on the model
   - **Hygiene**: redundancy, contradictory instructions, leftover patches to remove

3. **Questions**: at most 3, only ones that change the outcome. Use a multiple-choice question tool if available, with concrete options. If a detail has a reasonable default, assume it and state the assumption instead of asking.

4. **Rewrite** the prompt in a single code block. If it has more than two parts, structure it with XML tags (`<goal>`, `<context>`, `<constraints>`, `<success_criteria>`, `<output>`, `<out_of_scope>`). Keep the user's language and register — a draft written in Italian stays in Italian. Do not add requirements the user didn't express: assumptions go on a separate "Assumptions:" line, outside the prompt.

5. **Show** the rewritten prompt plus at most 3 lines of "What changed". Then ask: **Run / Edit / Save**.

6. **Iterate**: apply the feedback and repeat steps 4–5. At most 3 rounds, then offer to run it or stop.

7. **Close**:
   - Run → treat the rewritten prompt as the user's request and execute it in the same turn.
   - Save → write it to `prompts/<slug>.md` in the current repository (if there is no repository, return the text).

## Rules

- If the draft is already clear (goal, output and success criteria present), say so in one line and offer to run it right away.
- The rewritten prompt is as long as it needs to be, no longer: no boilerplate, no generic roles ("you are an expert...").
- One rewritten proposal per round, not multiple variants, unless asked.
- Never execute any part of the task during refinement.
