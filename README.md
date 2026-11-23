# GitHub Copilot Global Agents

A focused set of personal custom agents for **GitHub Copilot in Visual Studio Code**. This repository targets the Copilot harness only.

## Design

An agent owns one responsibility and the authority needed for that responsibility. Detailed repeatable procedures live in Agent Skills, not in duplicated agent prompts. Cross-cutting defaults live in the companion instructions repository.

The collection uses a single-orchestrator model. Agent 00 is the only user-invocable custom agent in this collection. Specialist agents are hidden from the agent picker and are used as delegated helpers rather than as independent entry points.

## Invocation and delegation model

- **Agent 00 is the collection's user-facing entry point.** It receives the user's request, selects the minimum relevant specialists, coordinates their work, and consolidates their results.
- **Agent 00 is the only agent profile in this collection granted the `agent` delegation tool.** Do not add that tool to specialist profiles.
- **Specialists do not initiate further delegation.** They complete their assigned responsibility and return their findings to the orchestrator.
- **Agent 13 is an internal research specialist intended to be invoked only by Agent 00.** Other specialists should route requests for substantial external technical research through Agent 00 rather than attempting to invoke Agent 13 themselves.

### Copilot caller-restriction limitation

The intended Agent 00-only use of Agent 13 is a collection-level workflow rule supported by least-privilege tool configuration. It is not a hard caller-identity restriction.

In the Copilot harness, the `agents` frontmatter allowlist is not supported. The `user-invocable: false` setting hides Agent 13 from the agent picker; it does not restrict which agent may call it. The `disable-model-invocation: false` setting keeps Agent 13 available for subagent invocation, but does not name or restrict its caller. Setting `disable-model-invocation: true` would prevent Agent 00 from invoking it as well.

Within this collection, Agent 00 is the only custom agent configured with the delegation tool, while specialist profiles omit it. Other agents or tools outside this collection that have their own delegation capability may still be able to invoke Agent 13. Do not describe the configuration as an enforced, system-wide allowlist.

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
- Agent 12 owns architecture and design artefacts, not application code. It retains web access for focused fact-checking directly relevant to architecture decisions.
- Agent 13 researches external technical information for Agent 00. It does not inspect the repository, implement code, make architecture decisions, or delegate to other agents.
- Agent 20 owns application changes and narrowly scoped implementation-level tests. Accessibility is part of implementation acceptance, not an optional add-on.
- Agent 30 owns independent testing and test-only artefacts, not production code. It includes proportionate accessibility verification where relevant.
- Agent 31 reviews code without editing or executing it, including accessibility implications visible in changed code.
- Agent 32 assesses security without editing, executing scanners, or probing systems. It may use web access for security-specific research within that assessment.
- Agent 40 owns technical and release documentation, not release execution. Generated documentation must be accessible.

The orchestrator assigns the minimum agents needed. A simple, bounded task does not require a full lifecycle pipeline. Specialists do not recursively delegate.
## Research-to-implementation workflow

Agent 00 decides whether external research is necessary and invokes Agent 13 when the research threshold above is met. Agent 13 returns a concise evidence package to Agent 00, including source URLs, relevant publication or release dates, versions, compatibility constraints, assumptions, risks, and unresolved questions.

When both repository context and external research are required, Agent 10 investigates the repository and Agent 13 researches external sources. These tasks may run in parallel when independent.

When research informs a material architecture decision, Agent 00 passes the relevant evidence package to Agent 12. Agent 12 owns the design decision; Agent 13 supplies evidence but does not select the architecture. Agent 12 may perform focused web verification directly related to that decision.

When research directly informs a bounded implementation and no material architecture decision is required, Agent 00 passes the concise evidence package to Agent 20. Agent 12 is not mandatory for every researched implementation.

## Accessibility

Accessibility is a mandatory quality and acceptance requirement for every generated or modified deliverable. It is not conditional on the user mentioning it and is not an optional skill invocation. Global and scoped instructions establish the baseline; responsible agents apply it within their roles. Use the accessibility skill for deeper audits, legal analysis, and format-specific procedures. Do not assume that one specialist owns accessibility for the whole workflow.

## Copilot-specific configuration

Every agent explicitly targets `github-copilot`. Tool lists follow least privilege. The `skills` frontmatter binds each agent to its relevant procedure without copying that procedure into the agent prompt.

Agent 00's `tools` list includes the Copilot `agent` delegation alias. The Copilot harness performs subagent delegation through its native `task` tool. No other specialist in this collection should receive the `agent` tool.

`user-invocable` and `disable-model-invocation` control different behaviours. Agent 00 is user-invocable and has model invocation disabled so it is not used as a subagent. Specialist agents are hidden from the picker but remain available for subagent invocation. These settings do not create a named-caller allowlist.

Do not add Local-harness-only fields such as `agents`, `handoffs`, or `argument-hint` to these Copilot agent files. The Copilot schema does not support those fields. In particular, the `agents` allowlist cannot be used to restrict Agent 00 to named subagents in this harness.

For current harness-specific behaviour, consult the [VS Code custom agent file reference](https://code.visualstudio.com/docs/agents/reference/custom-agent-file-reference) and [VS Code subagent guidance](https://code.visualstudio.com/docs/agents/run/subagents).

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

Keep `README.md` in this repository; do not copy it into the agents directory. Install the companion skills and instructions into their respective user-level directories as described in those repositories. Review local customisations before overwriting files with matching names. Restart or refresh the VS Code Chat view if the updated agents do not appear. Verify the active harness is GitHub Copilot.

## Companion repositories

- [Global Skills](https://github.com/ontario-lrc/github-copilot-global-skills)
- [Global Instructions](https://github.com/ontario-lrc/github-copilot-global-instructions)