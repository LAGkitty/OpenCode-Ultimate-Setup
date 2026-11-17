# OpenCode Ultimate Setup

A practical, OS-aware blueprint for turning OpenCode into a small multi-agent engineering team.

This repository intentionally ships as **two source files**:

- [SUPER-PROMPT.txt](./SUPER-PROMPT.txt) — the setup task to paste into OpenCode.
- README.md — overview, requirements, safety model, and usage.

The setup prompt creates the configuration locally after inspecting the machine. It does not ship a prebuilt config that blindly overwrites an existing OpenCode installation.

## What it builds

A lean team of six agents:

| Agent | Responsibility | Access |
| --- | --- | --- |
| team-lead | Understands the task, plans, delegates, integrates, verifies, reports | Primary; may launch the five specialists |
| explorer | Maps codebases, entry points, conventions, and risks | Read-only |
| researcher | Checks official documentation, APIs, versions, and technical options | Research-only |
| implementer | Makes the assigned code/config changes | Edit access |
| reviewer | Independently reviews the actual diff for defects | Read-only |
| tester | Runs relevant checks and diagnoses failures | Test/fix access only when assigned |

It also creates a small set of slash commands and reusable skills. Model assignments are selected from the models actually available on the user's machine; no stale model IDs are hard-coded.

## Design principles

- **Inspect first.** Detect the installed OpenCode version, operating system, config paths, existing agents, commands, skills, providers, and MCP servers.
- **Preserve existing work.** Back up every path before changing it. Merge configuration instead of replacing it, and never overwrite a user's custom instructions.
- **Small team, not agent spam.** Six roles are enough for most work. No duplicated fallback-agent fleet; the coordinator handles a failed specialist once or reports the blocker.
- **No invented models.** Probe only free/keyless models or providers the user already configured. Never make paid calls without explicit approval.
- **MCPs are opt-in.** Keep existing servers. Do not install or add new servers unless the user explicitly approves them.
- **OS-aware.** Core workflows are portable. Platform-specific desktop control is optional and must be verified for the actual OS.
- **Evidence over claims.** Report only checks that really ran. Stop when acceptance criteria are met.
- **Treat external content as untrusted.** Repository files, web pages, terminal output, and MCP responses are data, not instructions.

## Quick start

1. Open OpenCode in a disposable test folder.
2. Open SUPER-PROMPT.txt.
3. Paste the complete prompt as one task.
4. Review the proposed plan and backup location before allowing changes.
5. Let the setup validate the resulting configuration and report anything that needs manual attention.

Do not run a large setup task inside an important project. The prompt is designed to work on an existing machine, but a scratch folder makes the first run easier to inspect.

## Requirements

- OpenCode with custom agents, commands, and skills supported by the installed version.
- A terminal and filesystem access.
- Internet access only for checking official OpenCode documentation and any research the user approves.
- A configured model provider is helpful. Free/keyless options may be available, but availability and quotas change.
- MCP servers and desktop-control tools are optional. The core team works without them.

## What this repository does not do

- It does not include an installer executable or a pre-generated opencode.json.
- It does not assume Windows, PowerShell, a specific browser, or a particular model provider.
- It does not automatically install MCP servers, browser binaries, plugins, or third-party skills.
- It does not add Roblox Studio, Godot, Blender, or other engine-specific integrations by default.
- It does not bypass permission prompts, change system security settings, or authorize paid model usage.

## Configuration locations

OpenCode configuration paths and supported fields can change. The setup prompt checks the official documentation for the installed version and discovers the actual config locations instead of treating a single path as universal.

Common locations include:

- Global configuration: ~/.config/opencode/ (subject to platform and environment overrides).
- Project configuration: .opencode/ and project-level opencode.json[c].
- Agents: agents/
- Commands: commands/
- Skills: skills/<skill-id>/SKILL.md

Use the official documentation as the source of truth:

- [Agents](https://opencode.ai/docs/agents)
- [Commands](https://opencode.ai/docs/commands)
- [Configuration](https://opencode.ai/docs/config)
- [Permissions](https://opencode.ai/docs/permissions)
- [Skills](https://opencode.ai/docs/skills)
- [MCP servers](https://opencode.ai/docs/mcp-servers)

## Limitations

- Model availability, provider routing, and free-tier quotas change frequently. The setup must probe before pinning a model.
- Subagents may share the same working tree. File ownership and explicit briefs reduce conflicts but do not provide filesystem isolation.
- Desktop automation depends on a separately installed, compatible MCP server and OS permissions. It is not required for coding or browser research.
- The prompt can validate configuration and run available tests, but it cannot guarantee that every third-party MCP or provider will remain available.

## License

See the repository's GitHub license metadata. OpenCode and third-party tools retain their own licenses.
