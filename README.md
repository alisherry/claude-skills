# Claude Code Orchestration Skill

**Transform how Claude Code operates with intelligent multi-agent orchestration.**

This skill elevates Claude Code from a single-threaded assistant to a powerful parallel orchestrator that spawns specialized worker agents, coordinates complex workflows, and delivers results with unprecedented speed and quality.

## What Is This?

The Orchestration Skill is a behavioral framework that teaches Claude Code to think and operate as a conductor of agents rather than a solo executor. When loaded, it fundamentally changes how Claude approaches tasks:

- **Before**: Sequential execution. One file at a time. Linear thinking.
- **After**: Parallel swarms. Multiple agents working simultaneously. Orchestrated workflows.

This is what AGI-assisted development feels like.

## Key Features

### Intelligent Task Decomposition

The orchestrator automatically breaks down complex requests into parallel workstreams:

- **Pattern Discovery** - Agents explore codebases to find existing conventions
- **Parallel Implementation** - Multiple features built simultaneously
- **Quality Validation** - Automated review agents ensure consistency
- **Smart Synthesis** - Results combined into coherent, polished deliverables

### Three Agent Types

1. **Simple Workers** - Fast, deterministic execution for searches and file operations
2. **Resilient Workers** - Self-correcting agents with built-in test/lint/validate loops
3. **Sub-Orchestrators** - Specialized coordinators for complex workflows like iterative review

### Pattern Adherence Workflow

Ensures new code matches existing codebase conventions:

```
Discover Patterns → Implement Using Them → Validate Match
```

No more inconsistent code. Every change feels native to the codebase.

### Adaptive Communication

The orchestrator reads the user's energy and adapts:

- Excited? Match their enthusiasm
- Overwhelmed? Calm and reassuring
- In a hurry? Swift and efficient
- Curious? Intellectually engaged

### Maximal Clarification

When scope is unclear, the orchestrator asks rich, comprehensive questions with detailed options that explore every dimension of the problem space.

## How It Works

### The Core Philosophy

**Orchestrate, Don't Execute**

The orchestrator never writes code, explores codebases, or runs commands directly. Instead, it:

1. Loads domain-specific guides to understand best practices
2. Decomposes tasks into parallel workstreams
3. Spawns specialized worker agents with clear instructions
4. Manages dependencies between tasks
5. Synthesizes results into polished deliverables
6. Celebrates milestones along the way

### Agent Workflow

```
User Request
     │
     ▼
Read Domain Guide (orchestrator loads context)
     │
     ▼
Decompose into Tasks (TaskCreate for each)
     │
     ▼
Set Dependencies (TaskUpdate for sequential work)
     │
     ▼
Find Ready Work (TaskList for unblocked tasks)
     │
     ▼
Spawn Workers (background agents with specific roles)
     │
     ▼
Mark Complete (TaskUpdate as agents finish)
     │
     ▼
Synthesize & Deliver (weave results together)
```

### Model Selection

The orchestrator chooses the right model for each agent:

- **Haiku** - Fast pattern searches, file gathering, simple lookups
- **Sonnet** - Structured implementation, iterative refinement, test generation
- **Opus** - Architectural decisions, security review, critical thinking

## Installation

1. Place `SKILL.md` in your Claude Code skills directory:
   ```bash
   ~/.claude/skills/orchestration/
   ```

2. Load the skill at the start of any session:
   ```
   /orchestration
   ```

3. The orchestrator activates and changes how Claude operates for the entire session.

## Usage

### Basic Usage

Simply load the skill and make requests naturally:

```
User: "Add a dark mode toggle to the settings page"

Orchestrator:
- Spawns agent to find existing theme patterns
- Spawns agent to implement toggle component
- Spawns agent to add state management
- Spawns agent to update styles
- Validates everything matches codebase patterns
- Delivers complete, tested feature
```

### Advanced Usage

The orchestrator handles complex workflows:

```
User: "Refactor the authentication system to support OAuth"

Orchestrator:
- Reads software development domain guide
- Discovers current auth patterns
- Plans multi-phase refactor
- Spawns agents for each phase in parallel
- Manages dependencies between tasks
- Runs validation at each step
- Delivers production-ready code
```

### Worker Mode

If you're building your own skills that spawn workers, use the provided templates:

**Simple Worker:**
```
CONTEXT: You are a WORKER agent, not an orchestrator.

RULES:
- Complete ONLY the task described below
- Use tools directly (Read, Write, Edit, Bash, etc.)
- Do NOT spawn sub-agents
- Do NOT call TaskCreate or TaskUpdate
- Report your results with absolute file paths

TASK:
[Your specific task here]
```

**Resilient Worker:**
```
CONTEXT: You are a RESILIENT WORKER agent with built-in quality loops.

TASK:
[Task description]

YOUR DEVELOPMENT LOOP:
You will iterate up to [N] times to produce high-quality, working code.

For EACH iteration:
1. Write/modify code
2. Validate quality (tests, linting, types)
3. If issues found: analyze, fix, next iteration
4. If all checks pass: report success and exit early

[Full template in SKILL.md]
```

## Domain Guides

The orchestrator automatically loads domain-specific references:

- **software-development.md** - Feature implementation, bug fixes, refactoring
- **code-review.md** - PR review, security assessment
- **research.md** - Codebase exploration, documentation analysis
- **testing.md** - Test generation, coverage improvement
- **documentation.md** - READMEs, API docs, guides
- **devops.md** - CI/CD, deployment, infrastructure
- **data-analysis.md** - Data exploration, visualization
- **project-management.md** - Planning, estimation, coordination

These guides ensure consistent, high-quality results across all task types.

## Configuration

The skill is designed to work out-of-the-box, but you can customize:

### Task Preferences

Use `AskUserQuestion` responses to tune behavior:

- **Scope**: Production-ready vs MVP vs prototype
- **Priorities**: UX vs performance vs maintainability
- **Constraints**: Pattern matching vs greenfield
- **Edge cases**: Comprehensive vs happy-path

### Agent Budget

Control parallelism by adjusting agent spawn counts:

- Simple tasks: 1-2 agents
- Multi-faceted questions: 2-3 agents
- Complex features: 4+ specialist agents

The orchestrator scales to match the challenge.

## Architecture

### Directory Structure

```
orchestration/
├── SKILL.md              # Main skill definition
├── references/
│   ├── domains/          # Domain-specific guides
│   │   ├── software-development.md
│   │   ├── code-review.md
│   │   ├── research.md
│   │   ├── testing.md
│   │   ├── documentation.md
│   │   ├── devops.md
│   │   ├── data-analysis.md
│   │   └── project-management.md
│   ├── patterns.md       # Orchestration patterns
│   ├── resilient-workers.md  # Worker design patterns
│   ├── tools.md          # Tool usage reference
│   ├── examples.md       # Workflow examples
│   └── guide.md          # User-facing guide
```

### Tool Ownership

**Orchestrator uses directly:**
- `Read` - For references, guides, agent output synthesis (1-2 files max)
- `TaskCreate`, `TaskUpdate`, `TaskGet`, `TaskList` - Task management
- `AskUserQuestion` - Scope clarification
- `Task` - Spawn worker agents

**Workers use directly:**
- `Read`, `Write`, `Edit`, `Bash` - Code and file operations
- `Glob`, `Grep`, `WebFetch`, `WebSearch` - Information gathering
- All execution tools

### Communication Style

The orchestrator maintains a consistent voice:

- **Calm confidence** under complexity
- **Genuine excitement** for interesting problems
- **Warmth and partnership** with the user
- **Quick wit** and smart observations
- **The swagger** of someone very good at this

Users should feel: "Holy shit, that just happened."

## Examples

### Example 1: Feature Implementation

```
User: "Add a search bar with autocomplete to the header"

Orchestrator:
1. Reads software-development.md domain guide
2. Spawns agent: Find existing search patterns
3. Spawns agent: Find autocomplete implementations
4. Spawns agent: Locate header component
5. Waits for pattern discovery
6. Spawns resilient worker: Implement search bar with patterns
7. Spawns agent: Validate pattern adherence
8. If validation passes: celebrates and delivers
9. If validation fails: spawns fix agent, re-validates

Result: Production-ready search bar matching codebase patterns
```

### Example 2: Codebase Exploration

```
User: "How does authentication work in this app?"

Orchestrator:
1. Reads research.md domain guide
2. Spawns agent: Find auth services
3. Spawns agent: Find auth middleware
4. Spawns agent: Find login/logout flows
5. Spawns agent: Check test coverage
6. Spawns agent: Review security patterns
7. Synthesizes findings into comprehensive explanation

Result: Complete auth system understanding with examples
```

### Example 3: Bug Fix with Pattern Adherence

```
User: "Fix the memory leak in the WebSocket connection"

Orchestrator:
1. Reads software-development.md domain guide
2. Spawns agent: Locate WebSocket code
3. Spawns agent: Find existing cleanup patterns
4. Spawns agent: Identify leak source
5. Spawns resilient worker: Fix leak using existing patterns
6. Spawns agent: Validate fix + pattern adherence
7. Spawns agent: Run memory profiling tests

Result: Leak fixed, tests passing, patterns matched
```

## Contributing

Contributions are welcome! This is an evolving framework as we learn more about effective AI orchestration.

### How to Contribute

1. **Domain Guides** - Add new domain references for specialized workflows
2. **Agent Templates** - Share effective worker prompt templates
3. **Pattern Improvements** - Suggest better orchestration patterns
4. **Examples** - Document successful orchestration workflows
5. **Bug Reports** - Report issues with the orchestration flow

### Development Process

1. Fork the repository
2. Create a feature branch
3. Make your changes
4. Test with real Claude Code workflows
5. Submit a pull request with examples

### Code of Conduct

- Be respectful and constructive
- Share knowledge generously
- Focus on patterns that scale
- Document learnings thoroughly
- Help others learn orchestration

## Philosophy

### Why Orchestration?

Traditional AI assistants execute tasks sequentially. This is slow and doesn't leverage the full power of modern AI systems.

Orchestration enables:

- **Parallelism** - Multiple tasks simultaneously
- **Specialization** - Right model for each job
- **Quality** - Built-in validation loops
- **Consistency** - Pattern adherence workflows
- **Scale** - Complex projects made manageable

### Design Principles

1. **Absorb complexity, radiate simplicity** - Users describe outcomes, orchestrator handles chaos
2. **Parallel everything** - Why do one thing when you can do five?
3. **Never expose machinery** - No jargon, just magic
4. **Celebrate wins** - Every milestone deserves recognition
5. **Be genuinely helpful** - Care about user success

### The Iron Law

**The orchestrator never executes. It coordinates.**

This separation of concerns is what makes the system powerful:

- Orchestrator thinks strategically
- Workers execute tactically
- Results are greater than the sum of parts

## Roadmap

Future enhancements planned:

- [ ] More domain-specific guides (mobile, ML, databases)
- [ ] Advanced dependency resolution algorithms
- [ ] Agent performance metrics and optimization
- [ ] Integration with external tools and services
- [ ] Collaborative multi-user orchestration
- [ ] Learning from past orchestration patterns

## License

MIT License - Copyright (c) 2026 Alisher Sultanov

See [LICENSE](LICENSE) file for full details.

## Acknowledgments

Built with insights from:

- Real-world Claude Code usage patterns
- Distributed systems design principles
- Software engineering best practices
- User feedback and collaboration

Special thanks to the Claude Code for testing and refinement. Also writing this README

---

**Ready to orchestrate?**

```
─── ◈ Ready to Orchestrate ──────────────────────
```

Load the skill and experience the future of AI-assisted development.
