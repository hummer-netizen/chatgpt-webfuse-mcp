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
│  session-mcp.webfu.  │
│  se/mcp              │
└──────────┬───────────┘
           │
┌──────────▼───────────┐
│  Live browser        │
│  session (any site)  │
└──────────────────────┘
```

No backend server. No Python code. Just ChatGPT + Webfuse.

## Prerequisites

- ChatGPT Business, Enterprise, or Edu plan (MCP connector support)
- A [Webfuse](https://webfuse.com) account with a Space
- The Automation App installed on your Space

## Quick Start

1. Create a Webfuse Space and generate a REST API key (`rk_...`)
2. Install the Automation App (Session > Apps tab > Automation)
3. In ChatGPT, create a custom App with an MCP connector (see [SETUP-GUIDE.md](SETUP-GUIDE.md))
4. Open a Webfuse session, copy the session ID from the URL
5. Tell ChatGPT: "My session ID is [paste]. Take a snapshot of this page."

Full step-by-step: **[SETUP-GUIDE.md](SETUP-GUIDE.md)**

## MCP Tools Available

All tools require `session_id` as a parameter.

### Observation

| Tool | What it does |
|------|-------------|
| `see_domSnapshot` | Read page DOM structure (use `webfuseIDs: true` for reliable targeting) |
| `see_accessibilityTree` | Read the accessibility tree (compact page overview) |
| `see_guiSnapshot` | Take a visual screenshot |
| `see_textSelection` | Read currently selected text |

### Action

| Tool | What it does |
|------|-------------|
| `navigate` | Go to a URL |
| `act_click` | Click an element |
| `act_type` | Type into an input field |
| `act_keyPress` | Press a keyboard key (Enter, Backspace, Tab, etc.) |
| `act_scroll` | Scroll the page or a container |
| `act_select` | Pick a dropdown option (by value) |
| `act_mouseMove` | Hover over an element |
| `act_textSelect` | Select text on the page |
| `wait` | Pause briefly (use sparingly) |

### Targeting

Elements are targeted via the `target` parameter:

- **CSS selector** (preferred): `#search-btn`, `input[name="q"]`
- **Webfuse ID**: `wf-42` (from snapshots with `webfuseIDs: true`)
- **Coordinates**: `[350, 200]` (x, y pixels, last resort)

## Example Conversations

### Hotel search
> **You:** "My session ID is abc123. I'm on booking.com. Find hotels in Amsterdam, March 20-22, under 150 euros."
>
> ChatGPT snapshots the page, fills the search form, clicks search, and reads the results back to you with prices and ratings.

### Form filling
> **You:** "Fill in the contact form with: John Doe, john@example.com, +31612345678."
>
> ChatGPT reads the form structure, fills each field, and asks about any remaining fields (file uploads, textareas).

### Data extraction
> **You:** "Go to amazon.com/bestsellers and list the top 5 in Electronics."
>
> ChatGPT navigates, clicks the category, and returns a structured list.

See the [blog post](blog/draft.md) for detailed conversation flow examples.

## Tips

- **Scope snapshots on large pages.** Use `root` option with a CSS selector to avoid overflowing context. A full Amazon page has thousands of nodes.
- **Start with accessibility tree.** `see_accessibilityTree` is much smaller than a DOM snapshot. Good for understanding layout first.
- **Keep screenshots small.** Quality 0.1-0.2 for overviews. Only use 1.0 with a scoped `root` selector.
- **3-minute connection limit.** MCP connections time out after 3 minutes. ChatGPT reconnects automatically.

## Limits

| Limit | Value |
|-------|-------|
| Tool call timeout | 15s |
| MCP connection duration | 3 min (auto-reconnect) |
| Tool call input | 16 KiB |
| Tool call response | 10 MiB |

## Files

| File | Purpose |
|------|---------|
| `gpt-config.md` | System prompt and GPT configuration (copy into ChatGPT) |
| `SETUP-GUIDE.md` | Step-by-step setup instructions |
| `blog/draft.md` | Blog post (Webfuse format) |
| `README.md` | This file |

## The Same MCP, Everywhere

The Session MCP Server is not ChatGPT-specific. The same endpoint works with:

- [OpenAI Agents SDK](https://github.com/hummer-netizen/extension-openai-agents-mcp) (Python backend)
- ElevenLabs (voice agents)
- Claude Desktop / Cursor / VS Code
- Any MCP-compatible client

## Links

- [Webfuse Session MCP Server docs](https://dev.webfu.se/session-mcp-server/)
- [OpenAI ChatGPT MCP support](https://platform.openai.com/docs/guides/tools-mcp)
- [Webfuse](https://webfuse.com)

## Other Integrations

Webfuse MCP works with any AI framework. See the other demos:

- **[OpenAI Agents SDK](https://github.com/hummer-netizen/extension-openai-agents-mcp)** — Build a custom agent with the OpenAI Agents SDK
- **[Claude Desktop / Cursor / VS Code](https://github.com/hummer-netizen/extension-claude-mcp)** — Zero-code setup — just a config file
- **[Vercel AI SDK](https://github.com/hummer-netizen/extension-vercel-ai-mcp)** — TypeScript browsing assistant for Next.js
- **[LangChain / LangGraph](https://github.com/hummer-netizen/extension-langchain-mcp)** — Python research agent with multi-page reasoning
- **[LiveKit Voice Agent](https://github.com/hummer-netizen/extension-livekit-mcp)** — Voice-controlled browser agent with WebRTC
