---
name: scan-jobs
description: Scan target companies and job boards for new postings that match the user's criteria. Drops a dated markdown file into job-postings/inbox/ summarizing what's new, ranked by fit. Used by the daily-job-scan scheduled task and on-demand.
---

# scan-jobs — daily target-company sweep

## When to use
- Daily scheduled run (see `scheduled/daily-job-scan.md`)
- On-demand: "what new postings are at my target companies", "scan jobs for me", "what's new today"

## What you produce
A single markdown file written to `job-postings/inbox/YYYY-MM-DD.md` with:
1. New / changed postings discovered since the last scan
2. Each posting ranked **High / Medium / Low / Skip** for fit
3. A one-line "why this rating" for each
4. A "next actions" section flagging the High-fit ones

If no new postings worth surfacing exist, write a short file noting that — don't fabricate.

## Inputs you need before scanning
- `memory/target-companies.md` — the curated company list, by tier
- `memory/target-roles.md` — role types, level, deal-breakers
- `memory/applications-log.md` — so you don't surface roles already applied to
- `job-postings/seen.md` — registry of every URL already surfaced in a prior digest; use this as the authoritative dedup source

## Procedure

### 1. Load context
Read the five files above. Build a working list of:
- **Target companies** to check directly (Tier 1 → Tier 2 → Tier 3)
- **Role keywords** to filter for (from `memory/target-roles.md`)
- **Already-applied** companies/roles to skip
- **Seen URLs** — the full set of URLs from `job-postings/seen.md`. Any posting whose URL appears in this set is suppressed for this scan (silently — don't mention skipped-due-to-seen URLs in the inbox file), unless the re-surface rule applies (see Step 2). Rows rated `Suppressed` are always skipped with no re-surface exception.

### 2. Search
Use web search (or direct fetch on specific careers pages) to find new postings. Prioritize:
- Direct careers pages of Tier 1 + Tier 2 companies
- The job boards listed in `memory/target-companies.md`
- Early-stage boards (Wellfound, YC Work at a Startup, relevant portfolio pages)
- LinkedIn search for the role keywords from `memory/target-roles.md`

For each posting discovered, first check its URL against the seen set loaded in Step 1:
- **If the URL is in `job-postings/seen.md` with rating `Suppressed`:** skip silently, always — no re-surface exception.
- **If the URL is in `job-postings/seen.md` with rating `High`:** skip silently.
- **If the URL is in `job-postings/seen.md` with rating `Low` or `Medium`:** check the "Date first seen" column. If ≤30 days ago, skip silently. If >30 days ago, include it in the inbox as a `(re-surfaced)` posting with a note that it was previously seen.
- **If the URL is new:** proceed to scoring.

For each posting consider:
- Is the role level a fit? (within the experience range in `memory/target-roles.md`)
- Is the location workable? (check against location preferences)
- Does the role actually match the work described, not just the title?

### 3. Score each posting
**High** — Tier 1 or 2 company + role type matches Tier 1 in `target-roles.md` + experience range fits.
**Medium** — Tier 2 or 3 company OR the role is Tier 2 in `target-roles.md` + good fit otherwise.
**Low** — fits broad criteria but not exciting; worth knowing about.
**Skip** — actively wrong fit; don't list.

If a posting is great but the company isn't in `memory/target-companies.md`, call it out as "new candidate company" and propose adding it.

### 4. Write the inbox file
Use this template exactly:

```markdown
# Job scan — YYYY-MM-DD

_Scan run at HH:MM. N new postings found across M companies checked._

## High-fit

### [Company] — [Role title]
- **Link:** [url]
- **Location:** [city / remote]
- **Why:** [1 sentence — what makes this a high-fit]
- **Suggested next step:** Tailor resume + cover letter / Reach out to network for referral / Read JD more carefully

[...repeat...]

## Medium-fit

### [Company] — [Role title]
- **Link:** [url]
- **Location:** [city / remote]
- **Why:** [1 sentence]

## Low-fit (worth knowing about)

- [Company] — [Role] — [url] — [why low]

## New candidate companies to consider adding to target list

- [Company] — [reason it might fit]

## Skipped

(Briefly: companies checked with no new relevant postings.)
```

### 5. Update seen.md
After writing the inbox file, append one row to `job-postings/seen.md` for each URL that was included in the digest (High, Medium, or Low rated) **and does not already have a row**. Do **not** add rows for Skip-rated postings, and do **not** add a row if the URL is already present — one row per URL, no duplicates.

Format:
```
| [url] | [Company] | [Role title] | YYYY-MM-DD | [High / Medium / Low / Suppressed] |
```

### 6. Don't update applications-log.md
Scanning ≠ applying. Only the `log-application` skill writes to that log.

## Things to avoid

- **Don't list every posting at every company.** This is a curated digest, not a dump. If 12 new postings dropped at a company, surface the 2 that fit. Mention the rest as a one-liner if interesting.
- **Don't re-surface postings already in `seen.md`** unless the re-surface rule applies (`Date first seen` >30 days ago, Low/Medium rating). Do not diff against yesterday's inbox file; `seen.md` is the authoritative dedup source.
- **Don't fabricate.** If you can't reach a careers page, say so. If a posting's level is unclear, mark it Medium and say why.
- **Don't recommend roles that violate the deal-breakers in `memory/target-roles.md`.**

## After the scan completes (interactive sessions only)

If running interactively (not via scheduler), end with: *"Want me to tailor for any of the High-fit roles? Or update the target-companies list with the new candidates?"* This is the handoff to `apply-to-job`.
