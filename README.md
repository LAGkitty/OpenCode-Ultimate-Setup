# Ultimate OpenCode for Windows — Multi-Agent AI Team + Desktop Control

A battle-tested configuration that turns **OpenCode** into a full AI engineering studio:
a coordinator that actually delegates to named specialist subagents, verification and
fallback discipline that knows when to *stop*, real desktop control, browser automation,
and a reusable skill system — all with free/zero-key models by default.

> Verified against OpenCode **v2.2.0** (October 2026) on Windows 11.
> Everything is plain Markdown or JSON — no forks, no plugins to publish, no framework.

---

## What this setup gives you

### 1. A coordinator + named specialist team (native V2 `agents/`)

| Agent | Role | Default model (see §Models) |
| --- | --- | --- |
| `team-lead` | Coordinator: understands, plans, **launches subagents via the `subagent` tool**, integrates, verifies, reports | `opencode/glm-5.3-flash-free` (keyless) |
| `explorer` | Read-only codebase/stack inspection, conventions, integration risks | `google/gemini-3.5-flash-lite` |
| `researcher` | Official docs, APIs, versions, verified design options | `opencode/deepseek-v4.1-flash-free` |
| `implementer` | Code/file changes on assigned paths, self-checks, honest reporting | `opencode/deepseek-v4.1-flash-free` |
| `reviewer` | Independent review: requirements fit, logic, security, regressions | `opencode/glm-5.3-flash-free` |
| `tester` | Discover + run real check commands, write minimal tests when needed | `opencode/nemotron-3.5-lightning-free` |

**Fallback tier** (`implementer-b`, `researcher-b`, `tester-b`, `reviewer-b`, `explorer-b`):
identical rule-set, different verified provider path, dispatched by the coordinator when the
primary specialist fails (quota/auth/etc.). If a fallback also fails, the coordinator does
the step itself. Every failure and swap is reported — never silently retried.

### 2. Reusable slash commands

| Command | What it does |
| --- | --- |
| `/team <objective>` | Full multi-agent workflow: inspect → plan → parallel/sequential delegation → review → **budgeted** verification → evidence-based report |
| `/test <scope>` | Runs the project's real checks through the `tester` subagent |
| `/research <question>` | Docs/versions/API investigation with verified sources |
| `/review <scope>` | Independent defect review of the current diff |
| `/debug <issue>` | Root-cause-then-fix workflow |
| `/onboard` | Map an unfamiliar project and record durable notes |
| `/handoff` | Persist session state so the next session resumes cleanly |

### 3. Verification discipline (the important design lesson)

The team stops when **acceptance criteria are met — not when the work is "perfect"**:

- One tester run + one reviewer pass is the default budget; reruns only after an actual repair.
- The implementer's own successful test/build run **counts** as verified evidence.
- If the user says "it works already", the team treats that as verification and moves on.
- Interactive/browser work (games, UIs) = **one** real play pass, then done.
- Without this rule, agents happily test a finished game forever — we know, we watched.

### 4. Model-failure fallback ladder (all tiers live-verified)

1. Primary specialist (keyless `opencode/*-free` — fast, no quota anxiety)
2. `-b` fallback for that role (e.g. OpenRouter's *auto-free route* `openrouter/openrouter/free`)
3. Coordinator does the step itself
4. Paid models **only with the user's explicit authorization** (never auto-spent)

Dead ends discovered empirically (so you don't have to): Google's `*-pro-preview` models
have **zero free quota** on OAuth auth; `gemini-2.5-*` is deprecated for new API users;
Google's flash bucket costs 5 req/min / 20 req/day on the free tier; Groq's free 8k-TPM
cap is smaller than OpenCode's own prompt size.

### 5. Desktop + web control via MCP

- `computer` — native Windows desktop control (screenshots, clicks, typing, app launch)
- `playwright` — browser automation (tested live against example.com through an agent)
- `blender` — optional, for DCC scene work
- *(Roblox Studio MCP was intentionally not part of the public instructions in this repo)*

### 6. Skills installed

`computer-use`, `research-first`, `plan-and-execute`, `debug-and-verify`, `code-review`,
`project-onboarding`, `safe-install`, `windows-dev-env` — every agent can load the ones it
needs; skills are advertised as id+description and loaded on demand.

---

## Repository layout

```
opencode/
├── agents/            # 11 agents (6 primary + 5 fallbacks), plain Markdown
├── commands/          # 7 slash commands, plain Markdown
├── skills/            # 8 skills, each skills/<id>/SKILL.md
├── opencode.json      # permissions + MCP servers (no secrets — use auth store/env)
├── docs/
│   ├── TEAM-GUIDE.md  # usage, model swaps, diagnostics, rollback
│   └── agent-notes.md # dated decisions + failures (the most useful file here)
└── AGENTS.md          # short global instructions every session reads
```

## How to reproduce this on your machine

**Option A (recommended):** open OpenCode in any scratch folder and paste the contents of
[`SUPER-PROMPT.txt`](./SUPER-PROMPT.txt) as a single task. The prompt
inspects your environment first, researches the docs for **your installed version**, backs
up whatever exists, then creates every file (templates included in the prompt), validates,
and runs the test suite an agent team should pass before it reports success.

**Option B (manual):** copy this repo's templates yourself using the docs:
[`https://opencode.ai/v2/docs/agents/`](https://opencode.ai/v2/docs/agents/) and
[`https://opencode.ai/v2/docs/commands/`](https://opencode.ai/v2/docs/commands/).

## Requirements

- OpenCode **v2.x** (`opencode --version`)
- Windows 10/11 with PowerShell 5.1+ is fine; **macOS/Linux variants of the same idea work**
  if you swap the Windows-MCP server for your OS's desktop-control MCP.
- Node/npm for the Playwright MCP server.
- **No API keys required** for the default team — the `opencode/*` free models are keyless.
  Existing keys (Google/Groq/Mistral/Nvidia/OpenRouter…) simply add extra fallback tiers.

## Safety notes baked into the config

- Secrets never go in `opencode.json`, agent, or skill files — `opencode auth login` or env vars only.
- Destructive ops (`git push`, hard resets, registry, process kills, installs) require prompts.
- Reviewer/tester agents are read-only-by-default; the coordinator can't launch `subagents`.
- No AI provider gets automatically billed; `:free` and keyless models first.
- Web/MCP output is treated as untrusted data, never as instructions.

## Honest limitations

- Subagents share the coordinator's working directory — isolation is via file-ownership in
  the briefs, not sandboxing.
- Model availability changes constantly; the super prompt **probes before pinning**, and the
  tell-it-which-model syntax is `provider/model` (see `opencode models`).
- `request` (per-agent header/body overlays) exists in config but isn't yet applied by the
  V2 session runner.
- Free Google GPU-quota buckets are small (5/min, 20/day) — that's why high-frequency roles
  are pinned to keyless models.

---

*Built on OpenCode. All config is reversible — the setup script backs up your existing
`AGENTS.md`, `opencode.json`, `SKILLS.md`, and `commands/` before touching anything.*
