# SJIBL demo videos

Screen recordings of Unistax ERP against the Shahjalal Islami Bank Limited (SJIBL) RFP's 24 named
modules (`unistax/unistax` repo, `web/RFP-COVERAGE-SJIBL.md`), plus the sign-in flow. Recorded with
Playwright against a live kind cluster (main app, tenant `shatadal`, dev-auth) and a live
Casdoor-backed instance of the separate `unistax/accounting` app (module 24 only).

Each video is a short tour (real, live screens; no fabricated data) rather than a scripted demo
script. Two videos (`01`, `05`) also walk through a real end-to-end create flow (raising a
requisition, floating a tender) rather than just a read-only tour.

| File | Module |
|---|---|
| `00-login-flow.webm` | Sign-in flow (real Casdoor OIDC round trip, recorded against the `accounting` app since the main app's kind deployment runs `--dev-auth`) |
| `01-requisition-management-module.webm` | Requisition Management |
| `02-e-procurement-management-module.webm` | e-Procurement Management |
| `03-e-auction-management-module.webm` | e-Auction Management |
| `04-vendor-enlistment-portal.webm` | Vendor Enlistment Portal |
| `05-tender-bid-submission-module.webm` | Tender/Bid Submission (floating a tender end to end) |
| `06-electronic-dashboards.webm` | Electronic Dashboards |
| `07-contract-management.webm` | Contract Management |
| `08-invoicing-payment-module-procurement.webm` | Invoicing & Payment (procurement-relevant: PO amendment, retention, tender payment, warranty) |
| `09-audit-compliance-reports.webm` | Audit & Compliance Reports |
| `10-core-module.webm` | Core Module (users, roles, teams, inbox) |
| `11-workflow-delegation-management.webm` | Workflow/Delegation Management |
| `12-repair-maintenance-management-module.webm` | Repair & Maintenance Management |
| `13-bms-utility-management.webm` | Building Management System — Utility Management |
| `14-bms-interior-civil-works-management.webm` | Building Management System — Interior & Civil Works |
| `15-bms-medical-supplies-management.webm` | Building Management System — Medical Supplies |
| `16-bms-insurance-management.webm` | Building Management System — Insurance |
| `17-bms-transport-management.webm` | Building Management System — Transport |
| `18-bms-visitor-management.webm` | Building Management System — Visitor Management |
| `19-bms-canteen-management.webm` | Building Management System — Canteen Management |
| `20-dispatch-management.webm` | Dispatch Management |
| `21-warehouse-management.webm` | Warehouse Management |
| `22-inventory-management.webm` | Inventory Management |
| `23-others-functional-module-administrator.webm` | Others Functional Module (Administrator) — identity/access half |
| `23b-others-functional-module-administrator-platform.webm` | Others Functional Module (Administrator) — platform admin half (migration console, notifications, integrations) |
| `24-invoicing-payment-module-accounting-side.webm` | Invoicing & Payment, accounting-side (journal entries, budgets, VAT return Mushak-9.1) — a separate application, `unistax/accounting`, not the main monorepo |

## Known gaps found while recording

- `/inv/dispatch` and `/inv/gate-pass` (main app) hard-fail with a `query_not_a_reference` error in
  the generic query engine's entity definitions (`inv.dispatch.from_warehouse_id`,
  `inv.gate_pass.premises_org_unit_id`, `inv.issue.to_org_unit_id`). Worked around in `20-dispatch-
  management.webm` by using pick-list/packing-list/issue screens instead; not fixed.
- e-Auction and Vendor Enlistment portal screens render correctly but show an "account not linked"
  state for the dev-auth admin principal, since it has no portal-realm bidder/vendor identity.
