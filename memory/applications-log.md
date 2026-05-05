# Applications Log

> One row per application. Update status as things move. Folder column links to `applications/<Company>_<Role>/`.

## Active

| Date applied | Company | Role | Source | Status | Folder | Notes |
|---|---|---|---|---|---|---|
| YYYY-MM-DD | [Company] | [Role] | [Direct / Recruiter / Referral] | [Applied / Drafted / Phone screen / etc.] | `applications/Company_Role/` | [One-line note] |

## Closed — interviewing

_(Empty)_

## Closed — offer

_(Empty)_

## Closed — passed / withdrawn

_(Empty)_

---

## Status flow

Applied → Recruiter screen → Hiring manager → Take-home → Onsite → **Offer** / **Passed** / **Withdrew**

## Schema notes

- **Date applied:** YYYY-MM-DD. Use "_(pending submit — drafted YYYY-MM-DD)_" if materials are ready but not sent.
- **Source:** Direct, Recruiter, Referral, LinkedIn, Wellfound, YC, etc.
- **Status:** Use the flow above. "Drafted - pending review" for bundles not yet submitted.
- **Folder:** path to the `applications/<Company>_<Role>/` directory. Long notes go there, not here.
