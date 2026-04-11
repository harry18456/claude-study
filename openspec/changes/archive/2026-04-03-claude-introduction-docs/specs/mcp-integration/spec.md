## ADDED Requirements

### Requirement: Document MCP protocol fundamentals

MCP.md SHALL explain the Model Context Protocol concept: what it is, how it differs from the Claude API, and why it exists (enabling Claude to interact with external tools and data sources).

#### Scenario: Engineer understands MCP purpose

- **WHEN** an engineer reads the fundamentals section
- **THEN** they SHALL be able to explain the difference between calling Claude via API and connecting Claude to external tools via MCP

### Requirement: Document MCP server types

MCP.md SHALL describe the available MCP server categories including file system, databases, web services, and development tools, with examples of each.

#### Scenario: Engineer selects an MCP server

- **WHEN** an engineer wants to connect Claude to an external system
- **THEN** MCP.md SHALL provide enough context to identify whether an existing MCP server exists for that system

### Requirement: Document MCP configuration in Claude Code

MCP.md SHALL explain how to configure MCP servers in Claude Code via settings, including the configuration format and how to verify server connectivity.

#### Scenario: Engineer adds an MCP server to Claude Code

- **WHEN** an engineer reads the Claude Code MCP configuration section
- **THEN** they SHALL be able to add and verify an MCP server connection in their Claude Code setup

### Requirement: Document MCP configuration in Claude.ai

MCP.md SHALL explain how to configure and use MCP servers in the Claude.ai web interface.

#### Scenario: Engineer uses MCP in Claude.ai

- **WHEN** an engineer reads the Claude.ai MCP section
- **THEN** they SHALL be able to connect an MCP server to Claude.ai and use it in conversations
