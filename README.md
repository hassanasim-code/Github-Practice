# MCP Series — Part 00: The USB Port for AI (What is MCP and Why It Changes Everything)

*Series: Teaching AI to Use Tools — Securely, Universally, at Scale*
*Part 00 of 03 | Reading time: ~6 min*

---

## It Started With a Frustrating Problem

Imagine you just got a brand-new AI assistant. It's brilliant — it can write, reason, summarize, and plan. You're excited. You ask it:

> *"Can you check my open bugs in Jira and tell me which ones are blocking this sprint?"*

It says: *"I don't have access to Jira."*

Fine. You try something else:

> *"Can you read the latest test results from GitHub Actions?"*

*"I don't have access to GitHub."*

> *"Can you check what's in my Google Drive spec doc?"*

*"I don't have access to Google Drive."*

The AI is smart. But it's **trapped inside a box** — completely cut off from the real world you work in.

This was the reality for every developer, tester, and builder using AI in 2023 and early 2024. Every time you needed AI to talk to a real tool — a database, a ticket system, a file — you had to write custom code to connect them. And that code only worked for *that* AI, with *that* tool, on *that* project. It didn't transfer. It didn't scale. It broke constantly.

There had to be a better way.

---

## Enter MCP — The Universal Plug for AI

In **November 2024**, Anthropic introduced the **Model Context Protocol (MCP)** — an open standard that solved this problem once and for all.

The simplest way to understand MCP:

> **MCP is the USB port for AI.**

Before USB, every device used a different cable. Printers had one connector, keyboards had another. It was chaos. USB created one universal standard — one port, every device, plug and play.

MCP does the same thing for AI tools. Before MCP, every AI-to-tool connection needed its own hand-built adapter. With MCP, you build the connection once, and any AI model can use it.

---

## Why the Old Way Was Broken

Let's be specific about what was wrong before MCP:

**Custom code everywhere.** Every AI-tool pair needed its own hand-built adapter. A team connecting Claude to Jira would write completely different code than a team connecting GPT to the same Jira. Zero reuse. Zero shared standards.

**No security model.** Authentication varied wildly — API keys here, OAuth tokens there, custom headers somewhere else. No consistent way to say "this AI is allowed to read tickets, but not delete them."

**AI lived in isolation.** Language models could only use knowledge baked into their training data. No live context. No real-time answers. Every response was a best guess from months-old information.

**Impossible to scale.** Adding a new tool meant another bespoke integration. Maintenance piled up fast. Teams were spending more time maintaining AI integrations than actually using them.

---

## How MCP Works — The Simple Version

MCP introduces a clean four-layer flow. Here's how a single request travels through it:

```
USER → AI HOST → MCP CLIENT → MCP SERVER
```

A real example: you ask *"Show me all open P1 bugs from this sprint."*

1. The **AI Host** (Claude, GPT, any LLM) receives your message and recognizes it needs real-world data.
2. It routes the request to the **MCP Client** — a protocol bridge living inside the host.
3. The MCP Client connects to the **Jira MCP Server** and sends a structured tool call.
4. The server fetches live data and returns a clean JSON result.
5. The AI reads the result and gives you a grounded, accurate answer from your *actual* sprint — not a hallucinated guess.

---

## The Six Building Blocks of MCP

MCP is built on six concepts. Understand these, and everything else clicks.

**Host** — The AI app you talk to. Claude Desktop, a custom chatbot, VS Code with an AI plugin. The host controls which servers can connect.

**Client** — Lives inside the host. Manages connections, authentication, and routing of all tool calls. You never interact with it directly.

**Server** — A lightweight service exposing tools or data to the AI. Examples: GitHub MCP Server, Jira MCP Server, File System MCP Server. Each is sandboxed and permission-controlled.

**Tools** — Functions the AI can call: `read_file()`, `search_issues()`, `send_message()`. The AI decides *when* to invoke them based on what you ask.

**Resources** — Read-only data the server exposes — files, database rows, API responses. The AI uses these as live, grounded context.

**Prompts** — Pre-defined templates that servers surface to the host, guiding how the AI should interact with their specific tools.

---

## MCP vs Traditional APIs

If you've worked with REST APIs, you might wonder: *isn't this just another API?*

Not quite.

| | Traditional API | MCP |
|---|---|---|
| Who calls it? | Developer writes explicit code | AI decides when and which tool to call |
| Schema | REST / GraphQL / SOAP — varies | Unified JSON-RPC standard |
| Auth | Custom per service | Host-controlled, structured permissions |
| Integration cost | Custom code per service | Plug-and-play — one protocol for all |
| AI awareness | AI has no native tool understanding | AI natively understands available tools |

Traditional APIs are machine-to-machine. MCP is AI-to-tool. The AI doesn't just pass requests — it *understands* the tools available to it and decides intelligently how and when to use them.

---

## MCP Works With Every Major AI Model

MCP is **not** a Claude-only technology. It was introduced by Anthropic, but the protocol is completely model-agnostic.

- **Claude** — First major AI with native MCP support. Built into Claude Desktop.
- **OpenAI GPT** — Function calling is MCP-compatible.
- **Google Gemini** — Adopting MCP tooling standards.
- **Microsoft Copilot** — MCP integration in development.
- **Open-source models via Ollama** — MCP server support growing fast.

Any LLM can be MCP-enabled with the right client.

---

## 200+ Servers Ready to Use Right Now

You don't have to build everything from scratch. The MCP ecosystem already has 200+ community and official servers:

- **Development:** GitHub, GitLab, VS Code, Docker
- **Project tools:** Jira, Asana, Trello, Linear
- **Communication:** Slack, Gmail, Google Calendar, Teams
- **Data & files:** Google Drive, File System, PostgreSQL, SQLite
- **Web & testing:** Brave Search, HTTP Fetch, Puppeteer, Playwright
- **Cloud & DevOps:** AWS, Azure, Kubernetes, Terraform

This is why MCP matters — it's not just a concept. It's a living, growing ecosystem you can start using today.

---

## What's Coming in This Series

This was the foundation. Now you understand *what* MCP is and *why* it exists. The rest of the series is entirely hands-on.

- **Part 01** — Playwright MCP with Cursor IDE: Let AI write and run your browser tests
- **Part 02** — Postman MCP with Google Antigravity: AI-powered API testing and collection management
- **Part 03** — GitHub MCP with Kiro (by AWS): AI that finds a bug, writes a fix, and opens a Pull Request

Each part is a real integration you can set up yourself in under 15 minutes.

---

*Follow to get notified when Part 01 drops.*

*If this helped you understand MCP, share it with someone who's still writing custom AI adapters.*

---

**Tags:** `#MCP` `#AI` `#ModelContextProtocol` `#QA` `#SoftwareTesting` `#DevTools` `#Anthropic`
