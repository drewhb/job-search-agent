---
name: write-cover-letter
description: Draft a cover letter for a specific job in the user's voice. Anchored to their gold-standard sample. Saves to applications/[Company]_[Role]/.
---

# write-cover-letter — cover letter drafting

## When to use
"Write a cover letter for [role]", "draft a cover letter for [company]". Usually follows `tailor-resume`.

## Inputs you need
- The job description (`applications/[Company]_[Role]/job-description.md`)
- `cover-letters/samples/` — **the gold standard**, re-read every time before drafting
- `memory/voice-and-style.md` — voice rules, dos and don'ts
- `memory/about-me.md` — project list to draw from
- The tailored resume (if one exists yet) — for consistency in framing

## Procedure

### 1. Re-read the sample cover letter
Open the best sample in `cover-letters/samples/` and the voice file. Hold them in mind as you draft. The sample is the structural skeleton — study what it does:

1. **Para 1 (2–3 sentences):** Why *this role*, with the connecting thread to lived experience pulled forward.
2. **Para 2 (4–5 sentences):** Domain proof — specific knowledge accumulated, named with proper nouns.
3. **Para 3 (3–4 sentences):** Technical / builder proof — 2–3 named projects, each one sentence.
4. **Para 4 (3–4 sentences):** Self-aware framing of any pivot or stretch. Reframe as the point.
5. **Para 5 (1–2 sentences):** Culture / pace fit.
6. **Close (2 sentences):** "I'd welcome the chance to discuss…" + "Thank you for your time."

Not every letter needs all 5 paragraphs. Use judgment based on the role.

### 2. Pick the 2–3 anchor projects
From `memory/about-me.md`, pick the 2–3 specific projects that *most directly* answer "why is this person the right fit for *this* role?" Don't list everything. Don't repeat the resume.

### 3. Draft

Apply the voice rules from `memory/voice-and-style.md` strictly:
- Active voice, "I" sentences
- Named projects, not vague claims
- Short paragraphs
- No corporate buzzwords
- No weak openers
- ~300–400 words total

Open with a sentence that maps to *this company's* specific pitch — what makes *them* interesting, tied to what the user does. Show you read the JD beyond the title.

### 4. Save output
- Write the letter as a `.docx` (use `python-docx`) and save as `applications/[Company]_[Role]/[Company]_CoverLetter.docx`
- Convert to PDF and save as `[Company]_CoverLetter.pdf` in the same folder
- Match the sample's basic formatting: standard business letter, "Dear [Company] Hiring Team," opener, sign-off with the user's full name

### 5. Self-review before handing off
Read the draft one more time. Ask:
- Does any sentence sound like AI? Cut it or rewrite it.
- Could this letter be sent to *any other company* with light edits? If yes, it's not specific enough — make it more specific to *this* role.
- Is there anything the user didn't actually do? Fix it.
- Did you start any sentence with "Moreover," "Furthermore," "In addition"? Rewrite.

### 6. Hand off
Show the user the draft, point at the file, and ask which paragraphs feel like them and which feel off. Iterate from their feedback. Save iterations — don't overwrite the first draft until they approve.

## Quality bar — do not ship if any of these are true
- The letter could be sent unchanged to a different company
- Any project mentioned wasn't actually done
- The opener is "I am writing to apply for…" or anything similarly weak
- The letter is over 450 words
- It uses any of the buzzwords flagged in `memory/voice-and-style.md`
