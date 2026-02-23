# Baker's Club — Issues Found & Fixes Applied

## Issues Fixed

### 1. **Missing order_view branch (critical)**
- **Problem:** When user asked "Can you tell my pending orders?", the route went nowhere. The `order_view` output of the Switch had an empty connection `[]`.
- **Fix:** Added **Get Order Prompt** node and connected `order_view` → Get Order Prompt → Order Agent.

### 2. **Missing Get Order Prompt node**
- **Problem:** Order Agent had no system prompt. Other agents (Pricing, Marketing, etc.) get their prompts from "Get [X] Prompt" subflow; Order Agent was missing this.
- **Fix:** Added Get Order Prompt node that fetches `bakery/agents/order-agent.md` and passes it to Order Agent.

### 3. **Form URL placeholder**
- **Problem:** Setup Form Link and Order Form Link used `YOUR_N8N_INSTANCE.app.n8n.cloud` — a placeholder that would fail.
- **Fix:** Replaced with `https://n8n.tetrissoft.com`.

---

## Things to Verify

### 1. **Form paths**
- Workflow now uses: `https://n8n.tetrissoft.com/form/setup-store?bakerId=...` and `https://n8n.tetrissoft.com/form/place-order?bakerId=...`
- If your Place Order form uses a UUID path (e.g. `.../form/3ef13394-52fa-46fa-b5ce-22eff98abbad`), update the **Order Form Link** node to use that URL instead of `place-order`.

### 2. **Credentials**
- Google Sheets and Discord credentials use IDs from your instance. Reconnect if needed after import.

### 3. **Get Prompt Form Github subflow**
- Workflow ID `qZHrvRFBhUhmUHKc` must exist and accept `path` input. Ensure it returns `{ data: "prompt content" }`.

### 4. **Orders sheet column**
- Get Orders tool filters by column `id` (baker's Discord user ID). Ensure your Orders sheet has an `id` column.

---

## Form workflows (n8n-setup-form.json, n8n-place-order-form.json)

- **bakerId:** Hidden field must be pre-filled via `?bakerId=xxx` in the form URL. The main workflow passes this correctly.
- **Discord DM:** Uses `userId: $json.bakerId` — if bakerId is empty (form opened without query param), the DM will fail.
