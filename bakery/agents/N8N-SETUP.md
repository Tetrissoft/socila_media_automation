# n8n Workflow Setup Guide — Baker's Club Multi-Agent (Merged)

## Import the Workflows

1. **Main flow:** Import `n8n-workflow.json`
2. **Form workflows:** Import `n8n-setup-form.json` and `n8n-place-order-form.json`
3. **Prerequisite:** You need the **Get Prompt Form Github** subflow (workflow ID `qZHrvRFBhUhmUHKc`) that fetches prompts by `path`

### Form URL configuration

In the main workflow, edit **Setup Form Link** and **Order Form Link** nodes: replace `YOUR_N8N_INSTANCE.app.n8n.cloud` with your actual n8n base URL (e.g. `yourname.app.n8n.cloud` or your self-hosted URL).

---

## Flow Overview

```
When Executed by Another Workflow
    │
    ▼
Get Gate Prompt (path: bakery/agents/gate-agent.md)
    │
    ▼
Gate Agent (GET only) → Parse Route → IF (setup/main)
    │
    ├── setup  → Setup Form Link → Discord (sends form URL)
    │
    └── main   → Pass Context → Main Router → Switch
                      │
                      ├── order    → Order Form Link → Discord (sends form URL)
                      ├── pricing  → Get Pricing Prompt  → Pricing Agent  → Discord
                      ├── marketing→ Get Marketing Prompt→ Marketing Agent→ Discord
                      ├── customer → Get Customer Prompt→ Customer Agent → Discord
                      ├── business → Get Business Prompt→ Business Agent → Discord
                      └── general  → Get General Prompt  → General Agent  → Discord
```

---

## Merged Design (from your working agent)

| Component | Source |
|-----------|--------|
| **Trigger** | Execute Workflow Trigger — receives `chatInput`, `author_Id`, `channel` from parent |
| **Prompts** | Execute Workflow subflow "Get Prompt Form Github" with `path` (e.g. `bakery/agents/gate-agent.md`) |
| **Model** | Google Gemini Chat Model |
| **Memory** | Simple Memory, session key = `author_Id`, context window = 2 |
| **Tools** | GET, Update Details, Setup New Account, Add New Order (Google Sheets) |
| **Response** | Discord — send `$json.output` to user |

---

## Configuration Steps

### 1. Parent Workflow (calls this workflow)

Your parent workflow (e.g. Discord bot) must execute this workflow with:

- `chatInput` — user message
- `author_Id` — Discord user ID (used as Baker ID and session key)
- `channel` — (optional) channel ID

### 2. Get Prompt Form Github Subflow

Ensure workflow `qZHrvRFBhUhmUHKc` exists and accepts `path` (e.g. `bakery/agents/gate-agent.md`). It should return `{ data: "prompt content" }`.

### 3. Credentials

Reconnect if needed:

- **Google Gemini** — for all agents
- **Google Sheets OAuth2** — for GET, Update Details, Setup New Account, Add New Order
- **Discord Bot** — for Send a message

### 4. Discord Node

Update **Send a message**:

- `guildId` — your Discord server
- `userId` — `={{ $('When Executed by Another Workflow').item.json.author_Id }}`
- `content` — `={{ $json.output }}`

### 5. Google Sheets

- **Details** sheet — columns: Baker Id*, Name*, Business Name*, City*, Phone Number*
- **Orders** sheet — columns: id, customer_name, item, weight, price, phone_number, address, delivery_date, delivery_time

### 6. Form workflows (Setup Store & Place Order)

- Import and **publish** `n8n-setup-form.json` and `n8n-place-order-form.json`
- Form URLs (production): `.../form/setup-store?bakerId=...` and `.../form/place-order?bakerId=...`
- Pass `bakerId` (Discord user ID) as query param so the form pre-fills the hidden field and Discord can DM the user

---

## Tool Assignment

| Tool | Gate | Setup | Order | Others |
|------|------|-------|-------|--------|
| GET | ✅ | ✅ | ✅ | — |
| Setup New Account | ❌ | ✅ | ❌ | — |
| Update Details | ❌ | ✅ | ❌ | — |
| Add New Order | ❌ | ❌ | ✅ | — |

---

## Data Flow Notes

- **author_Id** = Baker ID = Discord user ID (session key)
- **Main Router** uses `chatInput` for keyword-based routing (order, pricing, marketing, etc.)
- **Pass Context to Main** adds `chatInput` and `author_Id` to the main branch for the Main Router
