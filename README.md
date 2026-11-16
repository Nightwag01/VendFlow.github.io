# VendFlow — Automated WhatsApp & Instagram Order Manager (Multi-Vendor SaaS)

A full-stack, multi-tenant SaaS platform that automatically converts **WhatsApp Business** messages, **Instagram DMs**, and **Instagram Post/Reel Comments** into structured, paid, and dispatched orders.

## Key Features

- **Multi-Vendor SaaS Architecture**: Isolated merchant catalogs, conversations, orders, delivery zones, riders, and Meta/payment credentials per store, plus a **SaaS Founder Console** with MRR/ARR (₦ NGN & $ USD), merchant GMV tracking, quota bars, editable subscription plans (`Starter`, `Growth`, `Pro Scale`), and a 3-step Vendor Onboarding Wizard.
- **24/7 Auto-Order Conversational Engine (`server/botEngine.js`)**:
  - Parses products, quantities, SKUs, variants, delivery areas, street addresses, and payment choices from standard English & local colloquial/Pidgin DMs.
  - **IG Comment-to-DM Auto-Closer**: Automatically DMs shoppers who comment trigger words (`PRICE`, `LINK`, product names) on Reels or posts.
  - Order tracking lookup (`VF-XXXX` / *"Where is my order?"*), automatic inventory deduction on confirmation, and keyword-triggered human handoff.
- **All Currencies & All Payment Workflows**:
  - Supports **NGN (₦), USD ($), GBP (£), EUR (€), GHS (GH₵), KES (KSh), ZAR (R), CAD (CA$)** with configurable exchange rates and multi-currency checkout per store.
  - **Paystack**, **Flutterwave**, and **Stripe** real API checkout initialization + HMAC-verified webhook listeners (with automatic fallback to an interactive built-in checkout page before live keys are added).
  - **Bank Transfer + 1-Click Receipt Verification** (local accounts + IBAN/SWIFT/Routing) and **Cash / POS on Delivery (COD)**.
- **Local Rider Dispatch, Global Couriers & Pickup**:
  - Local zone pricing & dispatch rider assignment with delivery PINs and automated WhatsApp/IG customer alerts.
  - Courier shipping (DHL Express, GIG Logistics, FedEx, UPS, Sendbox, Royal Mail) with live tracking links, plus in-store pickup.
- **Printable / Shareable Invoices & Live DM Simulator**:
  - Built-in WhatsApp & Instagram Phone Simulator connected to the exact same processing engine as real Meta webhooks, plus live SSE real-time updates across dashboard, inbox, and Kanban pipeline.

## Quick Start

```bash
cd vendflow
cp .env.example .env
npm install
npm run build
npm start        # Serves API, webhooks, SSE & SPA on http://0.0.0.0:3000
```

For active frontend + backend development:

```bash
npm run dev
```

## Connecting Live Webhooks

| Provider | Method | Endpoint | Verification |
| --- | --- | --- | --- |
| **Meta (WhatsApp + Instagram)** | `GET` / `POST` | `/api/webhooks/meta` | `hub.verify_token` + `X-Hub-Signature-256` (HMAC-SHA256) |
| **Paystack** | `POST` | `/api/webhooks/paystack` | `x-paystack-signature` (HMAC-SHA512) |
| **Flutterwave** | `POST` | `/api/webhooks/flutterwave` | `verif-hash` |
| **Stripe** | `POST` | `/api/webhooks/stripe` | `stripe-signature` (HMAC-SHA256) |

1. Open **Channels & Webhooks** inside any vendor workspace.
2. Enter your Meta `Phone Number ID`, `WABA ID`, `IG Business Account ID`, and Access Token, then toggle **Live Graph API** on.
3. Paste `https://your-domain.com/api/webhooks/meta` and your verify token into Meta Developer Console and subscribe to WhatsApp `messages` and Instagram `messages` + `comments`.
4. Open **Payments & Delivery**, enter your Paystack / Flutterwave / Stripe keys, toggle **Call Live API**, and add the corresponding payment webhook URL in each provider dashboard.
