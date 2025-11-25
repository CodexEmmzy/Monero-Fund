title: "[Grant] Web Checkout Widget — plug-and-play Monero merchant widget"
labels: ''
assignees: ''

---

### Candidate Background

My name is Charles Emmanuel (GitHub: [https://github.com/CodexEmmzy](https://github.com/CodexEmmzy)). I am a full-stack blockchain engineer with extensive experience building developer tooling, CLIs, integration SDKs, and test harnesses for L2 and storage ecosystems. Relevant prior work:

• Ox-rollup — local rollup/dev tooling (lead developer) — [https://github.com/CodexEmmzy/Ox-rollup](https://github.com/CodexEmmzy/Ox-rollup)
• RetrievalTester — storage testing tooling (lead developer) — [https://github.com/CodexEmmzy/retrieval-tester](https://github.com/CodexEmmzy/retrieval-tester)
• VoxBridge — asset conversion tooling (lead developer) — [https://github.com/CodexEmmzy/voxbridge](https://github.com/CodexEmmzy/voxbridge)

I will lead development, implement core wallet integrations, and own acceptance testing. I will contract 1–2 short-term specialists for frontend polish and QA where required. I will complete the required due diligence checklist and sanctions/KYC checks if awarded.

---

### Project Description

Project name: Web Checkout Widget (Monero)

One-sentence summary: A lightweight, secure, open-source, plug-and-play Monero checkout widget that merchants can embed with five lines of HTML/JS to accept XMR payments (invoice creation, payment detection, webhook callbacks, QR + fiat display), designed specifically for privacy-preserving UX and developer adoption.

Why Monero needs this: Monero lacks a widely-adopted, drop-in merchant widget that balances privacy, simplicity, and developer ergonomics. Existing payments stacks either require heavy server setup, lack easy client embeddings, or push merchants to self-host complex backends. This widget fills that gap: minimal integration friction, client+server options, and first-class support for subaddress-based payment detection and stagenet for testing.

License: MIT (open source). All code will be published to GitHub.

---

### Technical Approach

Architecture (two supported modes)

1. Merchant-hosted backend mode (recommended for production)
   • Merchant runs monero-wallet-rpc (or a secure hosted wallet backend).
   • Widget requests invoice creation from merchant backend (via a short API). Backend creates a subaddress/payment request, returns invoice data (subaddress, amount XMR, amount fiat, TTL, nonce).
   • Widget presents QR + copyable address, polls merchant backend for payment confirmation (backend listens using wallet RPC). Backend posts webhook / callback when confirmed.

2. Client-side/Headless mode (zero backend required for small merchants)
   • Widget uses monero-javascript (WASM) in the browser or worker to generate view-only scanning and monitor incoming payments to a merchant view-key + subaddress (merchant provides view key or a merchant scan endpoint). This is an opt-in flow (merchant must understand tradeoffs). This mode is ideal for demos and low-risk test setups.

Core components
• embeddable JS package (NPM) + CDN bundle: main widget library, headless API, and React/vanilla examples.
• server reference implementation (Node.js) for merchant wallet API and webhook handling.
• Invoice & payment manager: subaddress generation, TTL/cancellation handling, reliable payment detection, idempotency.
• Fiat pricing helper: pulls rates from a configurable pricing provider (CoinGecko or other). Rate lock TTL configurable.
• Webhook + webhook signature: merchant receives payment events.
• Testnet support: stagenet/testnet flows and test vectors.
• Security: zero storage of private keys by widget; full guidance on secure wallet/backends; CORS and CSRF guidance; rate limiting in server example.
• Docs: integration guide for static sites, Node/Express merchants, and common frameworks.

Tooling & libraries
• monero-javascript (WASM) for client-side primitives where needed.
• Use of monero-wallet-rpc for server integration.
• Node.js + Express reference backend; modern JS toolchain; Jest + Playwright for integration tests.
• CI: GitHub Actions running stagenet smoke tests.

Privacy & Safety design decisions
• Widget never asks for or stores spend keys.
• Default flow relies on merchant backend performing wallet functions (so private keys remain server-side).
• Client-side scanning is explicitly opt-in and documented as having different threat model.
• No telemetry or analytics shipped in core plugin by default; outreach metrics are opt-in and documented.

---

### Milestones and Budget (total = $12,000 USD)

Note: USD / XMR approximation: $12,000 ≈ 70–80 XMR as a ballpark. Final conversion will follow disbursement policies and the Fund’s exchange rate at payout time.

Milestone 0 — Mobilization & Spec (startup)
• Amount: $1,200 (10% startup)
• Duration: 1 week (kickoff)
• Deliverables: technical spec, acceptance tests, repo skeleton, CI smoke job, stagenet test plan.
• Acceptance criteria: Spec approved by reviewer; repo scaffold public; CI smoke demonstrates testnet connectivity.

Milestone 1 — Core widget + invoice API (MVP)
• Amount: $3,600 (30%)
• Duration: 2 weeks
• Deliverables: embeddable JS widget v0.1 (NPM + CDN build), Node reference backend API for invoice creation & payment detection (uses monero-wallet-rpc), QR display, fiat conversion helper, minimal docs and demo page.
• Acceptance criteria: On stagenet, merchant can create invoice, visitor can pay from a wallet, backend detects payment and posts a valid webhook to configured callback (demo video + logs).

Milestone 2 — Reliability, wallet integrations & headless mode
• Amount: $3,600 (30%)
• Duration: 2 weeks
• Deliverables: robust payment polling & confirmation logic, subaddress lifecycle management, stagenet/regtest scripts, monero-javascript headless (browser) demo mode, integration examples for common stacks (static site, React).
• Acceptance criteria: Headless demo completes end-to-end on stagenet; no private keys leaked; tests cover retry & TTL flows; integration guide ready.

Milestone 3 — Security review, docs, accessibility & polish
• Amount: $2,400 (20%)
• Duration: 1–2 weeks
• Deliverables: security checklist implementation, documentation (deployment guide, secure hosting guide, donation/campaign guidance), accessibility improvements, automated test coverage > 70%, packaged release (v1.0).
• Acceptance criteria: Review checklist passed, public docs published, v1.0 released to NPM/GitHub with demo site functioning.

Milestone 4 — Launch & community outreach (maintenance & adoption)
• Amount: $1,200 (10%)
• Duration: 1 week (plus optional 30-day support window)
• Deliverables: forum + dev announcements, one recorded short walkthrough, 1 workshop/demo with community (live or recorded), issue triage & minor bug fixes in 30-day window.
• Acceptance criteria: Forum post created and linked; at least one community workshop/demo scheduled or delivered; initial adopters + one integration PR or community endorsement.

Budget summary table (USD)

| Milestone |                                      Description |      Amount |
| --------- | -----------------------------------------------: | ----------: |
| Startup   |                                Spec & repo setup |      $1,200 |
| M1        |                  Core widget + invoice API (MVP) |      $3,600 |
| M2        | Reliability, wallet integrations & headless mode |      $3,600 |
| M3        |    Security review, docs, accessibility & polish |      $2,400 |
| M4        |         Launch & outreach + 30-day minor support |      $1,200 |
| **Total** |                                                  | **$12,000** |

---

### Measurable Outcomes & KPIs

Primary KPIs (to be reported at milestone completions)
• End-to-end payments: at least 10 successful stagenet transactions in automated test suite.
• Integration adoption: 3 independent demo integrations (example repos using widget).
• Developer onboarding: clear quickstart—“first payment on stagenet in under 10 minutes.”
• Package metrics: NPM package published; demo site running; GitHub repo public.
• Security: no secrets stored by widget; security checklist completed and documented.

Success criteria (how reviewers can validate)
• Provide demo video + link showing invoice creation → payment → webhook event.
• Provide test logs from CI showing automated stagenet transactions.
• Provide link to NPM/ CDN bundle and docs.
• Provide at least one independent integration (PR or example repo).

---

### Risks & Mitigations

Risk: Payment detection complexity and false positives (delays, reorgs)
• Mitigation: Use sequence-confirmation logic, configurable confirmations, and clear TTL semantics. Include automated tests simulating reorgs.

Risk: Merchant misconfiguration leading to lost funds or exposure
• Mitigation: Document secure deployment, recommend best practices, and default to server-side merchant wallet model. Provide preflight checks.

Risk: Misuse or privacy edge cases with headless mode
• Mitigation: Headless mode is opt-in, documented, and warns explicitly about its threat model. Core widget avoids sensitive client key material.

---

### Maintenance & Long-term plan

• 90-day maintenance window post-release for version bumps and compatibility fixes.
• Open governance for triage/issues; encourage community PRs.
• Possible sustainability: optional paid hosted reference service (if community demand arises); OSS remains MIT.

---

### Request & Closing

Requested grant amount: $12,000 USD.

If accepted I will: (1) complete the Due Diligence & Legal Checklist; (2) provide a receiving address or instructions for XMR transfer per the Fund’s guidelines; (3) begin work immediately on the startup milestone.

Contact / identity for follow up:
Charles Emmanuel
Email: [emmaxcharles123@gmail.com](mailto:emmaxcharles123@gmail.com)
GitHub: [https://github.com/CodexEmmzy](https://github.com/CodexEmmzy)

Notes: I request payment terms in milestone disbursements as above. I understand any sanction/KYC checks required by MAGIC must be completed before funds are disbursed.


Which of those should I produce next?
