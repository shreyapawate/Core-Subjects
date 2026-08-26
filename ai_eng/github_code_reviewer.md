# AI GitHub Code Review Bot with n8n — Interview-Ready Notes

## 1. What is the Project?

An **AI-powered GitHub Code Review Bot** automatically reviews code changes in a Pull Request using an LLM.

### Basic Flow

```text
Developer creates PR
        ↓
GitHub Webhook
        ↓
n8n Workflow
        ↓
Fetch PR Diff
        ↓
LLM analyzes code
        ↓
Generate Review
        ↓
GitHub API
        ↓
Comment + Label on PR
```

---

## 2. Tech Stack

* **GitHub** → Code repository + Pull Requests
* **n8n** → Workflow automation
* **OpenAI / Claude / Gemini** → Code analysis
* **GitHub API** → Fetch diff + post review
* **VPS** → Hosts n8n
* **Webhook** → Triggers workflow

---

## 3. Why n8n?

**n8n** is a workflow automation platform that connects different services through workflows.

Instead of writing all integration code manually:

```text
GitHub → API → LLM → GitHub
```

n8n provides visual workflow nodes for:

```text
Trigger → Process → AI → API → Output
```

### Interview Answer

> "n8n is a workflow automation platform that can connect APIs, applications and AI models. In this project, it orchestrates the GitHub webhook, code-diff retrieval, LLM analysis and posting the review back to GitHub."

---

## 4. Self-Hosting

n8n can be deployed on a **VPS**.

### Benefits

* More control
* Private infrastructure
* Custom configuration
* Can reduce SaaS costs
* Suitable for production workflows

```text
Developer
   ↓
GitHub
   ↓
Internet
   ↓
VPS
   ↓
n8n
```

---

# 5. GitHub Webhook

A **webhook** allows GitHub to notify n8n when an event occurs.

Example:

```text
Pull Request Opened
        ↓
GitHub Webhook
        ↓
n8n Trigger
```

Instead of n8n continuously asking GitHub:

> "Is there a new PR?"

GitHub pushes an event to n8n.

### Key Point

> **Webhook = event-driven communication.**

---

# 6. Pull Request Trigger

The workflow can be triggered when a PR is:

* Opened
* Updated
* Reopened
* Synchronized

For the basic workflow:

```text
PR opened
   ↓
Trigger workflow
```

---

# 7. What is a PR Diff?

A **diff** represents the changes introduced by a Pull Request.

Example:

```diff
- old code
+ new code
```

The bot doesn't necessarily need the entire repository.

It can analyse the **changed code**.

### Why?

* Less data
* Lower token usage
* Faster processing
* More focused review

---

# 8. AI Code Review

The LLM receives the PR changes along with instructions.

Example prompt:

```text
You are a Senior JavaScript developer.

Review the following code changes.

Check for:
- Bugs
- Security issues
- Performance problems
- Code quality
- Best practices

Provide actionable feedback.
```

Then:

```text
PR Diff
   ↓
Prompt + Diff
   ↓
LLM
   ↓
Review Comments
```

---

# 9. GitHub API

The workflow uses the **GitHub API** to interact with the repository.

Typical operations:

```text
GET  → Fetch PR information
GET  → Fetch PR diff/files
POST → Add review/comment
POST → Add label
```

The important concept:

> **GitHub webhook triggers the workflow; GitHub API allows the workflow to interact with GitHub.**

---

# 10. Complete Workflow

```text
        GitHub
           │
           │ PR opened
           ↓
      Webhook Trigger
           │
           ↓
          n8n
           │
           ↓
      Fetch PR Diff
           │
           ↓
       Build Prompt
           │
           ↓
          LLM
           │
           ↓
     Review Generated
           │
       ┌───┴────┐
       ↓        ↓
   Comment     Label
       ↓        ↓
      GitHub Pull Request
```

---

# 11. Model Flexibility

The architecture is **LLM-agnostic**.

The workflow can use:

* OpenAI
* Anthropic Claude
* Google Gemini
* Other compatible LLMs

Only the model integration/credentials need to change.

### Interview Point

> "The workflow separates orchestration from the LLM provider, so the model can be swapped without redesigning the entire pipeline."

---

# 12. Important Engineering Concepts

### Event-Driven Architecture

The workflow starts because an event occurs:

```text
PR opened → Webhook → Workflow
```

### API Integration

n8n communicates with GitHub and the LLM through APIs.

### Automation

The complete code-review process happens automatically.

### Human-in-the-Loop

AI review should ideally **assist developers rather than automatically approve or reject code**.

---

# 13. Potential Production Improvements

If asked **"How would you improve this project?"**, mention:

### 1. Review only changed files

Reduces token usage.

### 2. Add security scanning

Detect:

* Hardcoded secrets
* SQL injection
* XSS
* Unsafe dependencies

### 3. Add repository-specific context

Use:

* Coding standards
* README
* Architecture documentation
* Existing code

### 4. Add RAG

Retrieve relevant project documentation before review.

```text
PR Diff
   +
Project Documentation
   ↓
   LLM
```

### 5. Prevent duplicate reviews

Track reviewed PR commits/SHAs.

### 6. Handle large PRs

Chunk large diffs instead of sending everything in one request.

### 7. Add human approval

AI suggests → Developer reviews → Developer decides.

---

# 14. Common Interview Questions

### Q1. Why use a webhook instead of polling?

> Webhooks are event-driven and notify the workflow immediately, reducing unnecessary API requests and latency compared with continuously polling GitHub.

### Q2. Why send the diff instead of the entire repository?

> The diff focuses on the changed code, reducing token usage, latency and cost.

### Q3. How does the bot post the review?

> It uses the GitHub API with appropriate authentication to create comments or reviews on the Pull Request.

### Q4. Can you change the LLM?

> Yes. The architecture can be made model-agnostic, allowing OpenAI, Claude, Gemini or another model to be substituted.

### Q5. What are the limitations?

> Large PRs can exceed model context limits, LLMs can produce incorrect reviews, API costs can increase, and AI-generated feedback should be validated by developers.

### Q6. How would you reduce LLM cost?

> Send only relevant diffs, filter unnecessary files, chunk large PRs, use smaller models for simple checks, and cache repeated information.

---

# ⭐ 30-Second Project Explanation

> **"I built an AI-powered GitHub code-review workflow using n8n. When a Pull Request is opened, GitHub sends a webhook to n8n. The workflow fetches the PR diff, sends the changed code along with a code-review prompt to an LLM, and then uses the GitHub API to post the generated review back to the Pull Request and apply a label. The architecture is modular, so the LLM can be switched between providers such as OpenAI, Claude or Gemini. The main benefits are automated, event-driven code review with reduced manual effort."**

### Must Remember

```text
GitHub Webhook
      ↓
n8n
      ↓
PR Diff
      ↓
LLM
      ↓
GitHub API
      ↓
Review + Label
```

**Key interview keywords:**
**Webhook → Event-driven → n8n → PR Diff → LLM → Prompt → GitHub API → Automation → Model Agnostic → Token Optimization → Human-in-the-loop**
