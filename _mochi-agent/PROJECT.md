# 88eggs-www — what it is

The **static marketing and legal site** for 88eggs — `index.html`, `hatches/`, `privacy/`, `terms/`. Plain HTML and CSS, no build step and no dependencies.

**This is the product's own record**, kept current by the agents that work this repo and theirs to
read before changing anything. Created 2026-09-04 when the repo was onboarded.

> **Read the caveats, not just the facts.** Everything below was either checked on 2026-09-04 or is
> explicitly marked **not established**. Nothing was filled in from what a static marketing site usually does
> — a confident wrong line here is worse than a gap, because the next reader has no way to tell
> which is which.

## Where it runs

**Live at** <https://88eggs.com> — and this repo is what serves it. Confirmed 2026-09-04 against
GitHub's Pages API (`status: built`, `branch: main`) and by fetching the site: `server: GitHub.com`,
no framework markers, and `/hatches/` and `/privacy/` both 200. The `CNAME` file carries the
domain.

**Worth knowing:** `88eggs-frontend`'s README also claims `https://88eggs.com` as its production
URL. It is this repo that serves it today.

## How code reaches production

**A merge to `main` is a production deploy.** GitHub Pages publishes from `main` with no build
step and no workflow, so a merge is live within a minute or two and there is no staging.

**Rollback is a revert commit**, deployed the same way.

## How a change is verified

**There is nothing to run** — no build, no tests, no dependencies, no CI. Verification here is
opening the pages:

- `/` , `/hatches/`, `/privacy/`, `/terms/` all load
- the `CNAME` file still contains `88eggs.com` — deleting or changing it takes the domain down

**A browser is the only real check**, and an unattended run does not have one. Say so rather than
reporting a `curl` 200 as a verified page.

## What bites

- **`CNAME` is load-bearing.** It is one line, it is easy to lose in a merge, and losing it drops the custom domain.
- **No CI and no tests.** Nothing catches a broken link or malformed HTML before it is live — and a merge here is live immediately.

## Deliberately not doing

Nothing recorded yet.
