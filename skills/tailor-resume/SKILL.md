---
name: tailor-resume
description: Rework the master resume against a specific job description. Reorders bullets, sharpens language for the role, never fabricates. Saves output to applications/[Company]_[Role]/.
---

# tailor-resume — JD-specific resume tailoring

## When to use
"Tailor my resume for [role]", "make a resume for [company]", after `scan-jobs` surfaces a high-fit role and the user wants to apply.

## Inputs you need
- The job description (full text, not just the title) — paste from user or read from `applications/[Company]_[Role]/job-description.md`
- The master resume in `resumes/master/` — the canonical baseline
- `memory/about-me.md` — full project list, ground-truth for accomplishments
- `reference/performance-reviews/` — for additional context if available

## Procedure

### 1. Set up the application folder
If `applications/[Company]_[Role]/` doesn't exist yet:
- Copy the structure from `applications/_template/`
- Save the JD into `applications/[Company]_[Role]/job-description.md` with the company, role, and posting URL filled in

### 2. Read the JD carefully
Pull out:
- **Required skills/experience:** what they say they need
- **Nice-to-haves:** what they'd love
- **Domain signals:** any industry-specific language
- **Day-to-day responsibilities:** what someone in this role *does*
- **Seniority signals:** "associate", "senior", "lead" — calibrate the tone

### 3. Map JD → actual experience
For each required/desired item in the JD, find the *real* thing in the user's background (`memory/about-me.md` + the master resume) that matches. Build a mental table:

| JD requirement | User's evidence |
|---|---|
| "Experience with RAG systems" | [Specific project name and what it did] |
| "Customer-facing technical role" | [Specific context — who they worked with and how] |
| "AWS infrastructure" | [Specific AWS services used and outcome] |

If the JD asks for something the user genuinely *doesn't* have, **don't fabricate**. Decide whether the gap is small enough that the rest of the resume carries the application, or large enough to flag before continuing.

### 4. Tailor the resume — content only, never formatting

**Critical rule: only the *text content* of paragraphs and bullets may change. Layout, fonts, spacing, margins, indentation, bullet style, section order, and column structure must be identical to the master.**

**The right way to do this:**

1. Open the master resume `.docx` with `python-docx`.
2. Walk the document's paragraphs (and table cells, if the resume uses tables for layout) and edit text *in place* on each `Run` object. Never delete and recreate a paragraph — that loses its style.
3. To reorder bullets within a section, swap the *text* between two paragraphs while leaving their style containers untouched.
4. Never call `doc.add_paragraph()` to introduce new content. If a tailoring change requires a new bullet, ask first — usually the right move is to *replace* an existing bullet's text.
5. Save to the output path. **Don't touch styles, sections, or the document's underlying XML structure beyond text content.**

A reference snippet to start from:

```python
from docx import Document
doc = Document('resumes/master/YourName_Resume_v1.docx')

for para in doc.paragraphs:
    if para.text.startswith('Built a [specific project]'):
        new_text = '[Reworded bullet aligned to JD]'
        if len(para.runs) == 1:
            para.runs[0].text = new_text
        else:
            para.runs[0].text = new_text
            for r in para.runs[1:]:
                r.text = ''

doc.save('applications/[Company]_[Role]/YourName_Resume_[Company].docx')
```

**What you may change (text content only):**
- **Summary / headline at top.** Adjust framing to match the role type (e.g., client-facing vs. backend vs. AI engineering).
- **Bullet ordering within a role.** Move the most JD-relevant bullets earlier. Reorder by swapping text across existing paragraph slots.
- **Bullet wording.** Light edits to align language with the JD's vocabulary. Don't change facts or numbers.
- **Skills section.** Reorder lists within the same paragraph slots to surface the most relevant skills first.

**What NOT to change:**
- Layout, formatting, fonts, sizes, spacing, margins — *anything* visual
- Job titles, dates, employers — these are facts
- Projects the user didn't actually do — never add
- Quantitative claims — keep the actual numbers from the master
- The total number of bullets, sections, or pages

### 5. Save output, then verify it's still one page

- Save the tailored `.docx` as `applications/[Company]_[Role]/YourName_Resume_[Company].docx`
- Convert to PDF: `libreoffice --headless --convert-to pdf <docx>` (or `soffice` on macOS). Save as `YourName_Resume_[Company].pdf` in the same folder.
- **Verify page count = 1.** Count pages with `pdfplumber` or `pypdf`:
  ```python
  import pdfplumber
  with pdfplumber.open(pdf_path) as p:
      n = len(p.pages)
  assert n == 1, f"Resume overflowed to {n} pages — must be 1"
  ```
- If page count > 1: **tighten wording, do NOT change layout.** Shorten the longest bullets first. If still over after one round of tightening, stop and ask which bullets to compress further. Never remove a bullet without asking.
- Update `applications/[Company]_[Role]/notes.md` with a "Tailoring decisions made" section listing what you emphasized / de-emphasized and why.

### 6. Hand off
Tell the user the file is ready, point at the path, and call out:
- 2–3 specific tailoring decisions made (so they can sanity-check)
- Any JD requirements they genuinely don't meet — be honest
- Confirm page count is 1
- Suggest the next step: `write-cover-letter` skill

## Quality bar — do not ship if any of these are true
- The PDF is more than one page
- Any visual formatting changed vs. the master
- Any bullet describes work the user didn't do
- The number of bullets or sections changed vs. the master
- The summary reads like generic AI output ("results-driven engineer with passion for innovation")
- A skill is listed that the user doesn't actually have

When in doubt, ask before shipping.
