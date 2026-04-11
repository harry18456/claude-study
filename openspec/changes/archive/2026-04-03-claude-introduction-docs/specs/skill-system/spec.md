## ADDED Requirements

### Requirement: Document Skill system architecture

SKILL.md SHALL explain the Claude Code Skill system: what skills are (Markdown-defined workflow files), where they are stored (global vs. project-level), and how they are invoked.

#### Scenario: Engineer understands skills concept

- **WHEN** an engineer reads the architecture section
- **THEN** they SHALL understand the difference between built-in skills, project skills, and global skills

### Requirement: Document skill invocation

SKILL.md SHALL describe how to invoke skills via slash commands (e.g., `/skill-name`) and the Skill tool, including argument passing.

#### Scenario: Engineer invokes a skill

- **WHEN** an engineer reads the invocation section
- **THEN** they SHALL be able to invoke an existing skill with and without arguments

### Requirement: Document creating custom skills

SKILL.md SHALL explain how to create a custom skill, including the required Markdown structure, frontmatter fields, and how to place it in the correct directory.

#### Scenario: Engineer creates a team skill

- **WHEN** an engineer reads the custom skill creation section
- **THEN** they SHALL be able to create a skill file that is accessible to their project's Claude Code instance

### Requirement: Document skill best practices

SKILL.md SHALL describe best practices for writing effective skills, including clear trigger conditions, guardrails, and checklist-driven workflows.

#### Scenario: Engineer writes a reliable skill

- **WHEN** an engineer reads the best practices section
- **THEN** they SHALL understand how to write skills that produce consistent and predictable behavior

### Requirement: Document skill sharing

SKILL.md SHALL explain how to share skills across a team, including version control integration and the use of skill packages.

#### Scenario: Team shares a skill

- **WHEN** an engineer reads the sharing section
- **THEN** they SHALL be able to commit a skill to source control and make it available to all team members
