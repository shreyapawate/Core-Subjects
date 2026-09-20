# 2. LangGraph — Interview Notes

## 1. What is LangGraph?

**LangGraph** is a framework for building **stateful, multi-step, controllable AI/agent workflows** using a graph structure.

### Interview answer

> **“LangGraph allows us to build agentic workflows as graphs where nodes perform operations, edges control the flow, and shared state carries information between steps. It is useful when an agent needs conditional routing, loops, tool calls, persistence, checkpointing, or human approval.”**

### Why not simply call an LLM?

```text
Simple LLM:
Input → LLM → Output

LangGraph:
Input → Agent → Tool → Result → Agent
                 ↑             ↓
                 └── Loop ─────┘
```

LangGraph is useful when you need **control over the workflow**, not just a single LLM response.

---

# 2. Core LangGraph Concepts

## Graph

The overall workflow containing:

* Nodes
* Edges
* State
* Start/end points

Think:

> **Graph = complete workflow**

---

## Node

A **node is a function/unit of work**.

Examples:

```text
agent_node
retrieve_node
tool_node
validate_node
human_review_node
```

A node:

```text
State → Process → Updated State
```

---

## Edge

An **edge determines what runs next**.

```text
Node A → Node B
```

Types:

### Normal edge

Fixed transition:

```text
A → B
```

### Conditional edge

Next node depends on state/result:

```text
A
↓
Condition
├── B
└── C
```

### Loop

A node can route back to an earlier node:

```text
A → B → C
    ↑   ↓
    └───┘
```

---

# 3. State

**State is the shared data passed through the graph.**

Example:

```python
state = {
    "messages": [],
    "user_query": "",
    "tool_result": None,
    "status": "processing"
}
```

Nodes read and update this state.

```text
       Shared State
       ↙    ↓    ↘
    Agent  Tool  Validator
```

### Key interview point

> **State is what allows different nodes to share information and allows the workflow to track its current progress.**

---

# 4. StateGraph

`StateGraph` is used to define a graph whose nodes operate on a shared state.

Conceptually:

```python
graph = StateGraph(State)

graph.add_node("agent", agent)
graph.add_node("tool", tool)

graph.add_edge(START, "agent")
...
graph.add_edge("tool", "agent")
```

Then the graph is compiled before execution.

```text
Define graph
    ↓
Add nodes
    ↓
Add edges
    ↓
Compile
    ↓
Invoke
```

---

# 5. START and END

### START

Entry point of the graph.

```text
START
  ↓
Agent
```

### END

Terminates execution.

```text
Agent
  ↓
END
```

Typical graph:

```text
START → Agent → Tool → Agent → END
```

---

# 6. Sequential Graph

Nodes execute in a fixed sequence.

```text
START
  ↓
Node A
  ↓
Node B
  ↓
Node C
  ↓
END
```

Example:

```text
User Query
   ↓
Retrieve Document
   ↓
Generate Answer
   ↓
Validate
   ↓
END
```

**Use when:** workflow order is known and doesn't need dynamic decisions.

---

# 7. Conditional Graph

The next node depends on the current state/result.

```text
START
  ↓
Agent
  ↓
Condition
 ┌──────────┐
 ↓          ↓
Tool       END
```

Example:

```text
Agent
  ↓
Tool required?
 ├── YES → Tool
 └── NO  → END
```

This is **very important for agentic workflows**.

---

# 8. Looping Graph

A graph can repeat nodes until a condition is satisfied.

```text
START
  ↓
Agent
  ↓
Tool
  ↓
Result
  ↓
Agent
  ↓
Done?
 ├── NO → Tool
 └── YES → END
```

Example:

> Agent generates SQL → executes → detects error → fixes SQL → executes again → successful → END.

### Important

Always define a **termination condition / recursion limit** to prevent infinite loops.

---

# 9. State Management

State should contain only information required by the workflow.

Typical agent state:

```text
messages
user_query
tool_calls
tool_results
current_step
intermediate_data
final_answer
status
```

### Good design

```text
State
 ↓
Node reads required fields
 ↓
Node updates relevant fields
 ↓
Next node receives updated state
```

### Interview point

> LangGraph's state makes multi-step workflows easier to manage because every node can access the relevant shared execution context.

---

# 10. ReAct Agent in LangGraph

A ReAct agent follows:

```text
Reason / Decide
      ↓
Need tool?
 ┌────┴────┐
YES        NO
 ↓          ↓
Tool       END
 ↓
Observation
 ↓
Agent
```

This naturally maps to LangGraph:

```text
START
  ↓
Agent
  ↓
Should call tool?
 ├─────────────┐
 ↓             ↓
Tool           END
 ↓
Tool Result
 ↓
Agent
```

---

# 11. Tool Node

A **Tool Node** executes the tools requested by the agent.

Typical flow:

```text
Agent
 ↓
Tool call
 ↓
ToolNode
 ↓
Actual tool execution
 ↓
Tool result
 ↓
Agent
```

Example tools:

```python
get_incident()
search_knowledge_base()
create_ticket()
update_ticket()
```

### Important distinction

```text
Agent node
= decides which tool to use

Tool node
= executes the tool
```

This distinction is highly interview-worthy.

---

# 12. Tool Calling

The LLM doesn't directly execute your Python/API function.

Flow:

```text
User
 ↓
Agent / LLM
 ↓
Tool call decision
 ↓
ToolNode
 ↓
Function/API
 ↓
Result
 ↓
State
 ↓
Agent
```

Example:

```text
User:
"What's the status of INC1001?"

Agent:
→ get_incident("INC1001")

ToolNode:
→ calls ITSM API

Result:
→ "In Progress"

Agent:
→ generates final response
```

---

# 13. Agent State

For a ReAct agent, state commonly contains a **message history**.

Example:

```text
HumanMessage
    ↓
AIMessage(tool_call)
    ↓
ToolMessage(result)
    ↓
AIMessage(final_answer)
```

This allows the agent to know:

* What the user asked
* Which tool it called
* What the tool returned
* What happened previously

### Interview point

> Agent state provides the context required for the agent to continue its reasoning/action cycle.

---

# 14. Checkpointing

**Checkpointing = saving graph state at execution points.**

Instead of losing the workflow state if execution stops:

```text
Agent
 ↓
Tool
 ↓
CHECKPOINT
 ↓
Continue
```

If execution fails or pauses, the workflow can potentially resume from saved state.

### Useful for

* Long-running agents
* Human approval
* Fault recovery
* Debugging
* Conversation persistence
* Resuming interrupted workflows

### Key distinction

```text
State       = current workflow data

Checkpoint  = persisted snapshot of workflow state
```

---

# 15. Memory in LangGraph

LangGraph can use persistence/checkpointing to maintain conversation or workflow information across executions.

Example:

```text
Conversation 1
     ↓
Checkpoint / persistent state
     ↓
Conversation 2
     ↓
Retrieve relevant previous state
```

### Important distinction

**Checkpointing is the mechanism for persistence; memory is the information retained and reused.**

Don't say:

> "Checkpointing itself is memory."

Better:

> **“Checkpointing persists graph state, which can support conversational or workflow memory.”**

---

# 16. Human-in-the-Loop in LangGraph

LangGraph can pause execution and wait for human input/approval.

Example:

```text
START
 ↓
Agent
 ↓
Sensitive action?
 ↓
Human Approval
 ├── APPROVE → Tool
 └── REJECT  → END
```

Example:

> Agent wants to update a production incident.

Instead of automatically executing:

```text
Agent → Update Production
```

use:

```text
Agent
 ↓
Approval required
 ↓
Human
 ↓
Approve
 ↓
Tool
```

### Why LangGraph is useful here

Because the workflow can **pause, persist state, obtain human input, and resume**.

---

# 17. Complete LangGraph Agent Architecture

This is the architecture you should memorize:

```text
                         START
                           ↓
                        AGENT
                           ↓
                   Tool required?
                    ↙          ↘
                  YES           NO
                   ↓             ↓
                TOOL           END
                   ↓
              TOOL RESULT
                   ↓
                UPDATE
                 STATE
                   ↓
                 AGENT
                   ↓
             More work?
              ↙       ↘
            YES        NO
             ↓          ↓
           TOOL        END
```

With HITL:

```text
                    AGENT
                      ↓
              Sensitive action?
                 ↙         ↘
               YES          NO
                ↓            ↓
          HUMAN APPROVAL    TOOL
             ↙    ↘
          Reject  Approve
            ↓       ↓
           END     TOOL
                    ↓
                  AGENT
```

---

# 18. Why LangGraph instead of simply calling an LLM?

This is **one of the most important interview questions**.

### Simple LLM call

```text
Input
 ↓
LLM
 ↓
Output
```

Good for:

* Summarization
* Classification
* Simple Q&A
* Text generation

### LangGraph

```text
Input
 ↓
Agent
 ↓
Decision
 ↓
Tool
 ↓
Observation
 ↓
Decision
 ↓
Human approval
 ↓
Tool
 ↓
Validation
 ↓
END
```

Useful for:

* Multi-step workflows
* Tool use
* Conditional routing
* Loops
* State management
* Checkpointing
* Human approval
* Long-running tasks
* Error recovery

### Best interview answer

> **“A direct LLM call is suitable when I have a simple input-to-output task. LangGraph becomes useful when the application needs a stateful, multi-step workflow with conditional decisions, tool calls, loops, persistence, or human-in-the-loop. Instead of letting the LLM control everything implicitly, LangGraph lets me explicitly model and control the workflow.”**

---

# 19. LangGraph vs Traditional Workflow

The key difference isn't that LangGraph makes everything autonomous.

It's:

> **LangGraph gives you explicit control over an agentic workflow while allowing LLM-driven decisions where appropriate.**

You can combine deterministic and AI-driven nodes:

```text
Deterministic Node
       ↓
LLM Node
       ↓
Conditional Edge
       ↓
Tool Node
       ↓
Validation Node
```

This is extremely useful in enterprise systems.

---

# 20. BMC Helix Example

Imagine an **AI Incident Resolution Agent**.

```text
START
  ↓
Read Incident
  ↓
Agent
  ↓
Need Knowledge?
 ├── YES → RAG Search
 │           ↓
 │         Agent
 │
 └── NO
      ↓
Need ITSM Tool?
 ├── YES → Tool
 │           ↓
 │         Result
 │           ↓
 │         Agent
 │
 └── NO
      ↓
Generate Response
      ↓
END
```

For a sensitive action:

```text
Agent
 ↓
Update incident?
 ↓
Human Approval
 ↓
Tool
 ↓
Verify update
 ↓
END
```

### Strong interview explanation

> “For an enterprise ITSM agent, I would use LangGraph to explicitly model the workflow. The agent node would decide whether it needs knowledge retrieval or an ITSM tool. Tool nodes would execute validated operations, state would carry the incident and intermediate results, conditional edges would control routing, and checkpointing could support persistence and human approval. This gives more control and observability than simply sending the request to an LLM.”

---

# 21. LangGraph vs LangChain

Very common interview question.

### LangChain

Provides components for building LLM applications:

* Models
* Prompts
* Tools
* Retrievers
* Agents
* Integrations

### LangGraph

Focuses on **orchestrating stateful workflows/agents as graphs**.

Simple mental model:

```text
LangChain
= Components

LangGraph
= Workflow / orchestration
```

They can be used together.

Example:

```text
LangChain:
LLM + Retriever + Tools

        ↓

LangGraph:
Agent → Retriever → Tool → Agent → END
```

---

# 🔥 22. Must-Memorize Questions

### What is LangGraph?

> A framework for building stateful, multi-step, controllable LLM and agent workflows using graphs.

### What is a node?

> A unit of computation that reads and potentially updates graph state.

### What is an edge?

> A transition that determines which node executes next.

### What is state?

> Shared structured information representing the current workflow context.

### Why StateGraph?

> To define a graph whose nodes operate on shared state.

### What is a conditional edge?

> An edge whose destination is selected based on the current state or result.

### What is ToolNode?

> A node that executes tools requested by an agent.

### Why checkpointing?

> To persist graph state so workflows can support resumption, persistence, debugging, and human-in-the-loop pauses.

### Why LangGraph over an LLM call?

> Because it provides explicit control over multi-step, stateful, conditional, looping, tool-using workflows.

---

# ⭐ Final 1-Minute Revision

```text
LangGraph
   ↓
Graph-based agent orchestration

Graph
   ↓
Nodes + Edges + State

Node
   ↓
Does work

Edge
   ↓
Controls flow

State
   ↓
Shared workflow data

START
   ↓
Entry point

END
   ↓
Termination

Conditional Edge
   ↓
Dynamic routing

Loop
   ↓
Repeat until condition satisfied

Agent
   ↓
Decides what to do

ToolNode
   ↓
Executes tools

Checkpoint
   ↓
Persists state

HITL
   ↓
Pause → Human → Resume

ReAct
   ↓
Agent → Tool → Result → Agent

Main benefit
   ↓
Stateful + controllable + multi-step
agentic workflows
```

### The one sentence to remember

> **“LangGraph is useful when an LLM application needs to move beyond a simple prompt-response interaction into a controlled, stateful workflow involving decisions, tools, loops, persistence, and human intervention.”**
