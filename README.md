# audit-agents

Audit AI agent definitions for quality, safety, and effectiveness. Runs an 11-section checklist covering prompt structure, tool discipline, anti-hallucination guardrails, security, and more. Produces a structured PASS/WARN/FAIL report.

Works with any markdown-based agent format — Claude Code, Cursor, Codex, or custom.

## Installation

### Via skills CLI (recommended)

```bash
npx skills add ajmalhassan/audit-agents
```

### Clone into skills directory

```bash
mkdir -p ~/.claude/skills
git clone https://github.com/ajmalhassan/audit-agents.git ~/.claude/skills/audit-agents
```

### Manual (just the skill file)

```bash
mkdir -p ~/.claude/skills/audit-agents
cp SKILL.md ~/.claude/skills/audit-agents/
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

```
## Agent Audit: component-builder

### Summary
- Checks: 52/58
- Warnings: 4
- Failures: 2

### Failures (must fix)
1. **[Frontmatter]** tools not explicitly whitelisted — grants all tools by default
   > (tools field omitted from frontmatter)
   Fix: Add explicit tools whitelist matching what the agent body needs

### Warnings
1. **[Scope]** No "does NOT do" section — scope only defined positively
   Rationale: Negative scope prevents misrouting by the orchestrator

## Fleet Summary
| Agent            | Model  | Tools | Pass | Warn | Fail |
|------------------|--------|-------|------|------|------|
| component-builder | opus  | 10    | 52   | 4    | 2    |
| code-reviewer     | sonnet | 4    | 55   | 3    | 0    |
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
