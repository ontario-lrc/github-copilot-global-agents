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
| `11 - Requirements Analyst` | Requirements, non-functional constraints, and acceptance criteria | `read`, `search`, `edit` | `requirements-analysis` |
| `12 - Software Architect` | Architecture and technical design | `read`, `search`, `edit`, `web` | `architecture-design` |
| `13 - Technical Researcher` | Evidence-based research of external technical information | `web` | `technical-research` |
| `20 - Implementation Engineer` | Application code implementation | `read`, `search`, `edit`, `execute` | `implementation-workflow` |
| `30 - Test Engineer` | Independent testing and test artefacts | `read`, `search`, `edit`, `execute` | `software-testing` |
| `31 - Code Reviewer` | Read-only code and accessibility review | `read`, `search` | `code-review` |
| `32 - Security Reviewer` | Read-only security assessment | `read`, `search`, `web` | `security-assessment` |
| `40 - Technical Documentation Specialist` | Technical and accessible documentation | `read`, `search`, `edit` | `documentation-release` |

## Responsibility boundaries

- Agent 00 coordinates. It does not perform specialist work or edit project files.
- Agent 10 investigates existing behaviour and does not change files.
- Agent 11 owns requirements artefacts, including applicable accessibility requirements and acceptance criteria, not architecture or implementation.
- Agent 12 owns architecture and design artefacts, not application code. It retains web access for focused architecture verification.
- Agent 13 researches external technical information. It does not inspect the repository, implement code, or make architecture decisions.
- Agent 20 owns application changes and narrowly scoped implementation-level tests. Accessibility is part of implementation acceptance, not an optional add-on.
- Agent 30 owns independent testing and test-only artefacts, not production code. It includes proportionate accessibility verification where relevant.
- Agent 31 reviews code without editing or executing it, including accessibility implications visible in changed code.
- Agent 32 assesses security without editing, executing scanners, or probing systems.
- Agent 40 owns technical and release documentation, not release execution. Generated documentation must be accessible.

The orchestrator assigns the minimum agents needed. A simple, bounded task does not require a full lifecycle pipeline. Specialists do not recursively delegate.

## Research-to-implementation workflow

Use Agent 13 when unfamiliar or changing external facts materially affect the work. When needed, Agent 10 investigates repository context and Agent 13 researches external context, in parallel if independent. Agent 12 evaluates design implications when a material architecture decision is required. Agent 20 receives a concise evidence package for implementation. Do not invoke Agent 13 for routine tasks involving familiar, stable technologies.

Agent 13's findings should include source URLs, dates or versions, compatibility constraints, assumptions, and unresolved questions. Agent 12 retains web access for focused fact-checking tied to a design decision.

## Accessibility

Accessibility is a mandatory quality and acceptance requirement for every generated or modified deliverable. It is not conditional on the user mentioning it and is not an optional skill invocation. Global and scoped instructions establish the baseline; responsible agents apply it within their roles. Use the accessibility skill for deeper audits, legal analysis, and format-specific procedures. Do not assume that one specialist owns accessibility for the whole workflow.

## Copilot-specific configuration

Every agent explicitly targets `github-copilot`. Tool lists follow least privilege. The `skills` frontmatter binds each agent to its relevant procedure without copying that procedure into the agent prompt. The orchestrator uses the Copilot `agent` tool for delegation.

Do not add Local-harness-only fields such as `agents`, `handoffs`, or `argument-hint` to these Copilot agent files. Copilot does not support those fields in custom agent frontmatter.

## Installation

For user-level use across VS Code workspaces, copy `.agent.md` files to:

- Windows: `%USERPROFILE%\.copilot\agents`
- macOS/Linux: `~/.copilot/agents`

### Windows PowerShell

```powershell
$source = (Get-Location).Path
$destination = Join-Path $HOME '.copilot\agents'
New-Item -ItemType Directory -Force -Path $destination | Out-Null
Get-ChildItem -LiteralPath $source -Filter '*.agent.md' -File |
    Copy-Item -Destination $destination -Force
```

Keep `README.md` in this repository; do not copy it into the agents directory. Install the companion skills and instructions into their respective user-level directories as described in those repositories.

Review local customisations before overwriting files with matching names. Restart or refresh the VS Code Chat view if the updated agents do not appear. Verify the active harness is GitHub Copilot.

## Companion repositories

- [Global Skills](https://github.com/ontario-lrc/github-copilot-global-skills)
- [Global Instructions](https://github.com/ontario-lrc/github-copilot-global-instructions)
