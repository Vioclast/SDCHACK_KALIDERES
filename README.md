# SDCHACK_KALIDERES

# FlowUp

# Team Information 

# Links

#Overview

#Problem 
Micro and small grocery retailers in Malaysia run daily operations across WhatsApp, spreadsheets, paper notes and memory. Sales and stock are recorded in different places, owners often learn an item is out of stock only when a customer asks for it, and financial figures are hard to verify. Late reorders, expired goods and unchecked records quietly cost margin, and small teams cannot afford complex systems or formal training.

#Solution
FlowUp is a connected operations hub for a retail grocery store, designed as a reusable module for the Orbyte ecosystem. It brings sales, stock, suppliers and finance into one place and automates the routine checks that owners normally do by hand.

#Key Features 
- **Automatic stock reminders** — FlowUp checks stock conditions and flags items that need checking or restocking before they run out, so owners no longer find out when a customer asks.
- **One connected store system** — Cashier sales update stock automatically, while products, suppliers, finance and budget live in one place with dashboards and charts for sales, stock and spending.
- **Role-based control with human approval** — Owner, cashier, supply, finance and analyst each see only what they need. The system flags data that needs review, a person verifies or corrects it, and every action is kept in an audit log.

#Project Structure 
.
├── client/                      # React frontend: role dashboards, cashier, goods receiving
│   └── src/
├── server/                      # Express backend
│   ├── calc/                    # Pure formula functions: stock reminder, supplier score, discount
│   ├── ai/                      # RunPod client with template fallback
│   ├── routes/                  # REST API endpoints
│   └── db/                      # Schema and synthetic seed data
├── runpod/
│   └── handler.py               # RunPod serverless handler (GPU workload)
├── tests/
│   └── calc.test.ts             # Unit tests against reference numbers
├── docs/                        # Screenshots, diagrams
├── submission/                  # Required submission documents
│   ├── SDC2026_Kalideres_Preliminary.pdf       # Preliminary submission
│   ├── SDC2026_Kalideres_TechDeclaration.pdf   # Technology declaration
│   ├── SDC2026_Kalideres_Replit_POU.pdf        # Replit proof of use
│   └── SDC2026_Kalideres_Runpod_POU.pdf        # RunPod proof of use
├── .env.example                 # Example environment variables (no real keys)
├── .gitignore
├── .replit                      # Replit run configuration
├── LICENSE                      # Apache-2.0
├── package.json                 # Node dependencies
└── README.md

#Architecture 
User (Owner / Cashier / Warehouse / Finance)
        │
        ▼
Replit: FlowUp app
 ├─ Frontend   role-based dashboards
 ├─ Backend    rules and formulas (stock reminder, supplier score, discount),
 │             approvals, audit log
 └─ Database   synthetic store data
        │  computed numbers only
        ▼
RunPod Endpoint (GPU model)
        │  short explanations and supplier claim drafts (text only)
        ▼
Replit backend → shown in the dashboard → a person approves → action logged

All numbers come from deterministic backend rules. The model only writes text
and never calculates. If RunPod is unavailable, the app falls back to templates.

#How we use replit 
- **Development:** FlowUp was built in Replit. We first wrote a structured specification (roles, data flow, formulas and reference test numbers), then used Replit Agent to generate the app and refined it in the Replit workspace. [Add "in a shared Repl using Multiplayer" only if the team really did this.]
- **Hosting:** The app is deployed with Replit Deployments at the Live Demo link above. [Keep only after the app is deployed.]
- **Database:** Synthetic store data is stored in the Replit-hosted database. [Check whether the app uses SQLite or Postgres and name it.]
- **Secrets:** The RunPod API key and endpoint ID are stored in Replit Secrets, never in the code. `.env.example` lists variable names only

## Challenges & Learnings

**Challenges**
- **Scope.** Our first design covered the whole store (cashier, warehouse, finance, owner). The brief asks for one clear use case, so we narrowed the MVP to one journey: goods received → compared with the purchase order → refund claim approved → stock and supplier score updated → reorder and discount recommendations. Other modules moved to the roadmap.
- **Consistent numbers.** Dashboards must agree with each other. We defined every formula once (stock reminder, EOQ, holding cost, supplier score) and checked the results against a reference dataset and a reconciliation check (opening stock + goods in − cost of goods sold = closing stock).
- **Little data for a new store.** A new SME has almost no history. FlowUp collects two weeks of data, labels early results as estimates, and excludes out-of-stock days from average sales so demand is not understated.
- **Keeping AI honest.** We did not want a model inventing numbers. All numbers come from rules; AI only writes short explanations and supplier message drafts, with a template fallback if it is unavailable. [Delete this point if RunPod is not connected.]

**What we learned**
- A detailed specification before prompting Replit Agent gave far better results than short prompts.
- Human approval plus an audit log makes automation easier for SME owners to trust.
- Rules should decide the numbers; AI should only explain them.

  ## Future Improvements

- [ ] QR scanning for incoming stock
- [ ] Full purchase order workflow with supplier confirmation
- [ ] Shift cash close for cashiers
- [ ] AI demand forecasting and unusual-transaction detection
- [ ] Expiry-risk suggestions that learn from past discount results
- [ ] Connection to the Orbyte hub and messaging apps for supplier claims
- [ ] Multi-store support and a PDPA-ready production deployment


