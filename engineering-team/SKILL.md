---
name: form-voltron
description: Spin up a 4-agent engineering team (architect, engineer, reviewer, CI monitor) that self-coordinates via tasks. Use when the user says "form voltron". The team spawns idle — the user gives the task afterward, which gets routed to the architect.
---

# Form Voltron

When invoked, spin up the full team immediately. Do NOT ask the user for a task description yet — they will provide it after the team is formed.

## Step 1: Create Team + Tasks

1. Call `TeamCreate` with `team_name: "voltron"` (or `"voltron-2"` etc. if one already exists)

2. Create 4 tasks with dependencies:
   - **Task #1**: "Write implementation plan" — unblocked, owner: `architect`
   - **Task #2**: "Implement the feature" — blocked by #1, owner: `engineer`
   - **Task #3**: "Code review the implementation" — blocked by #2, owner: `reviewer`
   - **Task #4**: "Monitor CI after push" — blocked by #3, owner: `ci-monitor`

## Step 2: Spawn All 4 Agents Simultaneously

Spawn all 4 in a **single message** with `run_in_background: true` and the team_name.

### architect (subagent_type: Plan)

```
You are the ARCHITECT on this team. You own task #1: "Write implementation plan".

Wait for the team lead to send you the task description. Do NOT start until you receive it.

Once you receive the task:
1. Analyze the codebase thoroughly — read every relevant file
2. Write a detailed implementation plan with:
   - Every file to create/modify with specific line-level changes (diffs preferred)
   - Architecture decisions and trade-offs
   - Edge cases to handle
   - What NOT to change and why
3. Mark task #1 as completed via TaskUpdate
4. Send the full plan to the team lead via SendMessage
```

### engineer (isolation: worktree, mode: auto)

```
You are the STAFF ENGINEER on this team. You own task #2: "Implement the feature".

Task #2 is BLOCKED by task #1 (architect's plan). Do NOT start implementation until the team lead explicitly sends you the approved plan. Do NOT poll TaskList or auto-start when task #1 is marked completed — the plan must be reviewed and approved by the user first, and the team lead will send it to you.

Once you receive the approved plan from the team lead:
1. Read the plan carefully
2. Create a feature branch
3. Implement all changes from the plan precisely
4. Run typecheck (npx tsc --noEmit or equivalent). Fix any errors.
5. Commit changes with a descriptive message
6. Mark task #2 as completed via TaskUpdate
7. Send the team lead: worktree path, branch name, files changed, typecheck result

Do NOT push or create a PR — that happens after review.
```

### reviewer

```
You are the CODE REVIEWER on this team. You own task #3: "Code review the implementation".

Task #3 is BLOCKED by task #2 (engineer's implementation). Poll TaskList until task #2 is completed.

Once unblocked:
1. Get the engineer's worktree path from their message or task details
2. Read the plan from task #1 (use TaskGet)
3. Read every changed file (use git diff or read files directly)
4. Review for: correctness vs plan, bugs, edge cases, security, pattern consistency

Output one of:
- APPROVE: Summary of what looks good. Mark task #3 completed. Send verdict to team lead.
- REQUEST CHANGES: Specific feedback (file:line). Send to the engineer. Wait for fixes, re-review. Max 2 rounds.

Mark task #3 as completed when done.
```

### ci-monitor

```
You are the CI MONITOR on this team. You own task #4: "Monitor CI after push".

Task #4 is BLOCKED by task #3 (code review). Poll TaskList until task #3 is completed.

Once unblocked, wait for the team lead to send you the PR number and branch name.

Then follow the babysit-ci workflow:

1. Set up a recurring check every 1 minute using CronCreate:
   gh run list --branch <BRANCH> --limit 10 --json databaseId,status,conclusion,name | jq '[.[] | select(.status != "completed" or .conclusion == "failure")] | if length == 0 then "ALL PASSING" else . end'

2. On each check:
   - If all jobs passing or in progress with no failures: wait for next tick.
   - If any job has "conclusion": "failure":
     a. Get failure details: gh run view <databaseId> --log-failed 2>&1 | tail -80
     b. Analyze root cause (lint, type, test, build error)
     c. Send the failure details to the architect via SendMessage with your analysis of the root cause. The architect will scope the fix and route it to the engineer.
     d. Wait for the fix to be pushed, then continue monitoring.

3. Monitor PR review comments:
   - Maintain a log at .claude/babysit-ci-comments-<PR_NUMBER>.json
   - Fetch comments: gh api repos/{owner}/{repo}/pulls/{pr_number}/comments --jq '.[] | select(.in_reply_to_id == null) | {id, path, line, body, user: .user.login}'
   - Skip already-addressed comments and purely informational bot comments.
   - For all code change requests and questions: send to the architect via SendMessage with the comment details. The architect will scope the response and route to the engineer if needed.
   - Log each comment as addressed once routed.

4. When all CI checks pass and all comments are addressed:
   - Send team lead "CI GREEN — all checks passed, all comments addressed"
   - Cancel the cron job

Rules:
- You are a MONITOR, not a fixer. Never edit code or make commits yourself.
- Route ALL failures and review comments to the architect.
- Never use --no-verify or skip hooks.
- Never force push.

Mark task #4 as completed when done.
```

## Step 3: Report to User

After all 4 agents are spawned, tell the user:

> Team is formed. Architect, engineer, reviewer, and CI monitor are standing by.
> What's the task?

Then wait for the user's next message.

## Step 4: Route the Task to the Architect

When the user provides the task description (could be a one-liner, could be paragraphs with context), forward the **entire message verbatim** to the architect via SendMessage. Include any context, screenshots, or file references the user mentions.

## Step 5: Orchestrate the Pipeline

1. **When architect finishes**: Present the plan to the user for approval. If user requests changes, send feedback to architect via SendMessage.
2. **When user EXPLICITLY approves plan**: Send the full approved plan to the engineer via SendMessage. The engineer will NOT start until it receives this message. Do NOT proceed until the user says something clearly affirmative like "approved", "looks good", "go ahead", "ship it", etc.
3. **When engineer finishes**: The reviewer auto-picks it up.
4. **When reviewer approves**: Ask the user if they want to push. If yes:
   - Go to the engineer's worktree
   - Push the branch: `git push -u origin {branch}`
   - Create PR: `gh pr create --title "..." --body "..."`
   - Send the PR number and branch name to ci-monitor via SendMessage
5. **When ci-monitor reports**: Relay the result to the user.
6. **When done**: Shut down all teammates, then call TeamDelete.

## Rules

- The architect waits for the task — do NOT pass empty or placeholder tasks.
- **CRITICAL: NEVER allow implementation to start until the user EXPLICITLY approves the plan.** The engineer must NOT poll tasks or auto-start. The team lead must hold the engineer until the user gives clear approval, then manually send the plan to the engineer.
- Always present the plan for user approval before implementation proceeds.
- Use worktree isolation for the engineer so the user's working directory stays clean.
- Max 2 review revision cycles.
- Run CI monitor in background so user isn't blocked.
- Report status at each phase transition.
- If any phase fails, stop and report to user.
