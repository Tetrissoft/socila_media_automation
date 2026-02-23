# Baker's Club — Multi-Agent Architecture

Meera is split into multiple focused agents. **Gate Agent** routes by profile; **Main Agent** routes by intent; specialized agents handle the response.

---

## Flow

```
User Message
     │
     ▼
┌─────────────────┐
│   Gate Agent    │  ← Calls GET, checks profile
│   (GET only)    │
└────────┬────────┘
         │
    ┌────┴────┐
    │         │
    ▼         ▼
 setup      main
    │         │
    ▼         ▼
┌───────┐  ┌─────────────────┐
│Setup  │  │  Main Agent     │  ← Router: reads intent
│Agent  │  │  (no tools)     │
└───────┘  └────────┬────────┘
                    │
        ┌───────────┼───────────┼───────────┬──────────┬──────────┬──────────┐
        │           │           │           │          │          │          │
        ▼           ▼           ▼           ▼          ▼          ▼          ▼
    ┌───────┐  ┌─────────┐  ┌──────────┐  ┌────────┐  ┌────────┐  ┌────────┐  ┌────────┐
    │ Order │  │ Bakery  │  │ Pricing  │  │Marketing│  │Customer│  │Business│  │ General│
    │ Agent │  │ Lab     │  │ Agent    │  │ Agent   │  │ Agent  │  │ Agent  │  │ Agent  │
    └───────┘  └─────────┘  └─────────┘  └──────────┘  └────────┘  └────────┘  └────────┘
```

---

## Agents

| Agent | File | Tools | Purpose |
|-------|------|-------|---------|
| **Gate Agent** | `gate-agent.md` | GET | Check profile → `setup` \| `main` |
| **Setup Agent** | `setup-agent.md` | GET, Setup New Account, Update Details | Collect profile, save to sheet |
| **Main Agent** | `main-agent.md` | — | Router: intent → `order` \| `bakery_lab` \| `pricing` \| `marketing` \| `customer` \| `business` \| `general` |
| **Order Agent** | `order-agent.md` | GET, Add New Order | Save orders, order id (UUID), confirmations |
| **Bakery Lab Agent** | `bakery-lab-agent.md` | — | Recipes, techniques, troubleshooting, Indian baking (20 yrs experience) |
| **Pricing Agent** | `pricing-agent.md` | — | Recipe costing, profit margins, pricing |
| **Marketing Agent** | `marketing-agent.md` | — | Instagram, WhatsApp, content, hashtags |
| **Customer Agent** | `customer-agent.md` | — | Customer replies, complaints, confirmations |
| **Business Agent** | `business-agent.md` | — | FSSAI, expansion, corporate orders, prompts |
| **General Agent** | `general-agent.md` | — | Greetings, wins, stress, simple questions |

---

## Routing Logic

### Step 1: Gate Agent
- Profile incomplete → **Setup Agent**
- Profile complete → **Main Agent**

### Step 2: Main Agent (only when profile complete)
- Order-related → **Order Agent**
- Baking/recipes/techniques → **Bakery Lab Agent**
- Pricing-related → **Pricing Agent**
- Marketing-related → **Marketing Agent**
- Customer-related → **Customer Agent**
- Business-related → **Business Agent**
- General / unclear → **General Agent**

---

## Tool Assignment

| Tool | Gate | Setup | Order | Others |
|------|------|-------|-------|--------|
| GET | ✅ | ✅ | ✅ | — |
| Setup New Account | ❌ | ✅ | ❌ | — |
| Update Details | ❌ | ✅ | ❌ | — |
| Add New Order | ❌ | ❌ | ✅ | — |

---

## Workflow Implementation (n8n / LangGraph)

1. **Gate Agent** → GET → output `{"route": "setup"}` or `{"route": "main"}`
2. If `setup` → **Setup Agent** (tools: GET, Setup New Account, Update Details)
3. If `main` → **Main Agent** (no tools) → output `{"route": "order"|"bakery_lab"|"pricing"|"marketing"|"customer"|"business"|"general"}`
4. Branch to the chosen specialized agent
5. Specialized agent responds to the user

---

## n8n Workflow

- **Import:** `bakery/agents/n8n-workflow.json` — copy/paste into n8n (Import from File or Clipboard)
- **Setup guide:** `bakery/agents/N8N-SETUP.md` — configuration steps, tool assignment, prompts

---

## Shared Context

All agents receive:
- `{{BAKER_ID}}`
- Conversation history (from Memory)

Profile-complete agents also receive:
- `{{BAKER_NAME}}`, `{{BAKER_BUSINESS_NAME}}`, `{{BAKER_CITY}}`, `{{BAKER_PHONE}}`

Order Agent also receives:
- `{{ORDERS_LIST}}`
