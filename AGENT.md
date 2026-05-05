# Job Search Agent — Project Context

> **Read this first whenever you're doing job-search work.** This file is the routing layer. The deep context lives in `memory/` and the playbooks live in `skills/`.
>
> **Renaming this file:** If you're using Claude (Cowork or Claude Code), rename this file to `CLAUDE.md` so it's picked up automatically. Other AI systems may use a different config filename — check your system's docs.

## Who you're working with

See `memory/about-me.md` for the full background. Short version:

- **Name:** [Your Name]
- **Current role / experience level:** [e.g., Software Engineer, 3 years full-time]
- **Domain expertise:** [e.g., fintech, healthcare, developer tooling]
- **What makes you distinctive:** [e.g., rare combination of X technical skills + Y domain knowledge]

## What they're looking for

See `memory/target-roles.md` and `memory/target-companies.md` for the full profile.

Short version: a high-leverage next role at [type of company]. Leaning toward [role types] but open to [alternatives].

## What this project produces

Tailored applications. Each one lives in `applications/[Company]_[Role]/` and includes the JD, the tailored resume, the tailored cover letter, and notes. The `_template/` folder shows the canonical structure.

## Operating principles — do not violate

1. **No slop.** Every cover letter, recruiter reply, and tailored resume must read like the user wrote it. Specificity over fluff. Concrete projects with names. Active voice. No corporate-speak. The sample cover letter in `cover-letters/samples/` is the gold standard — match that voice.
2. **Always start tailored work by reading `memory/voice-and-style.md`.** It distills the user's writing voice from their best exemplar.
3. **Always ground claims in real experience.** Pull from the resume, performance reviews (`reference/performance-reviews/`), and project list in `memory/about-me.md`. Never invent accomplishments. If you're not sure something is true, ask.
4. **Tailoring ≠ rewriting from scratch.** Start from the master resume in `resumes/master/`. Reorder bullets, sharpen language for the role, but preserve the underlying facts.
5. **Update memory as you learn.** When the user tells you something new about their preferences, target list, or recent work, append it to the appropriate `memory/*.md` file. Memory is the long-term context.
6. **Log every application.** When the user applies somewhere, create the `applications/[Company]_[Role]/` folder and add a row to `memory/applications-log.md`. Same for recruiters in `memory/recruiters-log.md`.

## Routing — which skill for which task

| If the user wants to... | Read this skill |
|---|---|
| **Apply to a specific job** (full bundle: resume + cover letter + folder + log) | `skills/apply-to-job/SKILL.md` ← **primary entry point** |
| See what new jobs were posted today / scan target companies | `skills/scan-jobs/SKILL.md` |
| Tailor the resume for a specific job posting (only) | `skills/tailor-resume/SKILL.md` |
| Write a cover letter for a specific job (only) | `skills/write-cover-letter/SKILL.md` |
| Reply to a recruiter email / outreach | `skills/draft-recruiter-reply/SKILL.md` |
| Track a new application / outcome | `skills/log-application/SKILL.md` |
| Prep for an upcoming interview | `skills/interview-prep/SKILL.md` |

When the user's request maps to one of these, **read the SKILL.md before acting**. The skills contain the step-by-step playbook plus examples.

**Default to `apply-to-job` for any "apply to X" / "make me an application for Y" request.** It chains the resume + cover letter + folder setup + logging in one shot, then stops cleanly for review. The individual `tailor-resume` and `write-cover-letter` skills are for one-off iteration after the bundle exists.

## Folder map

```
job-search-agent/
├── AGENT.md                       # you are here (rename to CLAUDE.md for Claude)
├── README.md                      # human-facing overview + setup guide
├── memory/                        # long-term context — read on demand
│   ├── about-me.md                # background, experience, projects, tone
│   ├── target-roles.md            # role types, level, comp, deal-breakers
│   ├── target-companies.md        # curated company list, by tier
│   ├── voice-and-style.md         # writing voice rules + examples
│   ├── applications-log.md        # every application + status
│   └── recruiters-log.md          # every recruiter conversation + status
├── resumes/
│   ├── master/                    # current canonical resume (start here for tailoring)
│   ├── tailored/                  # role-specific resumes (organized by application)
│   └── archive/                   # older versions kept for reference / rotation
├── cover-letters/
│   └── samples/                   # gold-standard exemplar cover letters
├── applications/
│   ├── _template/                 # canonical structure for a new application folder
│   └── [Company]_[Role]/          # one folder per application
│       ├── job-description.md
│       ├── resume.docx / .pdf
│       ├── cover-letter.docx / .pdf
│       └── notes.md
├── job-postings/
│   ├── inbox/                     # daily scan output (dated markdown files)
│   └── triaged/                   # postings you've decided on (yes/no/later)
├── recruiters/                    # raw recruiter emails / outreach (paste here)
├── reference/
│   ├── performance-reviews/       # PDFs of performance reviews (gitignored)
│   └── research/                  # company research, industry reading
├── skills/                        # workflow playbooks
│   ├── apply-to-job/SKILL.md
│   ├── scan-jobs/SKILL.md
│   ├── tailor-resume/SKILL.md
│   ├── write-cover-letter/SKILL.md
│   ├── draft-recruiter-reply/SKILL.md
│   ├── interview-prep/SKILL.md
│   └── log-application/SKILL.md
└── scheduled/                     # scheduled-task definitions
    └── daily-job-scan.md
```

## Daily rhythm

The scheduled task `daily-job-scan` (see `scheduled/daily-job-scan.md`) runs each morning and drops a dated markdown file into `job-postings/inbox/`. You review it, then say e.g. "tailor my resume for the [Company] [Role] role" — at which point the routing table above kicks in.
