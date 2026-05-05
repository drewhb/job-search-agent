---
name: log-application
description: Record a new application — create the folder, save the JD, add a row to applications-log.md. Use when the user applies somewhere or moves a recruiter conversation into a real application.
---

# log-application — record a new application

## When to use
- User says "I applied to [company] for [role]"
- A recruiter conversation (Bucket A from `draft-recruiter-reply`) moves to a real application
- After `tailor-resume` + `write-cover-letter`, when the materials are sent

## Procedure

### 1. Gather the basics
Confirm with the user (if not already provided):
- **Company** + **Role title**
- **Date applied** (default to today)
- **Source:** Direct, Recruiter, Referral, LinkedIn, Wellfound, YC, etc.
- **Posting URL**
- **First contact / recruiter** (if applicable)

### 2. Create or update the application folder
Folder name pattern: `applications/[Company]_[Role-Slug]/` (use kebab-case for the role slug — e.g., `Acme_Solutions-Engineer/`).

If the folder doesn't exist:
- Copy the contents of `applications/_template/`
- Fill in `job-description.md` with the JD text and posting URL
- Fill in `notes.md` (Quick facts section)

If the folder already exists (because tailored materials are already there):
- Just update `notes.md` with the application date and status

### 3. Log it
Append a row to `memory/applications-log.md` under "Active":

```
| YYYY-MM-DD | Company | Role | Source | Applied | applications/Company_Role/ | one-line note |
```

### 4. Cross-reference
- If this application originated from a recruiter conversation, update `memory/recruiters-log.md` — move the row from "Active conversations" to "Closed — pursued (moved to applications-log.md)".
- If the company wasn't in `memory/target-companies.md`, propose adding it.

### 5. Status updates over time
When the user shares an update ("phone screen scheduled", "moved to onsite", "got the offer", "passed"):
- Update the **Status** column in `memory/applications-log.md`
- Add a "Conversation log" line in the application's `notes.md`
- If the status moves out of Active (offer / passed / withdrew), move the row to the appropriate section in the log
