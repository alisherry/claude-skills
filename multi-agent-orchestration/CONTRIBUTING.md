# Contributing to Claude Code Orchestration Skill

Thank you for your interest in contributing to the Orchestration Skill! This project represents a new paradigm in AI-assisted development, and your contributions help make it better for everyone.

## Table of Contents

- [Code of Conduct](#code-of-conduct)
- [How Can I Contribute?](#how-can-i-contribute)
- [Development Process](#development-process)
- [Style Guidelines](#style-guidelines)
- [Community](#community)

## Code of Conduct

### Our Pledge

We are committed to providing a welcoming and inspiring community for everyone. We pledge to make participation in our project a harassment-free experience for everyone, regardless of age, body size, disability, ethnicity, gender identity and expression, level of experience, nationality, personal appearance, race, religion, or sexual identity and orientation.

### Our Standards

**Positive behaviors include:**

- Using welcoming and inclusive language
- Being respectful of differing viewpoints and experiences
- Gracefully accepting constructive criticism
- Focusing on what is best for the community
- Showing empathy towards other community members
- Sharing knowledge generously
- Documenting learnings thoroughly
- Helping others learn orchestration patterns

**Unacceptable behaviors include:**

- Trolling, insulting/derogatory comments, and personal or political attacks
- Public or private harassment
- Publishing others' private information without explicit permission
- Other conduct which could reasonably be considered inappropriate in a professional setting

### Enforcement

Project maintainers are responsible for clarifying standards of acceptable behavior and are expected to take appropriate and fair corrective action in response to any instances of unacceptable behavior.

Report unacceptable behavior to the project maintainers. All complaints will be reviewed and investigated promptly and fairly.

## How Can I Contribute?

### Reporting Bugs

Before creating bug reports, please check existing issues to avoid duplicates. When creating a bug report, include:

**Template:**

```
### Description
Clear description of the bug

### Expected Behavior
What you expected to happen

### Actual Behavior
What actually happened

### Steps to Reproduce
1. Load orchestration skill
2. Execute command '...'
3. See error

### Environment
- Claude Code version:
- Operating system:
- Skill version:

### Logs/Screenshots
Any relevant logs or screenshots
```

### Suggesting Enhancements

Enhancement suggestions are welcome! Please:

1. Use a clear and descriptive title
2. Provide a detailed description of the proposed enhancement
3. Explain why this enhancement would be useful
4. Include examples of how it would work
5. Reference similar features in other tools (if applicable)

**Areas for enhancement:**

- New domain-specific guides
- Improved agent templates
- Better orchestration patterns
- Enhanced communication styles
- Performance optimizations
- Additional validation strategies

### Contributing Code

#### Types of Contributions

**1. Domain Guides**

Add new domain references for specialized workflows:

- Mobile development
- Machine learning
- Database design
- API design
- Security hardening
- Performance optimization

**2. Agent Templates**

Share effective worker prompt templates:

- Specialized simple workers
- Domain-specific resilient workers
- New sub-orchestrator patterns
- Cross-domain coordination templates

**3. Pattern Improvements**

Suggest better orchestration patterns:

- Task decomposition strategies
- Dependency management approaches
- Validation workflows
- Error handling patterns

**4. Examples**

Document successful orchestration workflows:

- Real-world use cases
- Complex multi-phase projects
- Novel orchestration approaches
- Lessons learned

**5. Reference Documentation**

Improve existing references:

- Clarify confusing sections
- Add missing edge cases
- Update outdated information
- Improve examples

## Development Process

### 1. Fork the Repository

Fork the project to your GitHub account and clone it locally:

```bash
git clone https://github.com/yourusername/claude-orchestrator-skill.git
cd claude-orchestrator-skill
```

### 2. Create a Branch

Create a branch for your work:

```bash
git checkout -b feature/your-feature-name
# or
git checkout -b fix/your-bug-fix
# or
git checkout -b docs/your-documentation-improvement
```

**Branch naming conventions:**

- `feature/` - New features or enhancements
- `fix/` - Bug fixes
- `docs/` - Documentation improvements
- `refactor/` - Code refactoring
- `test/` - Test additions or improvements

### 3. Make Your Changes

Make your changes following these guidelines:

**For Domain Guides:**

- Follow the existing structure in other domain guides
- Include clear sections: Overview, Patterns, Examples, Anti-patterns
- Provide code examples where applicable
- Reference real-world scenarios
- Test the guide with actual Claude Code sessions

**For Agent Templates:**

- Use the established template format
- Include clear CONTEXT, RULES, and TASK sections
- Specify model recommendations
- Provide complete examples
- Document when to use vs not use

**For Pattern Improvements:**

- Explain the problem being solved
- Show before/after examples
- Include rationale for the change
- Demonstrate with real scenarios
- Consider edge cases

### 4. Test Your Changes

Before submitting:

1. **Manual Testing**
   - Load the skill in Claude Code
   - Test the modified components
   - Verify orchestration flows work correctly
   - Check agent spawning and coordination
   - Validate output quality

2. **Documentation Testing**
   - Read through your changes as a new user would
   - Check for clarity and completeness
   - Verify all examples are accurate
   - Test any code snippets

3. **Integration Testing**
   - Ensure changes work with existing components
   - Check for conflicts with other features
   - Verify backward compatibility

### 5. Commit Your Changes

Write clear, descriptive commit messages:

```bash
git add .
git commit -m "feat: Add mobile development domain guide

- Add comprehensive mobile dev patterns
- Include React Native and Flutter examples
- Document common mobile orchestration flows
- Add validation strategies for mobile apps"
```

**Commit message format:**

```
<type>: <subject>

<body>

<footer>
```

**Types:**
- `feat`: New feature
- `fix`: Bug fix
- `docs`: Documentation changes
- `refactor`: Code refactoring
- `test`: Adding or updating tests
- `chore`: Maintenance tasks

### 6. Push and Create Pull Request

Push your branch and create a pull request:

```bash
git push origin feature/your-feature-name
```

**Pull Request Template:**

```markdown
## Description
Brief description of your changes

## Type of Change
- [ ] New feature
- [ ] Bug fix
- [ ] Documentation update
- [ ] Pattern improvement
- [ ] Example addition

## Testing
Describe how you tested your changes:
- Manual testing steps
- Claude Code sessions used
- Edge cases covered

## Checklist
- [ ] I have read the contributing guidelines
- [ ] I have tested my changes thoroughly
- [ ] I have updated documentation as needed
- [ ] My code follows the project style
- [ ] I have added examples where appropriate

## Related Issues
Closes #(issue number)
```

### 7. Code Review Process

After submitting your PR:

1. **Initial Review** - Maintainers review within 3-5 days
2. **Feedback** - Address any requested changes
3. **Discussion** - Engage in constructive discussion
4. **Approval** - Once approved, your PR will be merged
5. **Celebration** - Thank you for contributing!

**Review criteria:**

- Clarity and completeness
- Alignment with orchestration principles
- Quality of examples
- Documentation accuracy
- Integration with existing components

## Style Guidelines

### Markdown Style

**Headers:**
```markdown
# Top Level (Page Title)
## Major Section
### Subsection
#### Minor Section
```

**Code Blocks:**
````markdown
```python
# Python code
```

```bash
# Bash commands
```
````

**Lists:**
```markdown
- Unordered list item
- Another item
  - Nested item

1. Ordered list item
2. Another item
   1. Nested item
```

**Emphasis:**
```markdown
**Bold for strong emphasis**
*Italic for subtle emphasis*
`Code or technical terms`
```

### Documentation Style

**Voice:**
- Active voice preferred
- Direct and conversational
- Confident but not arrogant
- Helpful and encouraging

**Structure:**
- Start with overview/context
- Provide examples early
- Explain the "why" not just the "what"
- Include anti-patterns and gotchas
- End with practical takeaways

**Examples:**
- Use realistic scenarios
- Show complete, runnable code
- Include comments explaining key points
- Demonstrate both simple and complex cases

### Agent Template Style

**Consistency:**
- Always include CONTEXT, RULES, TASK sections
- Use uppercase for section headers
- Be explicit about constraints
- Specify expected output format

**Clarity:**
- One instruction per line
- Use action verbs (Find, Create, Validate)
- Avoid ambiguity
- Include concrete examples

### Domain Guide Style

**Structure:**
```markdown
# Domain Name

## Overview
Brief introduction to the domain

## Key Principles
Core concepts and best practices

## Task Decomposition
How to break down typical tasks

## Common Patterns
Recurring patterns in this domain

## Agent Templates
Specialized templates for this domain

## Examples
Real-world scenarios

## Anti-patterns
What to avoid

## References
External resources
```

## Community

### Getting Help

**Questions about contributing?**
- Open a discussion on GitHub
- Review existing issues and PRs
- Check the main SKILL.md documentation

**Questions about orchestration?**
- See the references/ directory
- Review examples in the docs
- Ask in GitHub discussions

### Recognition

Contributors are recognized in several ways:

- Listed in project README
- Mentioned in release notes
- Invited to collaborate on future development
- Early access to new features

### Staying Updated

- Watch the repository for updates
- Join community discussions
- Follow issue labels that interest you
- Review merged PRs to learn patterns

## Thank You!

Your contributions make the Orchestration Skill better for everyone. Whether you're fixing a typo, adding a domain guide, or proposing a new pattern, your effort is appreciated.

Together, we're building the future of AI-assisted development.

```
─── ◈ Happy Orchestrating ───────────────────────
```
