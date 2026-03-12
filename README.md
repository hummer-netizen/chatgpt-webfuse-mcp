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

- ChatGPT Business, Enterprise, or Edu plan (MCP connector support)
- A [Webfuse](https://webfuse.com) account with a Space
- The Automation App installed on your Space

## Setup

See [SETUP-GUIDE.md](SETUP-GUIDE.md) for the detailed 5-minute walkthrough.

Quick version:
1. Create a Webfuse Space, generate a REST key, install Automation App
2. In ChatGPT: create a custom App with MCP connector pointing to `https://session-mcp.webfu.se/mcp`
3. Authenticate with your REST key (Token auth)
4. Copy the system prompt from [gpt-config.md](gpt-config.md)
5. Open a Webfuse session, give ChatGPT the session ID, start chatting

## MCP Tools Available

| Tool | What it does |
|------|--------------|
| `navigate` | Go to a URL |
| `see_domSnapshot` | Read page DOM structure (use `webfuseIDs: true` for targeting) |
| `see_accessibilityTree` | Read the accessibility tree (compact page overview) |
| `see_guiSnapshot` | Take a visual screenshot |
| `see_textSelection` | Read currently selected text |
| `act_click` | Click an element |
| `act_type` | Type into a field |
| `act_keyPress` | Press a key (Enter, Backspace, Tab, etc.) |
| `act_scroll` | Scroll the page |
| `act_select` | Pick a dropdown option (by value) |
| `act_mouseMove` | Hover over an element |
| `act_textSelect` | Select text on the page |
| `wait` | Pause briefly (use sparingly) |

All tools require `session_id`. Element targeting uses CSS selectors, Webfuse IDs (`wf-42`), or `[x,y]` coordinates.

## Conversation Examples

### Hotel Search

**You:** "My session ID is abc123. I'm on booking.com. Find a hotel in Amsterdam for March 20-22."

**ChatGPT:** Takes a DOM snapshot, finds the search form, types "Amsterdam", selects dates, clicks Search. Then snapshots the results and lists options with prices and ratings. All happening in your real browser tab.

### Form Filling

**You:** "Fill in this contact form: John Doe, john@example.com, +31612345678."

**ChatGPT:** Snapshots the form to find fields, then clicks and types into each one. Reports back what it filled and what's left (file uploads, free text fields).

### Data Extraction

**You:** "Read the pricing table on this page and compare the plans."

**ChatGPT:** Takes a scoped DOM snapshot of the pricing section, extracts plan names, prices, and features, and presents a structured comparison.

### Multi-step Navigation

**You:** "Go to my account settings and change my display name to 'NicP'."

**ChatGPT:** Navigates to settings (clicking through menus), finds the display name field, clears it, types the new name, and clicks Save. Verifies the change with a follow-up snapshot.

## Tips

- **Use root selectors on large pages.** Tell ChatGPT "only look at the search form" to avoid context overflow on complex sites.
- **Start with accessibility tree.** Smaller than DOM snapshots, good for understanding page layout.
- **Keep screenshots low quality** (0.1-0.2) for overviews. High quality only for reading small text.
- **3-minute connection limit.** Long tasks may need reconnection. ChatGPT handles this automatically.

## Why Not ChatGPT's Built-in Browsing?

ChatGPT's browser opens pages in a sandbox. It can read public pages but can't use your session (cookies, auth), interact with the page (click, fill, submit), or control what you're actually looking at. Webfuse gives it your real session.

## Links

- [Setup Guide](SETUP-GUIDE.md)
- [GPT Configuration](gpt-config.md)
- [Blog Post](blog/draft.md)
- [Webfuse Session MCP Server docs](https://dev.webfu.se/session-mcp-server/)
- [Webfuse](https://webfuse.com)
