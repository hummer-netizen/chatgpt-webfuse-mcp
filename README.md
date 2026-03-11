# ChatGPT GPT: Webfuse Browser Agent

A custom ChatGPT GPT that connects to a live browser session via [Webfuse](https://webfuse.com) and the [Session MCP Server](https://dev.webfu.se/session-mcp-server/). ChatGPT can see, click, type, and navigate any website you're browsing.

No backend server. No Python code. Just ChatGPT + Webfuse.

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

## Prerequisites

- ChatGPT Business, Enterprise, or Edu plan (MCP connector access)
- A [Webfuse](https://webfuse.com) account with a Space
- The Automation App installed on your Space

## Quick Start

1. **Create a Webfuse Space** at [Webfuse Studio](https://webfuse.com/studio/). Generate a REST API key and install the Automation App.
2. **Create the GPT** in ChatGPT: add an MCP connector pointing to `https://session-mcp.webfu.se/mcp`, authenticate with your REST key as a Bearer token.
3. **Copy the system prompt** from [`gpt-config.md`](gpt-config.md) into the GPT instructions.
4. **Open a Webfuse session**, navigate to any site, copy the session ID from the URL.
5. **Chat with the GPT**: give it your session ID and tell it what to do.

For a detailed walkthrough with screenshots, see [`SETUP-GUIDE.md`](SETUP-GUIDE.md).

## Example Conversations

### Hotel Search on Booking.com

> **You:** My session ID is sGpUNaFXihCSxCUfb3zezgaCw. I'm on booking.com. Find hotels in Amsterdam for March 20-22.
>
> **GPT:** *takes a DOM snapshot, finds the search form*
> I can see the Booking.com homepage. Let me fill in the search.
>
> **GPT:** *clicks the destination field, types "Amsterdam", selects the suggestion, sets dates, clicks Search*
> Found results. Here are the top options under 150 euros: ...
>
> **You:** Open the second one.
>
> **GPT:** *clicks the listing, takes another snapshot*
> This hotel has free cancellation until March 18. Want me to book it?

### Form Filling

> **You:** I'm on a job application page. Fill it with: Jane Doe, jane@example.com, +31612345678.
>
> **GPT:** *snapshots the page, identifies form fields*
> I see fields for Name, Email, Phone, and Cover Letter. Filling in what I have.
>
> **GPT:** *clicks and types into each field*
> Done. Name, email, and phone filled in. Cover letter is empty. Want me to write one?

### Data Extraction

> **You:** Go to amazon.com/bestsellers and list the top 5 in Electronics.
>
> **GPT:** *navigates to the URL, clicks Electronics, snapshots the results*
> Top 5 Electronics Best Sellers:
> 1. Apple AirPods Pro (2nd Gen) - $189
> 2. Fire TV Stick 4K - $29
> ...

## MCP Tools Available

| Tool | What it does |
|------|--------------|
| `navigate` | Go to a URL |
| `see_domSnapshot` | Read page DOM structure (use `webfuseIDs: true` and `root` selector) |
| `see_accessibilityTree` | Read accessibility tree (compact page overview) |
| `see_guiSnapshot` | Take a visual screenshot |
| `see_textSelection` | Read currently selected text |
| `act_click` | Click an element |
| `act_type` | Type into a field |
| `act_keyPress` | Press a key (Enter, Tab, Escape, etc.) |
| `act_scroll` | Scroll the page |
| `act_select` | Pick a dropdown option |
| `act_mouseMove` | Hover over an element |
| `act_textSelect` | Select text on the page |
| `wait` | Pause briefly |

All tools require `session_id`. Targeting: CSS selectors > Webfuse IDs (`wf-42`) > coordinates (`[x, y]`).

## Screenshots

*Screenshots show the key steps of using this integration:*

1. **Webfuse Studio** - Creating a Space and generating a REST API key
   <!-- screenshot: webfuse-studio-space-setup.png -->
2. **ChatGPT GPT Editor** - Adding the MCP connector with the Webfuse endpoint
   <!-- screenshot: chatgpt-mcp-connector-setup.png -->
3. **Browsing Session** - A Webfuse session with booking.com loaded, session ID visible in URL
   <!-- screenshot: webfuse-session-booking.png -->
4. **ChatGPT in Action** - The GPT calling MCP tools to search for hotels, with tool calls visible
   <!-- screenshot: chatgpt-hotel-search-action.png -->
5. **Result** - Search results appearing in the browser after the GPT filled and submitted the form
   <!-- screenshot: browser-search-results.png -->

## How It Differs from ChatGPT's Built-in Browsing

| | ChatGPT Browsing | Webfuse + MCP |
|---|---|---|
| Runs in | Sandboxed environment | Your real browser session |
| Auth/cookies | None (public pages only) | Your real session state |
| Interaction | Read-only | Full: click, type, scroll, navigate |
| Login walls | Cannot pass | Works (your session is already logged in) |
| You see it happen | No | Yes, in your browser tab |

## Links

- [Webfuse Session MCP Server docs](https://dev.webfu.se/session-mcp-server/)
- [ChatGPT MCP Connectors](https://platform.openai.com/docs/guides/tools-remote-mcp)
- [Webfuse](https://webfuse.com)
- [Blog Post: Turn ChatGPT Into a Browser Agent](https://webfuse.com/blog/turn-chatgpt-into-a-browser-agent-with-webfuse)
- [Related: OpenAI Agents SDK + Webfuse MCP](https://github.com/hummer-netizen/extension-openai-agents-mcp)
