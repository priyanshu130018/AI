# Agentic AI Course: 14 → 30

This is the separate **Agentic AI track** that comes after the GenAI/RAG track.

## Sequence

- 14_Tool Calling
- 15_Tool Calling with LangChain
- 16_Tool Calling with LangGraph
- 17_Agentic AI
- 18_Agent State
- 19_Agent Memory
- 20_Planning
- 21_ReAct / Action-Observation Loop
- 22_Reflection / Self-Correction
- 23_Human-in-the-Loop
- 24_Agent Evaluation
- 25_Agentic RAG
- 26_Multimodal Agent
- 27_MCP
- 28_Agent Security / Guardrails
- 29_Agent Observability
- 30_Multi-Agent Systems

Every notebook is:

```text
Notes
  ↓
Concept
  ↓
Architecture
  ↓
Code
  ↓
Evaluation
  ↓
Improvement
  ↓
Practice
```


## Where this track starts

Your supplied agent notebook already covers the core foundation:

```text
LLM
 ↓
Tools
 ↓
Tool Schema
 ↓
Tool Binding
 ↓
Tool Call
 ↓
Tool Execution
 ↓
ToolMessage
 ↓
Final Answer
 ↓
Agent Loop
```

It also covers tool errors, schema validation, the difference between tool calling and agents, and a multi-tool agent demo. fileciteturn19file0

The notebooks after #17 are the next engineering layers: state, memory, planning, reflection, HITL, evaluation, Agentic RAG, multimodal agents, MCP, security, observability, and multi-agent systems.



## Big mental model

```text
                 AGENTIC AI
                     │
              ┌──────┴──────┐
              ↓             ↓
            LLM           TOOLS
         decisions        actions
              │             │
              └──────┬──────┘
                     ↓
                 AGENT LOOP
                     ↓
             STATE + MEMORY
                     ↓
              PLANNING / REACT
                     ↓
          VERIFICATION / REFLECTION
                     ↓
          HUMAN APPROVAL WHEN NEEDED
                     ↓
          EVALUATION + OBSERVABILITY
                     ↓
          SECURITY + PRODUCTION
                     ↓
             MULTI-AGENT SYSTEMS



## Current framework notes

The notebooks intentionally use current patterns where the framework has moved on:
- LangChain's current agent entry point is `create_agent`.
- LangGraph supplies explicit state/graph/persistence/interrupt primitives.
- HITL workflows use interrupts/checkpointing.
- MCP's current Python SDK docs are on v2, with servers exposing tools/resources/prompts.

APIs can change, so model names and package versions are configurable rather than baked into every example.


- LangChain reference: https://reference.langchain.com/python/langchain/overview
- LangGraph reference: https://reference.langchain.com/python/langgraph/overview
- LangGraph persistence: https://github.com/langchain-ai/docs/blob/main/src/oss/langgraph/persistence.mdx
- LangGraph interrupts: https://github.com/langchain-ai/docs/blob/main/src/oss/langgraph/interrupts.mdx
- LangChain human-in-the-loop: https://github.com/langchain-ai/docs/blob/main/src/oss/langchain/human-in-the-loop.mdx
- Ragas metrics: https://docs.ragas.io/en/latest/concepts/metrics/available_metrics/
- MCP Python SDK v2: https://py.sdk.modelcontextprotocol.io/
- MCP Python SDK getting started: https://py.sdk.modelcontextprotocol.io/get-started/