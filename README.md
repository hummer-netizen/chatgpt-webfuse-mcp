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

## Why Not ChatGPT's Built-in Browsing?

ChatGPT's browsing tool opens pages in a sandbox. It can read public pages, but it can't:

- Use **your** logged-in session (your cookies, your auth)
- **Interact** with the page (click, fill, submit)
- Work on pages behind a **login wall**
- Control the page **you're actually looking at**

Webfuse flips this. The session runs in YOUR browser. Real cookies. Real auth. Real state. ChatGPT just gets tools to interact with it.

## Prerequisites

- ChatGPT Business, Enterprise, or Edu account (with admin access for MCP connectors)
- A [Webfuse](https://webfuse.com) account with a Space
- The Automation App installed on your Space

## Quick Setup (5 minutes)

See [`SETUP-GUIDE.md`](SETUP-GUIDE.md) for step-by-step instructions.

**Short version:**

1. Create a Webfuse Space, generate a REST key, install Automation App
2. Create a ChatGPT App with MCP connector pointing to `https://session-mcp.webfu.se/mcp`
3. Open a Webfuse session, tell ChatGPT your session ID, start browsing

## MCP Tools Available

All tools require `session_id` as a parameter.

### Observation

| Tool | What it does |
|------|--------------|
| `see_domSnapshot` | Read page DOM structure (use `webfuseIDs: true` for targeting) |
| `see_accessibilityTree` | Read accessibility tree (compact, good for large pages) |
| `see_guiSnapshot` | Take a visual screenshot |
| `see_textSelection` | Read currently selected text |

### Action

| Tool | What it does |
|------|--------------|
| `navigate` | Go to a URL |
| `act_click` | Click an element |
| `act_type` | Type into a field |
| `act_keyPress` | Press a keyboard key |
| `act_scroll` | Scroll the page |
| `act_select` | Pick a dropdown option |
| `act_mouseMove` | Hover over an element |
| `act_textSelect` | Select text on the page |
| `wait` | Pause briefly (use sparingly) |

### Targeting elements

- **CSS selector** (preferred): `#search-btn`, `.result-item:first-child`
- **Webfuse ID**: `wf-42` (from snapshots with `webfuseIDs: true`)
- **Coordinates**: `[350, 200]` (x, y pixels, last resort)

## Conversation Examples

### Hotel Search

**You:** "My session ID is sGpUNaFXihCSxCUfb3zezgaCw. I'm on booking.com. Find me a hotel in Amsterdam for March 20-22, under 150 euros."

ChatGPT snapshots the page, finds the search form, types "Amsterdam", selects dates, clicks Search. Then reads the results:

> "Found 12 hotels under 150 euros. Top 3:
> 1. Hotel V Nesplein - 128 euros/night, 8.9 rating
> 2. The Hoxton - 142 euros/night, 9.1 rating
> 3. Conscious Hotel Museum Square - 119 euros/night, 8.5 rating
>
> Want me to open any of these?"

### Form Filling

**You:** "I'm on a job application page. Fill it with: John Doe, john@example.com, +31612345678."

ChatGPT snapshots the form, identifies fields, clicks and types into each one. Reports what it filled and what's left (cover letter, resume upload).

### Data Extraction

**You:** "Go to amazon.com/bestsellers and list the top 5 items in Electronics."

ChatGPT navigates to the URL, clicks Electronics, reads the results using a scoped DOM snapshot, and returns a clean list with prices.

## Pro Tips

- **Use root selectors on large pages.** A full Booking.com page has thousands of DOM nodes. Tell ChatGPT "only look at the search results" so it scopes the snapshot with a CSS root selector.
- **Start with the accessibility tree.** `see_accessibilityTree` is much smaller than a full DOM snapshot. Good for understanding page layout first.
- **Keep screenshots small.** Quality 0.1-0.2 for overviews. Quality 1.0 only with a root selector on a small element.
- **MCP connection limit is 3 minutes.** ChatGPT reconnects automatically, but long pauses between actions may require a reconnect.

## GPT Configuration

The full system prompt and GPT settings are in [`gpt-config.md`](gpt-config.md).

## Links

- [Webfuse Session MCP Server docs](https://dev.webfu.se/session-mcp-server/)
- [ChatGPT MCP connectors](https://help.openai.com/en/articles/11116676-connecting-to-third-party-tools-through-mcp-in-chatgpt)
- [Webfuse](https://webfuse.com)
