## ADDED Requirements

### Requirement: Document installation and setup

CODE.md SHALL provide installation instructions for Claude Code CLI across supported platforms and describe initial configuration steps including authentication.

#### Scenario: Engineer installs Claude Code

- **WHEN** an engineer reads the installation section
- **THEN** they SHALL be able to install and authenticate Claude Code CLI on their machine

### Requirement: Document core CLI commands

CODE.md SHALL document the primary Claude Code CLI commands and their usage patterns for common development tasks.

#### Scenario: Engineer uses Claude Code for a task

- **WHEN** an engineer reads the commands section
- **THEN** they SHALL know which commands to use for editing files, running tasks, and interacting with the codebase

### Requirement: Document CLAUDE.md configuration

CODE.md SHALL explain the CLAUDE.md file system for providing project-specific instructions to Claude Code, including global and per-project configuration.

#### Scenario: Engineer configures project instructions

- **WHEN** an engineer reads the CLAUDE.md section
- **THEN** they SHALL be able to create effective CLAUDE.md files that customize Claude Code behavior for their project

### Requirement: Document hooks system

CODE.md SHALL describe the hooks system (PreToolUse, PostToolUse, Stop) and how to configure automated behaviors.

#### Scenario: Engineer sets up automated formatting hook

- **WHEN** an engineer reads the hooks section
- **THEN** they SHALL be able to configure a hook that automatically runs after file edits

### Requirement: Document agent and subagent capabilities

CODE.md SHALL explain Claude Code's multi-agent capabilities, including available specialist agents and when to use them.

#### Scenario: Engineer delegates a task to a specialist agent

- **WHEN** an engineer reads the agents section
- **THEN** they SHALL understand how to invoke specialist agents (code-reviewer, tdd-guide, etc.) for specific tasks

### Requirement: Document No Flicker Mode (Fullscreen Rendering)

CODE.md SHALL document the fullscreen rendering mode (`CLAUDE_CODE_NO_FLICKER=1`), available since v2.1.89, including how it works, how to enable it permanently, mouse support capabilities, and known limitations.

#### Scenario: Engineer enables No Flicker Mode

- **WHEN** an engineer reads the No Flicker Mode section
- **THEN** they SHALL be able to enable it via environment variable and understand the vim/htop-style interface behavior

#### Scenario: Engineer uses mouse support in fullscreen mode

- **WHEN** an engineer reads the mouse support section
- **THEN** they SHALL know how to click, select text, scroll, and open URLs with the mouse, and how to disable mouse capture with `CLAUDE_CODE_DISABLE_MOUSE=1` if needed

#### Scenario: Engineer searches conversation in fullscreen mode

- **WHEN** an engineer reads about transcript mode
- **THEN** they SHALL know how to use `Ctrl+o` to enter transcript mode with vim-style search (`/` to search, `n`/`N` for next/previous match)

### Requirement: Document complete CLI commands

CODE.md SHALL provide a complete reference of all Claude Code CLI commands including basic operations (`claude`, `claude -p`, `claude -c`), authentication (`claude auth login/logout/status`), and management commands (`claude doctor`, `claude mcp`, `claude agents`, `claude remote-control`).

#### Scenario: Engineer discovers available CLI options

- **WHEN** an engineer reads the CLI commands reference
- **THEN** they SHALL find all available terminal commands with brief descriptions and usage examples

### Requirement: Document complete slash commands

CODE.md SHALL document all major slash commands organized by category: session management (`/clear`, `/compact`, `/fork`, `/rewind`), configuration (`/config`, `/model`, `/effort`, `/theme`, `/vim`), tools (`/voice`, `/diff`, `/copy`, `/export`), and information (`/help`, `/cost`, `/context`, `/skills`).

#### Scenario: Engineer uses slash commands during session

- **WHEN** an engineer reads the slash commands section
- **THEN** they SHALL know which slash command to use for common tasks like switching models, managing context, and exporting conversations

### Requirement: Document /buddy command

CODE.md SHALL introduce the `/buddy` command (shipped with v2.1.89), explaining that it generates a deterministic ASCII-art virtual pet companion derived from the user's ID hash, with 18 species, 5 stat attributes, rarity tiers (Common to Legendary), and cosmetic customization options.

#### Scenario: Engineer discovers their buddy

- **WHEN** an engineer reads the /buddy section and runs the command
- **THEN** they SHALL understand how their unique companion is generated and what customization options exist
