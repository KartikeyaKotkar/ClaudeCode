# Tool Reference

This document describes the model-callable tool surface visible in this source snapshot.

It is intentionally conservative:

- It is based on [src/tools.ts]( /src/tools.ts) and the tool modules that file registers.
- It uses verified runtime tool names from constants or `buildTool({ name: ... })`.
- It summarizes purpose from tool descriptions, prompts, and search hints present in source.
- It does not invent input schemas beyond what is obvious from names and documented prompt text.

## How Tool Availability Works

The source of truth for tool registration is [src/tools.ts]( /src/tools.ts).

Important behaviors visible there:

- `getAllBaseTools()` defines the exhaustive tool registry for the current environment.
- `getToolsForDefaultPreset()` returns the enabled subset for the default preset.
- `getTools()` further filters tools by deny rules and special runtime modes.
- `CLAUDE_CODE_SIMPLE` reduces the active tool set to a minimal mode.
- REPL mode hides primitive tools behind the REPL wrapper.
- Some tools are included only when feature flags, environment flags, account capabilities, or policy gates are enabled.
- `Glob` and `Grep` are omitted when embedded search tools are available.

## Core Tool Surface

These are the main tools that are directly registered in the default base tool list when their local enablement rules pass.

### File, Search, and Shell Tools

| Tool name | Verified purpose | Source |
| --- | --- | --- |
| `Bash` | Execute shell commands | [src/tools/BashTool/toolName.ts]( /src/tools/BashTool/toolName.ts) |
| `Read` | Read files, images, PDFs, and notebooks from the local filesystem | [src/tools/FileReadTool/prompt.ts]( /src/tools/FileReadTool/prompt.ts) |
| `Edit` | Perform exact string replacements in files | [src/tools/FileEditTool/constants.ts]( /src/tools/FileEditTool/constants.ts) |
| `Write` | Create or overwrite files on the local filesystem | [src/tools/FileWriteTool/prompt.ts]( /src/tools/FileWriteTool/prompt.ts) |
| `Glob` | Find files by name pattern or wildcard | [src/tools/GlobTool/prompt.ts]( /src/tools/GlobTool/prompt.ts) |
| `Grep` | Search file contents with ripgrep and regex | [src/tools/GrepTool/prompt.ts]( /src/tools/GrepTool/prompt.ts) |
| `NotebookEdit` | Replace, insert, or delete notebook cell contents in `.ipynb` files | [src/tools/NotebookEditTool/prompt.ts]( /src/tools/NotebookEditTool/prompt.ts) |
| `PowerShell` | Execute Windows PowerShell commands | [src/tools/PowerShellTool/toolName.ts]( /src/tools/PowerShellTool/toolName.ts) |

### Web and External Content

| Tool name | Verified purpose | Source |
| --- | --- | --- |
| `WebFetch` | Fetch a URL, convert HTML to markdown, and answer a prompt about the content | [src/tools/WebFetchTool/prompt.ts]( /src/tools/WebFetchTool/prompt.ts) |
| `web_search` | Perform web search | [src/tools/WebSearchTool/WebSearchTool.ts]( /src/tools/WebSearchTool/WebSearchTool.ts) |
| `ListMcpResourcesTool` | List available resources from configured MCP servers | [src/tools/ListMcpResourcesTool/prompt.ts]( /src/tools/ListMcpResourcesTool/prompt.ts) |
| `ReadMcpResourceTool` | Read a specific MCP resource by server name and URI | [src/tools/ReadMcpResourceTool/prompt.ts]( /src/tools/ReadMcpResourceTool/prompt.ts) |
| `ToolSearch` | Search the currently available tool set | [src/tools/ToolSearchTool/ToolSearchTool.ts]( /src/tools/ToolSearchTool/ToolSearchTool.ts) |

### Agent, Skill, and User-Interaction Tools

| Tool name | Verified purpose | Source |
| --- | --- | --- |
| `Agent` | Launch a new agent / delegate work to a subagent | [src/tools/AgentTool/AgentTool.tsx]( /src/tools/AgentTool/AgentTool.tsx) |
| `Task` | Legacy alias for `Agent` | [src/tools/AgentTool/constants.ts]( /src/tools/AgentTool/constants.ts) |
| `AskUserQuestion` | Ask the user multiple-choice questions | [src/tools/AskUserQuestionTool/prompt.ts]( /src/tools/AskUserQuestionTool/prompt.ts) |
| `Skill` | Invoke a slash-command skill | [src/tools/SkillTool/SkillTool.ts]( /src/tools/SkillTool/SkillTool.ts) |
| `SendUserMessage` | Send a message to the user | [src/tools/BriefTool/prompt.ts]( /src/tools/BriefTool/prompt.ts) |
| `Brief` | Legacy brief-tool name | [src/tools/BriefTool/prompt.ts]( /src/tools/BriefTool/prompt.ts) |

### Planning, Tasks, and Session Control

| Tool name | Verified purpose | Source |
| --- | --- | --- |
| `EnterPlanMode` | Request permission to enter plan mode for complex tasks requiring exploration and design | [src/tools/EnterPlanModeTool/EnterPlanModeTool.ts]( /src/tools/EnterPlanModeTool/EnterPlanModeTool.ts) |
| `ExitPlanModeV2` | Exit plan mode | [src/tools.ts]( /src/tools.ts) |
| `TodoWrite` | Create and manage a structured task list for the current session | [src/tools/TodoWriteTool/prompt.ts]( /src/tools/TodoWriteTool/prompt.ts) |
| `TaskStop` | Stop a running background task by ID | [src/tools/TaskStopTool/prompt.ts]( /src/tools/TaskStopTool/prompt.ts) |
| `TaskOutput` | Read output or logs from a background task | [src/tools/TaskOutputTool/TaskOutputTool.tsx]( /src/tools/TaskOutputTool/TaskOutputTool.tsx) |
| `TaskCreate` | Create a task in the task list | [src/tools/TaskCreateTool/TaskCreateTool.ts]( /src/tools/TaskCreateTool/TaskCreateTool.ts) |
| `TaskGet` | Retrieve a task by ID | [src/tools/TaskGetTool/TaskGetTool.ts]( /src/tools/TaskGetTool/TaskGetTool.ts) |
| `TaskUpdate` | Update a task | [src/tools/TaskUpdateTool/TaskUpdateTool.ts]( /src/tools/TaskUpdateTool/TaskUpdateTool.ts) |
| `TaskList` | List all tasks | [src/tools/TaskListTool/TaskListTool.ts]( /src/tools/TaskListTool/TaskListTool.ts) |

### Configuration, IDE, and Worktree Tools

| Tool name | Verified purpose | Source |
| --- | --- | --- |
| `Config` | Get or set Claude Code configuration settings | [src/tools/ConfigTool/prompt.ts]( /src/tools/ConfigTool/prompt.ts) |
| `LSP` | Interact with Language Server Protocol servers for code intelligence | [src/tools/LSPTool/prompt.ts]( /src/tools/LSPTool/prompt.ts) |
| `EnterWorktree` | Create an isolated git worktree and switch into it | [src/tools/EnterWorktreeTool/EnterWorktreeTool.ts]( /src/tools/EnterWorktreeTool/EnterWorktreeTool.ts) |
| `ExitWorktree` | Exit a worktree session and return to the original directory | [src/tools/ExitWorktreeTool/ExitWorktreeTool.ts]( /src/tools/ExitWorktreeTool/ExitWorktreeTool.ts) |

### Team and Swarm Coordination

| Tool name | Verified purpose | Source |
| --- | --- | --- |
| `SendMessage` | Send messages to agent teammates using the swarm protocol | [src/tools/SendMessageTool/SendMessageTool.ts]( /src/tools/SendMessageTool/SendMessageTool.ts) |
| `TeamCreate` | Create a new team for coordinating multiple agents | [src/tools/TeamCreateTool/TeamCreateTool.ts]( /src/tools/TeamCreateTool/TeamCreateTool.ts) |
| `TeamDelete` | Clean up team and task directories when the swarm is complete | [src/tools/TeamDeleteTool/TeamDeleteTool.ts]( /src/tools/TeamDeleteTool/TeamDeleteTool.ts) |

## Runtime Notes for Core Tools

### Read/Edit/Write relationship

The file tools have explicit coordination rules:

- `Edit` requires the file to have been read first.
- `Write` also requires a prior `Read` when overwriting an existing file.
- `Write` is intended for new files or complete rewrites; `Edit` is preferred for in-place changes.

Relevant files:

- [src/tools/FileEditTool/prompt.ts]( /src/tools/FileEditTool/prompt.ts)
- [src/tools/FileWriteTool/prompt.ts]( /src/tools/FileWriteTool/prompt.ts)

### Search tools vs shell search

The source explicitly prefers dedicated search tools over shell search:

- `Grep` should be used for content search instead of shell `grep`/`rg`.
- `Glob` should be used for filename pattern matching.
- Open-ended multi-step search can be handed off to `Agent`.

Relevant files:

- [src/tools/GrepTool/prompt.ts]( /src/tools/GrepTool/prompt.ts)
- [src/tools/GlobTool/prompt.ts]( /src/tools/GlobTool/prompt.ts)

### Plan mode tools

The planning flow is explicit in source:

- `EnterPlanMode` is for non-trivial implementation tasks that need exploration and design approval.
- `AskUserQuestion` should be used for clarifying requirements, but plan approval itself should go through the plan-mode flow.

Relevant files:

- [src/tools/EnterPlanModeTool/prompt.ts]( /src/tools/EnterPlanModeTool/prompt.ts)
- [src/tools/AskUserQuestionTool/prompt.ts]( /src/tools/AskUserQuestionTool/prompt.ts)

### Task tracking split

This snapshot shows two overlapping task-tracking systems:

- `TodoWrite` for the session checklist style flow
- `TaskCreate` / `TaskGet` / `TaskUpdate` / `TaskList` for task-list V2 flows

The registry only includes the V2 task tools when `isTodoV2Enabled()` is true.

## Conditional Tools Referenced by the Registry

`src/tools.ts` also conditionally references additional tools. Some are present in this snapshot; some are only referenced by require-path and their implementation files are not included here.

### Conditional tools with visible implementations in this snapshot

| Tool/module | Condition visible in `src/tools.ts` |
| --- | --- |
| `PowerShell` | Included only when PowerShell support is enabled |
| `LSP` | Included only when `ENABLE_LSP_TOOL` is truthy |
| `EnterWorktree` / `ExitWorktree` | Included only when worktree mode is enabled |
| `TaskCreate` / `TaskGet` / `TaskUpdate` / `TaskList` | Included only when Todo V2 is enabled |
| `ToolSearch` | Included only when tool search is optimistically enabled |
| `TeamCreate` / `TeamDelete` | Included only when agent swarms are enabled |

### Conditional tools referenced in the registry but not documented here in depth

These names are directly referenced in [src/tools.ts]( /src/tools.ts), but this snapshot does not provide enough local implementation detail to document them to the same standard:

- `TungstenTool`
- `SuggestBackgroundPRTool`
- `WebBrowserTool`
- `OverflowTestTool`
- `CtxInspectTool`
- `TerminalCaptureTool`
- `REPLTool`
- `WorkflowTool`
- `SleepTool`
- `CronCreateTool`
- `CronDeleteTool`
- `CronListTool`
- `RemoteTriggerTool`
- `MonitorTool`
- `SendUserFileTool`
- `PushNotificationTool`
- `SubscribePRTool`
- `TestingPermissionTool`

## Special Registry Behaviors

### Simple mode

When `CLAUDE_CODE_SIMPLE` is set, the active tool set is reduced to:

- `Bash`
- `Read`
- `Edit`

If coordinator mode is also active, the source adds:

- `Agent`
- `TaskStop`
- `SendMessage`

Relevant file:

- [src/tools.ts]( /src/tools.ts)

### Deny-rule filtering

Before tools are exposed to the model, `filterToolsByDenyRules()` removes tools covered by blanket deny rules in the permission context. The code explicitly notes that this also applies to MCP server-prefix rules.

Relevant file:

- [src/tools.ts]( /src/tools.ts)

### REPL mode

When REPL mode is enabled, primitive tools can be hidden from direct use and accessed through the REPL wrapper instead.

Relevant files:

- [src/tools.ts]( /src/tools.ts)
- [src/tools/REPLTool/constants.ts]( /src/tools/REPLTool/constants.ts)

## Related Files

- [src/tools.ts]( /src/tools.ts)
- [src/Tool.ts]( /src/Tool.ts)
- [src/tasks/]( /src/tasks/)
