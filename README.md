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

- ChatGPT Business, Enterprise, or Edu plan (MCP connector support)
- A [Webfuse](https://webfuse.com) account with a Space
- The Automation App installed on your Space

## Setup

See [SETUP-GUIDE.md](SETUP-GUIDE.md) for detailed step-by-step instructions.

Quick version:

1. Create a Webfuse Space, generate a REST API key, install the Automation App
2. In ChatGPT workspace settings, enable developer mode
3. Create an App with MCP Server URL: `https://session-mcp.webfu.se/mcp`
4. Authenticate with your REST API key as a Bearer token
5. Copy the system prompt from [gpt-config.md](gpt-config.md)

## MCP Tools Available

The Session MCP Server provides 13 tools:

### Observation
| Tool | What it does |
|------|--------------|
| `see_domSnapshot` | Read page DOM structure (use webfuseIDs=true for targeting) |
| `see_accessibilityTree` | Read the accessibility tree (compact page overview) |
| `see_guiSnapshot` | Take a visual screenshot |
| `see_textSelection` | Read currently selected text |

### Action
| Tool | What it does |
|------|--------------|
| `act_click` | Click an element |
| `act_type` | Type into an input field |
| `act_keyPress` | Press a keyboard key (Enter, Tab, Escape, etc.) |
| `act_scroll` | Scroll the page or a container |
| `act_select` | Pick a dropdown option (by value) |
| `act_mouseMove` | Hover over an element |
| `act_textSelect` | Select text on the page |
| `navigate` | Go to a URL |
| `wait` | Pause briefly (use sparingly) |

All tools require a `session_id` parameter.

## Example Conversation

**You:** My session ID is sGpUNaFXihCSxCUfb3zezgaCw. I'm on booking.com. Find me a hotel in Amsterdam for next weekend.

**ChatGPT:** *calls `see_domSnapshot` with webfuseIDs=true*
I can see the Booking.com homepage with a search form. Let me fill it in.

**ChatGPT:** *calls `act_click` on destination field, then `act_type` with "Amsterdam"*
Typed Amsterdam. Dropdown appeared. Selecting "Amsterdam, Netherlands."

**ChatGPT:** *calls `act_click` on suggestion, fills dates, clicks Search*
Found 12 hotels under your budget. Here are the top 3...

## Targeting Elements

The `target` parameter accepts:
- **CSS selectors** (preferred): `#search-btn`, `input[name="q"]`
- **Webfuse IDs**: `wf-42` (from snapshots with webfuseIDs option)
- **Coordinates**: `[350, 200]` (x, y pixels, last resort)

## Tips

- **Use root selectors on large pages.** A full Amazon or Booking.com page has thousands of DOM nodes. Scope snapshots with a CSS root selector to avoid overflowing ChatGPT's context window.
- **Start with `see_accessibilityTree`.** It returns a compact view, much smaller than the full DOM. Good for understanding page layout before diving in.
- **Keep screenshots small.** Use quality 0.1-0.2 for overviews. Full quality only for reading tiny text on small elements.

## Limits

| Limit | Value |
|-------|-------|
| Tool call timeout | 15s |
| MCP connection duration | 3 min (reconnects automatically) |
| Tool call input | 16 KiB |
| Tool call response | 10 MiB |

## Links

- [Setup Guide](SETUP-GUIDE.md)
- [GPT Config / System Prompt](gpt-config.md)
- [Webfuse Session MCP Server docs](https://dev.webfu.se/session-mcp-server/)
- [Webfuse](https://webfuse.com)
