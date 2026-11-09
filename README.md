# GitHub Copilot Global Agents

User-level specialist agent profiles for GitHub Copilot, designed for reuse across unrelated projects.

## Design
Agents define **identity, authority, responsibilities, boundaries, tools, and default deliverables**. Detailed repeatable procedures belong in the companion [global skills](https://github.com/ontario-lrc/github-copilot-global-skills) repository. Cross-cutting behavioural rules belong in [global instructions](https://github.com/ontario-lrc/github-copilot-global-instructions).

The collection uses functional prefixes: `0x` orchestration, `1x` analysis/design, `2x` development, `3x` quality, `4x` documentation/delivery, and `5x` operations/support.

## Agents

| Agent | Responsibility |
|---|---|
| 00 Project Manager / Orchestrator | Workflow coordination, specialist routing, approvals, and synthesis |
| 10 Codebase Explorer | Read-only repository discovery and impact investigation |
| 11 Requirements Analyst | Requirements and acceptance criteria |
| 12 Software Architect | Solution architecture and technical design |
| 20 Implementation Engineer | Application implementation and implementation-level checks |
| 30 Test Engineer | Independent quality assurance and test execution |
| 31 Code Reviewer | Read-only implementation review |
| 32 Security Reviewer | Read-only security assessment |
| 40 Documentation and Release Engineer | Documentation and release preparation |

Agent 00 is the central delegator. Specialists do not recursively delegate. Invoke only agents required by the task.

## Installation

Install the `.agent.md` files into the user-level Copilot agents directory.

### Windows PowerShell
```powershell
$source = (Get-Location).Path
$destination = Join-Path $HOME '.copilot\agents'
New-Item -ItemType Directory -Force -Path $destination | Out-Null
Get-ChildItem -LiteralPath $source -Filter '*.agent.md' -File |
    Copy-Item -Destination $destination -Force
```

### macOS / Linux
```bash
destination="$HOME/.copilot/agents"
mkdir -p "$destination"
find . -maxdepth 1 -type f -name '*.agent.md' -exec cp -f {} "$destination/" \;
```

Review local customisations before overwriting matching profiles. This installer does not remove unrelated agents.

## Companion repositories
- [Global Skills](https://github.com/ontario-lrc/github-copilot-global-skills)
- [Global Instructions](https://github.com/ontario-lrc/github-copilot-global-instructions)