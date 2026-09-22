# SJIBL demo personas

Sixteen people, each a user linked to an employee record, created by `unistax seed demo-personas`
(branch `seed-demo-personas`). Use these ids for every recording and workflow run so the same person
shows up in the inbox, the audit trail and the self-service screens.

## How to act as one under dev-auth

Send three headers: `X-Unistax-Tenant: shatadal`, `X-Unistax-Actor: <user id>` and
`X-Unistax-Roles: <the role's permission strings from design-contract/ui/role-catalogue.json>`.
The acting EMPLOYEE is found from the user record once the build carrying the dev-auth resolver is
deployed (this branch). Until then, also send `X-Unistax-Actor-Employee: <employee id>`, which always
works and wins when present. Sending both is safe. Self-service screens (My Unistax, My profile, My
leave, My attendance, My requests, Order lunch, health cover) then have an employee to show.

Every person starts the year with 20 annual, 10 casual and 14 sick leave days (a few already taken) and
has the last two weeks of attendance (in about 09:00, out about 17:30, Friday and Saturday off).

| No | Person | Role (catalogue name, seed key) | Username | User id (X-Unistax-Actor) | Employee id | Code | Division | Designation | Reports to |
|---|---|---|---|---|---|---|---|---|---|
| 01 | Md. Farhan Kabir | Procurement officer (`PROCOFF`) | `farhan.kabir` | `a5d1b3c7-0000-4000-8000-000000000201` | `a5d1b3c7-0000-4000-8000-000000000301` | EMP-1001 | PCSD | Senior Officer | Rokeya Begum |
| 02 | Rokeya Begum | Head of procurement (`PROCHEAD`) + Line manager (`LINEMGR`) | `rokeya.begum` | `a5d1b3c7-0000-4000-8000-000000000101` | `a5d1b3c7-0000-4000-8000-000000000302` | EMP-1002 | PCSD | Deputy General Manager | Md. Aminul Haque |
| 03 | Nasima Aktar | Finance officer (`FINOFF`) | `nasima.aktar` | `a5d1b3c7-0000-4000-8000-000000000203` | `a5d1b3c7-0000-4000-8000-000000000303` | EMP-1003 | FIN | Senior Officer | Golam Mostafa |
| 04 | Golam Mostafa | Financial controller (`FINCTRL`) | `golam.mostafa` | `a5d1b3c7-0000-4000-8000-000000000204` | `a5d1b3c7-0000-4000-8000-000000000304` | EMP-1004 | FIN | Senior Vice President | Md. Aminul Haque |
| 05 | Nasrin Akter | Internal auditor (`AUDITOR`) | `nasrin.akter` | `a5d1b3c7-0000-4000-8000-000000000103` | `a5d1b3c7-0000-4000-8000-000000000305` | EMP-1005 | ICC | Principal Officer | nobody |
| 06 | Md. Shahidul Islam | Tenant administrator (`SYSADMIN`) | `shahidul.islam` | `a5d1b3c7-0000-4000-8000-000000000102` | `a5d1b3c7-0000-4000-8000-000000000306` | EMP-1006 | ITD | Senior Assistant Vice President | Md. Aminul Haque |
| 07 | Tania Ferdous | Security administrator (`SECADMIN`) | `tania.ferdous` | `a5d1b3c7-0000-4000-8000-000000000207` | `a5d1b3c7-0000-4000-8000-000000000307` | EMP-1007 | ITD | Senior Officer | Md. Shahidul Islam |
| 08 | Sadia Afrin | Employee self service (`EMPLOYEE`) | `sadia.afrin` | `a5d1b3c7-0000-4000-8000-000000000208` | `a5d1b3c7-0000-4000-8000-000000000308` | EMP-1008 | PCSD | Officer | Rokeya Begum |
| 09 | Md. Jasim Uddin | Facilities manager (`FACMGR`) | `jasim.uddin` | `a5d1b3c7-0000-4000-8000-000000000209` | `a5d1b3c7-0000-4000-8000-000000000309` | EMP-1009 | PCSD | Senior Officer | Rokeya Begum |
| 10 | Dr. Farida Yasmin | Dispenser (`DISPENSER`) | `farida.yasmin` | `a5d1b3c7-0000-4000-8000-000000000210` | `a5d1b3c7-0000-4000-8000-000000000310` | EMP-1010 | MED | Medical Officer | Md. Aminul Haque |
| 11 | Mahmuda Khatun | Insurance officer (`INSOFF`) | `mahmuda.khatun` | `a5d1b3c7-0000-4000-8000-000000000211` | `a5d1b3c7-0000-4000-8000-000000000311` | EMP-1011 | PCSD | Senior Officer | Rokeya Begum |
| 12 | Abdul Kader | Fleet manager (`FLEETMGR`) | `abdul.kader` | `a5d1b3c7-0000-4000-8000-000000000212` | `a5d1b3c7-0000-4000-8000-000000000312` | EMP-1012 | PCSD | Principal Officer | Rokeya Begum |
| 13 | Md. Rafiqul Islam | Security and visitor desk (`SECOFF`) | `rafiqul.islam` | `a5d1b3c7-0000-4000-8000-000000000213` | `a5d1b3c7-0000-4000-8000-000000000313` | EMP-1013 | PCSD | Principal Officer | Rokeya Begum |
| 14 | Kamrul Hasan | Store keeper (`STORE`) | `kamrul.hasan` | `a5d1b3c7-0000-4000-8000-000000000214` | `a5d1b3c7-0000-4000-8000-000000000314` | EMP-1014 | PCSD | Senior Officer | Rokeya Begum |
| 15 | Md. Aminul Haque | Read only (`VIEWER`) | `aminul.haque` | `a5d1b3c7-0000-4000-8000-000000000215` | `a5d1b3c7-0000-4000-8000-000000000315` | EMP-1015 | OPS | Deputy Managing Director | nobody |
| 16 | Farhana Yasmin | Procurement officer (`PROCOFF`) | `farhana.yasmin` | `a5d1b3c7-0000-4000-8000-000000000104` | `a5d1b3c7-0000-4000-8000-000000000316` | EMP-1016 | PCSD | Officer | Rokeya Begum |

## What each one is for

- **Md. Farhan Kabir** (`farhan.kabir`): procurement desk officer, the maker.
- **Rokeya Begum** (`rokeya.begum`): DGM Procurement and Common Services, the checker; also line manager of her team.
- **Nasima Aktar** (`nasima.aktar`): finance desk officer, the maker.
- **Golam Mostafa** (`golam.mostafa`): head of finance, the checker.
- **Nasrin Akter** (`nasrin.akter`): internal control and compliance, read-only across the divisions.
- **Md. Shahidul Islam** (`shahidul.islam`): head of IT, the tenant administrator.
- **Tania Ferdous** (`tania.ferdous`): access administration, the security administrator.
- **Sadia Afrin** (`sadia.afrin`): an ordinary staff member, the employee self-service persona.
- **Md. Jasim Uddin** (`jasim.uddin`): estate and facilities, the facilities manager.
- **Dr. Farida Yasmin** (`farida.yasmin`): staff medical centre, the dispenser.
- **Mahmuda Khatun** (`mahmuda.khatun`): insurance and risk, the insurance officer.
- **Abdul Kader** (`abdul.kader`): transport pool, the fleet manager.
- **Md. Rafiqul Islam** (`rafiqul.islam`): physical security and visitor desk.
- **Kamrul Hasan** (`kamrul.hasan`): store and inventory, the store keeper.
- **Md. Aminul Haque** (`aminul.haque`): deputy managing director, a deliberately narrow read-only view.
- **Farhana Yasmin** (`farhana.yasmin`): the procurement seed's desk officer, kept as it was and now an employee too.

## Notes

- 01 to 15 are the officers in `demo/sjibl/officers/*.md`; 16 is the procurement seed's desk officer. Rokeya Begum, Md. Shahidul Islam, Nasrin Akter and Farhana Yasmin keep the user ids the procurement seed gave them, so a tenant that ran it has them linked, not duplicated.
- Rokeya Begum also holds the line manager role because eight people report to her; the roster gives her head of procurement only. Every other person holds exactly their roster role.
- The tenant administrator (Md. Shahidul Islam) has too many permissions for one header: collapse them to `<pack>.*` bundles, which the permission check accepts.
- These are users and employees only. No Casdoor account is created: under `--dev-auth` there is no sign-in, and under `--casdoor-auth` the tenant administrator creates each login.

## Apply it to a tenant

```
unistax seed demo-personas --dsn <dsn> --tenant shatadal --actor <the tenant administrator's user id>
```

It is safe to run twice. The tenant must be installed with a profile so the roles exist.
