---
name: qualify-contractor
description: Launch when you're ready to open a new city for the AI-visibility lead-gen business. Screens candidate cities against the qualifications, shows only the ones that pass, picks the best, confirms the city with you, then finds, ranks, vets, and fact-checks the contractors in that city most likely to say yes to an AI-visibility fix. Output is one call-tracker Google Sheet plus a personalized call-script for every contractor on it. Nothing is built, nothing is contacted, no full audit report is written until a contractor says yes. Trigger on /qualify-contractor, "open a new city", "find me a city and contractors", "find contractors to call", "run the funnel".
---

# /qualify-contractor

You are helping two beginners find contractors to cold-call and offer help with AI
visibility — what AI assistants say about them when a homeowner asks who to hire, and
fixing what's wrong or missing. The plan: they call until one gives a verbal yes, and
only then is a full audit report or anything else built. This skill's whole job is to
hand them the shortest, best-targeted call list possible — the shops most likely to say
yes, ranked, each one records-vetted and fact-checked so the caller walks in knowing the
shop's license, size, reputation, and the single sharpest thing an AI gets wrong or
misses about them.

Nothing here contacts a contractor. Nothing here builds a website or writes a polished
report for a shop that hasn't shown interest yet. Every run ends with two things: a
call-tracker Google Sheet, and a short personalized call-script file for every
contractor on it. A human makes every call; the full one-page audit report gets written
later, per contractor, once they've said yes to seeing it.

## How this filters — two layers

The skill is a **hybrid**: gate the few things we're certain about, score the things
we're still guessing about.

- **The gates (Step 4)** — hard auto-drop. Only things that are cheap to check and will
  never give a useful yes: out of area, franchise, PE/roll-up, regional/multi-location,
  fake lead-gen site, dead/duplicate, no reachable phone or owner. Nothing about size
  below that, age, ad spend, or website quality is a gate.
- **The score (Step 5)** — the 7 signals. Ranked, never rejected here. A weak score just
  puts a shop lower on the list.
- **The vet (Step 6)** — records-side confirmation. This is where the "more than ~8
  trucks" size gate finally bites, once real employee and truck counts are in, and where
  a genuinely bad reputation (≈2.5 stars, owner-conduct complaints) drops a shop. Nothing
  that needs a phone call is decided here.

The gates stay small **on purpose** — a wrong gate silently throws away shops that would
have said yes. Signals become gates only after real call results prove they predict a
yes (see "The feedback loop" at the end).

## What this skill does NOT do

- **No keyword or "can we rank a site" check.** That's deferred. Do not add market /
  rankability / "Gate 0" steps here.
- **No money math.** No fee, cost-per-lead, ad-budget, or ROI estimates anywhere. This
  skill judges *fit* — is a shop the right size, does it want to grow, can you reach the
  owner. The economics are worked out separately by the user.
- **No default trade.** The trade falls out of the city — run plumbing, HVAC, and
  electrical, and let the contractor counts decide which is best there.

## Before you start

Ask only what's missing:

- **Cities** — does the user have specific cities in mind, or should the skill propose
  candidates? Default: propose.
- **Trades** — default: plumbing, HVAC, electrical. Change only if the user says so.

Confirm the plan in three lines and proceed.

## File conventions

Everything is written to `first-product/reports/`. In filenames, `<city>` and `<trade>`
are lowercase-with-hyphens (`sioux-falls`, `hvac`). `<date>` is `YYYY-MM-DD`, `<time>`
is 24-hour `HHMM`, both from the machine clock. If a file with the exact name exists,
add a minute to `<time>` — never overwrite.

Per-run files: `<date>-<time>-city-screen.md`, `<date>-<time>-candidates-<city>.md`,
`<date>-<time>-funnel-<city>-<trade>.md`, `<date>-<time>-vetting-<city>.md`, and one
call-script per keeper at `first-product/call-scripts/<date>-<city>-<trade>-<business>-call-script.md`.
No audit report and no Drive doc get built in this skill — those wait for a contractor
to say yes (see Step 7). In Drive: only the `<City> Contractor Call Tracker` sheet.

## Step 0 — See what's already been done

Each run starts with no memory of the last one. Before anything else:

1. List `*-city-screen.md` files — which cities have already been screened, and what
   passed.
2. List `*-candidates-<city>.md`, `*-funnel-<city>-<trade>.md`, `*-vetting-<city>.md`,
   and `first-product/call-scripts/*-call-script.md` files — which cities and trades have
   been worked, how far each got (candidates → funnel → vetting → call scripts →
   tracker), and every business already looked at.

Tell the user in one line what you found ("screened 12 cities on 2026-09-09; Sioux Falls
and Clarksville worked; Sioux Falls plumbing is vetted with call scripts written, tracker
built").
Skip anything already done unless the user asks to redo it.

## Step 1 — Build the candidate city list

If the user named cities, use those. Otherwise propose **8–10 US cities** with a city
population roughly 80,000–250,000 that are plausibly growing — spread across regions
(Texas secondary cities, the Mountain West, the Southeast, the Carolinas, the Upper
Midwest). This list is only a starting point; Step 2 verifies every number and a
candidate that can't be verified does not pass.

Skip cities already screened in Step 0.

## Step 2 — Screen each candidate against the qualifications (facts only, each sourced)

For each candidate, pull and **cite the source** for:

- City population and metro population (Census / censusreporter.org — note the ACS year)
- Growth (Census population estimates 2020 → latest, or a stated annual rate — cite it)
- Median household income (Census ACS — cite)
- Owner-occupancy rate (Census ACS — cite)

If a number can't be found, write `unknown — <where it would come from>`. Never guess a
number. A candidate missing a load-bearing number (size or growth) does not pass.

Apply the qualifications (from memory: `lead-gen-target-city`):

- **Size** — city 80k–250k, metro 150k–400k. Reject outside. Reject 700k+ city or the
  core of a huge metro unless it's a geo-fenceable suburb. Reject under ~40k unless it
  covers 4–5 towns as one market.
- **Growth** — reject shrinking or dead-flat-for-years. Prefer above ~0.5%/yr. Bonus if
  2%+/yr or ~10%+ over a decade. This is a preference scale, not a hard 2% gate.
- **Income** — floor ~$65k median household. Preferred $78k+. Below $65k → reject.
- **Owner-occupancy** — ~55%+ is fine. Below → soft flag, not a reject. Under 50% with a
  huge renter base → caution.
- **Housing age** — older stock is fine (water heaters turn over in 8–15 years). Reject
  only if almost everything is brand-new construction with no replacement demand yet.

Write the **full screen** — every candidate, pass and fail, with the numbers and their
sources — to `first-product/reports/<date>-<time>-city-screen.md`.

## Step 3 — Show only the cities that pass, pick the best, confirm

In the chat, show **only the passing cities** as a short table: city, pop / metro,
growth, income. Do not show the rejects — they're in the file.

Pick the **single best** passing city. Reason in two lines — usually: biggest
contractor pool and call volume, steady growth, workable income, housing that supports
replacement demand.

Ask one question: **"Is `<city>` okay?"**

Stop and wait. This is the only check-in in the whole skill. On "yes", run Steps 4–8
straight through without pausing, then hand over at Step 9.

## Step 4 — Find the contractor candidates in the confirmed city

For each trade (plumbing, HVAC, electrical):

**Sweep for names** — run `<trade> <city>`, `<trade> repair <city>`,
`<trade> replacement <city>`, `best <trade> <city>`; the Google map pack and its "more
businesses" list; Yelp, Angi, BBB; and the local Chamber of Commerce member directory.
Collect every real distinct business name (aim for 15–20 per trade).

**Quick-tag each** (one search per name) and run it through **the gates**. A shop that
hits any gate is dropped — write it down with the reason, don't work it further.

- **Phone** — real, quoted, never guessed
- **The gates (drop on any one):**
  - `out of area` — based outside the target city and not a geo-fenceable suburb
  - `franchise` — Mr. Rooter, Roto-Rooter, One Hour, Benjamin Franklin, ARS, etc.
  - `regional / multi-location` — more than ~8 trucks, several cities
  - `PE / roll-up owned` — a parent company named on the About page, the same phone
    number on three different brand sites, or the license holder's name ≠ the
    About-page name
  - `fake-local / lead-gen site` — no findable owner, service pages for far-away cities
    on one domain, a name that's just "<City> Plumbing", stock photos only (note it —
    this is the competition)
  - `dead / duplicate` — no activity in 2+ years, or already listed under another name
  - `no way in` — no working phone number and no named owner anywhere

Everything that clears all seven gates is a **keeper** and goes to Step 5. Do NOT gate
here on size-below-8-trucks, age, ad spend, or website quality — those are scored.

Write every candidate — keeper and dropped, with a one-line reason — to
`first-product/reports/<date>-<time>-candidates-<city>.md`.

## Step 5 — Score the keeper shops on the 7 signals (observable facts only)

This is the **score, not a gate** — no shop is dropped here. A low score just puts a
shop lower on the ranked list.

For each keeper, record each signal as **yes / no / unknown**, with the source. If a
signal can't be checked, it's `unknown` — never a guessed yes or no. Never invent a
review count or a truck count.

1. **Already invests in their own web presence** — a real website beyond directory
   listings · signs they've paid someone for it (a professional build, not a free
   template) · an active blog or "recent projects" page · a claimed, active Angi /
   Thumbtack / Yelp-ads profile · a Meta pixel or Google Ads tag in the page source.
   *(Strongest predictor of a yes — counts double.)* This is deliberately **not** "runs
   ads" alone: paying Angi or Google for routed calls proves they'll pay for *leads*, but
   not that they'll pay to fix their *own* site — the two are different purchases under
   an AI-visibility pitch. A shop that's already invested in its own site is the one most
   likely to see the value of improving it further.
2. **Visibly growing** — a current hiring post (Indeed or their site) · "accepting new
   customers" · a recently added truck or second location. Reasoning under AEO: growth
   means budget *and* a live reason to keep investing in how they're found — not just
   spare capacity to take more calls. The inverse is a real negative signal, not just an
   absence of a positive one — see the growth-appetite note under signal 6.
3. **Established reputation** — a real number of Google reviews (not zero or a handful)
   and a decent rating. This is a shop with something worth protecting and buyers already
   trusting them — the opposite of the old "weak online presence" version of this signal.
   A shop with no reviews and no track record has nothing an AI-visibility fix would
   amplify.
4. **Owner-operated** — owner named on the site · owner in the reviews or photos ·
   speaks in the first person on the About page
5. **Right size** — looks like 2–8 trucks (count them in photos, read the team page,
   judge from the service-area size). Solo scores low here — it is *not* dropped. Under
   AEO the size band isn't about call-volume capacity — it's a proxy for **not already
   having in-house marketing or a paid agency handling their web content**. A 2–8 truck
   shop with a bare-bones site is a strong prospect for exactly that reason; a shop this
   size that already has a polished, professionally-maintained site is a weaker one, even
   though it clears the same truck count.
6. **In business 2+ years** — from the license issue date · BBB "date started" ·
   "serving since" on the site · Google profile badge · Facebook page age. **This is a
   floor, not a ranking bonus for going higher.** 2–15 years reads as established-but-
   still-building — the sweet spot. Decades in business (30–40+ years, especially
   multi-generation family shops) is still real credibility, but that same long tenure
   correlates with an owner who may be coasting toward retirement and uninterested in
   changing how the business gets customers. Don't let long tenure alone push a shop to
   the top of the rank — note it, and see "Watch for a business too settled to want
   change" below. A newer, growing shop can be a better prospect than the oldest,
   most-established one on the list.
7. **Decent-ticket trade** — the trade does jobs worth $1,500+ (true for
   plumbing / HVAC / electrical service and install). Note if this shop is mostly
   low-ticket maintenance. Under AEO the reasoning is ROI-per-fix, not rent-justification:
   the more a job is worth, the more a single AI-assistant-sourced customer is worth, so
   the more upside there is in being findable and cited correctly.

### Watch for a business too settled to want change

A shop can score well on every signal above and still be a dead end if the owner isn't
interested in doing anything differently — and that's genuinely not something records
can confirm; it only gets resolved on the call (Call 2's "are you trying to grow right
now, or is it about the size you want to be?" exists for exactly this). Two things worth
noting when you see them, even though neither is a gate or a guessed "no":

- **Observable soft signals worth a note** — phrasing like "word-of-mouth only," "not
  currently accepting new customers," or "we don't advertise, we don't need to" on a
  site or listing. Record it in the candidate or funnel notes; it's a real signal of
  resistance to change, not just a data gap.
- **Long tenure is a double-edged signal** — see signal 6. The credibility of "in
  business 40 years" and the risk of "owner may be done trying new things" come from the
  same fact. Don't resolve that tension by guessing; put it on the human's Step 9 list.

Then **rank** the keepers by how many of the 7 signals they hit, counting signal 1
(already invests in their own web presence) double. Every keeper stays on the list —
ranking only sets the order.
The only shops not on the list are the ones the gates dropped in Step 4.

Write the ranked list to
`first-product/reports/<date>-<time>-funnel-<city>-<trade>.md` — one line per shop:
name · phone · signals hit · rank. No Google Sheet yet — the one tracker is built at
Step 8, after vetting, the fact-check, and the call scripts.

## Step 6 — Vet each keeper against the public record

For every keeper from Step 5, pull what can be checked without a phone call. No guessing —
each item is a sourced fact or `unknown`.

- **License** — look it up. Plumbers: Texas State Board of Plumbing Examiners (TSBPE)
  license verification. HVAC and electricians: TDLR license search (TACL / TECL
  numbers). Record the number(s), whether the license is **active**, and whether the
  holder's name matches the business. A lapsed or mismatched license is a flag. Where a
  verification tool can't be reached, note the license number from BBB / BuildZoom and
  leave "active?" as `unknown — confirm at <site>`.
- **Size** — employee count from any listing (BBB, ZoomInfo, LinkedIn, D&B), truck count
  from photos or the team page. This is where the Step 4 "more than ~8 trucks" gate is
  finally applied with real numbers: a shop that turns out to be roughly 15+ trucks or
  20+ employees is **dropped now** — out of the "right size" band and big enough not to
  need leads.
- **Online presence** — the real Google review count and star rating. This is the real
  number behind signal 3 (established reputation) — re-score it with the confirmed count
  and rating. A rating below ~4.2 is a flag, not a reputation win.
- **Hiring** — a current job post (Indeed, their site) confirms "visibly growing"
  (signal 2).
- **Reputation red flags** — a bad BBB rating, an unresolved complaint, a
  consumer-complaint listing, owner-conduct complaints in the reviews. Record each as a
  separate, clearly labeled fact, never folded into the score. A shop with a genuinely
  bad reputation (≈2.5 stars, owner-behavior complaints) is **dropped** — sending leads
  there would burn the caller's credibility.

Re-score the 7 signals with what the vet turned up, re-rank, and write the changes (who
moved, who was dropped and why) to
`first-product/reports/<date>-<time>-vetting-<city>.md`.

**What the skill still cannot check — put these on the human's list, don't guess:**
- **Live ad spend, a supplementary check for signal 1** — needs a live Google results
  page (an "Ad" or green "Google Guaranteed" label) and the Facebook Ad Library, from a
  real browser. Keep it `?` unless there's a hard footprint (call-tracking numbers, a
  named marketing agency, an active paid Angi / Thumbtack profile). This is one more
  data point for signal 1, not the whole signal — the core check (their own site,
  professionally built vs. a free template) is doable from records.
- **Capacity** — how far out they're booked, whether a person answers the phone.
- **Average ticket and job mix** — feeds the user's separate money math.
- **Whether the owner actually wants to grow or change how they get customers** —
  tenure and reputation are visible in records; appetite for change is not. A
  long-established shop can be a great prospect (credible, never been pitched this
  before) or a dead end (owner's coasting, has said no to marketing before) — the score
  alone can't tell those apart. Flag it, don't guess it; it's the first thing Call 2
  screens for.

## Step 7 — Fact-check every surviving keeper

Run `/contractor-audit` in **fact-check only** mode on each shop still standing after
Step 6 — the seven buyer-question searches (run once per trade, reused across every shop
in that trade) and the nine-fact check against the shop's own website. No file, no
report, no Drive doc yet — that's the full-report mode, and it waits until a contractor
says yes (see Step 9's hand-over and the how-to-use block in Step 8).

- If a shop's website blocks automated reading (403), has no valid HTTPS certificate, or
  doesn't exist, that **is** the finding — say so plainly and build the rest from BBB
  and other listings.
- Phone numbers, hours, or service claims that differ between the site and the
  directories are sharp findings — record the split, don't pick one.
- Count how many of the nine facts are missing (`Gap`, e.g. `6/9`); if the site couldn't
  be read at all, `n/a`. Note the single sharpest finding in one line — this is the hook
  for the call script.

Add the Gap count and sharpest finding to
`first-product/reports/<date>-<time>-vetting-<city>.md`.

**Worth-calling verdict**, one short phrase per shop — needs both the opportunity and the
credibility to protect it:
- **Skip** — fewer than 3 of the 9 facts missing, OR reputation from Step 6 is too thin
  (near-zero reviews, no track record) to have anything worth fixing.
- **Caution** — site unreadable, or a real complaint / rating issue (still callable, say
  why it's different).
- **Good** — 3+ facts missing AND an established reputation (signal 3 / Step 6's review
  count and rating) — a real business that's invisible to AI despite deserving better.

## Step 7.5 — Write a personalized call-script for every keeper

For each shop with a Skip/Caution/Good verdict from Step 7, write a short call-script
file, the same shape as an existing one already written by hand — see
[first-product/call-scripts/2026-09-11-fenix-electric-call-script.md](../../first-product/call-scripts/2026-09-11-fenix-electric-call-script.md)
for the exact pattern to follow. Pull the personalized parts from what this skill already
has in hand — quick facts (owner, phone, trade, city, tracker rank), the hook (the single
sharpest finding from Step 7, put in plain, specific language: what an AI got wrong or
missed about them, and who it named instead) — and for the open, the ask, pushback
answers, voicemail, and everything generic, point back to the shared
[call-practice-pack](../../first-product/call-scripts/2026-09-09-call-practice-pack.md)
instead of repeating it.

Save each to
`first-product/call-scripts/<date>-<city>-<trade>-<business>-call-script.md`.

## Step 8 — Build the one call-tracker Sheet

One Google Sheet named `<City> Contractor Call Tracker`, in the Drive folder the user
names (ask if not already told — don't guess a location). At the top: a short
how-to-use block that says plainly — **the call script is the only thing to read before
dialing; the full one-page audit report gets written after a contractor says yes to
seeing it, not before** — the status words (`To call / No answer / Left msg / Interested
/ Callback set / Not now / No`), the "before every call" checklist (live ad check +
capacity call), and a cell labeled `Vetting process doc:` for the user to paste a link.
Then the surviving keepers **in rank order**, columns:

**# · Business · Trade · Phone · Ask for · Call script · Gap · Flag / note · Status ·
Date called · Result · Follow-up**

- **Call script** is a `=HYPERLINK("<file path or doc url>","call script")` to the Step
  7.5 file.
- **Gap** is the missing-of-9 count from Step 7.
- **Flag / note** is the single sharpest fact-check finding, or the caution reason.
- Leave Status, Date called, Result, Follow-up blank — the brothers fill them as they dial.

**Connector limits — tell the user every run:**
- This connector can't edit an existing Sheet's cells or delete files, and can only
  rename or replace files it created in the current session. So the tracker is built
  fresh each run; updating it later means a new copy, and the user has to trash the old
  files themselves. Name which ones.

## Step 9 — Hand over

In the chat: a short table — business, gap size, skip / caution / good, one-line why —
and the tracker link. Then the human's to-do list from Step 6 (live ad check, capacity
call, license-currency confirmation for any `unknown` row, truck counts for the
mid-size shops, and growth appetite for any shop whose main strength is long tenure —
see "Watch for a business too settled to want change" in Step 5). Remind them: the call
script is what they read before dialing; the full report only gets written, per
contractor, after a yes.

Per the project's workflow, recommend that a fresh session spot-check a sample of the
fact-checks and call scripts against the live sites before calling — the checker is
never the writer.

## The feedback loop — how the score becomes gates

This is the point of the hybrid. The 7 signals are current best guesses, not proven
predictors. Real calls prove them.

When the user has logged **~20–30 real call results** in the Call Tracker Sheet (the
Status and Result columns), they can ask this skill to run the review:

1. Split the called shops into **got a yes / got interest** vs. **flat no**.
2. For each of the 7 signals, compare how often it was `yes` in the first group vs. the
   second.
3. A signal that is strongly present in the yeses and absent in the nos → **promote it
   to a gate** in Step 4 (move it from "scored" to "auto-drop if missing").
4. A signal that shows up about equally in both → **drop it or lower its weight**.
5. Write the change to this SKILL.md, and note the date and the call count it was based
   on.

Until that review has run at least once, the gates stay as they are — small and
certain. Never promote a signal to a gate on a hunch; only on logged results.

## Rules

1. **No guessing.** Every number and every signal is an observable fact with a source,
   or it's marked `unknown — <how to find it>`. Thresholds are tuned by real call
   outcomes, never picked out of the air.
2. Never invent or guess a phone number, license number, review count, or truck count.
   Quote a stated fact; don't paraphrase it.
3. **No trade is baked in.** Run plumbing, HVAC, and electrical; let the counts decide
   which is best in that city.
4. The skill judges **fit**, never money. No fee, cost-per-lead, ad-budget, or ROI math.
5. Nothing is sent, and nothing is built. The output is a call-tracker Sheet and a
   short call-script per contractor, written to files. No full one-page audit report and
   no Drive doc get built here — those wait for a contractor to say yes, then get made
   one at a time. The skill never sends anything to a contractor. A human makes every
   call and closes every yes.
6. The **city is the only decision the user weighs in on.** Show only passing cities,
   confirm one, then run Steps 4–8 (including 7.5) without pausing and hand over at
   Step 9.
7. **Hybrid: gate what's certain, score what's a guess.** The Step 4 gates stay small —
   only cheap, certain disqualifiers. Everything else is a scored signal. A signal
   becomes a gate only through the feedback loop, on logged call results.
8. **Vetting (Step 6) and the fact-check (Step 7) are records-and-web only — no phone
   calls.** The size gate and a bad-reputation drop happen at Step 6 on real numbers.
   Anything that needs a call — capacity, live ad spend, ticket size — goes on the
   human's list, never guessed.
9. **The full audit report is per-contractor, on demand, after interest — never
   batch-built for a whole list.** It's the expensive step (write-up, Drive doc); Step 7
   only runs the cheap fact-check.
10. Rank-and-rent, keyword rankability, and "Gate 0" are **out of this skill** —
    dropped (2026-09-15), not deferred. Don't add market or keyword checks here.
11. Plain English. The reader is deciding who to dial next.
