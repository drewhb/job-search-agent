# Job Search Agent

An AI-native job search workspace. Drop it in a folder, point your AI agent at it, and get a system that handles the entire job search loop: daily scanning, resume tailoring, cover letter drafting, recruiter replies, interview prep, and application tracking — all in your voice, grounded in your real experience.

Built as a Cowork workspace for Claude, but the system is plain markdown and works with any AI agent that has file read/write access and web search.

---

## What it does

**Morning digest.** A scheduled scan runs each day, checks your target companies and job boards, and drops a ranked digest into `job-postings/inbox/YYYY-MM-DD.md`. You read it over coffee and tell your agent which ones to pursue.

**One-command applications.** Say: *"Apply to the [Company] [Role] from today's scan."* Your agent:
1. Creates `applications/[Company]_[Role]/` with the JD saved
2. Tailors your master resume against the JD (reorders bullets, sharpens language — never invents experience)
3. Drafts a cover letter in your voice, anchored to your real projects
4. Logs the application in `memory/applications-log.md`
5. Stops and hands it back to you for review — nothing is submitted automatically

**Recruiter replies.** Paste an inbound message and say "draft a reply." The agent classifies interest level (interested / unsure / pass) and drafts accordingly in your tone.

**Application tracking.** Every application, recruiter conversation, and status update goes into structured markdown logs. Ask "what's the status of my Acme application?" and get a direct answer from the log.

**Interview prep.** Before a screen or onsite, say "prep me for my [Company] [Role] interview." The agent researches recent news, predicts likely themes, stages talking points anchored to your projects, and drafts sharp questions for you to ask.

---

## Setup

### 1. Clone the repo

```bash
git clone https://github.com/[your-username]/job-search-agent.git
cd job-search-agent
```

### 2. Rename the config file for your AI system

- **Claude (Cowork or Claude Code):** rename `AGENT.md` → `CLAUDE.md`
- **Other systems:** check your system's docs for what config file it reads on startup

### 3. Fill in your memory files

These are the files the agent reads to understand who you are and what you want. Edit them directly — they're plain markdown.

| File | What to fill in |
|---|---|
| `memory/about-me.md` | Your background, experience, key projects, technical skills |
| `memory/target-roles.md` | Role types you want, seniority level, comp expectations, deal-breakers |
| `memory/target-companies.md` | Companies organized by how much you want to work there |
| `memory/voice-and-style.md` | Your writing voice rules, distilled from your best cover letter |

### 3a. Install Python dependencies

The resume tailoring and cover letter skills use Python to edit `.docx` files and verify PDF page count.

```bash
pip install -r requirements.txt
```

You also need **LibreOffice** for `.docx` → PDF conversion:

```bash
# macOS
brew install --cask libreoffice

# Linux (Debian/Ubuntu)
sudo apt install libreoffice
```

> **No LibreOffice?** The skills will still produce `.docx` files — just convert them to PDF manually before submitting. Everything else works fine.

### 4. Add your master resume

Put your current resume in `resumes/master/`. This is the baseline for all tailoring — it's never edited in place.

```
resumes/master/YourName_Resume_v1.docx
```

### 5. Add a cover letter sample

Put your best cover letter in `cover-letters/samples/`. The agent uses it as the voice benchmark for every cover letter it drafts.

```
cover-letters/samples/Company_CoverLetter.pdf
```

### 6. Update the AGENT.md (or CLAUDE.md) header

Open `AGENT.md` and fill in the snapshot at the top (name, role, domain, what makes you distinctive). This is what the agent reads first on every session.

### 7. Set up the daily scan (optional)

See `scheduled/daily-job-scan.md` for instructions on setting up the morning digest. With Claude Cowork, you can schedule it directly from the workspace.

---

## Using it day to day

**Find new jobs:**
> "What's in today's scan?" or "Scan for new postings at my target companies."

**Apply to a role:**
> "Apply to the [Company] [Role]." or "Bundle me an application for this: [paste JD or URL]"

**Reply to a recruiter:**
> Paste the email/message in chat, then: "Draft a reply — I'm interested" or "Draft a polite pass."

**Track an application:**
> "Log that I applied to [Company] for [Role] today."

**Update your search criteria:**
> "Add [Company] to my Tier 1 target list." or "I've decided I'm not interested in [Role type] — update my target roles."

**Prep for an interview:**
> "I have a [recruiter screen / hiring manager call / onsite] at [Company] — help me prep."

<img width="1176" height="753" alt="image" src="https://github.com/user-attachments/assets/baf6b742-05ab-4980-ba51-d536af66b3a6" />

---

## The skills

Each task has a dedicated playbook in `skills/`. The agent reads the relevant one before acting — you don't need to tell it which to use.

| Skill | What it does |
|---|---|
| `apply-to-job` | Full application bundle: folder + tailored resume + cover letter + log entry |
| `scan-jobs` | Searches target companies and boards for new postings, ranks by fit |
| `tailor-resume` | Reworks the master resume against a specific JD |
| `write-cover-letter` | Drafts a cover letter in your voice |
| `draft-recruiter-reply` | Drafts a reply to inbound recruiter outreach |
| `log-application` | Records a new application or status update |
| `interview-prep` | Researches the company, stages talking points, drafts questions to ask |

---

## Design principles

- **Everything is markdown.** No databases, no apps, no lock-in. The whole system is readable files your agent can work with.
- **Memory over context.** Long-term facts (your background, preferences, target list) live in `memory/` and are read on demand — not crammed into every prompt.
- **Human in the loop, always.** The agent drafts; you review and submit. Nothing goes out without your eyes on it.
- **No fabrication.** The skills are written to ground every claim in `memory/about-me.md` and your actual resume. If there's a gap vs. a JD, the agent names it honestly rather than papering over it.
- **Voice-first.** Cover letters and recruiter replies are anchored to a real sample of your writing, not generic AI prose.

---

## Folder structure

```
job-search-agent/
├── AGENT.md / CLAUDE.md           # AI routing layer — read on every session
├── memory/                        # Who you are, what you want, your history
├── resumes/                       # Master + tailored + archive
├── cover-letters/samples/         # Your voice exemplars
├── applications/                  # One folder per application, fully self-contained
├── job-postings/inbox/            # Daily scan digests
├── recruiters/                    # Recruiter message archive
├── reference/                     # Performance reviews, company research
├── skills/                        # Agent playbooks (one per task type)
└── scheduled/                     # Scheduled task definitions
```

---

## Contributing

This is a template repo. If you improve a skill, add a new workflow, or adapt it to a new AI system, PRs are welcome. The skills are plain markdown — no code required.

---

## License

MIT
