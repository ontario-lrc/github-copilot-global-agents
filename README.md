# GitHub Copilot Global Agents

A focused set of personal custom agents for **GitHub Copilot in Visual Studio Code**. This repository targets the Copilot harness only.

## Design

An agent owns one responsibility and the authority needed for that responsibility. Detailed repeatable procedures live in Agent Skills, not in duplicated agent prompts. Cross-cutting defaults live in the companion instructions repository.

The orchestrator is the user-facing entry point. Specialist agents are hidden from the picker and are invoked when the orchestrator delegates work.

## Agent roster

| Agent | Single responsibility | Tools | Bound skill |
|---|---|---|---|
| `00 - Software Engineering Orchestrator` | Plan, delegate, coordinate, and consolidate | `read`, `search`, `agent`, `todo` | `delegation-and-handoff`, `structured-reporting` |
| `10 - Codebase Explorer` | Read-only investigation of existing code | `read`, `search` | `repository-investigation` |
| `11 - Requirements Analyst` | Requirements and acceptance criteria | `read`, `search`, `edit` | `requirements-analysis` |
| `12 - Software Architect` | Architecture and technical design | `read`, `search`, `edit`, `web` | `architecture-design` |
| `20 - Implementation Engineer` | Application code implementation | `read`, `search`, `edit`, `execute` | `implementation-workflow` |
| `30 - Test Engineer` | Independent testing and test artefacts | `read`, `search`, `edit`, `execute` | `software-testing` |
| `31 - Code Reviewer` | Read-only code review | `read`, `search` | `code-review` |
| `32 - Security Reviewer` | Read-only security assessment | `read`, `search`, `web` | `security-assessment` |
| `40 - Technical Documentation Specialist` | Technical and release documentation | `read`, `search`, `edit` | `documentation-release` |

## Responsibility boundaries

- Agent 00 coordinates. It does not perform specialist work or edit project files.
- Agent 10 investigates existing behaviour and does not change files.
- Agent 11 owns requirements artefacts, not architecture or implementation.
- Agent 12 owns architecture and design artefacts, not application code.
- Agent 20 owns application changes and narrowly scoped implementation-level tests.
- Agent 30 owns independent testing and test-only artefacts, not production code.
- Agent 31 reviews code without editing or executing it.
- Agent 32 assesses security without editing, executing scanners, or probing systems.
- Agent 40 owns technical and release documentation, not release execution.

The orchestrator assigns the minimum agents needed. A simple, bounded task does not require a full lifecycle pipeline. Specialists do not recursively delegate.

## Copilot-specific configuration

Every agent explicitly targets `github-copilot`. Tool lists follow least privilege. The `skills` frontmatter binds each agent to its relevant procedure without copying that procedure into the agent prompt. The orchestrator uses the Copilot `agent` tool for delegation.

Do not add Local-harness-only fields such as `agents`, `handoffs`, or `argument-hint` to these Copilot agent files. Copilot does not support those fields in custom agent frontmatter.

## Installation

For user-level use across VS Code workspaces, copy `.agent.md` files to:

- Windows: `%USERPROFILE%\.copilot\agents`
- macOS/Linux: `~/.copilot/agents`

Keep `README.md` in this repository; do not copy it into the agents directory. Install the companion skills and instructions into their respective user-level directories as described in those repositories.

### Windows PowerShell

```powershell
$source = (Get-Location).Path
$destination = Join-Path $HOME '.copilot\agents'
New-Item -ItemType Directory -Force -Path $destination | Out-Null
Get-ChildItem -LiteralPath $source -Filter '*.agent.md' -File |
    Copy-Item -Destination $destination -Force
```

Review local customisations before overwriting files with matching names. Restart or refresh the VS Code Chat view if the updated agents do not appear. Verify the active harness is GitHub Copilot.

## Companion repositories

- [Global Skills](https://github.com/ontario-lrc/github-copilot-global-skills)
- [Global Instructions](https://github.com/ontario-lrc/github-copilot-global-instructions)
