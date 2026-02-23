# Baker's Club — Google Sheets Schema

How Google Sheets is used to fetch orders and the exact column schema for mapping.

---

## How Get Orders Works (Google Sheets)

### 1. **Get Orders tool** (n8n Google Sheets Tool)

```
Sheet: Orders
Operation: Read / Lookup
Filter: id = author_Id (Discord user ID)
```

- **lookupColumn:** `id` — matches baker's Discord user ID
- **lookupValue:** `$('When Executed by Another Workflow').item.json.author_Id`
- Returns all rows where `id` equals the current baker's Discord ID

### 2. **Flow**

```
Order Agent calls Get Orders
    → n8n queries: SELECT * FROM Orders WHERE id = '941061248391462942'
    → Returns array of order rows
    → Order Agent formats and displays to user
```

### 3. **Data source**

Orders are added by:
- **Place Order Form** — appends via `Save to Orders` node
- **Add New Order tool** — Order Agent (when creating via chat)

Both write to the same **Orders** sheet with the same column structure.

---

## Sheet 1: Details (Baker profiles)

| Column Name | Type | Required | Source | Description |
|-------------|------|----------|--------|-------------|
| **Baker Id*** | string | ✅ | Discord user ID | Unique baker identifier (e.g. `941061248391462942`) |
| **Name*** | string | ✅ | Setup form / Agent | Baker's name |
| **Business Name*** | string | ✅ | Setup form / Agent | Bakery or business name |
| **City*** | string | ✅ | Setup form / Agent | City (e.g. Mumbai, Delhi) |
| **Phone Number*** | string | ✅ | Setup form / Agent | Baker's phone number |

**Used by:** Gate Agent (GET), Setup Agent (GET, Update Details, Setup New Account)

> **Note:** Column names may include `*` in Google Sheets (e.g. `Baker Id*`). Use the exact header as shown in your sheet.

---

## Sheet 2: Orders

| Column Name | Type | Required | Source | Description |
|-------------|------|----------|--------|-------------|
| **id** | string | ✅ | `author_Id` / `bakerId` | Baker's Discord user ID — links order to baker |
| **customer_name** | string | ✅ | Form / Agent | Customer's name |
| **item** | string | ✅ | Form / Agent | Product (e.g. "1kg Black Forest", "12 cupcakes") |
| **weight** | string | ✅ | Form / Agent | Weight if applicable (e.g. "1", "2", "500g") |
| **price** | string | ✅ | Form / Agent | Total price in ₹ |
| **phone_number** | string | ✅ | Form / Agent | Customer's phone |
| **address** | string | ✅ | Form / Agent | Full delivery address |
| **delivery_date** | string | ✅ | Form / Agent | Delivery date (ISO or display format) |
| **delivery_time** | string | ✅ | Form / Agent | Delivery time (e.g. "10 AM - 12 PM") |

**Used by:** Get Orders (read), Add New Order (append), Place Order Form (append)

---

## Setup Instructions

### 1. Create the spreadsheet

1. Go to [Google Sheets](https://sheets.google.com)
2. Create a new spreadsheet
3. Create two sheets (tabs): **Details** and **Orders**

### 2. Details sheet — Row 1 (headers)

| A | B | C | D | E |
|---|---|---|----|---|
| Baker Id* | Name* | Business Name* | City* | Phone Number* |

### 3. Orders sheet — Row 1 (headers)

| A | B | C | D | E | F | G | H | I |
|---|---|---|---|---|---|---|---|---|
| id | customer_name | item | weight | price | phone_number | address | delivery_date | delivery_time |

### 4. Column order

- **Details:** `Baker Id*` | `Name*` | `Business Name*` | `City*` | `Phone Number*`
- **Orders:** `id` | `customer_name` | `item` | `weight` | `price` | `phone_number` | `address` | `delivery_date` | `delivery_time`

### 5. Share with n8n

- Use the spreadsheet URL in all workflow nodes
- Ensure Google Sheets OAuth2 credentials have read/write access

---

## Mapping Reference

### Get Orders (read)

| n8n config | Value |
|------------|-------|
| sheetName | `Orders` |
| lookupColumn | `id` |
| lookupValue | `{{ author_Id }}` |

### Place Order Form → Save to Orders (append)

| Form field | Sheet column |
|------------|--------------|
| bakerId | id |
| customer_name | customer_name |
| item | item |
| weight | weight |
| price | price |
| phone_number | phone_number |
| address | address |
| delivery_date | delivery_date |
| delivery_time | delivery_time |

### Setup Form → Save to Details (append)

| Form field | Sheet column |
|------------|--------------|
| bakerId | Baker Id* |
| Name | Name* |
| Business_Name | Business Name* |
| City | City* |
| Phone_Number | Phone Number* |
