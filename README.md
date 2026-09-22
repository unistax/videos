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

## `sjibl-demo-final.mp4` — the narrated, scored walkthrough

Unlike every other video in this folder, this is not a per-module or per-officer tour: it is a
single, continuous, narrated story (Google TTS) following `unistax/unistax`'s own
`demo/sjibl/DEMO-RUN-SHEET.md` end to end, structured around the five things the RFP evaluation
actually scores — functional capabilities, usability, workflow suitability, reporting, compliance —
named out loud as the video moves through them. 756.6s (12.6 min): tight because it is the
run sheet's own real click sequence at automation speed and real TTS pacing, not the run sheet's own
45-minute *live-presenter* timing padded out to match.

| Segment | Covers |
|---|---|
| 0 | Open, name the five criteria: SJIBL branding, light/dark theme, sign in as tenant-administrator |
| 1 | Raise & approve a requisition (maker/checker, two real logins) |
| 2 | Float a tender through its own approval graph |
| 3 | Five tender methods on one bank: OTM, LTM, QM, DPM, two-stage |
| 4 | Seeded bids: technical envelope opening, scoring by two evaluators, financial envelope, comparative statement |
| 5 | Purchase proposal → purchase order → award, all three bidders notified at once |
| 6 | The vendor/bidder portal, live and unassisted: self-signup, enlistment, staff approval, pseudonymous auction bidding |
| 7 | The requisition dashboard report: filtered, run, exported |
| 8 | The audit trail and its hash chain |
| 9 | Scorecard against all five criteria |
| 10 | Q&A |

Every action in it is a real click against the live `shatadal` tenant, not staged: real personas
(`farhan.kabir`, `rokeya.begum`, `nasrin.akter`, `shahidul.islam`, plus two freshly self-registered
portal accounts, a vendor and a bidder), real requisition/tender/bid/award records, a real winning
auction bid.

**Five real product defects were found and fixed while producing it**, each confirmed live before
the affected segment was recorded (or re-recorded):

- **Segment 2**: the seal-custodian picker on `/proc/tender/float` refused every search, tenant-wide
  — no two-envelope tender could ever have its envelopes opened by anyone.
  Fixed `unistax/unistax@5d7b8cc95`.
- **Segment 6 (vendor enlistment)**: the generic entity editor sent a checkbox field's value as JSON
  where PostgreSQL's array columns need `{...}` syntax, refusing every save with a matching field.
  Fixed `unistax/unistax@89ad7069c`.
- **Segment 6 (document upload)**: the tenant's file store (`rustfs`) had been stuck for 36 hours on
  an OpenShift-only security-context default that k3s refuses. Fixed `unistax/unistax@569f983aa`.
- **Segment 7**: the report-run screen's own copy promised "download, and email me when ready," but
  no download control existed for the small-answer case. Added `unistax/unistax@702f476f2`.
- **Segment 6 (auction approval)**: `DisposalService.ApproveRegistration` was real and reachable but
  had no screen calling it, so a bidder's registration could never actually be approved; the panel
  added to close that gap then hit its own bug (`row_version` never requested by name, so every
  approve attempt refused as "not loaded yet" forever). Both fixed `unistax/unistax@bd2ee7bc9`.

A sixth, related hardening (not itself visible in this video, since no locked account is ever shown
on screen): the seal-custodian picker's own fix above had dropped its `status = active` filter to
close the refusal, which left a locked or suspended account still searchable. Closed properly at
the shared read predicate rather than in the picker, `unistax/unistax@f25c7637e`.

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

## `officers/` — the full, uncapped workflow recordings

The module-by-module recordings above tour a curated selection per RFP module. `officers/` holds a
different, more exhaustive set: one video per named officer in `unistax/unistax`'s own
`demo/sjibl/officers/*.md` roster, each one walking every real UI workflow that officer's ERP role
can reach — not a curated top 20, every route their role's own permissions actually unlock,
mechanically derived by cross-referencing `design-contract/ui/role-catalogue.json` against
`web/src/router/screens.ts` and `entities.ts`, the identical logic the app's own router guard runs.
801 real workflow routes total across the 15 officers, every one of them actually visited in a real,
logged-in Casdoor session against this product's own dev instance (not a static list) before being
recorded.

Two file generations exist side by side in this folder for the same officer, kept distinct on
purpose:

| File | Contents |
|---|---|
| `NN-name.webm` (original, e.g. `01-md-farhan-kabir.webm`) | The officer's original curated top-20 walkthrough, recorded earlier |
| `NN-name-full.webm` (e.g. `01-md-farhan-kabir-full.webm`) | Every real workflow that officer's role can reach, uncapped |

The corresponding `demo/sjibl/officers/*.md` file in `unistax/unistax` is the authoritative,
readable list behind each `-full` video — route, plain-English workflow name and the exact
permission that gates it, grouped by pack/division. A handful of routes needing a specific record id
(a particular contract, a particular fiscal period) rather than a browsable index were left unvisited
and are named plainly in both the video's own source list and the markdown file, not guessed at.

**All 15 `-full` videos were re-recorded once**, after the first pass found every one of them opened
with a brief "404 page not found" flash right after sign-in. Root cause: the local `vst-svc`'s
`UNISTAX_CASDOOR_CALLBACK_URL` and the `shatadal-app` Casdoor Application's own registered
`redirect_uris` both pointed the OAuth round trip at the bare backend origin (`localhost:9191`,
which serves only `/erp.*` RPCs and `/auth/casdoor/*`, nothing at `/`) instead of the vite dev
server (`localhost:9192`) that actually serves the SPA and already proxied `/auth/casdoor/*`
through to 9191. Fixed by repointing both at 9192, so the whole flow, including the final
post-login redirect, lands on a real page. The re-recording used the identical, mechanically
derived route lists, so coverage and content are unchanged; only the login moment is different.

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
