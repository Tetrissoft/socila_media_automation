# Form-Based Setup & Order — Baker's Club

When the baker needs to **set up store** or **place order**, a form is triggered to collect input instead of conversational Q&A.

---

## Flow

| Trigger | Form | Action |
|---------|------|--------|
| Profile incomplete (Gate → setup) | **Setup Store Form** | Collect Name, Business Name, City, Phone → Save to Details sheet |
| Baker wants to create order | **Place Order Form** | Collect order details → Save to Orders sheet |

---

## Form URLs (pass bakerId via query param)

- **Setup Store:** `https://YOUR_N8N_INSTANCE.app.n8n.cloud/form/setup-store?bakerId={{ author_Id }}`
- **Place Order:** `https://YOUR_N8N_INSTANCE.app.n8n.cloud/form/place-order?bakerId={{ author_Id }}`

---

## 1. Setup Store Form Workflow (`n8n-setup-form.json`)

Import and publish this workflow. It includes:

1. **Form Trigger** — Form path: `setup-store`
   - Title: "Set up your store 🎂"
   - Fields: Name, Business Name, City, Phone Number, bakerId (hidden, from query param)
2. **Google Sheets** — Append to Details sheet
3. **Discord** — Send confirmation DM to user (userId = bakerId)

---

## 2. Place Order Form Workflow (`n8n-place-order-form.json`)

Import and publish this workflow. It includes:

1. **Form Trigger** — Form path: `place-order`
   - Title: "Add new order 🎂"
   - Fields: customer_name, item, weight, price, phone_number, address, delivery_date, delivery_time, bakerId (hidden)
2. **Google Sheets** — Append to Orders sheet
3. **Discord** — Send confirmation with order summary

---

## 3. Update Main Multi-Agent Flow

When route = **setup** or route = **order**, send the form link instead of running the agent:

**Setup branch:** Send Discord message:
```
Here's the form to set up your store: [Setup Form URL with ?bakerId=author_Id]
Fill it and submit — we'll save your details right away! 🎂
```

**Order branch:** Send Discord message:
```
Here's the form to add your order: [Order Form URL with ?bakerId=author_Id]
Fill in the details and submit — we'll save it and send you the order id! 🎂
```

---

## Implementation Options

### Option A: Replace agent with form link (simpler)

- When Gate routes to setup → Send form link (no Setup Agent)
- When Main routes to order → Send form link (no Order Agent)

### Option B: Agent sends form link (keeps agent for other cases)

- Setup Agent: When triggered, respond with form link
- Order Agent: When triggered, respond with form link

Use **Option A** if you want forms only for setup and order. Use **Option B** if you want the agent to handle follow-ups or mixed flows.
