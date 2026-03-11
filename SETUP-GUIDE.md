# ChatGPT + Webfuse: Setup Guide (5 minutes)

You need: ChatGPT Business plan with admin access.

## Step 1: Enable Developer Mode

1. Go to https://chatgpt.com/admin/ca (Workspace Settings)
2. Navigate to **Permissions & Roles**
3. Enable **Developer mode / Create custom MCP connectors**
4. (Business plan: only admins/owners can do this)

## Step 2: Create the App

1. Go to **Workspace Settings > Apps > Create**
   (Or: Settings > Apps > Create)
2. Fill in:
   - **Name:** Webfuse Browser Agent
   - **Description:** Connect ChatGPT to a live browser session via Webfuse. See, click, type, and navigate any website.
   - **MCP Server URL:** `https://session-mcp.webfu.se/mcp`
   - **Authentication:** Bearer Token
   - **Token:** `rk_YOUR_REST_KEY`
3. Click **Create**

The app appears as a draft with a "Dev" label.

## Step 3: Test It

1. Open a Webfuse session at https://webfu.se/+hummer-gptapp/
2. Navigate to any website (e.g., booking.com)
3. Copy the **session ID** from the URL (the string after the hostname, e.g., `sGpUNaFXihCSxCUfb3zezgaCw`)
4. Open a new ChatGPT chat
5. Click the **+ button** (tools menu) and select **Webfuse Browser Agent**
6. Type: "My session ID is [paste ID]. Take a snapshot of the page I'm on."
7. ChatGPT will call `see_domSnapshot` via MCP and describe what it sees

## Step 4: Try Real Tasks

- "Navigate to google.com and search for Webfuse"
- "Click the first search result"
- "Take a screenshot of this page"
- "Fill in the search box with 'Amsterdam hotels' and submit"

## Step 5: Publish (optional)

1. Go to Workspace Settings > Apps > Drafts
2. Click **Publish** on Webfuse Browser Agent
3. Review safety warnings
4. Published apps appear for all workspace members

## Troubleshooting

**App not showing in tools menu?**
- Make sure developer mode is enabled for your account
- Refresh ChatGPT

**"Connection failed" or timeout?**
- Check that the Webfuse session is active (not expired)
- The MCP connection has a 3-minute limit. ChatGPT should reconnect automatically.

**Tools not working?**
- Every tool needs `session_id`. Make sure you told ChatGPT your session ID.
- If the session expired, start a new one and give ChatGPT the new ID.

## What This Proves

- ChatGPT can control a live browser session with zero code
- No backend server, no Python, no deployment
- Works for any Business/Enterprise/Edu workspace
- Same MCP tools work across ChatGPT, OpenAI Agents SDK, Claude, Cursor, etc.
