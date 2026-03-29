# learning-forge

Turn experience into improvement. Every session produces learnings — most die in the chat. learning-forge captures them, classifies them, and routes each one to the project, forge, or guardrail that needs to hear it.

## The Problem

An agent finds a bug on Monday. Fixes it. The learning ("macOS apps don't inherit shell PATH") lives in the conversation. On Wednesday, a different agent makes the same mistake. The learning was never written down where it would be found.

## The Fix

```
/learn
```

Extracts learnings from the session, classifies each one (project-fix, forge-improvement, pattern, tool-gotcha, process, principle), and writes it to the specific file where the next agent will find it.

## Skills

| Skill | Description |
|-------|-------------|
| `/learn` | Capture, classify, and route learnings from a session |
| `/learn-review` | Aggregate learnings across the ecosystem, find patterns |

## Learning Types

| Type | Example | Destination |
|------|---------|-------------|
| project-fix | "Use absolute Python paths in WhisperService.swift" | Project's CLAUDE.md |
| forge-improvement | "improve-forge should check for built-in instrumentation" | Forge's skill file |
| pattern | "Instrument before testing externally" | improve-forge rubric |
| tool-gotcha | ".app bundles don't inherit PATH" | Project's CLAUDE.md gotchas |
| process | "Ship logging, not features" | Relevant forge's SOP |
| principle | "The first real user test beats all synthetic tests" | omni / visionlog |

## Usage

```bash
cp ~/repos-eidos-agi/learning-forge/.claude/skills/learn*.md .claude/skills/
```

Then after any session: `/learn`

## License

MIT
