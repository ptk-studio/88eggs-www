# 88eggs-www — journal

Newest first. One entry per session that changed something.

---

## 2026-09-04 · framework session — onboarded, and this directory created

Added to `webapp-team1` alongside the three sibling 88eggs repos. This directory is the product's
own record; the fleet's own notes on how it works this product stay in the fleet repo.

Everything below was **established by checking**, not assumed:

- **This repo serves `88eggs.com`**, via GitHub Pages from `main` — confirmed against the Pages API and by fetching the site.
- **`88eggs-frontend`'s README claims the same domain**, which is not what serves it today.
- No build, no CI, no tests: a merge is immediately live and nothing checks it first.

**Left for next time:** a declared metric, and a decision about the domain claim in the frontend's README.
