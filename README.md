# ClaudeCode Source Overview

This directory is a source snapshot of a large TypeScript codebase for a terminal-first AI coding assistant. The code is not a small library; it is an application platform with:

- a CLI entrypoint
- an Ink/React terminal UI
- built-in commands
- model-callable tools for shell, files, search, notebooks, MCP, and agent delegation
- plugin and skill loading
- local, background, and remote task execution
- optional remote-control and bridge features

This README is based only on the source files present in this snapshot. It avoids claims about packaging, release process, or exact install commands because the repository root metadata (`package.json`, lockfiles, CI config, etc.) is not included here.

## What This Code Can Do

From the inspected source, this application can support all of the following:

- Run as an interactive CLI assistant with a terminal UI.
- Execute shell commands through a controlled Bash or PowerShell tool layer.
- Read, edit, and write files, including notebook-aware editing.
- Search code and files with dedicated grep/glob style tools.
- Use web fetch and web search tools.
- Load and execute slash-style commands.
- Load reusable skills from markdown-based skill definitions.
- Load plugins that can contribute commands, skills, hooks, agents, and MCP servers.
- Connect to MCP servers, list resources, and read MCP resources.
- Spawn and manage subagents or teammate-style agents.
- Track background tasks and task output.
- Support planning modes and todo/task management.
- Capture repository context such as git status and recent commits for the assistant prompt.
- Support IDE integrations, remote sessions, and remote-control / bridge style workflows.
- Apply settings, policy limits, telemetry, authentication, and managed configuration during startup.

## Verified Technical Shape

The following points are directly supported by the source:

- `src/entrypoints/cli.tsx` is the bootstrap CLI entrypoint.
- `src/main.tsx` is the main application startup path and command dispatch layer.
- `src/commands.ts` registers a large built-in command surface.
- `src/tools.ts` registers the model-callable tool surface.
- The UI uses React with Ink-style terminal rendering.
- The code uses Bun build-time feature flags via `bun:bundle`.
- Skills are markdown/frontmatter driven and loaded from configured directories.
- Plugins are loaded from plugin manifests and can contribute markdown-defined commands and skills.
- MCP is a first-class integration, not an afterthought.
- Multiple task types exist for local shell work, local agents, remote agents, and teammate/background workflows.

## Main Capabilities By Area

### 1. Interactive CLI and TUI

The application is built as a terminal product, not just a headless SDK.

Relevant files:

- `src/entrypoints/cli.tsx`
- `src/main.tsx`
- `src/interactiveHelpers.tsx`
- `src/ink/*`
- `src/components/*`

What this means:

- fast CLI bootstrap paths
- interactive rendering in terminal
- command dispatch and session management
- support for both interactive and non-interactive operation

### 2. Slash Commands and Operator Commands

There is a large command registry in `src/commands.ts`, backed by many command modules under `src/commands/`.

Examples of command areas visible from the tree:

- config and permissions
- login/logout and OAuth refresh
- plugin management
- MCP management
- skills, hooks, and keybindings
- session/resume/rewind/history style commands
- review, diff, branch, files, and summary style commands
- remote setup, bridge, teleport, and environment commands

This suggests the product is intended both for daily agent use and for operator/admin workflows.

### 3. Tooling Available To The Model

`src/tools.ts` shows a broad internal tool API exposed to the model layer.

Verified tool categories include:

- shell execution
- file read/edit/write
- notebook edit
- grep/glob search
- web fetch and web search
- skills
- ask-user-question
- MCP resource listing and reading
- LSP access
- todo/task tools
- agent/team messaging and task output
- plan mode entry/exit

This is the part of the system that makes the assistant operational rather than purely conversational.

### 4. Agent and Task Orchestration

The codebase contains explicit task types:

- `src/tasks/LocalShellTask/`
- `src/tasks/LocalAgentTask/`
- `src/tasks/RemoteAgentTask/`
- `src/tasks/InProcessTeammateTask/`
- `src/tasks/LocalMainSessionTask.ts`

This indicates the runtime can manage more than one unit of work and present progress/status in the UI.

### 5. Plugins and Skills

The application supports extension in at least two ways:

- skills loaded from markdown/frontmatter
- plugins that can add commands, skills, hooks, agents, and MCP integrations

Relevant files:

- `src/skills/loadSkillsDir.ts`
- `src/skills/bundledSkills.ts`
- `src/utils/plugins/loadPluginCommands.ts`
- `src/cli/handlers/plugins.ts`

This is a strong indicator that the product is designed as an extensible agent platform, not only a fixed assistant binary.

### 6. MCP Integration

MCP support is deeply integrated:

- dedicated CLI handlers under `src/cli/handlers/mcp.tsx`
- MCP services under `src/services/mcp/`
- MCP-related tools in `src/tools/`
- MCP-backed skills via `src/skills/mcpSkillBuilders.ts`

So this source can act as both an MCP client and, through `src/entrypoints/mcp.ts`, provide MCP server functionality.

### 7. Remote Control, Bridge, and Background Sessions

The CLI bootstrap contains dedicated fast paths for:

- remote-control / bridge mode
- daemon workers
- background session management

Relevant files:

- `src/entrypoints/cli.tsx`
- `src/bridge/*`
- `src/cli/bg.js` referenced by the CLI entrypoint

This means the code is built to handle local interactive use and longer-lived remote/session workflows.

## High-Level Architecture

At a high level, the flow appears to be:

1. `src/entrypoints/cli.tsx` handles very early boot and fast-path flags.
2. `src/main.tsx` initializes config, auth, telemetry, policies, skills/plugins, MCP, and rendering.
3. `src/commands.ts` determines the available slash/operator commands.
4. `src/tools.ts` determines the model-callable tool surface.
5. Task and UI layers execute work and display progress/results.

## Source Tree Guide

Useful directories in this snapshot:

- `src/commands/` - user-facing command implementations
- `src/tools/` - model-callable tool implementations
- `src/tasks/` - execution units for shell, agent, remote, and teammate work
- `src/bridge/` - remote-control / bridge logic
- `src/services/` - shared services such as analytics, API clients, MCP, plugins, LSP, and settings
- `src/skills/` - bundled and loaded skills
- `src/plugins/` - built-in plugin registration
- `src/components/` - terminal UI components
- `src/ink/` - terminal rendering/runtime layer
- `src/context/` and `src/context.ts` - prompt/session context assembly
- `src/migrations/` - config/settings migrations

## Important Limits Of This Snapshot

These points are intentionally conservative:

- No root `package.json` is present here, so exact build/test scripts are not documented.
- No top-level README or release docs are present here, so product naming and external usage conventions may be incomplete.
- Some commands and tools are feature-flagged or internal-only, so presence in source does not guarantee they are enabled in every build.
- Some modules reference files not present in this snapshot, which suggests this is not the full repository root.

## Recommended Next Documentation Files

If you want this codebase documented further, the next high-value docs would be:

- an operator guide for commands and modes
- a tool reference grouped by capability and risk level
- a plugin authoring guide
- a skill authoring guide
- an MCP integration guide
- a startup and architecture deep-dive for contributors

See `docs/ARCHITECTURE.md` for a contributor-oriented map based on the same verified source inspection.
See `docs/COMMANDS.md` for a built-in command reference based on `src/commands.ts` and the registered command modules.
See `docs/TOOLS.md` for a tool reference based on `src/tools.ts` and the registered tool modules.
