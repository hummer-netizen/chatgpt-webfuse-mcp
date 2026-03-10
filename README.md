# ChatGPT GPT: Webfuse Browser Agent

A custom ChatGPT GPT that connects to a live browser session via [Webfuse](https://webfuse.com) and the [Session MCP Server](https://dev.webfu.se/session-mcp-server/). ChatGPT can see, click, type, and navigate any website you're browsing.

```
┌──────────────────────┐
│      ChatGPT         │
│   (GPT App)          │
└──────────┬───────────┘
           │ MCP (hosted connector)
┌──────────▼───────────┐
│  Webfuse Session     │
│  MCP Server          │
│  session-mcp.HOST/   │
└──────────┬───────────┘
           │
┌──────────▼───────────┐
│  Live browser        │
│  session (any site)  │
└──────────────────────┘
```

No backend server. No Python code. Just ChatGPT + Webfuse.

## Prerequisites

- ChatGPT Plus or Enterprise account (GPT access)
- A [Webfuse](https://webfuse.com) account with a Space
- The Automation App installed on your Space

## Setup

### 1. Create a Webfuse Space

1. Go to [Webfuse Studio](https://webfuse.com/studio/)
2. Create a new Space
3. Generate a REST API key (Settings > API Keys)
4. Install the Automation App (Session > Apps tab)
5. Restart the session

### 2. Create the GPT

1. Go to [ChatGPT GPT Editor](https://chatgpt.com/gpts/editor)
2. Name: "Webfuse Browser Agent"
3. Description: "Connect ChatGPT to a live browser session via Webfuse"
4. Copy the system prompt from `gpt-config.md`
5. Add MCP connector:
   - Server URL: `https://session-mcp.YOUR_HOSTNAME/mcp`
   - Auth: Bearer token with your REST API key

### 3. Use It

1. Open your Webfuse Space URL in a browser tab
2. Open ChatGPT and select the Webfuse Browser Agent GPT
3. Give it your session ID (from the URL)
4. Tell it what to do: "Open booking.com and find hotels in Amsterdam"

## MCP Tools Available

| Tool | What it does |
|------|--------------|
| `navigate` | Go to a URL |
| `see_domSnapshot` | Read page structure |
| `see_a11ySnapshot` | Read accessibility tree |
| `see_guiSnapshot` | Take a screenshot |
| `act_click` | Click an element |
| `act_type` | Type into a field |
| `act_pressKey` | Press a key |
| `act_scroll` | Scroll the page |
| `act_select` | Pick a dropdown option |
| `act_hover` | Hover over an element |

## Links

- [Webfuse Session MCP Server docs](https://dev.webfu.se/session-mcp-server/)
- [OpenAI GPT Actions](https://platform.openai.com/docs/actions)
- [Webfuse](https://webfuse.com)
