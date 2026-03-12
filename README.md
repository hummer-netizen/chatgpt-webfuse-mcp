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
│  session-mcp.        │
│  webfu.se/mcp        │
└──────────┬───────────┘
           │
┌──────────▼───────────┐
│  Live browser        │
│  session (any site)  │
└──────────────────────┘
```

No backend server. No Python code. Just ChatGPT + Webfuse.

## Prerequisites

- ChatGPT Business, Enterprise, or Edu plan (MCP connector access)
- A [Webfuse](https://webfuse.com) account with a Space
- The Automation App installed on your Space

## Quick Start

See [SETUP-GUIDE.md](SETUP-GUIDE.md) for detailed step-by-step instructions.

1. Create a Webfuse Space and generate a REST API key
2. Install the Automation App on the Space
3. In ChatGPT, create a new App with MCP connector:
   - URL: `https://session-mcp.webfu.se/mcp`
   - Auth: Token `rk_YOUR_REST_KEY`
4. Copy the GPT instructions from [gpt-config.md](gpt-config.md)
5. Open a Webfuse session, grab your session ID, and start chatting

## MCP Tools Available

| Tool | What it does |
|------|--------------|
| `navigate` | Go to a URL |
| `see_domSnapshot` | Read page DOM structure (use `webfuseIDs: true` for reliable targeting) |
| `see_accessibilityTree` | Read the accessibility tree (compact page overview) |
| `see_guiSnapshot` | Take a visual screenshot |
| `see_textSelection` | Read currently selected text |
| `act_click` | Click an element |
| `act_type` | Type into an input field |
| `act_keyPress` | Press a keyboard key (Enter, Tab, Escape, etc.) |
| `act_scroll` | Scroll the page or a container |
| `act_select` | Pick a dropdown option (by value) |
| `act_mouseMove` | Hover over an element |
| `act_textSelect` | Select text on the page |
| `wait` | Pause briefly (use sparingly) |

All tools require a `session_id` parameter. Target elements using CSS selectors, Webfuse IDs (`wf-42`), or coordinates (`[x, y]`).

## Example Conversations

### Hotel Search

> **You:** My session ID is sGpUNaFX. I'm on booking.com. Find hotels in Amsterdam for March 20-22.
>
> **ChatGPT:** *snapshots the page, finds the search form, fills destination, sets dates, clicks search*
>
> Found 12 hotels. Top 3:
> 1. Hotel V Nesplein -- 128 euros/night, 8.9 rating
> 2. The Hoxton -- 142 euros/night, 9.1 rating
> 3. Conscious Hotel -- 119 euros/night, 8.5 rating
>
> Want me to open any of these?

### Form Filling

> **You:** Fill in the contact form with: Jane Doe, jane@example.com, +31612345678
>
> **ChatGPT:** *snapshots form, fills each field using act_click + act_type*
>
> Done. Name, email, and phone filled in. The message field is empty. Want me to write something?

### Data Extraction

> **You:** Go to amazon.com/bestsellers and list the top 5 in Electronics.
>
> **ChatGPT:** *navigates, clicks Electronics, snapshots the list*
>
> Top 5 Electronics Best Sellers:
> 1. Apple AirPods Pro (2nd Gen) -- $189
> 2. Fire TV Stick 4K -- $29
> 3. Anker USB-C Charger -- $15
> ...

## Tips

- **Scope snapshots on large pages.** Full Amazon/Booking pages overflow context. Tell ChatGPT "only look at the search results" so it uses a `root` CSS selector.
- **Start with accessibility tree.** `see_accessibilityTree` is smaller than DOM snapshots. Good for getting oriented.
- **Low-quality screenshots.** Use quality 0.1-0.2 for overviews. Only use 1.0 with a root selector on small elements.
- **3-minute connection limit.** MCP connections timeout after 3 minutes. ChatGPT reconnects automatically on the next tool call.

## How It Compares to ChatGPT's Built-in Browsing

| Feature | ChatGPT Browsing | Webfuse + MCP |
|---------|-----------------|---------------|
| Uses your session (cookies, auth) | No | Yes |
| Can click, fill, submit | No | Yes |
| Works behind login walls | No | Yes |
| Controls the page you see | No | Yes |
| Read-only | Yes | No (full interaction) |

## Links

- [Webfuse Session MCP Server docs](https://dev.webfu.se/session-mcp-server/)
- [ChatGPT MCP Connectors](https://platform.openai.com/docs/guides/tools-remote-mcp)
- [Webfuse](https://webfuse.com)
- [Blog post: Turn ChatGPT Into a Browser Agent](https://webfuse.com/blog/turn-chatgpt-into-a-browser-agent-with-webfuse)
