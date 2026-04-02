# Appendix - Source File Index

## 1. Purpose

This appendix maps major architecture topics to concrete source files for deep reading and implementation verification.

## 2. Entrypoints and Startup

- `src/entrypoints/cli.tsx`
- `src/entrypoints/init.ts`
- `src/entrypoints/mcp.ts`
- `src/main.tsx`
- `src/setup.ts`

## 3. Runtime Core

- `src/QueryEngine.ts`
- `src/query.ts`
- `src/queryLoop.ts`
- `src/bootstrap/state.ts`

## 4. Prompt and Context

- `src/constants/prompts.ts`
- `src/context.ts`

## 5. Tools and Governance

- `src/Tool.ts`
- `src/tools.ts`
- `src/services/tools/toolOrchestration.ts`
- `src/services/tools/toolExecution.ts`
- `src/hooks/useCanUseTool.tsx`
- `src/utils/hooks.ts`

## 6. Agent Runtime

- `src/tools/AgentTool/AgentTool.tsx`
- `src/tools/AgentTool/runAgent.ts`
- `src/tools/AgentTool/builtInAgents.ts`
- `src/tools/AgentTool/built-in/exploreAgent.ts`
- `src/tools/AgentTool/built-in/planAgent.ts`
- `src/tools/AgentTool/built-in/verificationAgent.ts`

## 7. Extension Ecosystem

- `src/commands.ts`
- `src/skills/loadSkillsDir.ts`
- `src/skills/bundled/index.ts`
- `src/utils/plugins/loadPluginCommands.ts`
- `src/plugins/bundled/index.ts`
- `src/services/mcp/client.ts`

## 8. Observability and Diagnostics

- `src/services/analytics/index.ts`
- `src/utils/telemetry/events.ts`
- `src/utils/diagLogs.ts`
- `src/utils/hooks.ts` (hook timing and event emissions)

## 9. Reading Order Recommendation

1. `entrypoints/cli.tsx` -> `main.tsx` -> `setup.ts`
2. `QueryEngine.ts` -> `query.ts` -> `queryLoop.ts`
3. `tools.ts` -> `toolOrchestration.ts` -> `toolExecution.ts`
4. `AgentTool.tsx` -> `runAgent.ts` -> built-in agent definitions
5. `prompts.ts` -> `context.ts`
6. `commands.ts` -> skill/plugin/mcp loaders

## 10. Chapter Links

- [Chapter 01 - System Overview and Architectural Principles](./chapter-01-system-overview.md)
- [Chapter 02 - Startup, Bootstrap, and Session Initialization](./chapter-02-startup-bootstrap.md)
- [Chapter 03 - Prompt Assembly and Context Architecture](./chapter-03-prompt-architecture.md)
- [Chapter 04 - Query Engine and Turn Execution Loop](./chapter-04-query-engine-and-turn-loop.md)
- [Chapter 05 - Tool Governance and Execution Pipeline](./chapter-05-tool-governance-and-execution.md)
- [Chapter 06 - Agent Orchestration and Subagent Runtime](./chapter-06-agent-orchestration.md)
- [Chapter 07 - Skills, Plugins, Hooks, and MCP Integration](./chapter-07-skills-plugins-hooks-mcp.md)
- [Chapter 08 - State Model, Context, and Memory Surfaces](./chapter-08-state-context-and-memory.md)
- [Chapter 09 - Permission, Security, and Runtime Safety](./chapter-09-permission-security-and-safety.md)
- [Chapter 10 - Operating Modes, Observability, and Performance](./chapter-10-operating-modes-observability-and-performance.md)
