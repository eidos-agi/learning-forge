# learn — Capture Learnings and Route Them

Extract learnings from what just happened. Classify each one. Write it to the specific place where the next agent will find it.

## Trigger

User says `/learn` or asks to capture learnings, reflect, or do a post-mortem.

### Arguments

- `<context>` — optional. What happened. If omitted, review the recent conversation.

## Instructions

### Step 1: Extract Raw Learnings

Review the context (conversation, debug session, post-mortem, build log). For each distinct thing learned, write one clear sentence that another agent could act on without additional context.

Bad: "The shell environment was wrong"
Good: "macOS .app bundles don't inherit the user's shell PATH — use absolute paths to interpreters"

Aim for 5-25 learnings per session. Fewer means you're not looking hard enough. More means you're being redundant.

### Step 2: Classify Each Learning

Each learning has a **type** and a **destination**:

| Type | What it is | Where it lands |
|------|-----------|---------------|
| **project-fix** | Specific to one project | That project's `CLAUDE.md` or guardrails |
| **forge-improvement** | Improves a forge's skill | That forge's skill file or guardrails |
| **pattern** | Reusable across projects | improve-forge rubric, or a new guardrail template |
| **tool-gotcha** | Specific to a tool/platform | The project's `CLAUDE.md` under a "Gotchas" section |
| **process** | About how to work | The relevant forge's SOP or guardrail |
| **principle** | General truth | omni (via voice/knowledge bucket) or relevant visionlog |

### Step 3: Route Each Learning

For each learning, write it to its destination. Literally edit the file.

Format for CLAUDE.md entries:
```markdown
## Learnings

- **[date]** Use absolute Python paths from .app bundles — PATH not inherited from shell (learned: eidos-assistant debug session)
- **[date]** Instrument before testing externally — chain logging found the bug that 10 CI tests missed
```

Format for guardrails:
```markdown
---
title: "Never shell out via PATH from macOS .app"
---
## Rule
When a macOS .app bundle needs to call Python/Node/any interpreter, use the absolute path to the binary, not a bare `python3` command. macOS apps launched from Finder/Dock do not inherit the user's shell PATH, pyenv, nvm, or other version managers.

## Why
Eidos Assistant's Whisper transcription failed in production because `python3` resolved to system Python (no faster-whisper) instead of pyenv Python. Took 5 hours to find because synthetic tests ran from Terminal (which has PATH) and passed.
```

Format for forge skill improvements:
```markdown
# Added to improve.md Step 2:
- For macOS apps: verify that shelled-out processes use absolute interpreter paths, not PATH-dependent commands
```

### Step 4: Log the Routing

Create or append to `learnings/YYYY-MM-DD.json` in the **source project** (the project where the learning originated):

```json
{
  "date": "2026-03-29",
  "session": "eidos-assistant debug",
  "learnings": [
    {
      "text": "macOS .app bundles don't inherit shell PATH",
      "type": "tool-gotcha",
      "routed_to": "eidos-assistant/CLAUDE.md",
      "actionable": "Use absolute Python path in WhisperService.swift"
    },
    {
      "text": "Instrument before testing externally",
      "type": "process",
      "routed_to": "improve-forge/.claude/skills/improve.md",
      "actionable": "Add 'check for built-in logging/instrumentation' to Step 2"
    }
  ],
  "count": 2
}
```

### Step 5: Summary

Output: how many learnings captured, where they were routed, which projects were touched.

## Rules

- **Every learning must land in a file.** If you can't name the file, the learning isn't actionable enough.
- **Don't duplicate.** Check if the learning already exists in the destination before writing.
- **Be specific.** "Be careful with paths" is useless. "Use /Users/.../.pyenv/versions/3.12.7/bin/python3 not python3" is useful.
- **Route to the narrowest scope.** A project-specific gotcha goes in that project's CLAUDE.md, not in a global forge.
- **The log is proof.** `learnings/YYYY-MM-DD.json` is how we know the learning was captured, not just discussed.
