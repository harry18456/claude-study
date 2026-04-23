# cowork-collaboration Specification

## Purpose

TBD - created by archiving change 'claude-introduction-docs'. Update Purpose after archive.

## Requirements

### Requirement: Document team collaboration workflows

COWORK.md SHALL describe practical collaboration scenarios where Claude assists engineering teams, including code review, documentation writing, and technical decision-making.

#### Scenario: Team uses Claude for code review assistance

- **WHEN** an engineer reads the collaboration workflows section
- **THEN** they SHALL understand how to use Claude to assist with code review processes


<!-- @trace
source: claude-introduction-docs
updated: 2026-04-03
code:
  - CODE.md
  - GENERAL.md
  - COWORK.md
  - MCP.md
  - pics/buddy.png
  - CHAT.md
  - CLAUDE.md
  - .spectra.yaml
  - API.md
  - SKILL.md
-->

---
### Requirement: Document prompt sharing practices

COWORK.md SHALL describe how teams can share effective prompts and system instructions to maintain consistency across team members.

#### Scenario: Team standardizes Claude usage

- **WHEN** an engineer reads the prompt sharing section
- **THEN** they SHALL be able to establish shared prompt templates for common team tasks


<!-- @trace
source: claude-introduction-docs
updated: 2026-04-03
code:
  - CODE.md
  - GENERAL.md
  - COWORK.md
  - MCP.md
  - pics/buddy.png
  - CHAT.md
  - CLAUDE.md
  - .spectra.yaml
  - API.md
  - SKILL.md
-->

---
### Requirement: Document Claude for documentation generation

COWORK.md SHALL cover using Claude to generate and maintain technical documentation, including API docs, READMEs, and runbooks.

#### Scenario: Engineer generates documentation with Claude

- **WHEN** an engineer reads the documentation section
- **THEN** they SHALL understand how to use Claude to produce and update technical documentation efficiently

<!-- @trace
source: claude-introduction-docs
updated: 2026-04-03
code:
  - CODE.md
  - GENERAL.md
  - COWORK.md
  - MCP.md
  - pics/buddy.png
  - CHAT.md
  - CLAUDE.md
  - .spectra.yaml
  - API.md
  - SKILL.md
-->

---
### Requirement: Document Live Artifacts feature

03_COWORK.md SHALL document the Live Artifacts feature released on 2026-04-20, including its differences from traditional Artifacts, supported connectors via MCP, subscription-plan coverage (Pro / Max / Team / Enterprise on Claude web and desktop), the dedicated Live Artifacts tab with version history, and platform limitations (web / Desktop first-class, mobile limited).

#### Scenario: Reader understands Live Artifacts capability

- **WHEN** a reader reaches the Live Artifacts section of 03_COWORK.md
- **THEN** they SHALL understand that Live Artifacts are persistent dashboards/trackers that auto-refresh via MCP connectors, are saved in a dedicated tab with version history, work on Claude web and Desktop, and require a paid plan

#### Scenario: Reader differentiates Live Artifacts from traditional Artifacts

- **WHEN** a reader compares Artifacts versus Live Artifacts
- **THEN** they SHALL understand that traditional Artifacts are static snapshots tied to a single conversation, while Live Artifacts are connector-backed, cross-session reusable, and refresh on every open

<!-- @trace
source: ad-hoc-update
updated: 2026-04-23
reason: Anthropic announced Live Artifacts on 2026-04-20; section added to 03_COWORK.md
code:
  - 03_COWORK.md
  - verify/03_COWORK.md
-->