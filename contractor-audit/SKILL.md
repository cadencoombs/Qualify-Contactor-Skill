---
name: contractor-audit
description: Run the questions a homeowner would ask an AI assistant about hiring a local contractor, check the contractor's own website for the nine facts an AI needs, and build a two-page "AI Visibility Report" PDF scorecard the contractor can read on a phone. Trigger on /contractor-audit, "audit <business>", "what does AI say about <business>", or "run the audit on".
---

# /contractor-audit

You are checking what AI says about a local contractor. It must be true, specific, and short.

## Two modes — pick one before you start

- **fact-check only** (default when `qualify-contractor` calls this skill on a whole list
  of keepers) — run Steps 1–3 only. No file, no report, no Drive upload. Return just: the
  Gap count (how many of the nine facts are missing), and the single sharpest finding in
  one line. This is the cheap mode — it's what qualifies and ranks a shop, not what gets
  shown to them.
- **full report** (default when a human runs `/contractor-audit` directly on one named
  business) — run Steps 1–4. Builds the two-page AI Visibility Report PDF. This is a
  diagnosis, not a fix — it shows the contractor they have a real problem and that
  whoever handed them this report can solve it, without teaching them how. Build it for
  a shop that's already shown real interest (picked up, sounded interested, asked to see
  it), not for every shop on a list.

If it's not clear which mode is wanted, ask.

## Before you start

Ask for three things if they were not given: **business name**, **city**, **trade** (roofer, plumber, electrician, HVAC, landscaper, painter, etc.). Then confirm the plan in three lines and proceed.

## Step 1. The buyer's questions (no business name yet)

Run these with web search, exactly as a homeowner would type them. Swap in the trade and city.

1. best <trade> in <city>
2. who should I hire to <typical job> in <city>
3. how much does <typical job> cost in <city>
4. emergency <trade> <city>
5. most trusted <trade> company <city> reviews
6. <trade> <city> licensed and insured
7. what should I ask before hiring a <trade> in <city>

For each: who appeared, in what order, and did the business appear at all. Directories and lead-selling sites outranking real businesses is a finding. Write it down.

**"Appeared" means:** the business is named in the search engine's synthesized answer, OR the business's own website shows up among the returned links for that query. This exact definition is what the AI Visibility Score in Step 4 counts — it must be reproducible, not a judgment call each time.

While you're here, also note every other real, local, independently-owned business that appears across these 7 questions (not directories, not the target business). You'll need the two that appear most often for Step 4's comparison chart. Skip national franchise brands (Mr. Rooter, Roto-Rooter, ARS, One Hour, Neighborly-family brands, etc.) — franchise brand strength isn't something local marketing fixes, so it's not a fair comparison.

## Step 2. The business questions

8. is <business> any good
9. how much does <business> charge
10. is <business> licensed and insured
11. what areas does <business> serve
12. how long does <business> take to respond

For each, record what came back and whether it is **true**. Check against the business's own website. A wrong phone number, a service they stopped offering, a price that is off: these are the sharpest findings, because it is an active error going out to buyers all day.

## Step 3. The nine facts

Fetch the business's own website and the strongest competitor that appeared in step 1. For each of the nine, mark **STATED** (quote the exact words) or **NOT FOUND**. No guessing, no filling gaps. NOT FOUND is the product.

| # | Fact | Why an AI needs it |
|---|---|---|
| 1 | Price or price range | The most-asked question, the most-hidden answer |
| 2 | Named service area, actual city list | How "near me" gets resolved |
| 3 | License number, bonded, insured | A fact an AI can cite |
| 4 | Brands carried or certifications | Half of buyer questions name a brand |
| 5 | A real FAQ in plain text | This is what gets quoted |
| 6 | Financing terms | Removes the biggest objection on a big job |
| 7 | Warranty terms | How buyers compare |
| 8 | How to book, with a phone number in text | The step that makes money |
| 9 | Response and lead time | The other most-asked question |

Also check whether the business has a findable profile on each of 4 outside places: Yelp, Angi, HomeAdvisor, BBB. A profile that turns up in search with the business's real name, address, or phone counts as findable; nothing found counts as not findable. This is the Directory score in Step 4 — count only, don't say which ones are missing anywhere in the final report.

**Stop here in fact-check-only mode.** Report back the Gap count and the single sharpest
finding (the fact missing that costs this shop the most, or the sharpest thing an AI got
wrong about them) — no file, no Step 4.

## Step 4. Build the AI Visibility Report (full report mode only)

This is a **diagnosis, not a fix**. It exists to show the contractor, in under a minute of
reading, that AI rarely recommends them — and that whoever handed them this report knows
exactly how to solve it. It never teaches them how to solve it themselves.

**Compute three independent scores** (do not average them into one formula — the report
says so explicitly):

1. **AI Visibility Score** = (buyer questions from Step 1 the business appeared in ÷ 7) ×
   100, rounded. This is the headline number.
2. **Facts score** = (facts marked STATED in Step 3 ÷ 9) × 100.
3. **Directory score** = (directories with a findable profile ÷ 4 checked) × 100.

**Pick two comparison competitors**: the two real, local, independently-owned businesses
(never national franchises) that appeared most often in Step 1, with the same
appeared-count style used for the business's own score (e.g. "4 of 7").

**Find one sourced, general statistic** on why AI-referred traffic matters (conversion
rate, growth, or similar) — search for a current figure each time, always name the real
source inline. Never use an unsourced number, and never let this become the business's
own stat; it's general context, not a finding about them.

**Build the PDF** using `.claude/skills/contractor-audit/generate_report.py` as the
template: copy its `CONFIG` dict, fill in every field with the real values and prose above
(including a fresh `output_path` — never reuse a path that might already exist), then run
the script. It produces a two-page PDF: page 1 is the headline score, the "why this
matters" stat, the facts/directory breakdown, and the competitor comparison chart; page 2
is the missing-questions list, the "this is fixable" close, and the methodology footer.
Save to `first-product/reports/<date>-<city>-<trade>-<business>.pdf`.

**Never do any of this in the report**, even though earlier drafts of this skill did:

- Don't name which specific directories are missing (Step 3's directory check is a count
  only in the final report).
- Don't include a nine-facts comparison table, a numbered fix list, or any itemized
  how-to. The "this is fixable" close stays confident and specific enough to be credible,
  but gives away zero specifics.
- Don't invent a review count or star rating you couldn't verify from a real source outside
  the business's own claims — say so in the methodology text instead of scoring it.
- Don't compare against national franchise brands (Mr. Rooter, Roto-Rooter, ARS, One Hour,
  Neighborly-family brands, etc.).

## Rules

1. Quote the nine facts internally while you work, but never paraphrase a STATED as if it
   were vague, and never put the facts table itself in the final report.
2. Never claim a specific assistant (ChatGPT, Gemini) said something unless it was actually
   run there. Say "an AI assistant."
3. Every number in the report must trace back to something actually counted or a cited
   source. No guessed numbers, ever — say "could not verify" instead.
4. Never sell llms.txt or markup as magic. Say why if asked.
5. Findings about a real business are facts. No opinions about the owner.
6. The report is written to a file. It is never sent by you. A human sends it.
7. Plain English. No jargon. The reader is on a roof.
8. Bigger, easier-to-read fonts over cramming more in. If content doesn't fit two pages,
   trim prose and spacing before shrinking type.
