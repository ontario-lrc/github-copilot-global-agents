# GitHub Copilot Custom Agents

## Overview

This collection contains user-level agent profiles for GitHub Copilot. Install the profiles in your Copilot user folder to make them available across projects on your machine.

## Contents

Each profile defines a specialist role and its instructions. Use a specialist for focused work, or choose the project manager / orchestrator for broader work that needs planning and coordination. The orchestrator may delegate when the Copilot client and available tools support it.

| Agent | Use it for |
| --- | --- |
| [Project Manager / Orchestrator](00-project-manager-orchestrator.agent.md) | Break down software requests, plan work, coordinate specialists, and review the overall result. |
| [Codebase Explorer](10-codebase-explorer.agent.md) | Investigate a repository's structure, technologies, architecture, dependencies, and existing behaviour. |
| [Requirements Analyst](11-requirements-analyst.agent.md) | Clarify objectives and turn requests into requirements, constraints, assumptions, and acceptance criteria. |
| [Software Architect](12-software-architect.agent.md) | Design solutions that fit established requirements, architecture, and technical constraints. |
| [Implementation Engineer](20-implementation-engineer.agent.md) | Implement approved functionality and fixes in the existing codebase. |
| [Test Engineer](30-test-engineer.agent.md) | Plan and perform quality assurance, test implementation, and investigate failures. |
| [Code Reviewer](31-code-reviewer.agent.md) | Independently review changes for correctness, maintainability, and quality. |
| [Security Reviewer](32-security-reviewer.agent.md) | Assess security architecture, threats, vulnerabilities, and secure coding. |
| [Documentation and Release Engineer](40-documentation-release-engineer.agent.md) | Prepare technical documentation and release-related material. |

## Requirements

- A GitHub Copilot client that supports user-level custom agents, such as the Copilot harness in Visual Studio Code (VS Code) or GitHub Copilot command-line interface (CLI).
- No runtime, package, or build dependencies. Agent profiles are Markdown files.
- The user-level destination is `~/.copilot/agents/`. The files are local to the machine and do not automatically sync between computers.

## Installation

Run the commands for your operating system from the root of this repository. They copy only `*.agent.md` profiles; the collection's `README.md` and `LICENSE` stay in the source folder.

### Windows PowerShell

```powershell
$copilotRoot = Join-Path $HOME '.copilot'
$agentDestination = Join-Path $copilotRoot 'agents'
New-Item -ItemType Directory -Force -Path $agentDestination | Out-Null
Copy-Item -Path '.\github-copilot-global-agents\*.agent.md' -Destination $agentDestination -Force
```

### macOS and Linux

```bash
agent_destination="$HOME/.copilot/agents"
mkdir -p "$agent_destination"
cp -f github-copilot-global-agents/*.agent.md "$agent_destination/"
```

If GitHub Copilot CLI uses a non-default `COPILOT_HOME`, install the profiles under that directory's `agents/` folder for the CLI to find them. VS Code may use a separately configured user location. The commands overwrite profiles with matching names, so review any local edits first.

## Usage

In VS Code, choose an agent from the Copilot Chat agent picker. In an interactive Copilot CLI session, use `/agent` to select an available profile. Select the requirements analyst for acceptance criteria, the reviewer agents for independent review, or the orchestrator to coordinate larger tasks.

These profiles target the Copilot harness in VS Code and GitHub Copilot CLI. Other Copilot clients may use different customization locations or support different agent features. Profiles guide Copilot's behaviour; review generated changes as you normally would.

## Verification

- **VS Code:** Run **Chat: Open Customizations** from the Command Palette and confirm the profiles appear under user agents. Open a new Copilot chat after installing or updating.
- **Copilot CLI:** Run `/agent` and confirm the profiles are available. Start a new session after installing or updating.

Discovery means the client found a profile; it does not guarantee that a model will choose it automatically. If a profile is missing, check the selected harness, destination folder, filename, and any custom home-directory settings.

## Update and Uninstall

To update, copy the newer `*.agent.md` files using the installation commands. Compare source and destination folders and remove old profiles manually if an agent was renamed or removed. Preserve any local changes you want to keep.

To uninstall, remove only this collection's profiles from `~/.copilot/agents/`. Review the folder first and retain profiles added or edited independently. If Copilot CLI uses a custom `COPILOT_HOME`, use that directory's `agents/` folder.

## Customization

Edit a profile's role instructions in its Markdown file, keeping the YAML frontmatter valid and the filename unchanged. To add an agent, create a `.agent.md` file with a unique `name` and a clear `description`, then install it with the other profiles. Check the custom agent file reference below for supported metadata and fields.

## Folder Layout

```text
github-copilot-global-agents/
|-- 00-project-manager-orchestrator.agent.md
|-- 10-codebase-explorer.agent.md
|-- 11-requirements-analyst.agent.md
|-- 12-software-architect.agent.md
|-- 20-implementation-engineer.agent.md
|-- 30-test-engineer.agent.md
|-- 31-code-reviewer.agent.md
|-- 32-security-reviewer.agent.md
|-- 40-documentation-release-engineer.agent.md
|-- LICENSE
`-- README.md
```

## Licence

This collection is dedicated to the public domain under Creative Commons Zero (CC0) 1.0 Universal. See [LICENSE](LICENSE) for the full terms.

## Further Reading

- [VS Code: Custom agents](https://code.visualstudio.com/docs/agent-customization/custom-agents)
- [VS Code: Custom agent file reference](https://code.visualstudio.com/docs/agents/reference/custom-agent-file-reference)
- [GitHub Copilot: Use custom agents](https://docs.github.com/en/copilot/how-tos/copilot-in-your-ide/use-copilot-agents/use-custom-agents)
- [GitHub Copilot CLI: Command reference](https://docs.github.com/en/copilot/reference/copilot-cli-reference/cli-command-reference)
