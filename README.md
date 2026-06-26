# Google Tasks MCP Server

A powerful, privacy-first Model Context Protocol (MCP) server that brings your Google Tasks seamlessly into Claude and other MCP-compatible clients. Manage your tasks through natural conversation without leaving your workflow.

## Table of Contents

1. [What Can You Do With This](#what-can-you-do-with-this)
2. [For End Users: Using the Hosted Server](#for-end-users-using-the-hosted-server)
3. [Example Conversations](#example-conversations)
4. [Configuration Checklist](#configuration-checklist)
5. [Troubleshooting](#troubleshooting)
6. [Privacy & Security](#privacy--security)
7. [For Developers: Self-Hosting](#for-developers-self-hosting)
8. [API Reference](#api-reference)
9. [Contributing](#contributing)
10. [License](#license)

## What Can You Do With This

This MCP server grants Claude access to your Google Tasks data, enabling you to:

**Task Management**
- Create, read, update, and delete task lists
- Create, read, update, delete, and move tasks
- Mark tasks as complete or incomplete
- Batch clear all completed tasks from a list

**Task Organization**
- Create subtasks (nested tasks)
- Set due dates and times
- Add notes and descriptions
- Organize tasks across multiple lists

**Query & Filter**
- List all your task lists
- Retrieve tasks by list
- Filter tasks by completion status
- View tasks with upcoming due dates

All through natural conversation with Claude or any other MCP-compatible client.

## For End Users: Using the Hosted Server

The easiest way to use this MCP server is through the public hosted instance. No installation or configuration required.

### Prerequisites

- A Google Account with Google Tasks
- Claude Desktop installed on your computer
- Internet connection

### Setup Instructions

#### Step 1: Add Connector in Claude Desktop

1. Open **Claude Desktop**
2. Click the **Settings** gear icon in the bottom-left corner
3. Navigate to the **Connectors** tab
4. Click **Add Custom Connector**
5. Fill in the following details:
   - **Name:** Google Tasks (or any name you prefer)
   - **Remote MCP server URL:** `https://google-tasks-mcp.gabriel-dalton.deno.net/mcp`
6. Click **Add**

#### Step 2: Connect and Authorize

1. In the Connectors settings, find your new Google Tasks connector
2. Click **Connect** next to it
3. Your web browser will open with the Google authorization page
4. Sign in with your Google account
5. Review and approve the requested permissions
6. You will be redirected back to confirm the connection

Once authorized, Claude will have full access to your Google Tasks.

### Quick Start Examples

Try these prompts in Claude to get started:

```
"What task lists do I have?"
"Show me all tasks in my Work list"
"Create a new task list called 'Q4 Goals'"
"Add a task 'Review budget proposal' to my Work list with due date tomorrow"
"Mark 'Update documentation' as complete"
"Move 'Write proposal' to be a subtask of 'Client Project'"
"Clear all completed tasks from my Shopping list"
"What tasks are due this week?"
"Create a task 'Buy groceries' in my Shopping list with notes about items needed"
```

## Example Conversations

Here are realistic, multi-turn conversation examples showing how to use this MCP effectively.

### Example 1: Creating and Organizing a Project

**User:** I need to organize my Q4 projects. Can you create a task list called 'Q4 Projects'?

**Claude:** I'll create that task list for you.

*[Creates task list]*

I've created the 'Q4 Projects' task list. What projects would you like to add?

**User:** I have three: Complete the accessibility audit, redesign the landing page, and implement API rate limiting. Add those as tasks.

**Claude:** I'll add those three tasks to your Q4 Projects list.

*[Creates three tasks]*

All three tasks have been added to your Q4 Projects list. Would you like to set due dates for any of them?

**User:** Yes. The accessibility audit is due next Friday, landing page redesign is due in two weeks, and the rate limiting should be done in three weeks.

**Claude:** I'll set those due dates for you.

*[Updates tasks with due dates]*

Done! Your Q4 Projects are now organized with due dates. Is there anything else you'd like to add?

### Example 2: Daily Task Review

**User:** What do I have due this week?

**Claude:** Let me check your tasks across all lists.

*[Retrieves all tasks and filters by due date]*

You have 5 tasks due this week:
- Tuesday: Review Q4 reports (Work)
- Wednesday: Update documentation (Development)
- Thursday: Call client (Sales)
- Friday: Prepare presentation (Work)
- Saturday: Team retrospective (Work)

Would you like me to mark any as complete or adjust due dates?

**User:** I completed the client call yesterday. Mark it done.

**Claude:** I'll mark that task as complete.

*[Marks task as complete]*

Done! You now have 4 tasks remaining for this week. Anything else?

### Example 3: Subtask Organization

**User:** I'm working on a big project. Can you add it to my tasks and break it into steps?

**Claude:** I'd be happy to help. What's the project name, and what steps does it need?

**User:** Project name is 'Launch new API'. The steps are: design endpoints, implement authentication, write tests, create documentation, deploy to staging, and deploy to production.

**Claude:** I'll create the main task and add those as subtasks.

*[Creates main task and subtasks]*

Perfect! I've created 'Launch new API' with all 6 subtasks:
1. Design endpoints
2. Implement authentication
3. Write tests
4. Create documentation
5. Deploy to staging
6. Deploy to production

You can mark each subtask complete as you progress. Let me know when you're ready to move to the next step!

## Configuration Checklist

Use this checklist to ensure everything is properly configured:

### Google Cloud Setup

- [ ] Google Cloud Project created (visit console.cloud.google.com)
- [ ] Google Tasks API enabled (APIs & Services > Library > search "Google Tasks")
- [ ] OAuth 2.0 Web application credentials created
- [ ] Authorized redirect URI configured: `https://google-tasks-mcp.gabriel-dalton.deno.net/callback`
- [ ] OAuth consent screen configured (at minimum, add scopes: `https://www.googleapis.com/auth/tasks`)
- [ ] Client ID noted (looks like: `XXXXX-XXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXX.apps.googleusercontent.com`)
- [ ] Client Secret saved securely

### Claude Desktop Configuration

- [ ] Claude Desktop is installed and up to date
- [ ] Opened Settings > Connectors tab
- [ ] Added custom connector with:
  - [ ] Name: "Google Tasks" (or your preferred name)
  - [ ] URL: `https://google-tasks-mcp.gabriel-dalton.deno.net/mcp`
- [ ] Clicked "Connect" and authorized via Google
- [ ] Browser confirmed successful authorization

### Verification

- [ ] Claude shows "Google Tasks" as available in Connectors
- [ ] Can ask Claude "What task lists do I have?" and get a response
- [ ] Can create a new task and see it appear in Google Tasks

## Troubleshooting

### Authorization Issues

**Problem:** "Authorization failed. You can check your credentials and permissions if this persists."

**Causes & Solutions:**
- **Expired token:** Google's OAuth refresh tokens expire if unused for 6 months. Simply re-authorize by clicking "Connect" in Claude's Connectors settings again.
- **Wrong Google account:** Make sure you're signing in with the account that has your Google Tasks. You can have multiple Google accounts; use the correct one.
- **Permissions not granted:** When Google prompts you to approve permissions, make sure you click "Allow" or "Authorize". If you skipped this step, click "Connect" again.
- **Redirect URI mismatch:** If you're using a self-hosted version, verify that your redirect URI in the Google Cloud console exactly matches your deployment URL.

**Fix:** Re-authorize the connector:
1. Go to Claude Desktop > Settings > Connectors
2. Find Google Tasks connector
3. Click the "..." menu and select "Disconnect"
4. Click "Connect" again
5. Complete the authorization flow

### No Tasks or Task Lists Appear

**Problem:** Claude says "No task lists found" or "No tasks in this list"

**Causes & Solutions:**
- **Tasks in a different Google account:** Only your currently logged-in Google account's tasks are visible. Check that you're using the right account.
- **Google Tasks app not synced:** Make sure you have created or accessed your tasks in the Google Tasks app or Gmail before trying to access them via this MCP. Sometimes a first-time sync is needed.
- **Custom task lists not visible:** Only custom task lists you've created appear. The system default list (if you have one) may not be accessible through this API.

**Fix:** 
1. Go to Google Tasks (tasks.google.com or Gmail Tasks sidebar)
2. Create or modify at least one task
3. Wait 30 seconds for sync
4. Try again in Claude

### Task Creation Fails

**Problem:** "Error creating task" or similar error message when trying to add a task

**Likely Causes & Solutions:**
- **Task list ID missing:** Make sure you're specifying which task list to add the task to. Example: "Add 'Buy groceries' to my Shopping list" (not just "Add a task").
- **Invalid characters in title:** Avoid very long titles (>500 characters) or titles with special formatting that might not be compatible.
- **Permissions issue:** Verify the connector is still authorized (see Authorization Issues above).

**Fix:**
1. Make sure you specify the task list name clearly
2. Keep task titles concise and simple
3. If still failing, re-authorize the connector

### Some Task Lists Don't Show Up

**Problem:** "I know I have more task lists, but Claude only shows a few"

**Causes:**
- **Secondary accounts:** Task lists may be in a different Google account than the one you authorized
- **Shared task lists:** Shared task lists may not always sync immediately; wait a few moments and try again
- **API delay:** Sometimes the API takes a moment to sync. Try asking again in 30 seconds.

**Fix:**
1. Verify you're logged into the correct Google account
2. Refresh the connector (disconnect and reconnect)
3. Make sure all task lists are visible in Google Tasks (tasks.google.com)

### Performance or Timeouts

**Problem:** Claude takes a very long time to respond or times out

**Causes:**
- **Large task lists:** If you have thousands of tasks, retrieving them all can take time
- **Network latency:** Slow internet connection may cause delays
- **Rate limiting:** Very frequent requests may trigger Google's rate limits

**Fix:**
- Try asking more specific questions: "Show me tasks from my Work list due this week" instead of "Show me all my tasks"
- Ensure you have a stable internet connection
- Wait a few minutes if you've made many requests in quick succession

### Connector Shows "No Tools Available"

**Problem:** Claude says "This connector has no tools available" or won't respond to task requests

**Causes:**
- **Authorization incomplete:** The connector didn't fully authorize
- **Server issue:** The hosted server may be temporarily down
- **Browser cache:** Stale cache preventing proper connection

**Fix:**
1. Disconnect the connector (click "..." menu, select Disconnect)
2. Hard-refresh Claude Desktop (Cmd+Shift+R on Mac or Ctrl+Shift+R on Windows)
3. Reconnect the connector and complete authorization
4. Restart Claude Desktop if still failing

### Privacy Concerns

**Problem:** "Is my task data secure?" or "What is being logged?"

See the [Privacy & Security](#privacy--security) section below for detailed information.

## Privacy & Security

Your privacy is our top priority. This section explains the security measures protecting your data.

### Encrypted Token Storage

All Google authentication tokens are encrypted at rest using industry-standard encryption:

- **Algorithm:** AES-256-GCM (Galois/Counter Mode, authenticated encryption)
- **Key Derivation:** PBKDF2 with 100,000 iterations
- **Uniqueness:** Each user's tokens are encrypted with a unique salt per encryption operation
- **Tamper Detection:** GCM mode detects any tampering with encrypted data

Even if the server's database were compromised, your tokens would remain protected. See the implementation: [`src/utils/encryption.ts`](https://github.com/Gabriel-Dalton/google-tasks-mcp/blob/main/src/utils/encryption.ts)

### Privacy-Safe Logging

The server is configured to log operational events and errors, but sensitive information is automatically redacted:

**We DO log:**
- Request timestamps and endpoints accessed
- Success/failure of operations
- General error messages (without sensitive data)

**We DO NOT log:**
- Google OAuth tokens or refresh tokens
- User IDs or email addresses
- Task content or personal data
- API request/response payloads
- Authentication credentials

See the implementation: [`src/utils/logger.ts`](https://github.com/Gabriel-Dalton/google-tasks-mcp/blob/main/src/utils/logger.ts)

### No Analytics Tracking

This server does not use Google Analytics, tracking pixels, or any third-party analytics. We have no insight into what tasks you're working on or when you use this service.

### OAuth 2.0 Security

We use Google's OAuth 2.0 protocol for authentication:

- **Standard Protocol:** OAuth 2.0 is an industry standard used by virtually all major services
- **No Password Storage:** Your Google password is never stored or transmitted to us. Google handles authentication directly.
- **Scoped Access:** The server only requests access to the Google Tasks API scope, nothing else
- **User Control:** You can revoke access anytime from your Google Account settings (Manage your Google Account > Security > Third-party apps with account access)

### Token Refresh Lifecycle

**Important:** Google's OAuth refresh tokens expire if unused for 6 months. This means:

- **First use:** Your token works immediately upon authorization
- **Regular use:** As long as you use this connector at least once every 6 months, your token will remain valid
- **Expired token:** If more than 6 months pass without use, you'll need to re-authorize (see Troubleshooting: Authorization Issues)

This is a Google security feature to protect your account.

### Audit the Code

The source code is public and auditable:

- **Encryption logic:** [`src/utils/encryption.ts`](https://github.com/Gabriel-Dalton/google-tasks-mcp/blob/main/src/utils/encryption.ts)
- **Logging (redaction):** [`src/utils/logger.ts`](https://github.com/Gabriel-Dalton/google-tasks-mcp/blob/main/src/utils/logger.ts)
- **Token management:** [`src/auth/token-store.ts`](https://github.com/Gabriel-Dalton/google-tasks-mcp/blob/main/src/auth/token-store.ts)
- **OAuth flow:** [`src/auth/oauth.ts`](https://github.com/Gabriel-Dalton/google-tasks-mcp/blob/main/src/auth/oauth.ts)
- **Server middleware:** [`src/server/middleware.ts`](https://github.com/Gabriel-Dalton/google-tasks-mcp/blob/main/src/server/middleware.ts)

### Disclaimer

This server is provided as-is without any guarantees or warranties. While every effort has been made to ensure security and privacy, no guarantees are made about availability, data integrity, or security. Use at your own risk. For production use cases with sensitive data, consider self-hosting your own instance on your own infrastructure.

## For Developers: Self-Hosting

Want to run your own instance? Here's how to deploy this MCP server yourself.

### Prerequisites

- Node.js 18+ and npm installed
- Deno CLI installed (for deployment)
- A Google Cloud Platform account
- A hosting platform (Deno Deploy, Vercel, or your own server)

### Step 1: Create Google Cloud Project

1. Go to [Google Cloud Console](https://console.cloud.google.com)
2. Create a new project or select an existing one
3. Enable the Google Tasks API:
   - Go to APIs & Services > Library
   - Search for "Google Tasks API"
   - Click Enable
4. Create OAuth 2.0 credentials:
   - Go to APIs & Services > Credentials
   - Click "Create Credentials" > "OAuth client ID"
   - Choose "Web application"
   - Add authorized redirect URIs: `https://your-domain.com/callback`
   - Save your Client ID and Client Secret
5. Configure OAuth consent screen:
   - Go to APIs & Services > OAuth consent screen
   - Set up minimal requirements
   - Add scopes: `https://www.googleapis.com/auth/tasks`

### Step 2: Clone and Setup

```bash
# Clone the repository
git clone https://github.com/Gabriel-Dalton/google-tasks-mcp.git
cd google-tasks-mcp

# Install dependencies
npm install

# Generate encryption secret
npm run generate-secret
# Copy the output - you'll need it for environment variables
```

### Step 3: Local Development

```bash
# Copy environment template
cp .env.example .env

# Edit .env with your values:
# GOOGLE_CLIENT_ID=your_client_id
# GOOGLE_CLIENT_SECRET=your_client_secret
# GOOGLE_REDIRECT_URI=https://your-tunnel-url.com/callback
# ENCRYPTION_SECRET=paste_generated_secret_here
# PORT=3000

# Build the project
npm run build

# Run with Deno
deno task dev
```

Note: Google requires a publicly accessible URL for OAuth callbacks. For local development, use ngrok or similar tunneling service to expose your local server.

### Step 4: Deploy to Deno Deploy (Recommended)

```bash
# Install Deno Deploy CLI
deno install -A --unstable https://deno.land/x/deploy/deployctl.ts

# Build first
npm run build

# Deploy to Deno Deploy
deployctl deploy --project=your-project-name build/index.js
```

Then, in the Deno Deploy dashboard:
1. Go to your project settings
2. Add all required environment variables (see .env.example)
3. Restart the deployment

### Step 5: Update Google OAuth Settings

1. Go back to Google Cloud Console
2. Navigate to Credentials
3. Update the OAuth client redirect URI to match your deployed URL: `https://your-domain.com/callback`

### Step 6: Configure Your MCP Client

**For Claude Desktop:**
1. Open Claude Desktop
2. Go to Settings > Connectors tab
3. Click Add Custom Connector
4. Fill in:
   - Name: Google Tasks
   - URL: `https://your-domain.com/mcp`
5. Click Add and connect

**For Other MCP Clients:**

Configure with the following details:
- **Server URL:** `https://your-domain.com`
- **Transport:** Server-Sent Events (SSE)
- **Endpoint:** `/mcp`
- **Authentication:** OAuth 2.0
- **Discovery URL:** `/.well-known/oauth-authorization-server`

## API Reference

### Available Tools

#### Task Lists

- **list_task_lists** - Returns all authenticated user's task lists
- **get_task_list** - Returns a specific task list by ID
- **insert_task_list** - Creates a new task list
- **update_task_list** - Updates a task list (full update)
- **patch_task_list** - Updates a task list (partial update)
- **delete_task_list** - Deletes a task list

#### Tasks

- **list_tasks** - Returns all tasks in a task list (optionally filtered by status)
- **get_task** - Returns a specific task by ID
- **insert_task** - Creates a new task
- **update_task** - Updates a task (full update)
- **patch_task** - Updates a task (partial update)
- **delete_task** - Deletes a task
- **clear_completed_tasks** - Clears all completed tasks from a list
- **move_task** - Moves a task to a different position or parent (for subtasks)

### Environment Variables Reference

| Variable | Required | Description |
|----------|----------|-------------|
| GOOGLE_CLIENT_ID | Yes | Your Google OAuth client ID |
| GOOGLE_CLIENT_SECRET | Yes | Your Google OAuth client secret |
| GOOGLE_REDIRECT_URI | Yes | OAuth callback URL (must match Google Cloud Console settings) |
| ENCRYPTION_SECRET | Yes | 32+ character secret for token encryption (generate with npm run generate-secret) |
| PORT | No | Server port (default: 3000) |
| LOG_LEVEL | No | Logging level: trace, debug, info, warn, error (default: info) |
| ALLOWED_ORIGINS | No | Comma-separated list of allowed CORS origins for browser clients |

### Development Commands

```bash
npm run build          # Compile TypeScript to JavaScript
npm run dev            # Watch mode - recompile on changes
npm run generate-secret # Generate encryption secret for ENCRYPTION_SECRET env variable
```

### Project Structure

```
src/
├── auth/              # OAuth 2.0 authentication & token storage
│   ├── oauth.ts       # OAuth flow implementation
│   └── token-store.ts # Encrypted token persistence
├── server/            # Hono app, MCP endpoints, middleware
│   ├── app.ts         # Main server setup
│   ├── mcp-endpoints.ts # MCP protocol handlers
│   ├── middleware.ts  # Security & CORS middleware
│   └── rate-limiter.ts # Rate limiting
├── tools/             # MCP tools for Google Tasks API
│   ├── tasks.ts       # Task management tools
│   └── tasklists.ts   # Task list management tools
├── google/            # Google Tasks API client
├── utils/             # Utilities
│   ├── encryption.ts  # Token encryption/decryption
│   ├── logger.ts      # Privacy-safe logging
│   └── timestamp.ts   # Date/time utilities
└── index.ts           # Main entry point
```

## Contributing

This is an open-source project and contributions are welcome! Please:

1. Fork the repository
2. Create a feature branch (`git checkout -b feature/your-feature`)
3. Make your changes
4. Submit a pull request

Please ensure your code follows the existing style and includes appropriate tests.

## License

MIT License - see [LICENSE](LICENSE) file for details.

## Support

- **Issues:** Report bugs or request features on [GitHub Issues](https://github.com/Gabriel-Dalton/google-tasks-mcp/issues)
- **Google Tasks API Docs:** [Google Tasks API Documentation](https://developers.google.com/tasks)
- **MCP Protocol:** [Model Context Protocol Documentation](https://modelcontextprotocol.io)

## Acknowledgments

Built with:
- [Model Context Protocol](https://modelcontextprotocol.io) by Anthropic
- [Google Tasks API](https://developers.google.com/tasks)
- [Hono](https://hono.dev) web framework
- [Deno Deploy](https://deno.com/deploy) for hosting
