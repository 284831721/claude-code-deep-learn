# Chapter 04 - Query Engine and Turn Execution Loop

## 1. Overview

The query loop is the execution heart of the platform. It coordinates model turns, tool calls, retries, state mutation, and completion signaling.

## 2. High-Level Runtime Contract

### 2.1 `QueryEngine` as Session Facade

`QueryEngine` encapsulates mutable per-session resources:

- message history
- read-file state caches
- permission denial history
- usage accounting
- abort control

### 2.2 `query` as Turn Loop Executor

`query(...)` and its internal loop manage one assistant turn end-to-end, including iterative tool-call rounds until terminal response.

## 3. Core Design Decisions

### 3.1 Generator-Based Streaming

The loop is modeled as async generators to emit intermediate events and incremental updates.

### 3.2 Long-Session Stability

Compaction and token budgeting are integrated into the loop to support effectively unbounded conversational workflows.

### 3.3 Runtime-Managed Abortability

Abort controllers are threaded through context to make long operations interruptible.

## 4. Low-Level Execution Anatomy

### 4.1 Message Submission Path

Typical path:

1. Caller invokes `QueryEngine.submitMessage(...)`.
2. Engine updates session state and metadata.
3. Engine invokes `query(...)`.
4. Loop drives model requests and processes model outputs.
5. Tool calls are delegated to tool orchestration.
6. Results are appended and loop continues.
7. Final assistant response is emitted.

### 4.2 Turn Loop Interactions

- prompt assembly
- model invocation
- tool call extraction
- tool orchestration
- hook and permission interactions
- usage/telemetry reporting

## 5. Diagrams

### 5.1 Turn Execution Sequence

```mermaid
sequenceDiagram
    participant Caller as CLI/SDK Caller
    participant QE as QueryEngine
    participant Q as query loop
    participant Model as LLM API
    participant TO as toolOrchestration
    participant TE as toolExecution

    Caller->>QE: submitMessage(userInput)
    QE->>Q: query(sessionState, messages, context)
    Q->>Model: request with assembled prompt + messages
    Model-->>Q: assistant output (+ optional tool calls)
    alt tool calls present
        Q->>TO: runTools(toolCalls)
        TO->>TE: execute each tool through pipeline
        TE-->>TO: tool results
        TO-->>Q: merged tool results
        Q->>Model: follow-up with tool results
        Model-->>Q: next assistant output
    end
    Q-->>QE: final turn output
    QE-->>Caller: streamed events + final response
```

### 5.2 Turn Lifecycle State Machine

```mermaid
stateDiagram-v2
    [*] --> Idle
    Idle --> ReceivingInput: submitMessage
    ReceivingInput --> BuildingPrompt
    BuildingPrompt --> CallingModel
    CallingModel --> ProcessingModelOutput
    ProcessingModelOutput --> RunningTools: tool_use emitted
    RunningTools --> CallingModel: tool results appended
    ProcessingModelOutput --> Finalizing: terminal response
    Finalizing --> Idle
    RunningTools --> Aborted: abort/error
    CallingModel --> Aborted: abort/error
    Aborted --> Idle
```

## 6. Source File Mapping

- `src/QueryEngine.ts`
- `src/query.ts`
- `src/queryLoop.ts`

## 7. Implementation Guidance

- Keep per-turn side effects in explicit lifecycle phases.
- Preserve deterministic ordering when appending model/tool messages.
- Add new turn-level policies in the loop, not ad-hoc in tool implementations.

## 8. Next Chapter

Continue with [Chapter 05 - Tool Governance and Execution Pipeline](./chapter-05-tool-governance-and-execution.md).
