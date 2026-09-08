# House Standard — the portable baseline (internal source rendering)

**Status**: Proposed — Draft v0.1.1, revised 2026-09-08 after the independent second-opinion review (adjudication: `products/desk/reviews/2026-09-08-adjudication.md`, findings STD-01..03, JSON-01..21, X-02, X-04). v0.1 authored 2026-09-07 on the founder's "go" to option (b). Enters `governance/` under the change regime (cold-read D-120/D-121 at landing); adopted only by recorded ruling.
**Governs**: what may travel — the practices applied to every GitHub organization and repository Integrator Consulting operates, whether Company of One, Integrator Consulting, or a client's own.
**Home (proposed)**: `governance/house-standard/house-standard.md` (this file — internal); `published/HOUSE-STANDARD.md`; `house-standard.json`; `preflight-spec.md`; `applied.md`.
**Review by**: 2026-12-07 (≤ 100 days), then every ≤ 100 days.

## 0. What this is, and is not

| tier | what | where it lives | value when seen |
|---|---|---|---|
| the canon | the combination, ordering and interlocks of Company of One's rules, gates, lanes and record series (D-109) | home only; never ships (D-110); trade secret (D-113) | **falls** |
| the product | what a client buys and runs | the client's instance | the client's |
| **the house standard** | ordinary engineering and organizational hygiene | **every organization and repository we operate** | **rises** |

**Admission test (working; not the D-114 crossing test, which stays deferred):** would we be glad if a client's engineer read it? If yes, it travels; if it would teach them how Company of One works *as a system*, it stays home. Corollary from D-109 — the standard is a **menu of practices, never a sequence with our names on it**.

**Two renderings from one source.** This file carries the *descends from* column. The published edition carries the same items in plain industry language with the ties stripped and **must pass the canon screen before every publication** (a D-113 measure-5 screened release).

**Evidentiary honesty (v0.1.1, from the review):** an automated check proves a setting or file exists as declared, not that the control operates (the B-210 shape: *presence is not compliance*). The report therefore says "N automated checks passed; M items need human verification" and never "compliant." Items are labeled *auto*, *content* or *manual*.

**Ruled 2026-09-07 (founder, Q4):** the agent working conventions are **hygiene**, admitted at REPO-10 in generic language.

## 1. Tier 1 — Organization

| id | requirement | descends from | evaluation |
|---|---|---|---|
| ORG-01 | Two-factor authentication required for every member | new element — industry baseline | auto (`orgs/{org}.two_factor_requirement_enabled`) |
| ORG-02 | Base permission none/read; creation owners-only or private-only; private forking off | new element — least privilege | auto |
| ORG-03 | New repositories private by default; default branch `main` (the corpus's `master` grandfathered) | new element | auto / manual |
| ORG-04 | Ruleset on **every** repository's default branch: PR required; force-push blocked; deletion blocked. **Coverage verified per repository** (`repos/{o}/{r}/rules/branches/{default}`), not by reading the ruleset alone [JSON-04] | GR-007 — *both corpus lanes are gated*; B-18 — per-layer history. **Exception on record:** the corpus repository keeps its founder-direct lane gated by the pre-push hook (GR-007 chose hook over branch protection); the Company of One ruleset excludes `company-of-one/company-of-one` by name, recorded in `applied.md` with approval fields | auto (per repository) |
| ORG-05 | Dependabot alerts and security updates on for **every existing** repository and new ones; **checked per repository** (`repos/{o}/{r}/vulnerability-alerts`) [JSON-05] | the experience recorded at H-462 | auto (per repository) |
| ORG-06 | Secret scanning and push protection where the plan allows; where not, REPO-07 in CI **and** a recorded exception; detection ≠ prevention stated [STD-02, JSON-06] | new element; plan-dependent — `integrator-consulting` declined the paid add-on 2026-09-07 (exception ORG-06, expiry 2026-12-06, approved by the founder in `applied.md`) | auto / manual |
| ORG-07 | `.github` repository with `SECURITY.md` (a working contact), `CONTRIBUTING.md`, the PR template, `HOUSE-STANDARD.md`, `house-standard.json` — **with content, not merely present** [JSON-07] | the AGENTS.md practice generalized | content + manual — **verify** that a private `.github` propagates to private repositories (H-e) |
| ORG-08 | Access review at least every **100 days**; recorded with date, who, and what changed [STD-03, X-04, JSON-08] | least privilege — also D-113 measure 2 for the canon, applied generically here | manual (recorded in `applied.md`) |

## 2. Tier 2 — Repository

| id | requirement | descends from | evaluation |
|---|---|---|---|
| REPO-01 | Private; default branch `main`; auto-delete head branches | B-18 — `--delete-branch` | auto |
| REPO-02 | Default branch requires a PR and **the repository's own named checks** (from its stamp); force-push/deletion blocked. **PR ≠ review:** with one maintainer the human merge is the control, stated as such; with a second writer, one approving review required [STD-01, JSON-10] | H-405 — four required blocking checks; B-15; B-07 — the founder walkthrough is the load-bearing gate, which is why "the human merge is the control" is true here rather than a euphemism | auto + manual |
| REPO-03 | Merge commits only | B-18 — *never squash* | auto |
| REPO-04 | CI runs the gates **as separate ordered steps**, `continue-on-error` absent, on PR and push to default; check names match REPO-02; CI is the authority [JSON-12] | B-15; *CI is the authoritative check* | auto (workflow YAML read) |
| REPO-05 | Conventional lowercase subjects; the lint config **defines `subject-case` and `type-enum` and is invoked in CI** [JSON-13] | B-40 | content |
| REPO-06 | Required files **with content**: README; ownership/license; SECURITY (contact); CODEOWNERS; PR template (own/inherited); `.gitignore` covering env/secrets; `.env.example` with **no values**; `AGENTS.md`; `.house-standard.json` [JSON-14] | env contract (Desk PRD §7.3); the AGENTS.md convention | content |
| REPO-07 | **No secret lands unnoticed on the main line:** scanner as a **required check**, fails on findings; env/key material never tracked (`.env*` except `.env.example`, keys, pem, p12); **one-time history scan at adoption, recorded**; rotation on exposure, recorded; detection-not-prevention stated where push protection is off [STD-02, JSON-15] | new element | auto + content + manual |
| REPO-08 | Lockfile committed (`requirements.txt` only if every line pinned); `dependabot.yml` with a declared schedule [JSON-16] | H-462 | content |
| REPO-09 | Database changes only via committed migrations — **at least one migration file**; applicability includes `*.sql` migrations, `schema.*`, `supabase/`; unrecognized layouts attest manually [JSON-17] | GR-008; B-120 / B-126 — *migrations from committed bytes only* | content + manual |
| REPO-10 | `AGENTS.md` carries the working conventions in generic language; presence and key phrases checked by script, **content confirmed by a person** [JSON-18] | B-178; B-06; B-18; *CC stops at PR-open*; B-83; *not run ≠ passing* | content + manual |
| REPO-11 | Preflight is a **required check**, runs monthly, **produces a report artifact**; stamp carries version, **digest**, `required_checks[]`; required missing/drifted/**cannot-evaluate** fails [JSON-19, PRE-01] | new element — this exercise; B-210 — a check that cannot fail on the thing it checks is not a check | auto |
| REPO-12 | Deployment verified by address **and** source SHA, **recorded** in release notes; applicability widened [JSON-20] | B-132 | manual (recorded) |

## 3. Exceptions [STD-03]

Item id, reason, owner, **`approved_by`, `approved_on`**, expiry ≤ 90 days **after `approved_on`**, restoration path. Approval = a PR approval by the `CODEOWNERS` maintainer, or the owner's dated note in `applied.md` for organization-tier items. Recorded in `.house-standard.json` → `exceptions[]` and in `applied.md`. Missing approval fields → not an exception; expired → drift. Never inherited.

## 4. Versioning and review

`MAJOR.MINOR.PATCH`. **PATCH** wording only; **MINOR** a new or tightened check (older stamps report drift by version); **MAJOR** a check removed or changed in meaning. Stamps record version **and the published JSON's SHA-256 digest**. Review at least every 100 days (first by 2026-12-07); a platform announcement affecting an item may trigger an early review.

## 5. Applied log

`applied.md`: one row per application — organization or repository, version, date, who, exceptions **with approval fields**, manual verifications with `verified_on`/`next_due`, notes. Row 1: the founder's Tier-1 application to `integrator-consulting` (2026-09-07; approved_by founder; exceptions ORG-06 and ORG-07-partial, expiry 2026-12-06).

## 6. What enforces what — stated plainly (B-181)

- **Controls (platform-enforced once set):** ORG-01, ORG-02, ORG-04, REPO-02, REPO-03.
- **Tripwires (detect and report; the review they open is the control):** the preflight, the secret scan, the commit lint.
- **Human only:** ORG-08, REPO-09 for unrecognized layouts, REPO-10's content, REPO-12, every exception, every recorded manual verification. **The preflight cannot see whether humans did their part; it can only see when they last recorded that they did.**

## 7. Open decisions

| decision | options / constraint | owner | needed by |
|---|---|---|---|
| Private `.github` propagation to private repositories | verify; else carry files in the template only | founder / landing lane | before ORG-07 marked applied |
| Secret Protection purchase | at exception expiry 2026-12-06: buy ($19/active committer/mo as quoted 2026-09-07) or renew the exception with the history-scan evidence | founder | 2026-12-06 |
| Signed commits | require or not | founder | next review |
| Company of One organization adoption | Tier 1 with the ORG-04 exception recorded | founder | after `integrator-consulting` proven (done 2026-09-07) |
| Lighter template for document-only repositories | README, SECURITY, CODEOWNERS, stamp, preflight — no CI gates; or inherit ORG only | architect proposes | before the next client repo |
| Approving-review threshold | one approving review once a second writer exists (drafted) | founder | when a second member is added |

## 8. Revision history

- **0.1.1 (2026-09-08)** — independent-review revisions; see the published edition's revision history for the plain-language list; adjudication ids in brackets above.
- **0.1.0 (2026-09-07)** — first draft.

---

*v0.1.1 — Proposed 2026-09-08. This rendering never leaves the corpus. The published edition is the only form that travels, and only after the canon screen passes.*
