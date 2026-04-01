# audit-agents

**Stop repeating the same agent failures.**

A Claude Code skill that runs an 11-section PASS/WARN/FAIL audit on your agent definitions — prompt structure, tool discipline, anti-hallucination, security, and more. Gives the exact line and fix suggestion for every finding. Works with Claude Code, Cursor, Codex, and any markdown-based agent format.

[![License: MIT](https://img.shields.io/badge/License-MIT-blue.svg)](LICENSE)
[![Install with skills CLI](https://img.shields.io/badge/install-npx%20skills%20add-brightgreen)](https://skills.sh)

## Install

```bash
npx skills add ajmalhassan/audit-agents
```

Or clone directly:

```bash
git clone https://github.com/ajmalhassan/audit-agents.git ~/.claude/skills/audit-agents
```

## Why I built this

I keep writing new agents — different projects, different purposes. But the things that go wrong are always the same. Agent scope-creeps, hallucinates findings, declares "done" too early. I open the definition and find a gap I've already fixed in another agent.

There's no quick way to check if a definition is solid before you start using it. You run the agent, watch it break, fix the definition, repeat. So I wrote down the criteria I keep checking for against industry best practices and packaged it as a skill. I run it after every revision now to see if the score improves.

## What It Checks

| # | Section | Covers |
|---|---------|--------|
| 1 | Frontmatter Integrity | name, description, tool whitelist, model selection |
| 2 | Scope Boundaries | purpose, handoffs, exit conditions, agent spiral/relentless patterns |
| 3 | Anti-Hallucination | read-before-claim, evidence requirements, exhaustion gates |
| 4 | Prompt Structure | phase breakdown, approval gates, thin agent principle |
| 5 | Output Format | structured templates, severity levels |
| 6 | Tool Usage Discipline | least-privilege, role archetypes, command restrictions |
| 7 | Robustness | missing file handling, failure behavior, fallbacks |
| 8 | Hook Awareness | redundancy with lifecycle hooks |
| 9 | Reusability | generic instructions, no hardcoded context |
| 10 | Security | secrets, prompt injection, unsafe commands, scoped access |
| 11 | Model-Capability Alignment | instructions match model strengths |

Plus cross-agent checks for scope overlap, handoff coherence, and defense-in-depth.

## Example Output

Real audit of a component-reviewer agent (60/63 checks passed):

```
Agent Audit: component-reviewer

Summary
- Checks: 60/63
- Warnings: 3
- Failures: 0

Warnings (suggested improvements)
1. [Prompt Structure — Thin Agent] 340 lines (2.3× guideline). The 7 category
   contracts (lines 185–237) could be a companion file loaded based on the
   component's detected category.
   Rationale: Reduces main body by ~50 lines and limits attention dilution
   for mid-capability model.

2. [Robustness] No explicit instruction for what to do if the component
   directory doesn't exist (user provides wrong name).
   Rationale: The reviewer reads packages/react/src/components/{Name}/index.tsx
   — if it's missing, behavior is undefined.

3. [Model-Capability] 80+ checklist items may exceed mid-capability model's
   attention span for a single pass.
   Rationale: Sonnet works best with concise, outcome-oriented rules. Consider
   priority-ordering items or splitting into focused passes.

Passes
- [Frontmatter] name, description, tools (8), model (sonnet) all correct;
  Judge archetype (no Write/Edit)
- [Scope] Purpose clear; known codebase-wide gaps called out; layout
  components explicitly excluded; exit = structured report
- [Anti-Hallucination] "re-read the actual file" before FAILs; UNDETERMINED
  escape hatch; evidence requires 3-question reasoning
- [Output Format] Severity levels defined; structured report template with
  Failures/Warnings/Passes
- [Tool Usage] All 8 tools have concrete use cases; Bash constrained to
  grep, pnpm run check, pnpm run test
- [Reusability] Generic placeholders; category-based conditional contracts
- [Security] No secrets, scoped to packages/, no unsafe commands
```

## Usage

Invoke directly:

```
/audit-agents
```

Or describe what you need:

```
Audit my agent definitions
Review the agents in this project for best practices
```

## Complementary Tools

This skill does semantic, expert-level prompt review. For programmatic CI checks, pair with:

| Tool | What it does |
|------|-------------|
| [AgentLinter](https://github.com/seojoonkim/agentlinter) | 25+ syntax/secrets/schema rules, VS Code + GitHub Action |
| [cclint](https://github.com/carlrannaberg/cclint) | Frontmatter validation, heading structure, dangerous commands |
| [Agent Audit](https://github.com/HeadyZhang/agent-audit) | Security scanner, 53 rules mapped to OWASP Agentic Top 10 |
| [Promptfoo](https://github.com/promptfoo/promptfoo) | Agent evaluation framework, red teaming |

## License

MIT
