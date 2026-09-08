# Integrator Consulting House Standard

**Version 0.1.1 · September 2026 · next review December 2026**

This is how Integrator Consulting sets up and runs every code organization and every repository it operates — its own and those it builds for clients. Nothing here is proprietary. It is the ordinary discipline of a careful engineering shop, written down so that it is applied the same way every time and can be checked. This document travels with every repository we create.

## How to read it

- **Organization** items are set once at the organization level and inherited by every repository in it.
- **Repository** items are the standard build every repository starts from.
- Each item states how it is verified: **auto** (a script reads a setting or a file), **content** (a script reads inside a file), or **manual** (a person looks and records that they did, with a date).
- **A passing automated check proves that a setting or file exists as declared. It does not prove that the control operates.** Items marked *manual* or *content* need a person. The preflight report says so in its summary line: *"N automated checks passed; M items need human verification."* It never says "compliant."
- Anything we cannot do on a given plan or platform is recorded as an exception with an owner, an approver, a date and an expiry — never silently skipped.

## Organization

| id | requirement | why | verified |
|---|---|---|---|
| ORG-01 | Two-factor authentication is required for every member | a single stolen password should not be enough | auto |
| ORG-02 | Members start with no permission (or read-only); only owners create repositories, or members may create private ones only; forking of private repositories is off | least privilege by default | auto |
| ORG-03 | New repositories are private by default and use `main` as the default branch | one convention, no surprises | auto / manual |
| ORG-04 | A ruleset on **every** repository's default branch: changes land only through a pull request; force-pushes are blocked; the branch cannot be deleted. Coverage is checked **per repository**, not by reading the ruleset alone; any repository the ruleset does not reach is a finding unless it has a recorded exception | the main line of code is always reviewable and never rewritten | auto (per repository) |
| ORG-05 | Dependency alerts and automatic security updates are on for **every existing repository** and for new ones; checked per repository | known vulnerabilities get surfaced and fixed instead of piling up | auto (per repository) |
| ORG-06 | Secret scanning and push protection are on where the plan allows. Where it does not, each repository scans in CI (REPO-07) **and** the organization records the gap as an exception — CI scanning detects; only push protection prevents | a leaked key should be caught before it lands; where it cannot be, it must at least be caught | auto / manual |
| ORG-07 | The organization's `.github` repository carries the defaults every repository inherits unless it has its own: a security contact that is a working address, how to contribute, the pull-request template, and this standard | every repository tells people and agents how work lands here | content + manual |
| ORG-08 | Access review at least every **100 days**: members, outside collaborators, installed apps and tokens; a look at the audit log; the review is recorded with a date, who did it, and what changed | access drifts upward unless someone looks | manual (recorded) |

## Repository

| id | requirement | why | verified |
|---|---|---|---|
| REPO-01 | Private by default; default branch `main`; branches are deleted automatically after merge | tidy history, no stale branches | auto |
| REPO-02 | The default branch requires a pull request and requires **the repository's own named checks** (listed in its stamp) to pass; force-push and deletion are blocked. **A pull request is the review point, not proof of review:** while the repository has one maintainer, the human merge is the control and is stated as such; once a second member has write access, one approving review is required | nothing reaches the main line unreviewed or unchecked — and the standard says plainly which of those two it can guarantee | auto + manual |
| REPO-03 | Merge commits only — squash and rebase merging are disabled | each verified step stays visible in history | auto |
| REPO-04 | CI runs the project's quality gates **as separate, ordered steps** on every pull request and every push to the default branch; check names match REPO-02; no gate is marked to continue on error; **CI is the authority — a local pass proves nothing** | the same gates, every time, on neutral ground | auto (workflow read) |
| REPO-05 | Commit subjects use a conventional type prefix and lowercase, enforced by a lint configuration **that defines those two rules and is invoked in CI** | history reads like a log | content |
| REPO-06 | Required files, each with real content: `README.md`; an ownership or license notice; `SECURITY.md` with a contact (own or inherited); `CODEOWNERS`; a pull-request template (own or inherited); a `.gitignore` covering env and secret files; `.env.example` with variable names and **no values**; `AGENTS.md`; `.house-standard.json` | the repository explains itself and names who signs off | content |
| REPO-07 | **No secret lands unnoticed on the main line:** a secret scanner runs in CI as a **required check** and fails on any finding; env files and key material are ignored and never tracked; **a one-time history scan is run and recorded when the standard is adopted**; any exposed credential is rotated and the rotation recorded. Where push protection is off (ORG-06), this is detection, not prevention, and the standard says so | secrets belong in a secret store, not in history — and a control that only detects should not claim to prevent | auto + content + manual |
| REPO-08 | Dependencies pinned by a committed lockfile (a Python requirements file counts only if every line is pinned); a dependency-update configuration with a **declared schedule** proposes grouped updates | reproducible builds; updates arrive as reviewable pull requests | content |
| REPO-09 | Where there is a database, schema changes land only from committed migration files — **at least one migration file exists** — never from a dashboard, a chat transcript or ad-hoc SQL; repositories with an unrecognized layout attest manually | the database's history is in the repository | content + manual |
| REPO-10 | `AGENTS.md` states the working conventions for anyone — human or AI — changing the code: verify before acting; propose before changing; one change per verified step, committed; stop when the pull request opens — a human merges; do not add scope that was not asked for — park it and say so; report anything that could not be verified as *not verified*, never as passing. **A script can confirm the file is present and mentions these things; only a person can confirm it says them properly** | the same discipline whether the hands on the keyboard are a person's or an agent's | content + manual |
| REPO-11 | A preflight check is present and is a **required check** on the default branch, runs monthly as well, and produces a **report artifact** for every run; the repository carries `.house-standard.json` recording the standard version and **digest** it was built to and the names of its required checks; a required item that is missing, drifted **or cannot be evaluated** fails the check | the standard is checked, not assumed — and when it moves, everything built to the old one says so | auto |
| REPO-12 | Where the repository deploys: a deployment is verified by its address **and** its source commit, and that verification is **recorded** in the release notes | "deployed" means the right code is live | manual (recorded) |

## Exceptions

An exception is written down and **approved**, never implied: the item, the reason, an owner, **who approved it and on what date** (approval is a pull-request approval by the maintainer named in `CODEOWNERS`, or the owner's dated note in the organization's applied log), an expiry **no more than 90 days after the approval date**, and how it will be restored. Exceptions live in the repository's `.house-standard.json` and in the organization's applied log. An exception without approval fields is not an exception; an expired one counts as drift. Exceptions are never inherited — each repository states its own.

## Versions and review

This standard is versioned `MAJOR.MINOR.PATCH`. Every organization and repository records the version **and digest** it was set up to. When the standard changes, everything set up to an older version reports drift on its next preflight, and a person decides what to do. The standard itself is reviewed at least every **100 days** against what the platform and the wider industry now offer; "quarterly" in this document means that.

## What is enforced, and how

Some items are enforced by the platform once set (two-factor, permissions, branch rules, merge methods). Others are tripwires — the preflight, the secret scan, the commit lint — which detect and report; the review they trigger is the control. A few are human-only: the access review, the database-change policy for unrecognized layouts, the content of `AGENTS.md`, deployment verification, and every exception. **The preflight can tell you that the tripwires and settings are present. It cannot tell you that the humans did their part; it can only tell you when they last said they did.**

## Revision history

- **0.1.1 (2026-09-08)** — after an independent review: review distinguished from pull request (REPO-02); secret-scanning claim made honest, history scan and rotation added (REPO-07, ORG-06); exceptions gain approver and approval date (Exceptions); "quarterly" defined as 100 days (ORG-08, Versions); per-repository coverage for rulesets and dependency alerts (ORG-04, ORG-05); content checks for required files (REPO-06, REPO-08, REPO-09); stamp carries digest and required checks; required-but-unevaluable items fail (REPO-11); report language never says "compliant."
- **0.1.0 (2026-09-07)** — first edition.

---

*Integrator Consulting, LLC · House Standard 0.1.1 · Questions and proposed changes go to the maintainer named in `CODEOWNERS`.*
