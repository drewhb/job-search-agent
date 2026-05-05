---
name: interview-prep
description: Research a company + role and produce focused interview prep — likely questions, talking points anchored to the user's experience, and questions for them to ask. Saves into the application folder.
---

# interview-prep — pre-interview prep doc

## When to use
"I have an interview at [company], help me prep", "what should I know going in", "draft questions to ask".

## Inputs you need
- The company + role (and which round — recruiter screen, hiring manager, technical, onsite)
- The interviewer's name(s) if known
- `applications/[Company]_[Role]/job-description.md` — the JD
- `applications/[Company]_[Role]/notes.md` — application history
- `memory/about-me.md` — for talking-point anchors
- `reference/research/` — any existing research on the company

## Procedure

### 1. Confirm what the user already knows
Don't redo work. Ask: "Have we already done research on this company? What do you already know going in?" Skim `reference/research/` for anything saved.

### 2. Research the company (if not already done)
Use web search. Gather:
- **Latest news / funding / product announcements** (last 6 months)
- **Founders + key leaders** (background, where they came from)
- **Investors + valuation / funding stage**
- **Customers + case studies** — especially anything relevant to the user's domain experience
- **Product architecture** — at the level of detail a technical candidate needs
- **Competitive landscape** — who they compete against and how they position
- **Recent thought-leadership** — blog posts, founder essays, podcast appearances

Save the research to `applications/[Company]_[Role]/research.md`.

### 3. Identify likely interview themes
Based on the round + the JD, predict what they'll dig into. Common themes by round:

**Recruiter screen:** background fit, motivation for the role, comp expectations, timeline. Light technical.
**Hiring manager:** depth on relevant projects, why this role/company, team dynamics, work style.
**Technical:** system design, code, architecture, customer-facing scenarios — depending on role type.
**Onsite / final round:** combination of the above + culture fit + leadership/scope discussion.

### 4. Build anchor talking points
For each likely theme, pull from `memory/about-me.md` the 1–2 specific projects/experiences that best answer it. Pre-stage the proper nouns and numbers so the user doesn't have to dig in real time.

Format each talking point as:
- **Theme:** [what they'll ask / what this is about]
- **Anchor project:** [specific project name]
- **What to say:** [2–4 sentences — the setup, the specific work, the outcome]

### 5. Draft questions to ask
**Strong questions — these signal taste and engagement:**
- About the role: "What does success in this role look like at 3 months and 12 months?" / "What does the day-to-day actually look like?"
- About the team: "How does the team make decisions when there's disagreement on technical direction?" / "What's the most recent significant change you've made to how the team operates?"
- About the company: "Where do you think the product is most differentiated 12 months from now?" / "What's the part of the roadmap you're most uncertain about?"
- About the interviewer: "What's the thing about working here that surprised you most after joining?"

**Weak questions to avoid:**
- "What's the company culture like?" (vague)
- "What are growth opportunities?" (transactional)
- "Tell me about the engineering process." (let them lead)

Tailor the questions to the interviewer's role and what's already been covered in prior rounds.

### 6. Build the prep doc
Save to `applications/[Company]_[Role]/prep_[round]_[date].md`. Structure:

```markdown
# Interview prep — [Company] [Role] — [Round] — [Date]

## Interviewer(s)
- [Name, title, brief background note]

## Recent context (read first)
- [5–8 bullet points: the things to remember about the company right now — funding, product, recent news]

## Likely themes + anchors
### [Theme 1]
- **Anchor:** [Project name]
- **What to say:** [2–4 sentences]

### [Theme 2]
- **Anchor:** [Project name]
- **What to say:** [2–4 sentences]

[...repeat for 4–6 themes...]

## Questions to ask
1. [Question — calibrated to round and interviewer]
2. [Question]
3. [Question]
4. [Question]

## Things NOT to say
[Optional — anything to be careful around: framing on current employer, salary discussion timing, etc.]
```

### 7. Hand off
Walk the user through the doc, especially the anchor talking points and the questions to ask. Offer to mock-interview a question or two if they want.
