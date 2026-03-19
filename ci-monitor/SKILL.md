---
name: babysit-ci
description: Monitor GitHub Actions CI for the current branch. Automatically detect failures, fix them, and push. Also monitor and address PR review comments.
---

# Babysit CI

Monitor GitHub Actions CI for the current branch. Check every minute, flag failures, fix them, and push. Also monitor and address PR review comments.

## Instructions

1. **Determine the current branch and PR number:**
   ```
   git branch --show-current
   gh pr view --json number -q .number
   ```

2. **Set up a recurring check every 1 minute** using CronCreate with this prompt:
   ```
   Run the babysit-ci check: gh run list --branch <BRANCH> --limit 10 --json databaseId,status,conclusion,name | jq '[.[] | select(.status != "completed" or .conclusion == "failure")] | if length == 0 then "ALL PASSING" else . end'
   ```

3. **Run the first check immediately** (don't wait for the cron).

4. **On each check:**

   ### CI Failures
   - If all jobs are passing or still in progress with no failures: report status briefly and wait for the next tick.
   - If any job has `"conclusion": "failure"`:
     1. **Get the failure details**: `gh run view <databaseId> --log-failed 2>&1 | tail -80`
     2. **Analyze the failure**: Determine root cause (lint error, type error, test failure, build error, etc.)
     3. **Make a concise plan**: State what needs to change and in which file(s). Keep it to 2-3 sentences max.
     4. **Fix it**: Edit the necessary files. Run your project's lint/format command if it's a lint/format issue.
     5. **Commit and push**: Create a new commit with a descriptive message. Push to the same branch.
     6. **Continue monitoring**: The cron will pick up the new CI run automatically.

   ### PR Review Comments
   - **Maintain a local log** at `.claude/babysit-ci-comments-<PR_NUMBER>.json` tracking addressed comments:
     ```json
     { "addressed": { "<comment_id>": { "timestamp": "ISO8601", "action": "fixed|replied|skipped" } } }
     ```
   - On each tick, fetch PR comments:
     ```
     gh api repos/{owner}/{repo}/pulls/{pr_number}/comments --jq '.[] | select(.in_reply_to_id == null) | {id, path, line, body, user: .user.login, created_at: .created_at}'
     ```
   - **Skip any comment whose ID is already in the log.**
   - For each new comment:
     1. **Read the comment**: Understand what the reviewer is asking for.
     2. **Evaluate**: Is it a valid code change request, a question, or just informational?
     3. **For code change requests**: Read the referenced file/line, make the fix, include it in the next commit.
     4. **For questions**: Reply to the comment with a brief answer using `gh api repos/{owner}/{repo}/pulls/{pr_number}/comments -f body="<reply>" -f in_reply_to_id=<comment_id>`.
     5. **Skip bot comments** that are purely informational (e.g., deployment status, coverage reports).
     6. **Log the comment** as addressed with timestamp and action taken.

5. **When all jobs show `"status": "completed"` with `"conclusion": "success"` and all comments are addressed:**
   - Report "All CI checks passing, all comments addressed" with a summary.
   - Cancel the cron job using CronDelete.

## Rules
- Never use `--no-verify` or skip hooks.
- Never force push.
- Each fix gets its own commit (don't amend).
- If a failure is unclear or seems unrelated to our changes, flag it to the user before attempting a fix.
- If the same job fails twice after a fix attempt, stop and ask the user for help.
- If a review comment disagrees with the overall approach (not just a code tweak), flag it to the user rather than making sweeping changes.
- Batch multiple comment fixes into a single commit when they're related.
