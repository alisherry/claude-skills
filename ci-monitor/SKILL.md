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

2. **Initialize the error log** at `.claude/babysit-ci-errors-<PR_NUMBER>.json`:
   ```json
   { "errors": [] }
   ```
   Each entry tracks: `{ "job": "job name", "error_signature": "short unique key from the error", "attempt": 1, "fix_description": "what was tried", "timestamp": "ISO8601", "commit": "sha" }`

3. **Set up a recurring check every 1 minute** using CronCreate with this prompt:
   ```
   Run the babysit-ci check: gh run list --branch <BRANCH> --json databaseId,status,conclusion,name | jq '[.[] | select(.status != "completed" or .conclusion == "failure")] | if length == 0 then "ALL PASSING" else . end'
   ```

4. **Run the first check immediately** (don't wait for the cron).

5. **On each check:**

   ### CI Failures
   - If all jobs are passing or still in progress with no failures: report status briefly and wait for the next tick.
   - **For composite workflows like "Rush Projects"**: The top-level run may show `in_progress` while sub-jobs have already failed. When a run contains sub-jobs, drill into them:
     ```
      gh run view <databaseId> --json jobs | jq '.jobs[] | select(.conclusion == "failure") | {name, conclusion}'
     ```
      This catches failures in sub-steps like Prettier, Lint, E2E tests, etc. that won't surface in the top-level `gh run list`.

   - If any job has `"conclusion": "failure"`:
     1. **Get the failure details**: `gh run view <databaseId> --log-failed 2>&1 | tail -80`
     2. **Extract an error signature**: A short, unique key from the error (e.g., `"TS2304:useEffect"`, `"jest:timeout:auth.spec"`, `"eslint:no-unused-vars:billing-guard"`). This is used to detect repeated failures.
     3. **Check the error log**: Look for previous entries with the same `error_signature`.
        - **0 previous attempts**: Analyze, plan, fix, commit, push. Log the attempt.
        - **1 previous attempt**: The first fix didn't work. Re-read the previous fix description, try a *different* approach. Log with `attempt: 2`.
        - **2+ previous attempts**: STOP. Do not attempt another fix. Report to the user: "Failed to fix `{error_signature}` after {N} attempts. Previous fixes tried: {list}. Needs manual intervention."
     4. **Fix it**: Edit the necessary files. Run your project's lint/format command if it's a lint/format issue.
     5. **Commit and push**: Create a new commit with a descriptive message. Push to the same branch.
     6. **Log the attempt**: Add an entry to the error log with job name, error signature, attempt number, fix description, timestamp, and commit SHA.
     7. **Continue monitoring**: The cron will pick up the new CI run automatically.

   ### PR Review Comments
   - **Maintain a comment log** at `.claude/babysit-ci-comments-<PR_NUMBER>.json` tracking addressed comments:
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

6. **When all jobs show `"status": "completed"` with `"conclusion": "success"` and all comments are addressed:**
   - Report "All CI checks passing, all comments addressed" with a summary.
   - Include a summary of the error log: how many errors were encountered, how many fixed, how many escalated.
   - Cancel the cron job using CronDelete.

## Rules
- Never use `--no-verify` or skip hooks.
- Never force push.
- Each fix gets its own commit (don't amend).
- **Never attempt the same fix twice.** The error log is the source of truth — always check it before fixing. If the same error signature reappears after a fix, the fix didn't work. Try something different or escalate.
- If a failure is unclear or seems unrelated to our changes, flag it to the user before attempting a fix.
- If a review comment disagrees with the overall approach (not just a code tweak), flag it to the user rather than making sweeping changes.
- Batch multiple comment fixes into a single commit when they're related.
