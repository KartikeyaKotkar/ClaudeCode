# Command Reference

This document describes the command surface visible in this source snapshot.

It is intentionally conservative:

- It documents commands registered in [src/commands.ts](/home/kk/Downloads/ClaudeCode/src/commands.ts).
- It uses names, aliases, descriptions, and visibility rules that are directly present in source.
- It does not invent full CLI syntax for commands whose argument handling lives in lazily loaded modules.

## How Commands Are Assembled

Built-in commands are registered in [src/commands.ts](/home/kk/Downloads/ClaudeCode/src/commands.ts).

At runtime, `getCommands()` can also add:

- bundled skills
- built-in plugin skills
- user/project skill-directory commands
- workflow commands
- plugin commands
- plugin skills
- dynamic skills discovered during file operations

So the list below is the built-in command surface from this snapshot, not the complete runtime surface for every session.

## User-Facing Built-In Commands

### Session and Conversation

| Command | Aliases | Verified purpose | Notes |
| --- | --- | --- | --- |
| `/add-dir` |  | Add a new working directory | |
| `/branch` |  | Create a branch of the current conversation at this point | Conversation branch, not git branch creation |
| `/clear` | `reset`, `new` | Clear conversation history and free up context | |
| `/copy` |  | Copy Claude's last response to clipboard (or `/copy N` for the Nth-latest) | |
| `/export` |  | Export the current conversation to a file or clipboard | |
| `/rename` |  | Rename the current conversation | Immediate command |
| `/resume` | `continue` | Resume a previous conversation | |
| `/rewind` | `checkpoint` | Restore the code and/or conversation to a previous point | Interactive only |
| `/tag` |  | Toggle a searchable tag on the current session | Enabled only for `USER_TYPE=ant` |

### Context, Planning, and Memory

| Command | Aliases | Verified purpose | Notes |
| --- | --- | --- | --- |
| `/context` |  | Visualize current context usage as a colored grid | Interactive mode |
| `/context` |  | Show current context usage | Non-interactive variant |
| `/files` |  | List all files currently in context | Enabled only for `USER_TYPE=ant` |
| `/init` |  | Initialize a new `CLAUDE.md` file with codebase documentation | Description becomes broader when `NEW_INIT` path is enabled |
| `/memory` |  | Edit Claude memory files | |
| `/plan` |  | Enable plan mode or view the current session plan | |

### Model and Runtime Configuration

| Command | Aliases | Verified purpose | Notes |
| --- | --- | --- | --- |
| `/advisor` |  | Configure the advisor model | Only enabled when advisor configuration is allowed |
| `/color` |  | Set the prompt bar color for this session | |
| `/compact` |  | Clear conversation history and free up context | Compaction-oriented command; source description is this exact text |
| `/config` | `settings` | Open config panel | |
| `/effort` |  | Set effort level for model usage | |
| `/fast` |  | Toggle fast mode | Only enabled for supported accounts and when fast mode is available |
| `/model` |  | Set the AI model for Claude Code | Description is dynamic and shows current model |
| `/output-style` |  | Deprecated: use `/config` to change output style | Hidden |
| `/permissions` | `allowed-tools` | Manage allow & deny tool permission rules | |
| `/sandbox` |  | Configure shell sandboxing state | Hidden on unsupported platforms |
| `/theme` |  | Change the theme | |
| `/vim` |  | Toggle between Vim and Normal editing modes | |

### Status, Usage, and Diagnostics

| Command | Aliases | Verified purpose | Notes |
| --- | --- | --- | --- |
| `/cost` |  | Show the total cost and duration of the current session | Hidden behavior depends on runtime state |
| `/doctor` |  | Diagnose and verify your Claude Code installation and settings | Can be disabled by env var |
| `/help` |  | Show help and available commands | |
| `/insights` |  | Generate a report analyzing your Claude Code sessions | |
| `/stats` |  | Show your Claude Code usage statistics and activity | |
| `/status` |  | Show Claude Code status including version, model, account, API connectivity, and tool statuses | Immediate command |
| `/usage` |  | Show plan usage limits | Claude.ai availability only |

### Integrations and Platform

| Command | Aliases | Verified purpose | Notes |
| --- | --- | --- | --- |
| `/agents` |  | Manage agent configurations | |
| `/chrome` |  | Claude in Chrome (Beta) settings | Interactive only |
| `/desktop` | `app` | Continue the current session in Claude Desktop | Claude.ai only; supported platforms only |
| `/hooks` |  | View hook configurations for tool events | Immediate command |
| `/ide` |  | Manage IDE integrations and show status | |
| `/install-github-app` |  | Set up Claude GitHub Actions for a repository | |
| `/install-slack-app` |  | Install the Claude Slack app | |
| `/keybindings` |  | Open or create your keybindings configuration file | Only enabled when keybinding customization is enabled |
| `/mcp` |  | Manage MCP servers | Immediate command |
| `/mobile` | `ios`, `android` | Show QR code to download the Claude mobile app | |
| `/plugin` | `plugins`, `marketplace` | Manage Claude Code plugins | Immediate command |
| `/pr-comments` |  | Get comments from a GitHub pull request | |
| `/reload-plugins` |  | Activate pending plugin changes in the current session | |
| `/remote-control` | `rc` | Connect this terminal for remote-control sessions | Hidden unless bridge mode is enabled |
| `/remote-env` |  | Configure the default remote environment for teleport sessions | Hidden unless remote sessions are allowed |
| `/session` | `remote` | Show remote session URL and QR code | Only enabled in remote mode |
| `/skills` |  | List available skills | |
| `/statusline` |  | Set up Claude Code's status line UI | Prompt command that delegates to an agent |
| `/tasks` | `bashes` | List and manage background tasks | |
| `/voice` |  | Toggle voice mode | Claude.ai only; feature-gated |
| `/web-setup` |  | Setup Claude Code on the web (requires connecting your GitHub account) | Claude.ai only; feature and policy gated |

### Account, Access, and Commercial Flows

| Command | Aliases | Verified purpose | Notes |
| --- | --- | --- | --- |
| `/extra-usage` |  | Configure extra usage to keep working when limits are hit | Has interactive and non-interactive variants |
| `/login` |  | Sign in with your Anthropic account | Description changes to "Switch Anthropic accounts" when API-key auth is present |
| `/logout` |  | Sign out from your Anthropic account | |
| `/passes` |  | Share a free week of Claude Code with friends | Hidden unless passes eligibility is cached and allowed |
| `/privacy-settings` |  | View and update your privacy settings | Enabled only for consumer subscribers |
| `/upgrade` |  | Upgrade to Max for higher rate limits and more Opus | Claude.ai only; disabled for enterprise |

### Review and Repository Workflows

| Command | Aliases | Verified purpose | Notes |
| --- | --- | --- | --- |
| `/btw` |  | Ask a quick side question without interrupting the main conversation | Immediate command |
| `/diff` |  | View uncommitted changes and per-turn diffs | |
| `/feedback` | `bug` | Send feedback / bug report flow | Enabled only in supported contexts |
| `/release-notes` |  | View release notes | |
| `/review` |  | Review a pull request | Prompt command |
| `/security-review` |  | Complete a security review of the pending changes on the current branch | Implemented as a moved-to-plugin style command wrapper |
| `/stickers` |  | Order Claude Code stickers | |
| `/terminal-setup` |  | Install Shift+Enter key binding for newlines | On Apple Terminal, description changes to enabling Option+Enter and visual bell; hidden in terminals with native support |

### Seasonal or Feature-Gated Commands

| Command | Aliases | Verified purpose | Notes |
| --- | --- | --- | --- |
| `/think-back` |  | Your 2025 Claude Code Year in Review | Feature-gated |
| `/thinkback-play` |  | Play the thinkback animation | Hidden helper command |
| `/ultrareview` |  | Finds and verifies bugs in your branch; runs in Claude Code on the web | Enabled only when ultrareview is available |

## Hidden, Internal, or Special-Purpose Built-Ins

These commands are present in source but are hidden, internal-only, or clearly not part of the normal external command surface.

### Hidden or conditional helpers

| Command | Reason visible in source |
| --- | --- |
| `/output-style` | Hidden and deprecated |
| `/rate-limit-options` | Hidden; internal rate-limit flow |
| `/heapdump` | Hidden |
| `/thinkback-play` | Hidden helper for thinkback flow |

### Internal-only commands referenced in `INTERNAL_ONLY_COMMANDS`

These are explicitly assembled into the command list only for certain internal runtime conditions in [src/commands.ts](/home/kk/Downloads/ClaudeCode/src/commands.ts).

| Command name in source | Notes |
| --- | --- |
| `/commit` | Create a git commit |
| `/commit-push-pr` | Commit, push, and open a PR |
| `/version` | Internal-only command object in this snapshot |
| `/bridge-kick` | Manual bridge failure injection for testing |
| `/autofix-pr` | Present in registry as internal-only path |
| `/issue` | Present as stub in this snapshot |
| `/bughunter` | Present as stub in this snapshot |
| `/backfill-sessions` | Present as stub in this snapshot |
| `/ctx_viz` | Present as stub in this snapshot |
| `/good-claude` | Present as stub in this snapshot |
| `/init-verifiers` | Internal helper command |
| `/summary` | Present as stub in this snapshot |
| `/env` | Present as stub in this snapshot |
| `/oauth-refresh` | Present as stub in this snapshot |
| `/debug-tool-call` | Present as stub in this snapshot |
| `/ant-trace` | Present as stub in this snapshot |
| `/perf-issue` | Present as stub in this snapshot |

### Stubbed placeholders in this snapshot

Several command directories export stub commands with `isEnabled: () => false` and `isHidden: true`. In this snapshot that includes at least:

- `backfill-sessions`
- `onboarding`
- `good-claude`
- `ctx_viz`
- `teleport`
- `env`
- `oauth-refresh`
- `share`
- `autofix-pr`
- `summary`
- `bughunter`
- `issue`
- `perf-issue`
- `mock-limits`
- `debug-tool-call`
- `break-cache`
- `reset-limits`

## Command Behavior Notes

- Some commands expose the same name with different interactive and non-interactive implementations, such as `/context` and `/extra-usage`.
- Some commands are only available to specific account types via `availability`, such as Claude.ai-only commands.
- Some commands are feature-gated, policy-gated, platform-gated, or hidden unless the environment is in a particular mode.
- The command list shown in the UI can therefore be smaller than the full built-in registry in source.

## Related Files

- [src/commands.ts](/home/kk/Downloads/ClaudeCode/src/commands.ts)
- [src/main.tsx](/home/kk/Downloads/ClaudeCode/src/main.tsx)
- [src/skills/loadSkillsDir.ts](/home/kk/Downloads/ClaudeCode/src/skills/loadSkillsDir.ts)
- [src/utils/plugins/loadPluginCommands.ts](/home/kk/Downloads/ClaudeCode/src/utils/plugins/loadPluginCommands.ts)
