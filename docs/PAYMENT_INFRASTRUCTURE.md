# CicilyHomes Estates - Payment & Infrastructure Workflow

## 1. Overview & Architecture
This document outlines the secure payment gateway integration, WAF security layers, and transaction ledger architecture for **CicilyHomes Estates** (`cicilyhomes.com`). The system handles secure escrow-style revenue flows between end users, verified property owners, and the platform.

### Tech Stack & Security Layers
* **Origin Hosting:** Hostinger VPS / Shared Environment
* **Security & CDN:** Cloudflare (Custom WAF Skip rules for webhook endpoints)
* **Payment Processor:** Paystack (Test & Live Environment toggle)
* **Backend Runtime:** PHP (`/api/payments/` & `/api/webhooks/`)

---

## 2. Escrow Revenue Model & User Roles
To ensure transparent financial tracking, transactions flow through a secure escrow architecture involving three primary entities:

1. **The Tenant (Buyer/Client):**
   * Initiates the lease or property purchase from verified listings (e.g., `listing-detail.php?id=8`).
   * Authorizes the charge via the Paystack checkout modal using test/live debit cards or bank transfers.
2. **CicilyHomes Estates (Platform Escrow Holder):**
   * Acts as the intermediary holding funds securely via Paystack splits or subaccounts.
   * Listens for server-side webhook events (`/api/webhooks/paystack.php`) to confirm funds are locked in escrow before updating the database booking status to `PENDING_VERIFICATION` or `CONFIRMED`.
3. **The Landlord / Property Owner (Beneficiary):**
   * Receives payout disbursements via Paystack transfer splits or manual subaccount settlements only after lease agreements are fulfilled and verified by platform administration.

---

## 3. Endpoint Configuration & Security Routing

### A. Callback URL (Client-Side Redirection)
* **Path:** `https://cicilyhomes.com/api/payments/paystack-callback.php`
* **Purpose:** Manages browser redirects after a user completes or drops out of the Paystack checkout modal. Validates transaction reference parameters against the backend database.

### B. Webhook URL (Server-to-Server Ledger)
* **Path:** `https://cicilyhomes.com/api/webhooks/paystack.php`
* **Purpose:** Receives asynchronous `charge.success` POST events directly from Paystack servers to guarantee reliable order fulfillment, even if the user closes their browser prematurely.

### C. Cloudflare WAF Exemption Rule
Because automated server webhooks from Paystack can be flagged by aggressive bot filters, a custom Cloudflare **Skip** rule is active:
* **Rule Name:** Allow Paystack Webhooks
* **Trigger:** URI Path equals `/api/webhooks/paystack.php`
* **Actions Bypassed:** Managed WAF Rules, Bot Fight Mode, Browser Integrity Check.

---

## 4. Idempotency & Reliability Protocols
To prevent duplicate database records or double-charging during network drops or automatic webhook retries from Paystack:
* Backend handlers extract the unique transaction `reference`.
* Database checks are executed prior to ledger updates; if an event reference is already marked `COMPLETED`, the server responds immediately with an HTTP `200 OK` to gracefully terminate retry loops without duplicating entries.
