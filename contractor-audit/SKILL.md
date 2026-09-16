---
name: contractor-audit
description: Run the questions a homeowner would ask an AI assistant about hiring a local contractor, check the contractor's own website for the nine facts an AI needs, and write an honest one-page report the contractor can read on a phone. Trigger on /contractor-audit, "audit <business>", "what does AI say about <business>", or "run the audit on".
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
  business) — run Steps 1–4. Writes the one-page file. This is a gift, not a scare, meant
  to be read by the contractor on a phone — build it for a shop that's already shown real
  interest (picked up, sounded interested, asked to see it), not for every shop on a list.

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

**Stop here in fact-check-only mode.** Report back the Gap count and the single sharpest
finding (the fact missing that costs this shop the most, or the sharpest thing an AI got
wrong about them) — no file, no Step 4.

## Step 4. Write the report (full report mode only)

Save to `first-product/reports/<date>-<city>-<trade>-<business>.md`. One page. This shape, these headings:

```
WHAT AI SAYS ABOUT <BUSINESS> · <date>

1. THE QUESTION BUYERS ASK
   <the cost or hire question, word for word>

2. WHAT CAME BACK
   <who appeared, in order. Where the business was, or that it was absent.>

3. WHAT IT GOT WRONG ABOUT YOU
   <factual errors, each one checked against your own site. If none, say none.>

4. THE NINE FACTS
   <table: fact | you | the competitor who beat you. STATED with the quote, or NOT FOUND.>

5. WHY <COMPETITOR> WINS
   <one paragraph, specific. What they publish that you don't.>

6. THE FIX, IN ORDER
   1. Put the facts on the page in plain text: price range, license number, service area, phone.
   2. A real FAQ page that answers the buyer's actual questions.
   3. Fix anything the AI got wrong at the source.
   (We do not sell "llms.txt" files or hidden markup. They mostly do nothing. Ask us why.)

7. THE HONEST SIZE OF THIS
   AI assistants are roughly 1 percent of website traffic in 2026. People who arrive
   through them buy about five times as often as people from a normal search, and the
   channel is growing fast. The fixes above also help in Google, which is 88 percent of
   traffic, so this is one piece of work that pays twice.

Prepared <date and time>. Run through one AI assistant with live web search, not every assistant.
```

## Rules

1. Quote the nine facts. Never paraphrase a STATED.
2. Never claim a specific assistant (ChatGPT, Gemini) said something unless it was actually run there. Say "an AI assistant."
3. Never leave out section 7. The small number is what makes the rest believable.
4. Never sell llms.txt or markup as magic. Say why if asked.
5. Findings about a real business are facts. No opinions about the owner.
6. The report is written to a file. It is never sent by you. A human sends it.
7. Plain English. No jargon. The reader is on a roof.
