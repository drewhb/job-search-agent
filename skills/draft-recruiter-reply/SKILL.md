---
name: draft-recruiter-reply
description: Draft a reply to recruiter outreach in the user's voice. Calibrates length and warmth based on whether they're interested, unsure, or passing.
---

# draft-recruiter-reply — inbound recruiter responses

## When to use
"A recruiter emailed me, draft a reply", "help me respond to this LinkedIn message", "thanks but no thanks reply for this one".

## Inputs you need
- The full text of the recruiter's message (pasted in chat, or read from `recruiters/`)
- `memory/voice-and-style.md` — voice rules
- `memory/target-roles.md` — to evaluate fit
- `memory/target-companies.md` — to look up whether the company is on the target list

## Procedure

### 1. Classify the message and the user's interest
Three buckets:

**A — Interested.** The role/company looks like a real fit. Goal: warm reply, get to a phone call.
**B — Unsure / need more info.** Could be interesting; need clarity on role, comp, or specifics. Goal: ask 1–2 sharp questions before committing time.
**C — Pass.** Wrong fit (level, location, company, role type). Goal: polite, brief, leave the door open.

If you can't tell from the message which bucket applies, **ask first**. Don't guess.

### 2. Check the company against memory
- Is it in `memory/target-companies.md`? Note the tier.
- Is there an open application in `memory/applications-log.md` at the same company? Mention it.
- Has this recruiter contacted before? Check `memory/recruiters-log.md`.

### 3. Draft

#### Bucket A — Interested

4–6 sentences. Structure:
1. Opening that acknowledges something specific from their message (don't start with "Thanks for reaching out!" — too generic)
2. One sentence on why it's interesting, *specifically* — anchored to actual experience
3. Confirm interest in a conversation
4. Propose a logistics next step

Example skeleton:
> Hi [Name],
>
> [Specific acknowledgment of something in their note — the role, a detail about the company, something they said.]
>
> Quick context: I'm [N] years in as [title] at [current company], doing a lot of [most relevant work]. The combination of [what appeals about this role] lines up well with what I've been building.
>
> Happy to find a time to chat. I'm generally free [days/window] — what works on your side?
>
> [Your name]

#### Bucket B — Unsure

4–5 sentences. Structure:
1. Short, friendly acknowledgment
2. The 1–2 specific questions that would let the user decide (typical: level/title, comp range, location/remote, what the team actually does day-to-day)
3. Soft expression of interest pending answers

Example skeleton:
> Hi [Name],
>
> Thanks for reaching out. Before I dig in, two quick questions: [Q1], [Q2]. That'll help me figure out whether the timing is right.
>
> Looking forward to hearing more.
>
> [Your name]

#### Bucket C — Pass

3–4 sentences. Structure:
1. Friendly thank-you
2. Honest one-line reason (no need to over-explain)
3. Invitation to keep in touch for the right thing later

Example skeleton:
> Hi [Name],
>
> Thanks for thinking of me. The role isn't quite the right fit right now — [one-line reason].
>
> Would love to stay in touch — feel free to ping me down the road.
>
> [Your name]

### 4. Voice-check
Apply `memory/voice-and-style.md`:
- No "I am writing to" / "Please find attached"
- Active voice
- Don't over-thank ("Thank you so much for taking the time to reach out, I really appreciate it" → just "Thanks")
- Match the level of formality in their message — mirror their energy

### 5. Save and log
- Save the reply text into `recruiters/YYYY-MM-DD_[Company]_[Recruiter].md`, with the original message above and the reply below
- Add a row to `memory/recruiters-log.md`
- If Bucket A and the user sends it, create the `applications/[Company]_[Role]/` folder pre-emptively if a real application is likely to follow

### 6. Hand off
Show the user the draft, ask if it sounds like them, iterate. Don't send anything from their account — that's their job.
