# Claude Skills

A collection of reusable Claude Code skills for common development workflows.

## Skills

### [Multi-Agent Orchestration](./multi-agent-orchestration/)

Transform Claude Code from a single-threaded assistant into a parallel orchestrator that spawns specialized worker agents, coordinates complex workflows, and delivers results with unprecedented speed.

- Intelligent task decomposition into parallel workstreams
- Three agent types: Simple Workers, Resilient Workers, Sub-Orchestrators
- Pattern adherence workflow to match existing codebase conventions
- Domain-specific guides for software development, code review, testing, and more

### [Engineering Team](./engineering-team/)

Spin up a 4-agent engineering team (architect, engineer, reviewer, CI monitor) that self-coordinates via tasks. Say "form voltron" and the team spawns idle — you give the task, and they handle the rest.

- Architect plans, engineer implements in an isolated worktree, reviewer checks code, CI monitor babysits the pipeline
- CI monitor escalates architectural decisions back to the architect instead of guessing
- User approves the plan before implementation starts
- Max 2 review cycles, then the PR is created and CI is monitored

### [CI Monitor](./ci-monitor/)

Babysit GitHub Actions CI for your current branch. Automatically monitors CI runs, detects failures, fixes them, and pushes — all while tracking and addressing PR review comments.

- Polls CI status every minute via CronCreate
- Auto-fixes lint, type, and build errors
- Tracks and responds to PR review comments
- Stops when all checks pass and all comments are addressed

## Installation

Copy any skill's `SKILL.md` into your Claude Code commands directory:

```bash
# Example: install the CI monitor skill
cp ci-monitor/SKILL.md ~/.claude/commands/babysit-ci.md

# Example: install the orchestration skill
cp multi-agent-orchestration/SKILL.md ~/.claude/commands/orchestration.md

# Example: install the engineering team skill
cp engineering-team/SKILL.md ~/.claude/skills/form-voltron/SKILL.md
```

Then invoke with `/babysit-ci`, `/orchestration`, or `/form-voltron` in Claude Code.

## License

MIT License - see [LICENSE](LICENSE) for details.
