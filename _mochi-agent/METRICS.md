# 88eggs-www — declared metrics

**What this product is trying to move.** Public on purpose: a metric nobody can see is a metric
nobody can be held to.

> **Declared 2026-09-05 by `product1`**, on this project's second tick. Nothing is inherited from a
> sibling: the four 88eggs repos are four products in the fleet's model, and the API, the client,
> the worker and the marketing site are not moving the same number. In particular **this file must
> never claim a sale.** Money is taken on `app.88eggs.com`, which is a different repo and a
> different board; this site's output ends at the handoff.
>
> **No value below has been read.** The instrument is Statsig and nobody in the fleet can open its
> console — [`88eggs-frontend#403`](https://github.com/ptk-studio/88eggs-frontend/issues/403) and
> [`mochi-agent-manager#11`](https://github.com/ptk-studio/mochi-agent-manager/issues/11) are the
> open asks. Every `Baseline` and `Current` here says "never read" rather than carrying a plausible
> number.

## What this site is, in one line, because the metrics follow from it

A **five-page static site** at <https://88eggs.com> — `/`, `/hatches/`, `/hatches/song-story/`,
`/privacy/`, `/terms/` — with no build step, no dependencies and no CI. It sells nothing and
generates nothing. Its whole job is to be found and then to **hand the visitor over to
`app.88eggs.com`**, which is where the product actually is.

## What is actually instrumented today, measured rather than assumed

Read off the five pages at `1c92d7e` on 2026-09-05, not assumed from what a marketing site usually
carries:

| | |
|---|---|
| Where it reports | **Statsig web analytics**, the `js-client+web-analytics` bundle loaded from jsDelivr with the client key `client-8PKsS9mhukEWaXJPLqwUbnVpxpDSXx1dZknQieBJBcw` in the query string. Which Statsig project that key belongs to has **not** been established — nobody in the fleet has opened the console |
| What it captures on its own | Page views, clicks on links and buttons, web vitals, JavaScript errors, plus a randomly generated browser identifier for return visits (the wording is `/privacy/` section 4's, and it matches the bundle that is loaded) |
| Which pages carry it | **All five.** Counted in the tree, not assumed: 5 of 5 |
| Which pages log custom events | **Three.** `/` , `/hatches/` and `/hatches/song-story/`. `/privacy/` and `/terms/` carry the snippet and nothing else |
| The three custom events | `egg_crack` on `/` (the egg, or the "psst" hint) · `hatch_open` on `/hatches/` (the one live card) · `cta_click` on `/hatches/song-story/` (the "Hatch Your Song Now" button) |
| Consent | **None, and none is asked for.** The script tag is unconditional on every page. `/privacy/` tells visitors they can opt out by enabling Do Not Track — and **no page contains any check of `navigator.doNotTrack` or `globalPrivacyControl`**, so that promise rests entirely on vendor behaviour nobody has verified. Filed as [`#2`](https://github.com/ptk-studio/88eggs-www/issues/2) |

**So the funnel is fully wired end to end**, which is unusual for this fleet and is why two metrics
can be defined here at all:

```
/  --egg_crack-->  /hatches/  --hatch_open-->  /hatches/song-story/  --cta_click-->  app.88eggs.com
```

**But the funnel is not the only way in, and that shapes metric 1.** `/hatches/song-story/` is the
page written for search — a keyword title, a long description, `Product` structured data, an
embedded video — so a visitor can arrive there straight from a search result having never seen `/`.
Any definition anchored on home arrivals would miss exactly the traffic the page was built to
attract.

---

## 1. Handoffs to the app — primary

| | |
|---|---|
| **Name** | Handoffs |
| **Definition** | A session on `88eggs.com` in which at least one `cta_click` event is logged — the visitor pressed "Hatch Your Song Now" and left for `app.88eggs.com/shop/offers/song-story-maker`. **Once per session, not per click.** Watch it as a rate over **sessions on the host**, not over arrivals at `/`: the deep page is the one written for search, and a session that starts there is a real chance at a handoff |
| **Why it matters** | This site has no product of its own, so a handoff is the only thing it can be said to have produced. It also separates the two failures that look identical from here: arrivals with no handoff mean the site is found and not persuading; handoffs with no arrivals mean the front door is not the route people take |
| **Where to read it** | Statsig console, the project holding the client key above, custom event `cta_click`, as a share of sessions. **The fleet cannot read it today** — no console access ([`88eggs-frontend#403`](https://github.com/ptk-studio/88eggs-frontend/issues/403), [`mochi-agent-manager#11`](https://github.com/ptk-studio/mochi-agent-manager/issues/11)) — and a cloud tick could not read it even with access, because the console needs a browser. **Local-only work** |
| **Baseline** | **None.** Nothing has ever been read. The instrumentation predates this declaration, so the first reading will be a history rather than a baseline — record it as the baseline with its date, and say which days it covers |
| **Target** | **Not set**, deliberately, until it has been read once. A target on a number nobody can read is a wish |
| **Current** | **Never read** (as at 2026-09-05) |

## 2. Crack-through rate — secondary, and the one this site controls

| | |
|---|---|
| **Name** | Crack-through |
| **Definition** | Of the sessions that view `/`, the share that log `egg_crack`. Per session, not per click — the egg and the "psst" hint both fire it and both go to the same place |
| **Why it matters** | The home page is a wordless egg on a pink field: no headline, no explanation, one affordance. Nothing downstream can happen to a visitor who does not click it, and this is the single number that tells "found and did not understand" apart from "not found at all" — which is otherwise invisible, because both look like zero handoffs |
| **Where to read it** | Statsig console, `egg_crack` against autocaptured page views of `/`. Same access block and same browser requirement as metric 1 — **local-only work** |
| **Baseline** | **None.** Never read |
| **Target** | **Not set** until there is a first reading |
| **Current** | **Never read** (as at 2026-09-05) |

**`hatch_open` is a diagnostic, not a third metric.** It sits between the two above and there is
exactly one live card for it to fire on, so it moves with `egg_crack` and adds nothing to watch
weekly. It earns a metric of its own the day there is a second hatch.

**The day boundary is unestablished on both metrics.** Nobody has read what reporting timezone the
Statsig project uses. **Do not write "UTC day" in this file until somebody has** — the same trap
that [`all-printable-app-www#48`](https://github.com/ptk-studio/all-printable-app-www/issues/48)
records for GA4, and the honest form until then is "per reporting day, timezone unread".

**A zero is not evidence of no traffic until the key is confirmed live.** An unregistered or
rotated client key produces an empty console that is indistinguishable from a site nobody visits.
The first reading has to establish which it is looking at before any zero here is believed —
exactly the trap [`88eggs-frontend#403`](https://github.com/ptk-studio/88eggs-frontend/issues/403)
names for the client tier.

---

## Deliberately not metrics

- **Purchases, revenue, or anything after the handoff.** They happen on `app.88eggs.com` and are
  the client's and the API's to measure. This site cannot see them, and a marketing page claiming
  credit for a sale is how four tiers end up reporting one number four times.
- **Page views on `/privacy/` and `/terms/`.** They carry the analytics snippet because every page
  does. They are obligations, not a funnel, and nothing should ever be done to move them.
- **Time on page, and bounce rate.** A front door works by being left quickly *in the right
  direction*. Optimising for a visitor who lingers here is optimising against the product.
- **Web vitals and JavaScript error counts**, which Statsig autocaptures. Worth looking at when
  something is wrong; they are a health check, not a thing this site is trying to move.
- **Search impressions or rank as a headline number.** Search reach is the main **lever** on
  arrivals — and this repo has no `sitemap.xml` and no `robots.txt`, checked 2026-09-05 — but the
  thing being moved is what a visit does, not how many people saw a blue link.

## Rules this file is kept under

**Never write a value nobody read.** If a reading cannot be taken, leave the old value with its old
date and say the reading was skipped — an undated fresh-looking number is worse than a stale dated
one.

**A definition never changes in the same commit as a value.** Changing both at once is the one edit
that makes this file worthless.
