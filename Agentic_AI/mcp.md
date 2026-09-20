Got it — this is the **“MCP Servers – Next Big Thing in AI”** lecture by Piyush Garg. ([LinkedIn][1])

Since your **BMC Helix interview is tomorrow** and they’re interested in **Agentic AI**, I’d study this lecture primarily as an **MCP + Agentic AI interview topic**, not just as a coding tutorial.

## MCP — Interview Notes

### 1. What is MCP?

**MCP = Model Context Protocol**

MCP is an **open protocol/standard that allows AI applications to connect with external tools, data sources, and services in a standardized way.**

The simplest interview definition:

> **“MCP is a standardized protocol that allows an AI application to discover and interact with external tools and data sources without having to build a custom integration for every tool.”**

Think:

```text
LLM / AI Agent
       ↓
    MCP Client
       ↓
    MCP Server
       ↓
 ┌─────┼──────────┐
 ↓     ↓          ↓
DB    APIs      Files
```

MCP is often explained using the **USB-C analogy**: instead of creating a completely different connector for every device, you have a standardized interface. ([LinkedIn][2])

---

# 2. Why do we need MCP?

Suppose you create an AI agent for a company.

The agent needs access to:

* Employee database
* Jira
* Slack
* GitHub
* Company documents
* CRM
* Internal APIs

Without a standardized approach:

```text
Agent → Custom Jira integration
Agent → Custom Slack integration
Agent → Custom GitHub integration
Agent → Custom DB integration
Agent → Custom CRM integration
```

This becomes difficult to maintain.

With MCP:

```text
                    ┌── Jira
                    │
AI Application ─ MCP ┼── Database
                    │
                    ├── GitHub
                    │
                    ├── Slack
                    │
                    └── Internal APIs
```

The AI application communicates through the MCP protocol.

### Interview point

**MCP does NOT replace APIs.**

An MCP server can internally call REST APIs, databases, SDKs, etc.

```text
AI Agent
   ↓
MCP
   ↓
MCP Server
   ↓
REST API / Database / Service
```

This distinction is very important.

---

# 3. MCP Architecture

Remember these **three terms**:

### MCP Host

The **application in which the AI experience runs**.

Examples could include an AI assistant or an AI-powered IDE.

```text
MCP Host
   ↓
MCP Client
   ↓
MCP Server
```

### MCP Client

The component that maintains communication with an MCP server.

It acts as the **bridge between the host application and MCP server**.

### MCP Server

A program that exposes capabilities to the AI application.

It can expose:

* **Tools**
* **Resources**
* **Prompts**

This architecture is commonly described as:

```text
Host
 │
 ├── MCP Client
 │
 ▼
MCP Server
 │
 ├── Tools
 ├── Resources
 └── Prompts
```

([LinkedIn][3])

---

# 4. Tools

This is probably the **most important part for your Agentic AI interview**.

A **tool is an action that an AI model can request the system to execute.**

Examples:

```text
get_employee(id)
create_ticket(title, description)
search_jira(query)
get_customer(customer_id)
send_email(to, message)
get_weather(city)
```

The LLM decides:

> “I need this tool.”

Then the MCP client/server mechanism executes it and returns the result.

### Example

User:

> “What is the status of ticket INC12345?”

Agent:

```text
User question
      ↓
LLM
      ↓
Need ticket information
      ↓
Call get_ticket_status()
      ↓
MCP Server
      ↓
Ticketing system
      ↓
Result
      ↓
LLM
      ↓
Natural-language answer
```

This is exactly the kind of workflow you should understand for an **Agentic AI interview**.

---

# 5. Resources

**Resources are data/context exposed to an AI application.**

Examples:

```text
Company policy document
Customer information
Database records
Files
Documentation
System information
```

Think:

### Tool = Do something

```text
create_ticket()
send_email()
update_record()
```

### Resource = Read/access something

```text
company_policy.pdf
customer_record
database information
documentation
```

---

# 6. Prompts

MCP can also expose **reusable prompt templates**.

For example:

```text
customer_support_prompt
code_review_prompt
incident_analysis_prompt
```

Instead of every client independently implementing the same prompt structure, the server can expose a reusable prompt.

---

# 7. Tools vs Resources vs Prompts

Memorize this table:

| Component     | Purpose                   | Example                    |
| ------------- | ------------------------- | -------------------------- |
| **Tools**     | Perform actions           | `create_ticket()`          |
| **Resources** | Provide data/context      | `employee_policy.pdf`      |
| **Prompts**   | Reusable prompt templates | `incident_analysis_prompt` |

### One-line memory trick

> **Tool = Action, Resource = Information, Prompt = Instruction**

This is a very good interview answer.

---

# 8. MCP vs Function Calling

This is a **very likely interview question**.

### Function calling

The LLM is given functions/tools directly.

Example:

```text
LLM
 ↓
get_weather()
 ↓
Weather API
```

You generally define the available functions yourself inside your application.

### MCP

MCP provides a **standardized protocol** for exposing and discovering tools/resources/prompts.

```text
LLM Application
      ↓
MCP Client
      ↓
MCP Server
      ↓
Multiple tools/services
```

### Important distinction

> **Function calling is a mechanism for allowing a model to invoke functions. MCP is a standardized protocol for connecting AI applications with tools, resources, and prompts.**

MCP can therefore help when you have **many tools and integrations** and want a standardized architecture. ([LinkedIn][4])

---

# 9. MCP vs API

Another important interview question.

### Traditional API

Usually:

```text
Application → API → Service
```

Example:

```text
React/Node
   ↓
Jira REST API
   ↓
Jira
```

### MCP

```text
AI Application
      ↓
MCP Client
      ↓
MCP Server
      ↓
Jira API
```

MCP is **not another replacement for REST APIs**.

Instead, the MCP server can act as an AI-friendly integration layer over existing APIs.

### Interview answer

> “An API defines how software systems communicate, whereas MCP standardizes how AI applications discover and interact with tools, resources and prompts. An MCP server can internally use APIs.”

---

# 10. MCP + Agentic AI

This is where you should connect the lecture to your interview.

An LLM by itself primarily generates text.

```text
LLM
 ↓
Generate response
```

An agent can:

```text
Understand goal
      ↓
Reason about required action
      ↓
Select tool
      ↓
Execute tool
      ↓
Observe result
      ↓
Continue
      ↓
Final response
```

MCP provides a standardized way for the agent to access those capabilities.

So:

> **LLM = reasoning/generation**
>
> **Agent = LLM + decision-making + tools/workflow**
>
> **MCP = standardized interface for connecting the agent to tools/data**

This mental model is extremely useful.

---

# 11. Example: BMC Helix-style IT support agent

Imagine an employee says:

> “My laptop VPN isn't working. Check if there is an existing incident and create one if necessary.”

An agent could have MCP tools:

```text
search_incidents()
get_incident()
create_incident()
update_incident()
get_employee()
```

Workflow:

```text
Employee
   ↓
AI Agent
   ↓
Understand request
   ↓
search_incidents()
   ↓
MCP Server
   ↓
ITSM system
   ↓
Existing incident?
   │
   ├── YES → get status → answer user
   │
   └── NO
        ↓
   create_incident()
        ↓
   MCP Server
        ↓
   ITSM system
        ↓
   Ticket created
        ↓
   Agent responds
```

**This is the kind of example I would use in your BMC Helix interview.**

---

# 12. MCP + RAG

Don't confuse these.

### RAG

Used primarily to give an LLM **relevant knowledge/context**.

```text
Documents
   ↓
Chunking
   ↓
Embeddings
   ↓
Vector DB
   ↓
Retrieve relevant chunks
   ↓
LLM
```

### MCP

Used primarily to provide a **standardized interface to external capabilities/data**.

```text
AI Agent
   ↓
MCP
   ↓
Tools / Resources
```

They can work together.

### Enterprise example

Suppose an employee asks:

> “According to company policy, can I work from home tomorrow, and submit the request?”

The system could:

```text
                    ┌── RAG → company policy
User → Agent ───────┤
                    └── MCP → HR system
```

RAG answers:

> What does the policy say?

MCP tool performs:

> Submit/update the actual request.

That's a **very strong Agentic AI architecture explanation**.

---

# 13. MCP + Your AI Ticket Management Project

Your project is actually a very good place to explain this.

Your existing architecture is roughly:

```text
User
 ↓
Ticket API
 ↓
MongoDB
 ↓
Inngest
 ↓
Gemini
 ↓
Categorization / Priority / Required Skills
 ↓
Moderator Assignment
 ↓
Email
```

If you wanted to make it more agentic with MCP:

```text
                    ┌── get_ticket()
                    ├── search_tickets()
AI Agent → MCP ─────┼── update_ticket()
                    ├── assign_ticket()
                    └── send_notification()
```

Then the agent could dynamically decide which operation it needs.

### Interview answer

> “In my ticket management system, I used Gemini for ticket classification and Inngest for asynchronous processing. If I extended it with MCP, I could expose ticket operations such as searching, updating and assigning tickets as MCP tools. The agent could then select the appropriate tool based on the user's request.”

That answer connects **your project + Agentic AI + MCP** very nicely.

---

# 14. Why MCP is useful

Remember these keywords:

### 1. Standardization

Common protocol for AI-tool communication.

### 2. Interoperability

Different AI applications can work with standardized integrations.

### 3. Modularity

Tools can be developed independently.

### 4. Scalability

Easier to manage many tools/services.

### 5. Reusability

The same MCP server can expose capabilities to multiple AI clients.

### 6. Separation of concerns

```text
AI reasoning
      ≠
Business logic
      ≠
External service
```

This makes architecture cleaner.

---

# 15. Security — VERY IMPORTANT

Giving an AI agent tools means giving it **real capabilities**.

For example:

```text
delete_user()
send_email()
transfer_money()
update_ticket()
```

Therefore, MCP-based systems need controls such as:

* Authentication
* Authorization
* Least privilege
* User consent/approval where appropriate
* Input validation
* Tool permission controls
* Secure communication
* Logging/auditing
* Rate limiting
* Sandboxing where appropriate

For example, don't give an agent:

```text
Database:
FULL ACCESS
```

Instead:

```text
Database:
READ tickets
CREATE tickets
UPDATE tickets

NO DELETE
```

This follows the **principle of least privilege**.

---

# 16. MCP doesn't make an LLM intelligent

Important conceptual point.

MCP does **not**:

* Train the LLM
* Increase model intelligence
* Replace the LLM
* Replace APIs
* Automatically make something an agent

Instead:

> **MCP gives an AI application a standardized way to interact with external capabilities.**

---

# 17. Complete Agent + MCP architecture

Memorize this:

```text
                  USER
                    ↓
              AI APPLICATION
                    ↓
                 LLM
                    ↓
             Agent / Planner
                    ↓
             MCP Client
                    ↓
              MCP Server
          ┌─────────┼─────────┐
          ↓         ↓         ↓
        Tools    Resources   Prompts
          ↓         ↓
       APIs       Data
          ↓         ↓
       External Systems
```

The response comes back:

```text
External System
      ↓
MCP Server
      ↓
MCP Client
      ↓
Agent / LLM
      ↓
Final Response
```

---

# 18. Most likely interview questions

### Q1. What is MCP?

**Answer:**

> MCP stands for Model Context Protocol. It is an open protocol that standardizes how AI applications connect with external tools, resources and services.

---

### Q2. Why do we need MCP?

> Without a standard protocol, every AI application may need custom integrations for different tools and services. MCP provides a standardized interface that makes these integrations more modular and reusable.

---

### Q3. What are the main components of MCP?

> MCP Host, MCP Client and MCP Server. The server exposes capabilities such as tools, resources and prompts.

---

### Q4. What is an MCP tool?

> A tool is an executable capability that an AI application can invoke, such as searching a database, creating a ticket or calling an API.

---

### Q5. MCP vs API?

> API is a general software-to-software communication interface. MCP specifically standardizes how AI applications interact with tools, resources and prompts. MCP servers can internally use APIs.

---

### Q6. MCP vs function calling?

> Function calling allows an LLM to invoke predefined functions. MCP provides a standardized protocol for discovering and interacting with tools and other context sources across AI applications.

---

### Q7. Does MCP replace RAG?

**No.**

> RAG retrieves relevant information from a knowledge base, while MCP provides a standardized mechanism for AI applications to interact with tools and resources. They can be used together.

---

### Q8. Does MCP make an AI agent?

**No.**

> MCP provides connectivity to capabilities. An agent additionally needs decision-making/orchestration logic that determines when and how to use those capabilities.

---

### Q9. Give an enterprise use case.

> An IT support agent could use MCP tools to search incidents, retrieve employee information, create tickets and update ticket status. The LLM determines which tool is appropriate based on the user's request.

---

### Q10. What security concerns exist?

> Since agents can invoke real-world actions, we need authentication, authorization, least privilege, input validation, auditing, secure communication and appropriate human approval for sensitive operations.

---

# 19. The 30-second explanation to memorize

If the interviewer says:

**“Explain MCP.”**

Say:

> **“MCP stands for Model Context Protocol. It is an open protocol that standardizes how AI applications connect to external tools and data sources. The architecture generally consists of an MCP host, MCP client and MCP server. The server can expose tools, resources and prompts. For example, in an IT support system, an AI agent could use MCP tools to search incidents, create tickets or update their status. MCP doesn't replace APIs or RAG; rather, it provides a standardized integration layer through which an AI application can interact with external capabilities.”**

That's a **strong interview-level answer**.

---

## ⭐ What you should memorize tonight

If you have limited time, memorize these **8 things**:

```text
1. MCP = Model Context Protocol

2. Purpose = standardized AI ↔ external tools/data communication

3. Architecture:
   Host → Client → Server

4. Server exposes:
   Tools + Resources + Prompts

5. Tool = action
   Resource = data
   Prompt = reusable instruction

6. MCP ≠ API
   MCP can use APIs underneath

7. MCP ≠ RAG
   RAG = retrieve knowledge
   MCP = connect to capabilities

8. MCP + Agent = powerful enterprise automation
```

The lecture itself is specifically presented as **“MCP Servers - Next Big Thing in AI”** by Piyush Garg, and MCP is also part of the broader Agentic AI curriculum covering agents, RAG, LangGraph and tool integration. ([LinkedIn][1])

**For your BMC Helix interview, the highest-value connection is:**
**Agent → MCP → ITSM tools → ticket/incident automation.**

[1]: https://www.linkedin.com/posts/piyushgarg195_ai-aiagents-javascript-activity-7307328630542544896-gSW6?utm_source=chatgpt.com "#ai #aiagents #javascript | Piyush Garg"
[2]: https://dk.linkedin.com/pulse/mcp-protocol-standard-way-feed-context-kunal-k-shaw-thmxf?tl=da&utm_source=chatgpt.com "MCP-protokol - Standard måde at give kontekst på!"
[3]: https://www.linkedin.com/posts/chaudhary-paritosh_ai-genai-llm-activity-7497903453550305280-L3M6?utm_source=chatgpt.com "MCP: Model Context Protocol for AI Applications | Paritosh Chaudhary posted on the topic | LinkedIn"
[4]: https://www.linkedin.com/posts/handotdev_until-recently-ai-was-like-a-mind-trapped-activity-7356715653044555778-HOEi?utm_source=chatgpt.com "Until recently, AI was like a mind trapped in a jar. | Han Wang"
