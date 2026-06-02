# prompt-pack

A Claude Code skill for **breaking big, risky work into a sequence of small, self-contained prompts** — each run in its own fresh chat so a single change never dies to a context/token limit. It also writes paste-ready **handoff briefings** to resume a dying chat or relay work to another tool (e.g. Codex).

It encodes a battle-tested format (refined across many real packs) so every pack is consistent, safe, and resumable.

## What it does

Three modes:

1. **Author a pack** — turn a large request into an ordered set of self-contained prompts saved to `docs/<TOPIC>_PROMPT_PACK.md`. Each prompt carries its own context, file list, acceptance criteria, "what must not change" guardrails, verification matrix, and commit message. Each is independently shippable.
2. **Execute a pack prompt** — run exactly one prompt in a fresh chat with a disciplined loop: read-first, verify references against current code, do only the scoped change, verify (regression + new behavior), report, and **don't commit until told**.
3. **Handoff** — write a single paste-ready briefing (a degenerate one-prompt pack) so a new chat — or another agent — resumes with full context, including what's shipped, the landmines, and the exact next step.

The pack file lives in the repo, so work survives context limits, spans multiple chats and tools, and never leaves the app half-broken. The skill reads the project's `CLAUDE.md` / `AGENTS.md` / memory and bakes the right build/test commands and conventions into every pack.

## Install

Skills live in `~/.claude/skills/` (all projects) or `.claude/skills/` (a single project). Put the `prompt-pack/` folder in either one.

**From the packaged file:**

```bash
mkdir -p ~/.claude/skills
unzip prompt-pack.skill -d ~/.claude/skills/
```

**Or from a clone of this repo:**

```bash
git clone https://github.com/nelsonwerd/prompt-pack.skill.git
mkdir -p ~/.claude/skills
cp -r prompt-pack.skill/prompt-pack ~/.claude/skills/
```

No restart needed — Claude Code detects it in-session. Verify with `/skills`, or just ask Claude what skills are available, and confirm `prompt-pack` is listed.

## Use it

- **Manually:** type `/prompt-pack` and describe the big job (or paste a pack prompt to execute it).
- **Automatically:** Claude invokes it when you say things like "make a prompt pack", "break this into phases", "I'm running out of context — write me a handoff", or "execute P3 from the pack."

Examples:

- "This refactor is too big for one chat — make me a prompt pack."
- "Break the onboarding rebuild into phases I can ship one at a time."
- "I'm almost out of tokens. Write a handoff I can paste into a new chat."
- "Execute P2 from the currency-fix pack."

## What's in this repo

- `prompt-pack/` — the skill itself (`SKILL.md` + reference playbooks). This is what you install.
  - `references/pack-template.md` — the canonical fill-in scaffold for a pack.
  - `references/authoring-guide.md` — how to turn a big job into a pack.
  - `references/execution-guide.md` — how to run one pack prompt safely.
  - `references/handoff-guide.md` — how to write a paste-ready handoff.
- `prompt-pack.skill` — the same folder, zipped, for a one-step download.
