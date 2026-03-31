# Architecture Notes

This document describes the architecture visible in the current source snapshot. It is intentionally limited to what can be verified from the checked-in files.

## 1. Runtime Layers

### Bootstrap

`src/entrypoints/cli.tsx` is the earliest CLI bootstrap layer.

Observed responsibilities:

- environment setup for specific modes
- fast-path handling for version and special flags
- remote-control / bridge boot
- daemon worker boot
- background session command dispatch
- MCP server mode dispatch

This file exists to keep startup fast and avoid loading the full application for simple code paths.

### Main Application Startup

`src/main.tsx` is the main orchestration layer once the full app is needed.

Observed responsibilities:

- startup profiling
- config loading
- trust and managed environment setup
- telemetry and analytics initialization
- auth/bootstrap/session preparation
- loading commands and tools
- plugin and skill initialization
- MCP and LSP related setup
- rendering and interactive flow setup

### Initialization Support

`src/entrypoints/init.ts` contains early initialization that is shared across runtime paths.

Observed responsibilities:

- enabling config system
- applying safe environment variables
- graceful shutdown registration
- remote managed settings initialization
- policy limits initialization
- proxy / mTLS / CA certificate setup
- OAuth/account bootstrap helpers

## 2. Command Model

`src/commands.ts` acts as the central registry for built-in commands.

The command model appears to support:

- built-in commands
- dynamically loaded skills as commands
- plugin-provided commands
- feature-flagged commands
- internal-only commands

This means command availability is assembled from multiple sources rather than hardcoded in one static list.

## 3. Tool Model

`src/tools.ts` is the central registry of tools available to the agent/model.

Verified tool families:

- shell tools
- file tools
- search tools
- notebook tools
- web tools
- MCP tools
- LSP tools
- planning and todo/task tools
- skill invocation
- agent/team communication tools

The tool registry also applies environment and feature-flag checks, so the available toolset is runtime-dependent.

## 4. Context Assembly

`src/context.ts` builds conversation context for the model.

Verified context inputs:

- git state
- recent commit log
- git user name
- memory/CLAUDE.md style project context
- current date

The design intent is clear: before the model acts, it receives operational context about the repository and session.

## 5. Task Execution Model

The task system separates kinds of work into distinct runtime units:

- `LocalShellTask`
- `LocalAgentTask`
- `RemoteAgentTask`
- `InProcessTeammateTask`
- `LocalMainSessionTask`

This suggests:

- tool actions may become tracked background/foreground jobs
- the UI can show task progress and detail
- the system can represent multiple concurrent execution paths

## 6. Extensibility Model

### Skills

`src/skills/loadSkillsDir.ts` shows a markdown/frontmatter based skill-loading system.

Verified properties:

- skills can be loaded from multiple scopes
- skills can declare metadata in frontmatter
- skills can define allowed tools, arguments, models, effort, hooks, and execution context
- bundled skills ship in source
- MCP can contribute skills

### Plugins

`src/utils/plugins/loadPluginCommands.ts` and `src/cli/handlers/plugins.ts` show a plugin system with CLI management.

Verified plugin capabilities:

- install/enable/disable/uninstall/update
- validate manifests and plugin contents
- load plugin commands from markdown
- support plugin skills
- integrate MCP servers from plugins

## 7. External Integration Surfaces

### MCP

MCP is deeply embedded across:

- `src/entrypoints/mcp.ts`
- `src/cli/handlers/mcp.tsx`
- `src/services/mcp/`
- MCP-related tools and skill builders

This appears to support:

- configuring MCP servers
- checking their health
- connecting to them
- exposing their resources/tools inside the assistant

### IDE / Editor Support

The code contains explicit IDE-oriented modules and commands:

- `src/commands/ide/`
- JetBrains detection helpers
- IDE connection hooks/utilities

### Remote / Bridge

The `src/bridge/` subtree and CLI fast paths indicate a substantial remote-control subsystem rather than a minimal helper.

## 8. Contributor Orientation

For a new contributor, the most useful reading order is:

1. `src/entrypoints/cli.tsx`
2. `src/main.tsx`
3. `src/entrypoints/init.ts`
4. `src/commands.ts`
5. `src/tools.ts`
6. `src/context.ts`
7. `src/tasks/`
8. `src/services/mcp/`, `src/skills/`, and `src/utils/plugins/`

## 9. What Is Not Safe To Assume

Because the repository root metadata is not present here, do not assume:

- exact install/build/test commands
- exact release artifact names
- exact product branding outside the source strings
- which feature flags are enabled in production

Those details should be documented only after inspecting the full repository root and release configuration.
