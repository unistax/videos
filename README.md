# SJIBL demo videos

Screen recordings of Unistax ERP against the Shahjalal Islami Bank Limited (SJIBL) RFP's 24 named
modules (`unistax/unistax` repo, `web/RFP-COVERAGE-SJIBL.md`), plus the sign-in flow.

**Every video is recorded from a real, completed login.** Nothing here shows a logged-out screen or
a dev-auth "you are not signed in" state. Two of the infra pieces this required:

- The main app's local kind deployment normally runs `--dev-auth` (header-trusted, no real login) for
  developer convenience. For these recordings the tenant (`shatadal`) was additionally provisioned
  with a real Casdoor Organization/Application (`unistax install --casdoor-endpoint ...`), and a
  second `vst-svc` instance was run locally with `--casdoor-auth --dev-roots` (the `--dev-roots` flag
  is new: it lets `--casdoor-auth` be exercised against a dev-signed license locally, the same escape
  hatch `unistax install` already had, just not `vst-svc` until now — see commit history).
- Real Casdoor users were created for 11 SJIBL-relevant personas (procurement-officer, finance-officer,
  auditor, employee, facilities-manager, dispenser, insurance-officer, fleet-manager, security-officer,
  store-keeper, security-administrator) plus the existing tenant admin, each granted the matching
  `design-contract/ui/role-catalogue.json` role, so the "user" video per module is a real, narrowly-
  scoped staff login and the "admin" video is the tenant administrator.

Each module has two videos: **user** (the day-to-day staff role that domain's RFP text describes) and
**admin** (the tenant administrator). Module 24 is a separate application (`unistax/accounting`, not
the monorepo) with only one seeded login, so both its videos share that login and differ by which
screens they tour.

| File | Module | User persona |
|---|---|---|
| `00a-login-flow-staff.webm` / `00b-login-flow-admin.webm` | Sign-in flow | procurement-officer / admin |
| `01-requisition-management-module-*` | Requisition Management | procurement-officer |
| `02-e-procurement-management-module-*` | e-Procurement Management | procurement-officer |
| `03-e-auction-management-module-*` | e-Auction Management | procurement-officer |
| `04-vendor-enlistment-portal-*` | Vendor Enlistment Portal | procurement-officer |
| `05-tender-bid-submission-module-*` | Tender/Bid Submission | admin (see note) |
| `06-electronic-dashboards-*` | Electronic Dashboards | auditor |
| `07-contract-management-*` | Contract Management | procurement-officer |
| `08-invoicing-payment-module-procurement-*` | Invoicing & Payment (procurement-relevant) | finance-officer |
| `09-audit-compliance-reports-*` | Audit & Compliance Reports | auditor |
| `10-core-module-*` | Core Module | employee |
| `11-workflow-delegation-management-*` | Workflow/Delegation Management | employee |
| `12-repair-maintenance-management-module-*` | Repair & Maintenance Management | facilities-manager |
| `13-bms-utility-management-*` | BMS — Utility Management | facilities-manager |
| `14-bms-interior-civil-works-management-*` | BMS — Interior & Civil Works | facilities-manager |
| `15-bms-medical-supplies-management-*` | BMS — Medical Supplies | dispenser |
| `16-bms-insurance-management-*` | BMS — Insurance | insurance-officer |
| `17-bms-transport-management-*` | BMS — Transport | fleet-manager |
| `18-bms-visitor-management-*` | BMS — Visitor Management | security-officer |
| `19-bms-canteen-management-*` | BMS — Canteen Management | employee |
| `20-dispatch-management-*` | Dispatch Management | store-keeper |
| `21-warehouse-management-*` | Warehouse Management | store-keeper |
| `22-inventory-management-*` | Inventory Management | store-keeper |
| `23-others-functional-module-administrator-*` | Others Functional Module (Administrator) | security-administrator |
| `24-invoicing-payment-module-accounting-side-*` | Invoicing & Payment, accounting-side | dev@example.com (separate app) |

## Notes and known gaps, found while recording

- **05, user video**: `procurement-officer` does not hold `proc.sourcing_event.post` (floating a
  tender is reserved to `head-of-procurement`/admin per the role catalogue), so the user video for
  this module uses the admin login for the create flow instead. Everything else in this module
  (documents, slabs) is toured as intended.
- **08 (Invoicing/Payment, procurement)**: `finance-officer` cannot reach `/proc/po-amendment`,
  `/proc/retention-entry`, or `/fin/warranty` (all denied) — only `/proc/tender-payment`. Substituted
  `/fin/dashboard` and `/fin/forecast`, both real screens this role does hold. The admin video tours
  all four original routes.
- **15 (Medical Supplies), user video**: `dispenser` holds `med.prescription.read` but not `.write`,
  so `/med/prescription/new` is denied; substituted the read-only `/med/prescription` index.
- **17 (Transport)**: `/fleet/driver` is denied for both the `fleet-manager` persona and the `admin`
  persona used here — a real gap, not a swap-around choice.
- **20/22 (Dispatch, Inventory)**: `/inv/dispatch` and `/inv/gate-pass` hard-failed with a real
  `query_not_a_reference` error in the generic query engine's entity definitions
  (`inv.dispatch.from_warehouse_id`, `inv.gate_pass.premises_org_unit_id`,
  `inv.issue.to_org_unit_id`). **Fixed** (`unistax/unistax@37d2d867f`, `unistax/design@fffdda8`):
  all six columns were bare UUIDs with no `REFERENCES` clause in both the promoted migration and
  the design source, despite every one of the three screens already assuming the reference. These
  videos still route around the bug since they were recorded before the fix; the routes now work.
- **03 (e-Auction), admin video**: the "Approve disposal proposals" panel showed a real backend
  error, `query_template_column: proc.disposal_proposal.row_version is an entity-template
  column...`. **Fixed** (`unistax/unistax@7208ea27e`): both approval panels requested `row_version`
  on a browse-shaped query, which the query builder's own security rule refuses; now fetched via a
  record-shaped query right before each approve action instead.
- **04 (Vendor Enlistment)**: the vendor-application list's row click was dead code —
  `IndexPage.vue`'s `openRow` looked for an `id` column the query never requested, so `router.push`
  never fired. **Fixed** (`unistax/unistax@9d6488fcd`), and not just for this entity: `id` is now
  always requested and always hidden from the grid, so the same dead click is fixed on all 625
  affected generated index pages, not only this one.
- **06 (Dashboards)**: `/reports/budget-vs-actual` does not exist in this repo (budget/GL reporting
  lives in the separate `accounting` app). Substituted the real `proc.tender_dashboard` report.
- **23 (Others/Administrator)**: `/notify/webhook` and `/intg/message` are not real routes in
  `web/src/router/screens.ts`. Substituted `/admin/roles` and `/admin/teams`.
