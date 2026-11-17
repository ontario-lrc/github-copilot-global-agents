# GitHub Copilot Custom Agents

## 1. Overview

This repository contains a collection of specialised, user-level agent profiles for GitHub Copilot.

Each agent has a clearly defined responsibility, scope, and set of permitted tools. The collection is designed to support a structured, technology-agnostic software engineering workflow using GitHub Copilot in Visual Studio Code and other compatible Copilot clients.

The framework uses a central orchestrator to coordinate specialist agents. Each specialist owns a distinct engineering responsibility and operates within explicitly defined boundaries.

The agents are designed to be reusable across different projects, programming languages, frameworks, platforms, and technology stacks.

## 2. Agent Architecture

The agents are organised using functional numeric prefixes.

| Prefix | Discipline | Purpose |
|---|---|---|
| `00` | Orchestration | Coordinates specialist agents and consolidates their work. |
| `1x` | Analysis and Design | Investigates existing systems, defines requirements, and establishes solution designs. |
| `2x` | Development | Implements approved software changes. |
| `3x` | Quality | Independently tests, reviews, and assesses software. |
| `4x` | Documentation and Delivery | Produces and maintains technical documentation. |
| `5x` | Operations and Support | Reserved for future operational and support responsibilities. |

### 2.1 Agent Directory

| Agent | Responsibility | Use When |
|---|---|---|
| **00 - Software Engineering Orchestrator** | Orchestration and coordination. | A request requires multiple specialist activities or coordinated work. |
| **10 - Codebase Explorer** | Existing-system investigation. | Understanding unfamiliar repositories, locating functionality, or tracing implementation behaviour. |
| **11 - Requirements Analyst** | Requirements definition. | Establishing objectives, functional requirements, constraints, business rules, and acceptance criteria. |
| **12 - Software Architect** | Solution architecture and design. | Defining technical solutions, architectural decisions, and component designs. |
| **20 - Implementation Engineer** | Software implementation. | Implementing approved functionality, modifying application code, or correcting defects. |
| **30 - Test Engineer** | Independent software testing. | Designing, executing, and evaluating software tests. |
| **31 - Code Reviewer** | Implementation review. | Independently assessing code changes for correctness, maintainability, and quality. |
| **32 - Security Reviewer** | Security assessment. | Assessing vulnerabilities, security controls, and software security risks. |
| **40 - Technical Documentation Specialist** | Technical documentation. | Creating, updating, and validating software documentation and release-related documentation. |

### 2.2 Responsibility Separation

Each agent has one primary responsibility.

The agents may perform several related activities when those activities contribute to their assigned outcome. They must not assume responsibilities assigned to another specialist.

The intended progression is:

**Investigate → Define → Design → Implement → Validate → Review → Document**

Not every request requires every stage. The orchestrator selects only the specialists necessary to achieve the requested outcome.

### 2.3 Orchestration Model

The Software Engineering Orchestrator is the central coordination point.

It is responsible for:

- Understanding the user's objective and constraints.
- Selecting the appropriate specialists.
- Defining bounded assignments and completion criteria.
- Coordinating task dependencies and handoffs.
- Consolidating specialist deliverables.
- Communicating results, limitations, and outstanding decisions.

The orchestrator does not replace specialist work or independently perform their assigned engineering activities.

Specialist agents do not recursively delegate work.

## 3. Repository Contents

Each agent is defined by an individual Markdown profile with YAML frontmatter.

```text
github-copilot-global-agents/
|
|-- 00-orchestrator.agent.md
|
|-- 10-codebase-explorer.agent.md
|-- 11-requirements-analyst.agent.md
|-- 12-software-architect.agent.md
|
|-- 20-implementation-engineer.agent.md
|
|-- 30-test-engineer.agent.md
|-- 31-code-reviewer.agent.md
|-- 32-security-reviewer.agent.md
|
|-- 40-technical-documentation-specialist.agent.md
|
|-- README.md
`-- LICENSE
```

## 4. Agent Design Principles

All agents follow a consistent design philosophy.

### 4.1 Singular Responsibility

Each agent has one clearly defined area of ownership.

Responsibilities are separated to minimise overlap, unnecessary delegation, and conflicting instructions.

### 4.2 Explicit Boundaries

Each profile defines:

- Identity and role.
- Mission.
- Responsibilities.
- Authority and restrictions.
- Working methodology.
- Collaboration expectations.
- Expected deliverables.
- Definition of success.

An agent must not independently assume another specialist's responsibilities.

### 4.3 Minimal Tool Permissions

Agents receive only the tools necessary for their responsibilities.

Read-only specialists should not receive modification or execution capabilities.

Tool availability does not replace behavioural restrictions. Agent instructions must also establish appropriate boundaries.

### 4.4 Technology Agnostic

The framework does not prescribe a particular technology stack.

Specialists must adapt their investigation and working methods to the technologies and conventions established by the target project.

### 4.5 Evidence-Based Work

Technical findings and recommendations must be supported by available evidence.

Agents must distinguish verified facts from assumptions, inferences, and unresolved questions.

### 4.6 Focused Deliverables

Agents should produce outputs proportional to the assigned task.

Avoid unnecessary reports, duplicated findings, exhaustive inventories, and repeated investigation.

## 5. Requirements

- A GitHub Copilot client supporting custom agent profiles.
- Visual Studio Code with GitHub Copilot is the primary intended environment.
- No runtime, package, or build dependencies are required.
- Agent profiles are Markdown files with YAML frontmatter.

## 6. Installation

Install the agent profiles in the user-level Copilot agents directory.

The standard user-level destination is:

```text
~/.copilot/agents/
```

On Windows, this typically resolves to:

```text
%USERPROFILE%\.copilot\agents\
```

### 6.1 Windows PowerShell

Run the following commands from the root of this repository:

```powershell
$agentDestination = Join-Path $HOME '.copilot\agents'

New-Item -ItemType Directory -Force -Path $agentDestination | Out-Null

Copy-Item -Path '.\*.agent.md' -Destination $agentDestination -Force
```

### 6.2 macOS and Linux

Run the following commands from the root of this repository:

```bash
agent_destination="$HOME/.copilot/agents"

mkdir -p "$agent_destination"

cp -f ./*.agent.md "$agent_destination/"
```

These commands copy the agent profiles without copying repository documentation or licensing files.

**Important:** The commands overwrite profiles with matching filenames. Review existing local customisations before installing updates.

If using a Copilot client with a separately configured user-level customization directory, follow that client's installation requirements.

## 7. Usage

### 7.1 Software Engineering Orchestrator

Select the Software Engineering Orchestrator when a request requires coordinated specialist work.

Examples:

- Investigating and extending an unfamiliar application.
- Defining requirements and designing a new software solution.
- Implementing a feature that requires architectural consideration and independent validation.
- Coordinating implementation, testing, security assessment, and documentation.

The orchestrator determines which specialists are necessary.

### 7.2 Specialist Agents

Specialists can be invoked by the orchestrator when their expertise is required.

The framework is designed to keep specialist agents focused on their respective responsibilities rather than requiring users to manage every stage manually.

Where direct specialist invocation is supported and enabled, a specialist can also be used for a narrowly scoped task.

### 7.3 Review and Approval

Agent outputs are subject to normal engineering review.

The presence of an agent profile does not guarantee that its instructions will always be followed or that its findings are correct.

Review proposed changes, validate important findings, and retain appropriate human approval for consequential operations.

## 8. Verification

After installation, verify that the profiles are available to GitHub Copilot.

### Visual Studio Code

1. Open Visual Studio Code.
2. Open the Command Palette.
3. Run **Chat: Open Customizations**.
4. Confirm that the installed agent profiles are listed.
5. Start a new Copilot Chat session.

### GitHub Copilot CLI

1. Start an interactive Copilot CLI session.
2. Run `/agent`.
3. Confirm that the installed profiles are available.
4. Start a new session after installing or updating profiles.

Profile discovery does not guarantee that a specialist will automatically be selected for every relevant task. Invocation behaviour depends on the Copilot client and agent configuration.

## 9. Maintenance and Updates

### 9.1 Updating

To update the collection:

1. Obtain the latest repository version.
2. Review changes to existing agent profiles.
3. Compare local customisations with repository versions.
4. Copy the updated profiles into the user-level agents directory.
5. Remove obsolete profiles when agents have been renamed or retired.
6. Restart or open a new Copilot session.

When an agent is renamed, remove its previous filename from the installation directory to avoid retaining an obsolete profile.

### 9.2 Adding Agents

New agents should only be introduced when a distinct engineering responsibility cannot be adequately represented by an existing specialist.

Before creating an agent, establish:

- Its singular responsibility.
- Its relationship to existing agents.
- Its explicit exclusions.
- Its required tools.
- Its expected deliverables.
- Whether its responsibilities overlap with an existing agent.

Avoid creating agents solely to represent individual workflow steps or procedures.

### 9.3 Changing Agent Responsibilities

Changes to an agent's responsibility must be evaluated against the entire collection.

If a responsibility is moved, removed, or reassigned, update affected agent descriptions, delegation guidance, and repository documentation accordingly.

## 10. Related Repositories

This agent collection is part of a broader GitHub Copilot customization framework.

| Repository | Purpose |
|---|---|
| [GitHub Copilot Global Agents](https://github.com/ontario-lrc/github-copilot-global-agents) | Specialist agent profiles and orchestration. |
| [GitHub Copilot Global Skills](https://github.com/ontario-lrc/github-copilot-global-skills) | Reusable task-specific procedures and supporting resources. |
| [GitHub Copilot Global Instructions](https://github.com/ontario-lrc/github-copilot-global-instructions) | Global behavioural guidance and scoped technical standards. |

The three repositories have distinct responsibilities:

- **Agents:** Define who performs the work and the boundaries of their responsibilities.
- **Skills:** Define reusable procedures and provide task-specific supporting knowledge.
- **Instructions:** Define persistent behavioural requirements and technical standards.

Avoid duplicating detailed procedures across agents, skills, and instructions.

## 11. Licence

This repository is dedicated to the public domain under the Creative Commons Zero (CC0) 1.0 Universal licence.

See [LICENSE](LICENSE) for the full terms.

## 12. Further Reading

- [Visual Studio Code: Custom Agents](https://code.visualstudio.com/docs/agent-customization/custom-agents)
- [Visual Studio Code: Custom Agent File Reference](https://code.visualstudio.com/docs/agents/reference/custom-agent-file-reference)
- [GitHub Copilot: Custom Agents](https://docs.github.com/en/copilot/concepts/agents/about-custom-agents)
- [GitHub Copilot: Custom Agent Configuration](https://docs.github.com/en/copilot/reference/custom-agents-configuration)