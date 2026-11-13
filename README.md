# GenAI + RAG + Agentic AI Notes

This repository is a practical collection of Python and Jupyter Notebook
examples for learning NLP, GenAI, RAG, and Agentic AI.

## Repository folders

- [NLP](./NLP/) - Natural Language Processing workflows
- [GenAI](./GenAI/) - a 13-notebook GenAI and RAG course
- [RAG](./RAG/) - focused RAG experiments and implementations
- [AgenticAI](./AgenticAI/) - tool calling, agents, and multi-agent systems
- [langchain-academy](./langchain-academy/) - LangGraph and LangChain Academy lessons

## Getting started

Create a virtual environment and install the shared dependencies:

```powershell
python -m venv .venv
.\.venv\Scripts\Activate.ps1
pip install -r requirements.txt
```

Open the notebooks with Jupyter:

```powershell
jupyter notebook
```

Copy `.env.example` files to `.env` only when a notebook requires API keys.
Never commit secrets or generated local databases and vector stores.

## 0. Learning Goal

This note is a simple roadmap for learning:

1. Basic RAG
2. RAG using LangChain
3. Advanced RAG
4. Agentic AI
5. Tool-calling agents
6. Evaluation and improvement
7. The important topics that come after these

The uploaded learning material mainly covers **chunking, embeddings, vector search, FAISS, Chroma, RAG, LangChain RAG, tools, tool calling, and a basic agent loop**.

---

# 1. What is Generative AI?

Generative AI means an AI system that can **generate new content**.

Examples:

- Text
- Code
- Images
- Audio
- Video
- Structured JSON

A Large Language Model (LLM) generates text by predicting what should come next based on the input context.

### Simple idea

```text
User question
      ↓
     LLM
      ↓
Generated answer
```

### Important terms

**Token**  
A small piece of text processed by the model.

**Context window**  
The amount of information the model can consider in one request.

**Prompt**  
The instructions and information given to the model.

**Inference**  
Using a trained model to produce an answer.

**Hallucination**  
When a model gives information that sounds correct but is unsupported or wrong.

---

# 2. Prompt Engineering

Prompt engineering means writing better instructions for an LLM.

## Common techniques

### Zero-shot

Ask the model directly.

```text
Classify this review as positive or negative.
```

### Few-shot

Give examples first.

```text
Review: "I love this phone."
Label: Positive

Review: "The battery is terrible."
Label: Negative

Review: "The camera is good."
Label:
```

### Structured prompting

Tell the model exactly what format you want.

```text
Return JSON with:
- name
- age
- occupation
```

### Good prompt structure

```text
Role
↓
Task
↓
Context
↓
Rules
↓
Output format
```

Prompt engineering becomes especially important when building RAG systems and agents because the model must follow instructions while also using retrieved information and tools.

---

# 3. What is RAG?

RAG = **Retrieval-Augmented Generation**

The main idea:

> First retrieve useful information, then give that information to the LLM so it can generate a better answer.

Without RAG:

```text
Question → LLM → Answer
```

With RAG:

```text
Question
   ↓
Retriever
   ↓
Relevant documents
   ↓
LLM
   ↓
Answer
```

## Why use RAG?

An LLM may not know:

- Your private documents
- Your company's latest information
- A newly created document
- Your internal policies
- Large collections of reference material

RAG lets the system search external knowledge at query time.

---

# 4. Basic RAG Pipeline

The basic RAG pipeline is:

```text
Documents
   ↓
Load
   ↓
Chunk
   ↓
Embed
   ↓
Store vectors
   ↓
Retrieve
   ↓
Build context
   ↓
LLM
   ↓
Answer
```

## Step 1: Documents

Examples:

- PDF
- TXT
- Markdown
- DOCX
- Web pages
- Database records

## Step 2: Chunking

Large documents are split into smaller pieces.

Example:

```text
Large PDF
   ↓
Chunk 1
Chunk 2
Chunk 3
Chunk 4
```

Why?

Because searching an entire huge document is less useful than searching meaningful smaller pieces.

---

# 5. Chunking

The uploaded material covers several chunking approaches.

## Fixed-size chunking

Split text into a fixed number of characters/tokens.

Example:

```text
Every 500 characters
```

Simple, but it may split a sentence or idea in the middle.

## Overlap chunking

Chunks share some text.

```text
Chunk 1: A B C D E
Chunk 2:       D E F G H
```

The overlap helps preserve context between chunks.

## Sentence chunking

Split using sentence boundaries.

This usually keeps complete sentences together.

## Recursive chunking

Try to split using increasingly smaller separators.

Conceptually:

```text
Paragraph
   ↓
Sentence
   ↓
Smaller text
```

This is commonly used in LangChain.

## Token chunking

Split according to token count.

Useful when model context limits matter.

## Semantic chunking

Split when the meaning/topic changes.

Example:

```text
Topic A
Topic A
Topic A

Topic B
Topic B
Topic B
```

This can create more meaningful chunks, but it is more expensive/complex.

---

# 6. Embeddings

An embedding converts text into a numerical vector.

Example:

```text
"dog"
   ↓
[0.12, -0.44, 0.91, ...]
```

The goal is that text with similar meaning has similar vectors.

Example:

```text
"How do I reset my password?"
        ↓
     embedding

"I forgot my password. How can I change it?"
        ↓
     embedding

Vectors are close
```

---

# 7. Cosine Similarity

Cosine similarity measures how similar two vectors point in direction.

Conceptually:

```text
similar meaning
      ↓
vectors closer in direction
      ↓
higher similarity
```

This is commonly used for semantic retrieval.

---

# 8. Vector Databases

A vector database stores embeddings and allows similarity search.

The uploaded material uses:

- FAISS
- Chroma

## FAISS

FAISS is a library for efficient similarity search.

Basic idea:

```text
Documents
   ↓
Embeddings
   ↓
FAISS index
   ↓
Similarity search
```

## Chroma

Chroma is a vector database/store often used in RAG applications.

It can store:

- Text
- Embeddings
- Metadata

Example metadata:

```text
{
    "source": "employee_handbook.pdf",
    "page": 12
}
```

Metadata is useful for filtering and citations.

---

# 9. Retrieval

Retrieval means finding the most relevant information for the user's question.

## Semantic search

Uses embeddings.

```text
Question
   ↓
Embedding
   ↓
Vector search
   ↓
Similar chunks
```

## Keyword search

Looks for matching words.

Example:

```text
"refund policy"
```

## Hybrid search

Combines:

```text
Semantic search
      +
Keyword search
      ↓
Better retrieval
```

Hybrid retrieval is useful because semantic search understands meaning while keyword search is good for exact terms.

---

# 10. Reranking

Initial retrieval may return several results.

Example:

```text
Top 10 retrieved chunks
        ↓
Reranker
        ↓
Best 3 chunks
```

A cross-encoder/reranker can compare:

```text
Question + Candidate document
```

and score how relevant they are together.

This is often more accurate than relying only on vector similarity.

---

# 11. Context Construction

After retrieval, selected chunks are placed into the prompt.

Example:

```text
System instructions

Context:
[Chunk 1]
[Chunk 2]
[Chunk 3]

Question:
[user question]
```

The LLM should answer using the supplied context.

A useful rule:

> Do not give the model unnecessary retrieved text.

Too much context can increase cost and may make the answer worse.

---

# 12. Citations

RAG systems can keep source metadata.

Example:

```text
Answer:
The company provides 30 days of paid leave.

Source:
employee_handbook.pdf, page 15
```

Citations make answers easier to verify.

---

# 13. LangChain RAG

LangChain provides components for connecting:

- Documents
- Text splitters
- Embeddings
- Vector stores
- Retrievers
- Prompts
- LLMs

Typical flow:

```text
Document
   ↓
Text Splitter
   ↓
Embeddings
   ↓
Vector Store
   ↓
Retriever
   ↓
Prompt
   ↓
LLM
```

A retriever can be treated as a component that accepts a question and returns relevant documents.

---

# 14. Advanced RAG

Basic RAG is only the beginning.

A stronger RAG system can include:

```text
User Question
      ↓
Query transformation
      ↓
Hybrid retrieval
      ↓
Metadata filtering
      ↓
Reranking
      ↓
Context compression
      ↓
LLM
      ↓
Answer + citations
```

## Important advanced techniques

### Query rewriting

Rewrite a vague user question into a better search query.

### Query expansion

Generate additional search terms.

### Multi-query retrieval

Generate multiple versions of the question and search with all of them.

### HyDE

Hypothetical Document Embeddings.

The system creates a hypothetical answer/document and uses it to improve retrieval.

### Query decomposition

Break a complex question into smaller questions.

```text
Complex question
      ↓
Question 1
Question 2
Question 3
      ↓
Retrieve
      ↓
Combine evidence
```

---

# 15. Parent-Child Retrieval

Sometimes a small chunk is best for retrieval, but a larger section is needed for context.

Example:

```text
Large document
      ↓
Parent section
      ↓
Child chunks
```

Search the child chunk, then return the parent section.

This can improve context quality.

---

# 16. Context Compression

Instead of sending full retrieved documents to the LLM:

```text
Retrieved documents
      ↓
Remove irrelevant information
      ↓
Smaller useful context
      ↓
LLM
```

Benefits:

- Less token usage
- Lower cost
- Less noise
- Better focus

---

# 17. RAG Evaluation

A RAG system should not be judged only by looking at one answer.

Evaluation should test both:

1. Retrieval quality
2. Answer quality

## Retrieval metrics

### Recall@K

Did the relevant document appear in the top K results?

### Precision@K

How many of the top K results were relevant?

### MRR

Mean Reciprocal Rank.

It rewards systems that place the first relevant result near the top.

### NDCG

Measures ranking quality while considering the position of results.

### Hit Rate

Whether at least one relevant result was retrieved.

---

# 18. RAG Answer Evaluation

Important dimensions include:

### Context Precision

Are the retrieved pieces actually relevant?

### Context Recall

Did retrieval find the important information?

### Faithfulness

Is the answer supported by the retrieved context?

### Answer Relevance

Does the answer actually answer the user's question?

A useful mental model:

```text
Bad retrieval
     ↓
Bad context
     ↓
Bad answer
```

Improving retrieval can therefore improve the final answer.

---

# 19. Improving RAG

If the answer is bad, do not immediately change the LLM.

Check the pipeline.

## Improvement checklist

### Problem: Relevant information was not retrieved

Try:

- Better chunking
- Better embeddings
- Hybrid search
- Query rewriting
- More retrieval results
- Metadata filtering
- Reranking

### Problem: Correct information was retrieved but answer is wrong

Try:

- Better prompt
- Stronger grounding instructions
- Context compression
- Better model
- Citation requirements

### Problem: Too much irrelevant context

Try:

- Fewer chunks
- Reranking
- Compression
- Metadata filtering

### Problem: High cost

Try:

- Smaller chunks where appropriate
- Fewer retrieved chunks
- Compression
- Smaller embedding/model choice
- Caching

---

# 20. Agentic AI

A normal LLM mostly does:

```text
Question → Answer
```

An agent can:

```text
Question
   ↓
Think / decide
   ↓
Choose action
   ↓
Use tool
   ↓
Observe result
   ↓
Decide again
   ↓
Final answer
```

The important idea is **action**.

---

# 21. Tool

A tool is a function an AI can use.

Examples:

```text
search_web()
calculator()
get_weather()
query_database()
send_email()
search_documents()
```

A tool normally has:

- Name
- Description
- Input schema
- Function
- Output

The description matters because the model uses it to decide when the tool is appropriate.

---

# 22. Tool Calling

Tool calling means the model asks the application to call a specific tool.

Example:

```text
User:
What is 25 * 18?

LLM:
Call calculator
arguments = {"a": 25, "b": 18}

Application:
calculator(25, 18)

Tool:
450

LLM:
The answer is 450.
```

Important:

> The model usually does not directly execute the function. The application executes the tool and sends the result back.

---

# 23. AIMessage and ToolMessage

A tool-calling workflow commonly contains messages like:

```text
HumanMessage
     ↓
AIMessage
(tool call)
     ↓
ToolMessage
(tool result)
     ↓
AIMessage
(final answer)
```

The uploaded agent material uses this message-based flow.

---

# 24. Agent Loop

A basic agent loop is:

```text
START
  ↓
Model decides
  ↓
Tool needed?
 /       \
No       Yes
 |        |
Final    Execute tool
          ↓
       Tool result
          ↓
       Model again
```

This can repeat multiple times.

The uploaded material presents the progression roughly as:

```text
Functions
   ↓
Tools
   ↓
Tool Calling
   ↓
Tool Loop
   ↓
Agent
```

---

# 25. RAG as a Tool

Instead of putting the entire RAG pipeline directly into every agent step, expose retrieval as a tool.

Example:

```python
search_company_docs(question)
```

Then:

```text
User question
      ↓
Agent
      ↓
Needs company information?
      ↓
RAG search tool
      ↓
Relevant documents
      ↓
Agent
      ↓
Answer
```

This is an important bridge between RAG and Agentic AI.

---

# 26. Agentic RAG

Basic RAG:

```text
Question
   ↓
Retriever
   ↓
Answer
```

Agentic RAG:

```text
Question
   ↓
Agent
   ↓
Decide whether to search
   ↓
Search
   ↓
Read result
   ↓
Maybe search again
   ↓
Use another tool
   ↓
Answer
```

The agent can choose when and how to retrieve.

---

# 27. Agent Evaluation

Agents need more than normal answer evaluation.

Useful metrics:

### Tool selection accuracy

Did the agent choose the correct tool?

### Argument accuracy

Did it send the correct inputs?

### Task success

Did the overall task succeed?

### Trajectory quality

Did the agent take a sensible sequence of actions?

### Failure recovery

Can it recover when a tool fails?

### Latency

How long did the task take?

### Cost

How many model/tool calls and tokens were used?

---

# 28. Structured Output

Structured output means asking the model to return data in a fixed schema.

Example:

```python
class Person(BaseModel):
    name: str
    age: int
```

The output should match the schema.

This is different from tool calling.

### Structured output

```text
LLM → structured data
```

### Tool calling

```text
LLM → request to execute a function
```

Both are important in reliable AI applications.

---

# 29. What is Missing After Basic Agents?

The uploaded material gives a good starting point, but a production-level Agentic AI roadmap needs additional concepts.

## Agent State

State stores what the agent currently knows about the task.

Example:

```text
state = {
    messages: [...],
    user_goal: "...",
    search_results: [...],
    current_step: "research"
}
```

---

# 30. Agent Memory

Memory can be thought of in several forms.

### Short-term memory

Current conversation/task state.

### Long-term memory

Information kept across conversations.

### Working memory

Temporary information needed during a task.

### Semantic memory

Facts or knowledge stored for later retrieval.

### Episodic memory

Past experiences or events.

---

# 31. Planning

A more advanced agent can create a plan before acting.

Example:

```text
User:
Research three laptops and recommend one.

Agent plan:
1. Find requirements
2. Search products
3. Compare specifications
4. Compare price
5. Recommend
```

Planning reduces random tool usage.

---

# 32. ReAct

A common agent pattern is:

```text
Reason
  ↓
Act
  ↓
Observe
  ↓
Reason
  ↓
Act
  ↓
Observe
```

In practice, implementations should expose only the appropriate reasoning/tool behavior rather than relying on unrestricted hidden reasoning.

The important engineering concept is the **action-observation loop**.

---

# 33. Reflection and Self-Correction

An agent can check its own work.

Example:

```text
Generate answer
      ↓
Check answer
      ↓
Problem found?
   /       \
 Yes       No
  ↓         ↓
Fix       Finish
```

A separate critic/verifier can also be used.

---

# 34. LangGraph

For more complex agents, a simple while-loop becomes difficult to maintain.

LangGraph provides a graph-style way to build workflows.

Concepts to learn:

- State
- Nodes
- Edges
- Conditional edges
- Loops
- Checkpoints
- Persistence
- Interrupts
- Human-in-the-loop
- Retries
- Branching
- Subgraphs

Conceptually:

```text
START
  ↓
Planner
  ↓
Research
  ↓
Check
 ┌┴┐
 ↓ ↓
Fix Finish
```

---

# 35. Human-in-the-Loop

Some actions should require human approval.

Example:

```text
Agent wants to send email
        ↓
Approval required
        ↓
Human approves
        ↓
Tool executes
```

This is especially important for:

- Sending messages
- Financial actions
- Deleting data
- Production changes
- External side effects

---

# 36. MCP

MCP = Model Context Protocol.

It provides a standard way for AI applications to connect with external capabilities.

Important concepts:

- MCP client
- MCP server
- Tools
- Resources
- Prompts

Conceptually:

```text
AI Application
      ↓
MCP
      ↓
External systems/tools
```

MCP becomes useful when an application needs standardized access to many external capabilities.

---

# 37. Multi-Agent Systems

Instead of one agent doing everything:

```text
Supervisor Agent
      ↓
 ┌────┼────┐
 ↓    ↓    ↓
Research  Coding  Review
Agent     Agent   Agent
```

Common patterns:

- Supervisor/worker
- Delegation
- Handoffs
- Parallel agents
- Shared state
- Agent communication

Do this only after understanding single-agent systems well.

---

# 38. Agent Security

Agents can take actions, so security becomes critical.

Important threats:

### Prompt injection

Malicious instructions attempt to change the model's behavior.

### Indirect prompt injection

Malicious instructions are hidden inside retrieved documents or web pages.

### Tool abuse

An agent may call a powerful tool incorrectly.

### Data exfiltration

Sensitive information could be sent to an external service.

---

# 39. Security Controls

Useful controls include:

- Least privilege
- Authentication
- Authorization
- Tool allowlists
- Input validation
- Output validation
- Sandboxing
- Rate limits
- Human approval
- Maximum tool-call limits
- Timeouts
- Secret protection

A good principle:

> Give an agent only the permissions it actually needs.

---

# 40. Observability

When an agent fails, you need to know why.

Track:

```text
User request
   ↓
Model call
   ↓
Tool call
   ↓
Tool result
   ↓
Model call
   ↓
Final answer
```

Useful information:

- Latency
- Token usage
- Model calls
- Tool calls
- Errors
- Retrieved documents
- Cost
- Final result

Tracing systems such as LangSmith/OpenTelemetry-style observability can help.

---

# 41. Production LLMOps

A prototype is not automatically a production system. Humanity has learned this approximately 14 seconds after every prototype.

Important production topics:

- Model selection
- Model routing
- Caching
- Retries
- Timeouts
- Fallback models
- Rate limits
- Streaming
- Batching
- Cost tracking
- Secret management
- Monitoring
- Logging
- Deployment
- Regression testing

---

# 42. Fine-Tuning

Fine-tuning changes model behavior using training data.

Important concepts:

- SFT
- LoRA
- QLoRA
- PEFT
- Dataset preparation
- Training evaluation

A simple decision rule:

```text
Need private/up-to-date knowledge?
        ↓
       RAG

Need a different behavior/style?
        ↓
   Fine-tuning

Need both?
        ↓
 RAG + Fine-tuning
```

---

# 43. Multimodal GenAI

Modern GenAI is not only text.

Important areas:

- Vision-language models
- Image understanding
- OCR
- Document AI
- Image generation
- Speech-to-text
- Text-to-speech
- Audio understanding
- Multimodal RAG

Example:

```text
PDF
 ↓
Text + Tables + Images
 ↓
Multimodal processing
 ↓
Retrieval
 ↓
LLM/VLM
 ↓
Answer
```

---

# 44. Complete Learning Roadmap

## Phase 1: LLM Fundamentals

Learn:

- Tokens
- Context windows
- Transformers
- Attention
- Inference
- Temperature
- Top-p
- Hallucinations
- Model limitations

## Phase 2: Prompt Engineering

Learn:

- Zero-shot
- Few-shot
- System prompts
- Structured prompts
- Output constraints
- Prompt injection
- Prompt evaluation

## Phase 3: Basic RAG

Already covered strongly in the source material:

- Documents
- Chunking
- Embeddings
- Vector search
- FAISS
- Chroma
- Retrieval
- Context
- Generation

## Phase 4: LangChain RAG

Learn:

- Document loaders
- Text splitters
- Embeddings
- Vector stores
- Retrievers
- Prompt templates
- Chains
- Structured output

## Phase 5: Advanced RAG

Learn:

- Hybrid search
- BM25
- Metadata filtering
- MMR
- Query rewriting
- Multi-query
- HyDE
- Query decomposition
- Reranking
- Context compression
- Parent-child retrieval
- Citation systems

## Phase 6: RAG Evaluation

Learn:

- Recall@K
- Precision@K
- MRR
- NDCG
- Hit Rate
- Context precision
- Context recall
- Faithfulness
- Answer relevance

## Phase 7: Tool Calling

Already covered strongly in the source material:

- Tools
- Tool schemas
- Tool descriptions
- Binding tools
- AIMessage
- ToolMessage
- Tool execution
- Tool loops

## Phase 8: Agents

Already partially covered:

- Agent loop
- Multiple tools
- Tool errors
- RAG as a tool

Next:

- State
- Memory
- Planning
- ReAct
- Reflection
- Self-correction

## Phase 9: LangGraph

Learn:

- State
- Nodes
- Edges
- Conditional routing
- Loops
- Persistence
- Checkpoints
- Human approval
- Retries
- Subgraphs

## Phase 10: Agentic RAG

Combine:

```text
Agent
 +
RAG
 +
Tools
 +
Planning
 +
Evaluation
```

## Phase 11: MCP

Learn:

- Client
- Server
- Tools
- Resources
- Prompts
- External integrations

## Phase 12: Security

Learn:

- Prompt injection
- Indirect injection
- Tool permissions
- Data leakage
- Sandboxing
- Human approval
- Rate limits

## Phase 13: Observability + LLMOps

Learn:

- Tracing
- Logging
- Cost
- Latency
- Monitoring
- Evaluation pipelines
- Caching
- Retries
- Model routing
- Production deployment

## Phase 14: Advanced Topics

Finally:

- Multi-agent systems
- Fine-tuning
- LoRA/QLoRA
- Multimodal AI
- Voice AI
- Large-scale production systems

---

# 45. Where the Current Learning Material Stands

| Concept | Status |
|---|---|
| Chunking | Covered |
| Fixed/overlap/sentence/recursive/token/semantic chunking | Covered |
| Embeddings | Covered |
| Cosine similarity | Covered |
| FAISS | Covered |
| Chroma | Covered |
| Semantic retrieval | Covered |
| Keyword search | Covered |
| Hybrid search | Covered |
| RAG basics | Covered |
| LangChain RAG | Covered |
| Citations/source metadata | Covered |
| Basic retrieval evaluation | Covered |
| Reranking | Covered |
| Context compression | Introduced |
| Tool concepts | Covered |
| Custom tools | Covered |
| Tool schemas | Covered |
| Tool binding | Covered |
| Tool calling | Covered |
| AIMessage/ToolMessage | Covered |
| Tool execution | Covered |
| Agent loop | Covered |
| Tool errors | Covered |
| Basic agent evaluation | Covered |
| Agent state | Next |
| Agent memory | Next |
| Planning | Next |
| Reflection/self-correction | Next |
| LangGraph | Next |
| Agentic RAG | Next |
| MCP | Next |
| Multi-agent | Later |
| Security/guardrails | Next |
| Observability | Next |
| Production LLMOps | Next |
| Fine-tuning | Later |
| Multimodal AI | Later |

---

# 46. Practical Project Sequence

A strong coding sequence is:

### Project 1: Basic RAG

```text
PDF
→ chunks
→ embeddings
→ Chroma
→ retrieval
→ LLM
```

### Project 2: LangChain RAG

```text
Document loader
→ splitter
→ embeddings
→ vector store
→ retriever
→ prompt
→ LLM
```

### Project 3: Advanced RAG

```text
Hybrid search
→ reranker
→ compression
→ citations
→ evaluation
```

### Project 4: Tool Calling

Build an agent with:

```text
calculator
weather/search tool
document search tool
```

### Project 5: Agentic RAG

```text
User
 ↓
Agent
 ↓
Decide whether to search
 ↓
RAG tool
 ↓
Evaluate evidence
 ↓
Search again if needed
 ↓
Answer
```

### Project 6: LangGraph Agent

Build:

```text
Planner
 ↓
Researcher
 ↓
Tool executor
 ↓
Verifier
 ↓
Final answer
```

### Project 7: Production Agent

Add:

- Evaluation
- Tracing
- Security
- Human approval
- Retry
- Cost monitoring
- Persistent state

---

# 47. The Most Important Mental Model

Think of the entire field as layers:

```text
                Production AI
                     ↑
          Observability + Security
                     ↑
              Multi-Agent / MCP
                     ↑
           LangGraph + Planning
                     ↑
               Agentic AI
                     ↑
              Tool Calling
                     ↑
               Advanced RAG
                     ↑
                 Basic RAG
                     ↑
              Vector Search
                     ↑
                Embeddings
                     ↑
                 Chunking
                     ↑
               LLM Basics
```

Do not jump directly to multi-agent systems because a tutorial made three bots talk to each other and added a dramatic architecture diagram. Learn the lower layers first.

---

# 48. Final Checklist

Before calling yourself comfortable with GenAI + Agentic AI, you should be able to build and explain:

## GenAI

- [ ] What an LLM does
- [ ] Tokens
- [ ] Context window
- [ ] Prompt engineering
- [ ] Structured output
- [ ] Hallucination

## RAG

- [ ] Document loading
- [ ] Chunking
- [ ] Embeddings
- [ ] Vector databases
- [ ] FAISS
- [ ] Chroma
- [ ] Semantic search
- [ ] Keyword search
- [ ] Hybrid search
- [ ] Reranking
- [ ] Query rewriting
- [ ] Context compression
- [ ] Citations

## Evaluation

- [ ] Recall@K
- [ ] Precision@K
- [ ] MRR
- [ ] NDCG
- [ ] Faithfulness
- [ ] Context precision
- [ ] Context recall
- [ ] Answer relevance

## Agents

- [ ] Tool definitions
- [ ] Tool schemas
- [ ] Tool binding
- [ ] Tool calling
- [ ] Tool execution
- [ ] Tool loops
- [ ] Agent state
- [ ] Memory
- [ ] Planning
- [ ] ReAct/action-observation loops
- [ ] Reflection
- [ ] Error recovery

## Agent frameworks

- [ ] LangChain
- [ ] LangGraph
- [ ] MCP

## Production

- [ ] Security
- [ ] Prompt injection defense
- [ ] Permissions
- [ ] Human-in-the-loop
- [ ] Observability
- [ ] Tracing
- [ ] Cost monitoring
- [ ] Latency monitoring
- [ ] Retries
- [ ] Caching
- [ ] Deployment

## Advanced

- [ ] Multi-agent
- [ ] Fine-tuning
- [ ] LoRA/QLoRA
- [ ] Multimodal AI
- [ ] Voice AI

---

# 49. One-Line Summary

**GenAI** generates content.

**RAG** gives the model external knowledge.

**Advanced RAG** improves how that knowledge is found and ranked.

**Tool calling** lets the model request actions.

**Agentic AI** lets the system decide which actions to take and repeat the process toward a goal.

**LangGraph** helps build complex stateful agent workflows.

**MCP** standardizes connections between AI applications and external capabilities.

**Evaluation + security + observability** turn a demo into something that can actually survive contact with reality.
