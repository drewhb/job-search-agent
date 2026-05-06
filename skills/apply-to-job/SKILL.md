---
name: apply-to-job
description: One-command application bundler. Takes a JD (URL, pasted text, or reference to today's scan) and produces a complete, ready-to-review bundle — tailored resume, cover letter, application folder, and log entry. Stops for human review before anything is submitted.
---

# apply-to-job — single-command application bundler

This is the **primary entry point** when the user has decided to apply to a specific role. It chains the underlying skills (`tailor-resume`, `write-cover-letter`, `log-application`) into one workflow with a clean handoff.

## When to use
- "Apply to the [Company] [Role]" / "Bundle me an application for this"
- "Tailor a resume + cover letter for this role: [URL or pasted JD]"
- "Help me apply to the X role from today's scan"
- Whenever the user has picked a posting and wants the full prep done in one shot

## Before you start — gather inputs

> **Preflight check:** Steps 2 and 3 use Python to generate `.docx` files. If `python-docx` is not installed, they will fail silently. Run `pip install -r requirements.txt` before proceeding if you haven't already (see `README.md` step 3a).

You need exactly one of:
- A **URL** to the job posting → fetch it and extract the JD text
- The **full pasted JD text** in chat
- A **reference to today's scan** (e.g., "the Acme Solutions Engineer one") → look it up in `job-postings/inbox/[today].md` and follow the link

If you only have a vague reference and can't pin down which role, **ask the user to clarify before doing anything**. Don't guess.

Then ask up to **two** quick clarifying questions, only if needed:
1. **Anything specific to emphasize?** (e.g., "I want to lead with the customer-facing angle" or "downplay the AWS work, this team uses GCP")
2. **Any known referral or contact?** (so you can mention it in the cover letter or notes)

Skip these questions if you can answer them from the JD + memory.

## Procedure

### Step 1 — Set up the application folder

1. Pick a clean folder name: `applications/[Company]_[Role-Slug]/` using kebab-case for the role (e.g., `Acme_Solutions-Engineer`, `Startup_Forward-Deployed-Engineer`). Keep it consistent with other folders in `applications/`.
2. If the folder doesn't exist: copy from `applications/_template/`.
3. Save the JD into `applications/[Company]_[Role-Slug]/job-description.md` with the company name, role title, posting URL, and date captured filled in at the top, then the full JD text below.
4. Initialize `notes.md` from the template — fill in the Quick facts section.

### Step 2 — Tailor the resume

Read `skills/tailor-resume/SKILL.md` in full and execute it against this JD. Key things to remember:
- Start from the master resume in `resumes/master/`
- Edit text in place; never touch formatting
- One page only — verify page count after PDF conversion
- Save output to `applications/[Company]_[Role-Slug]/[YourName]_Resume_[Company].docx` and `.pdf`

### Step 3 — Write the cover letter

Read `skills/write-cover-letter/SKILL.md` in full and execute it against this JD. Key things to remember:
- Re-read `cover-letters/samples/` and `memory/voice-and-style.md` first
- Anchor to 2–3 specific named projects from `memory/about-me.md`
- 300–400 words, 4–5 short paragraphs
- Save to `applications/[Company]_[Role-Slug]/[Company]_CoverLetter.docx` and `.pdf`

### Step 4 — Update the application notes

In `applications/[Company]_[Role-Slug]/notes.md`, fill in:
- **Why this one** (2–3 bullets — pull from `memory/target-companies.md` if the company is on the list, or write a quick rationale based on the JD)
- **Tailoring decisions made** — list what you emphasized / de-emphasized in both the resume and the cover letter, and why

### Step 5 — Log the application

Read `skills/log-application/SKILL.md` and add a row to `memory/applications-log.md` under "Active". **Status: "Drafted - pending review"** (not "Applied" yet — the user hasn't submitted).

### Step 6 — Hand off for review

Stop here. Don't submit anything. Don't move beyond "drafted." Present a concise handoff:

```
Bundle ready for review at applications/[Company]_[Role-Slug]/

Files:
  - job-description.md                         (JD captured)
  - [YourName]_Resume_[Company].pdf            (1 page, verified)
  - [Company]_CoverLetter.pdf
  - notes.md                                   (rationale + tailoring decisions)

Tailoring decisions I made:
  1. [decision 1 — what + why]
  2. [decision 2 — what + why]
  3. [decision 3 — what + why]

Cover letter anchor projects:
  - [Project 1] — why it fits this role
  - [Project 2] — why it fits this role

Honest gaps vs. JD:
  - [Anything in the JD the user genuinely doesn't have — be honest]
  - [Or: "no significant gaps" if true]

Suggested next steps:
  1. Open the PDFs and read both end-to-end
  2. Tell me what to tighten — paragraph numbers, specific bullets, tone
  3. When approved, I'll mark applications-log.md as "Applied" once you submit
```

## After review — iteration

When the user responds with edits, iterate:
- Make the change to the source `.docx`
- Re-convert to PDF
- Re-verify page count
- Show what changed in 1–2 sentences
- Ask if they want more changes or if it's ready

When the user says they've submitted, update `memory/applications-log.md` status from "Drafted - pending review" → "Applied" and add a Conversation log line in `notes.md`.

## What this skill never does

- **Never auto-submit anything.** No emails sent, no forms filled, no portals touched. The user submits.
- **Never invent experience to cover JD gaps.** If there's a real gap, name it in the handoff.
- **Never skip the review step.** Even if the bundle "looks clean," the user reads before submit.
- **Never overwrite a previously-approved version without asking.** If iterating, keep the prior file as `Resume_[Company]_v1.docx` etc.

## Failure modes to watch for

- **The JD is vague or short.** If the posting is just a paragraph and a list of buzzwords, the tailoring won't be sharp — say so and ask whether to push forward or pass.
- **The role is a stretch on level.** If the JD asks for significantly more years than the user has, name that in the gaps section.
- **Wrong company tier signal.** If the company isn't in `memory/target-companies.md`, surface that ("This company isn't on your target list — want me to add it, or are we just exploring?").
