# n8n-OMS — Order Management System Dashboard

> Live demo: [n8n-oms.vercel.app](https://n8n-oms.vercel.app)

## Description

**n8n-OMS** is a full-stack Order Management System (OMS) dashboard built to sit on top of **n8n** workflow automations. It gives operations teams a single interface to manage **Shopify orders**, trigger and track **WhatsApp order confirmations**, and push confirmed orders to **delivery/courier services** — all orchestrated through n8n automation workflows rather than manual, repetitive back-office work.

The project is split into two main parts:

- **`Front/`** — the client-facing dashboard (TypeScript-based web app) that order managers and support agents use daily.
- **`Back/`** — the backend service layer (TypeScript) that talks to Shopify, n8n, WhatsApp, and delivery providers, and exposes the API consumed by the frontend.

This structure lets the dashboard stay a thin, fast UI while all the heavy integration/automation logic is centralized in the backend and in n8n workflows.

## Features

- 📦 **Shopify order sync** — pulls and displays orders from a connected Shopify store in a unified dashboard view.
- 💬 **WhatsApp order confirmation** — triggers automated WhatsApp messages to customers to confirm orders before fulfillment, reducing failed/undelivered COD orders.
- 🚚 **Delivery push automation** — pushes confirmed orders to delivery/courier partners directly from the dashboard, removing manual data entry.
- ⚙️ **n8n-powered automation core** — all cross-system logic (Shopify ↔ WhatsApp ↔ Delivery) is handled via n8n workflows, making the automation logic visual, auditable, and easy to extend without redeploying code.
- 🖥️ **Centralized OMS dashboard** — a single pane of glass for order status, confirmation state, and delivery state instead of switching between Shopify admin, WhatsApp, and courier dashboards.
- 🔗 **Decoupled architecture** — clear separation between `Front` (UI) and `Back` (API/integration layer), making each side independently maintainable and deployable.
- ☁️ **Cloud-ready deployment** — live production deployment demonstrated on Vercel.

> Note: Exact feature scope depends on the n8n workflows configured for your store. The list above reflects the core purpose declared by the project (order management, WhatsApp confirmations, and delivery pushes via n8n).

## Tech Stack

- **Language:** TypeScript (~99.7% of codebase)
- **Automation engine:** [n8n](https://n8n.io) (self-hosted or n8n cloud instance, connected via webhooks/API)
- **Integrations:** Shopify Admin/API, WhatsApp (Business API or a WhatsApp-sending n8n node/service), delivery/courier provider(s)
- **Deployment:** Vercel (see live demo above)

## Installation

### Prerequisites

- [Node.js](https://nodejs.org/) (LTS version recommended) and npm/yarn/pnpm
- A running **n8n instance** (self-hosted via Docker, or n8n Cloud) with workflows set up for Shopify, WhatsApp, and your delivery provider
- A **Shopify store** with API/Admin access (API key & access token)
- **WhatsApp Business API** credentials (or the provider used by your n8n WhatsApp node)
- Delivery/courier provider API credentials

### 1. Clone the repository

```bash
git clone https://github.com/Bendaoud-Bilal/n8n-OMS.git
cd n8n-OMS
```

### 2. Set up the backend

```bash
cd Back
npm install
```

Create a `.env` file in `Back/` with your configuration, for example:

```env
PORT=4000
N8N_WEBHOOK_URL=https://your-n8n-instance.com/webhook/oms
SHOPIFY_STORE_URL=your-store.myshopify.com
SHOPIFY_API_KEY=your_shopify_api_key
SHOPIFY_ACCESS_TOKEN=your_shopify_access_token
WHATSAPP_API_KEY=your_whatsapp_api_key
DELIVERY_API_KEY=your_delivery_provider_key
```

Start the backend:

```bash
npm run dev
# or
npm run build && npm start
```

### 3. Set up the frontend

```bash
cd ../Front
npm install
```

Create a `.env` (or `.env.local`) file in `Front/` pointing to your backend, for example:

```env
NEXT_PUBLIC_API_URL=http://localhost:4000
```

Start the frontend:

```bash
npm run dev
```

The dashboard should now be available at `http://localhost:3000` (or the port shown in your terminal).

### 4. Configure n8n workflows

Import/create the n8n workflows that:
1. Receive new Shopify orders (via webhook or polling).
2. Send a WhatsApp confirmation message to the customer.
3. On confirmation, push the order to your delivery/courier provider.

Point the backend's `N8N_WEBHOOK_URL` to the corresponding n8n webhook endpoints.

## Usage Examples

**Viewing orders on the dashboard**
1. Log in to the OMS dashboard (`Front`).
2. Orders synced from Shopify appear in the main orders table with their current status (New, Awaiting Confirmation, Confirmed, Pushed to Delivery, etc.).

**Confirming an order via WhatsApp**
1. Select an order marked "New" or "Pending Confirmation."
2. Click **Send WhatsApp Confirmation**.
3. The backend triggers the corresponding n8n workflow, which sends a WhatsApp message to the customer.
4. Once the customer confirms, the order status updates automatically (via n8n webhook callback) to "Confirmed."

**Pushing a confirmed order to delivery**
1. Filter orders by status "Confirmed."
2. Select one or more orders and click **Push to Delivery**.
3. The backend calls the delivery-provider n8n workflow, creating a shipment/delivery request, and updates the order status to "In Delivery."

**Example API call (backend)**

```bash
curl -X POST http://localhost:4000/api/orders/confirm \
  -H "Content-Type: application/json" \
  -d '{"orderId": "123456"}'
```

## Contributing

Contributions are welcome! To contribute:

1. **Fork** the repository.
2. **Create a feature branch:**
   ```bash
   git checkout -b feature/your-feature-name
   ```
3. **Make your changes**, following the existing code style in `Back/` and `Front/`.
4. **Commit your changes** with clear messages:
   ```bash
   git commit -m "feat: add X"
   ```
5. **Push to your fork** and **open a Pull Request** against `main`, describing:
   - What the change does
   - Why it's needed
   - Any related n8n workflow changes required to use it
6. Ensure the app builds and runs locally (`Back` and `Front`) before submitting.

Please open an **Issue** first for large changes or new integrations (new delivery providers, new messaging channels, etc.) so the approach can be discussed beforehand.

## License

all rights are reserved by the author by default.


---
