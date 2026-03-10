# ChatGPT GPT App + Webfuse Session MCP

## What This Is

A custom ChatGPT GPT that connects to a live Webfuse browser session via MCP.
Users install the GPT, open a Webfuse session, and ChatGPT can see and control
the web page they're browsing.

## Why This Matters

OpenAI's GPT platform supports MCP connectors natively. This means:
- No backend server needed (ChatGPT calls MCP directly)
- Works in ChatGPT web and mobile
- Millions of ChatGPT users can access it
- Zero infrastructure for the end user

## Architecture

```
ChatGPT GPT App
    ↓ MCP (hosted connector)
Webfuse Session MCP Server
    ↓
Live browser session (any website)
```

The GPT uses OpenAI's hosted MCP integration. The user provides:
1. Their Webfuse Space hostname
2. Their REST API key
3. The session ID from their active session

## Deliverables

1. GPT configuration (instructions, name, description)
2. MCP connector config for the GPT
3. README with setup steps
4. Blog post draft
5. Screenshots/diagrams

## Key Difference from OpenAI Agents SDK Version

- Agents SDK = developer builds a Python agent (programmatic)
- GPT App = end user installs a GPT in ChatGPT (no-code, consumer-facing)

Both use the same Webfuse Session MCP Server underneath.
