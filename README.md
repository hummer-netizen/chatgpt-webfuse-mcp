# ChatGPT + Webfuse MCP

A custom ChatGPT GPT that connects to a live browser session via [Webfuse Session MCP](https://dev.webfu.se/session-mcp-server/). ChatGPT can see, click, type, and navigate any website you're browsing. No backend server required.

## What It Does

Open a Webfuse session in your browser, paste the session ID into ChatGPT, and start giving instructions. ChatGPT uses 13 browser tools (via MCP) to read pages, fill forms, click links, and extract data. You watch it happen in your browser in real time.

## Architecture

```
+----------------------+
|      ChatGPT         |
|   (Custom GPT)       |
+----------+-----------+
           | MCP connector
+----------v-----------+
|  Webfuse Session     |
|  MCP Server          |
|  session-mcp.webfu.  |
|  se/mcp              |
+----------+-----------+
           |
+----------v-----------+
|  Your live browser   |
|  session (any site)  |
+----------------------+
```

No backend server. No Python code. Just ChatGPT's MCP connector pointed at Webfuse.

## Prerequisites

- ChatGPT Business, Enterprise, or Edu plan (MCP connector support)
- A [Webfuse](https://webfuse.com) account with a Space
- The Automation App installed on your Space

## Quick Start

1. Create a Webfuse Space and generate a REST API key (`rk_...`)
2. Install the Automation App (Space > Apps > Automation)
3. In ChatGPT, create a custom App with an MCP connector (see [SETUP-GUIDE.md](SETUP-GUIDE.md))
4. Open a Webfuse session, copy the session ID from the URL
5. Tell ChatGPT: "My session ID is [paste]. Take a snapshot of this page."

Full step-by-step: **[SETUP-GUIDE.md](SETUP-GUIDE.md)**

## Configuration

| Variable | Description | Where to get it |
|----------|-------------|----------------|
| Webfuse REST API key | Used in the MCP connector config | Webfuse dashboard > Space > API Keys |
| MCP endpoint URL | `https://session-mcp.webfu.se/mcp` | Fixed, same for all users |

No `.env` file needed. The API key goes directly into the ChatGPT MCP connector configuration.

## How It Works

ChatGPT gets all 13 Webfuse Session MCP tools:

| Category | Tools |
|----------|-------|
| **See** | `see_domSnapshot`, `see_guiSnapshot`, `see_accessibilityTree`, `see_textSelection` |
| **Act** | `act_click`, `act_type`, `act_keyPress`, `act_scroll`, `act_mouseMove`, `act_select`, `act_textSelect` |
| **Navigate** | `navigate` |
| **Wait** | `wait` |

All tools require `session_id` as a parameter. Target elements via CSS selectors, Webfuse IDs (`wf-42`), or coordinates `[x,y]`.

**Tips:**
- Scope snapshots on large pages using the `root` option with a CSS selector
- Start with `see_accessibilityTree` for a compact page overview
- Use quality 0.1 for text-only snapshots (compact and readable)
- MCP connections time out after 3 minutes (ChatGPT reconnects automatically)

**Files:**

```
gpt-config.md          System prompt and GPT configuration
SETUP-GUIDE.md         Step-by-step setup instructions
README.md              This file
```

## Links

- [Webfuse](https://webfuse.com)
- [Session MCP Server docs](https://dev.webfu.se/session-mcp-server/)
- [ChatGPT MCP support](https://platform.openai.com/docs/guides/tools-mcp)

## Other Webfuse Integrations

- [OpenAI Agents SDK](https://github.com/webfuse-com/extension-openai-agents-mcp) - Python agent with browser control
- [LangChain / LangGraph](https://github.com/webfuse-com/extension-langchain-mcp) - Multi-page research agent
- [Vercel AI SDK](https://github.com/webfuse-com/extension-vercel-ai-mcp) - Next.js browsing assistant
- [LiveKit Voice Agent](https://github.com/webfuse-com/extension-livekit-mcp) - Voice-controlled browser

## License

MIT
