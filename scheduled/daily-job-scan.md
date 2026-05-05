# Scheduled task: daily-job-scan

**Suggested schedule:** Every weekday at 7:00–8:00 AM local time (cron: `0 7 * * 1-5`)
**What it does:** Runs the `scan-jobs` skill against your target-companies list and dumps a ranked digest into `job-postings/inbox/YYYY-MM-DD.md`.

## How to set it up

### Claude Cowork
You can schedule this directly from a Cowork session:
1. Open a session in this folder
2. Say: *"Set up a daily job scan that runs at 7 AM every weekday"*
3. Claude will create and register the scheduled task

### Other AI systems
Check your system's scheduling documentation. The task should:
- Open this workspace folder as context
- Execute the `scan-jobs` skill (reading `skills/scan-jobs/SKILL.md`)
- Write output to `job-postings/inbox/YYYY-MM-DD.md`

### Manual / cron
If your system doesn't support scheduling natively, you can run the scan on-demand by telling your agent: *"Run the job scan now."*

## How to manage it

- **View / list tasks:** ask your agent — "list my scheduled tasks"
- **Pause:** "disable the daily-job-scan task"
- **Re-enable:** "enable the daily-job-scan task"
- **Run on-demand right now:** "run the job scan now"
- **Change the time:** "change the daily-job-scan to run at [new time]"
- **Edit what it scans for:** edit `memory/target-companies.md` and `memory/target-roles.md` directly — the scan reads those on each run

## What you'll see in the inbox each morning

A markdown file with five sections:
1. **High-fit** — jobs worth tailoring for today
2. **Medium-fit** — worth a closer look
3. **Low-fit** — FYI only
4. **New candidate companies** — companies not yet in the target list that look interesting
5. **Skipped** — what was checked with nothing relevant

When a High-fit catches your eye, say: *"Apply to the [Company] [Role]."* That kicks off the full `apply-to-job` workflow.
