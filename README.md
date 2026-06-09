# prompt-pack

An **Agent Skill — for Claude and OpenAI Codex** — for **breaking big, risky work into a sequence of small, self-contained prompts** — each run in its own fresh chat so a single change never dies to a context/token limit. It also writes paste-ready **handoff briefings** to resume a dying chat or relay work to another tool (e.g. Codex).

It encodes a battle-tested format (refined across many real packs) so every pack is consistent, safe, and resumable.

## What it does

Three modes:

1. **Author a pack** — turn a large request into an ordered set of self-contained prompts saved to `docs/<TOPIC>_PROMPT_PACK.md`. Each prompt carries its own context, file list, acceptance criteria, "what must not change" guardrails, verification matrix, and commit message. Each is independently shippable.
2. **Execute a pack prompt** — run exactly one prompt in a fresh chat with a disciplined loop: read-first, verify references against current code, do only the scoped change, verify (regression + new behavior), report, and **don't commit until told**.
3. **Handoff** — write a single paste-ready briefing (a degenerate one-prompt pack) so a new chat — or another agent — resumes with full context, including what's shipped, the landmines, and the exact next step.

The pack file lives in the repo, so work survives context limits, spans multiple chats and tools, and never leaves the app half-broken. The skill reads the project's `CLAUDE.md` / `AGENTS.md` / memory and bakes the right build/test commands and conventions into every pack.

> **Lightweight by design.** Authoring a pack is one focused planning pass, and each prompt runs in its own short, fresh chat — so token use stays modest and predictable. The cost discipline *is* the point: no single chat has to hold the whole job.

## Quickstart — try this first

> **You type:** "This *<refactor / feature / migration>* is too big for one chat — make me a prompt pack." (Or, if you used ideate: "Make a prompt pack from `docs/CONCEPT_BRIEF.md`.")
>
> **You get back:** **`docs/<TOPIC>_PROMPT_PACK.md`** — an ordered set of self-contained prompts. Run each in its own fresh chat, verify, commit, move to the next. No single step can die to a context limit.

## Install

Skills live in `~/.claude/skills/` (all projects) or `.claude/skills/` (a single project). Put the `prompt-pack/` folder in either one.

**From the packaged file:**

```bash
mkdir -p ~/.claude/skills
unzip prompt-pack.skill -d ~/.claude/skills/
```

**Or from a clone of this repo:**

```bash
git clone https://github.com/nelsonwerd/prompt-pack-skill.git
mkdir -p ~/.claude/skills
cp -r prompt-pack-skill/prompt-pack ~/.claude/skills/
```

No restart needed — Claude Code detects it in-session. Verify with `/skills`, or just ask Claude what skills are available, and confirm `prompt-pack` is listed.

## Works in Claude *and* Codex

This follows the open **[Agent Skills](https://agentskills.io) standard**, so the same `SKILL.md` works in **Claude** and **OpenAI Codex**:

| You use… | Add it by… |
|---|---|
| **Claude Code** — terminal, the **Code** tab of the Claude desktop app, [claude.ai/code](https://claude.ai/code), or an IDE | the install above (drop `prompt-pack/` in `~/.claude/skills/`) |
| **OpenAI Codex** — CLI, app, or IDE | copy `prompt-pack/SKILL.md` (+ `references/`) into `.agents/skills/prompt-pack/` (repo) or `~/.agents/skills/prompt-pack/` (global) → [Codex skills docs](https://developers.openai.com/codex/skills) |
| **Claude chat** — the **Chat** tab of the desktop app, or [claude.ai](https://claude.ai) | uploading **`prompt-pack.skill`** (the zip) under **Customize → Skills** → [using Skills in Claude](https://support.claude.com/en/articles/12512180-use-skills-in-claude) |
| **Any other agent** | pointing it at `SKILL.md` — it's just instructions |

<sub>Exact in-app menu names and commands shift between versions — the linked docs are the source of truth. Claude-specific behaviors (auto-activation by description) are invoked explicitly in Codex; the *methodology* itself is fully portable.</sub>

**Runtime support:**

| | Claude chat | Claude Code | OpenAI Codex | Other agents |
|---|---|---|---|---|
| **prompt-pack** | Limited — best for high-level planning/handoffs; weak without repo access | **Best** | **Best** — reads `AGENTS.md`, full repo access | Works — with repo/file access |

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
