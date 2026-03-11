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

- ChatGPT Business, Enterprise, or Edu account (MCP connector access)
- A [Webfuse](https://webfuse.com) account with a Space
- The Automation App installed on your Space

## Setup

See [SETUP-GUIDE.md](SETUP-GUIDE.md) for detailed step-by-step instructions.

Quick version:

1. Create a Webfuse Space with the Automation App installed
2. Generate a REST API key (`rk_...`)
3. In ChatGPT, create an App with MCP connector pointed at `https://session-mcp.webfu.se/mcp`
4. Set Token auth with your REST key
5. Copy the system prompt from [gpt-config.md](gpt-config.md)

## Example Conversations

### Hotel search on Booking.com

> **You:** My session ID is sGpUNaFXihCSxCUfb3zezgaCw. I'm on booking.com. Find me a hotel in Amsterdam for March 20-22, under 150 euros.
>
> **ChatGPT:** *takes a DOM snapshot of the page*
> I can see the Booking.com homepage with the search form. Let me fill it in.
>
> *clicks the destination field, types "Amsterdam", selects the suggestion*
> *sets check-in and check-out dates*
> *clicks Search*
>
> Found 12 hotels under 150 euros. Here are the top 3:
> 1. Hotel V Nesplein -- 128 euros/night, 8.9 rating
> 2. The Hoxton -- 142 euros/night, 9.1 rating
> 3. Conscious Hotel Museum Square -- 119 euros/night, 8.5 rating
>
> Want me to open any of these?

ChatGPT reads the live page via `see_domSnapshot`, fills the form with `act_click` and `act_type`, then extracts results from the updated page.

### Data extraction from Amazon

> **You:** Go to amazon.com/bestsellers and list the top 5 in Electronics.
>
> **ChatGPT:** *navigates to the URL, takes a snapshot scoped to the Electronics section*
> Top 5 Electronics Best Sellers:
> 1. Apple AirPods Pro (2nd Gen) -- $189
> 2. Amazon Fire TV Stick 4K -- $29
> 3. Anker USB-C Charger -- $15
> 4. Apple AirTag (4 pack) -- $79
> 5. Samsung 128GB microSD -- $12

ChatGPT uses `navigate` to open the page, then `see_domSnapshot` with a `root` CSS selector to read only the relevant section (avoiding context overflow on large pages).

### Form filling

> **You:** I'm on a job application page. Fill it in: John Doe, john@example.com, +31612345678.
>
> **ChatGPT:** *snapshots the form, identifies fields, fills each one*
> Done. Name, email, and phone are filled in. The Cover Letter field is empty and there's a Resume upload button. Want me to write a cover letter?

ChatGPT reads the form structure first, then uses `act_click` + `act_type` on each field.

## MCP Tools Available

| Tool | What it does |
|------|--------------|
| `navigate` | Go to a URL |
| `see_domSnapshot` | Read page DOM (use `webfuseIDs` and `root` options) |
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
| `wait` | Pause briefly (use sparingly) |

All tools require a `session_id` parameter.

## Key Advantages

- **Zero code** -- no backend, no scripts, no deployment
- **Real browser session** -- your cookies, your auth, your state
- **Any website** -- Webfuse proxies everything, no per-site setup
- **Same MCP tools** -- works identically with OpenAI Agents SDK, Claude, Cursor, etc.
- **13 tools** covering navigation, observation, and interaction

## Limits

| Limit | Value |
|-------|-------|
| MCP connection duration | 3 min (auto-reconnects) |
| Tool call timeout | 15s |
| Tool call input | 16 KiB |
| Tool call response | 10 MiB |

## Links

- [Setup Guide](SETUP-GUIDE.md)
- [GPT Configuration](gpt-config.md)
- [Webfuse Session MCP Server docs](https://dev.webfu.se/session-mcp-server/)
- [OpenAI Agents SDK version](https://github.com/hummer-netizen/extension-openai-agents-mcp) (for programmatic agents)
- [Webfuse](https://webfuse.com)
