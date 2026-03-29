# learn — Capture Learnings and Route Them

Extract learnings from what just happened. Classify each one. Project-level learnings go directly to that project. Forge-level learnings go to forge-forge as proposals — forge-forge is the overseer.

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

Every learning is one of two kinds:

**A. Project-level** — specific to one project. You can write this directly.

| Type | Where it lands |
|------|---------------|
| **project-fix** | That project's `CLAUDE.md` gotchas or guardrails |
| **tool-gotcha** | That project's `CLAUDE.md` under "Gotchas" |

**B. Forge-level** — improves how a forge works, which affects every future agent that uses that forge. These are **proposals to forge-forge**, not direct edits.

| Type | Proposed to |
|------|------------|
| **forge-improvement** | forge-forge: "improve-forge Step 2 should check for built-in instrumentation" |
| **pattern** | forge-forge: "improve-forge rubric should score language boundaries in multi-language apps" |
| **process** | forge-forge: "learning-forge should route forge-level learnings as proposals, not direct edits" |
| **principle** | forge-forge: "ship logging before features — should this be a guardrail for all forges?" |

### Step 3: Route Project-Level Learnings

For each project-level learning, write it directly to the destination file:

```markdown
## Gotchas (learned the hard way)

- **[date]** Use absolute Python paths from .app bundles — PATH not inherited from shell
- **[date]** Test from the real .app, not Terminal — synthetic tests hide environment bugs
```

### Step 4: Propose Forge-Level Learnings to Forge-Forge

For each forge-level learning, write a proposal file to `forge-forge/proposals/`:

```
~/repos-eidos-agi/forge-forge/proposals/YYYY-MM-DD-<short-name>.md
```

Format:
```markdown
# Proposal: <title>

## Learning
<what was learned>

## Source
<which session/project/debug produced this>

## Proposed Change
- **Target forge:** <forge name>
- **Target file:** <specific skill/guardrail/template>
- **Change:** <what to add/modify>

## Why This Matters
<how this improves every future agent that uses the target forge>

## Evidence
<the specific incident that produced this learning>
```

Create `forge-forge/proposals/` directory if it doesn't exist. forge-forge's maintainer (human or overseer agent) reviews and applies.

**Do not edit other forges directly.** learning-forge captures. forge-forge governs.

### Step 5: Log the Routing

Create or append to `learnings/YYYY-MM-DD.json` in the **source project**:

```json
{
  "date": "2026-03-29",
  "session": "eidos-assistant debug",
  "learnings": [
    {
      "text": "macOS .app bundles don't inherit shell PATH",
      "type": "tool-gotcha",
      "routed_to": "eidos-assistant/CLAUDE.md",
      "status": "applied"
    },
    {
      "text": "improve-forge should check for built-in instrumentation in Step 2",
      "type": "forge-improvement",
      "routed_to": "forge-forge/proposals/2026-03-29-instrumentation-check.md",
      "status": "proposed"
    }
  ],
  "applied": 1,
  "proposed": 1
}
```

### Step 6: Summary

Output:
- How many learnings captured
- How many applied directly (project-level)
- How many proposed to forge-forge (forge-level)
- Which projects touched
- Which proposals created

## Rules

- **Project-level: apply directly.** CLAUDE.md gotchas, project guardrails.
- **Forge-level: propose to forge-forge.** Never edit another forge's skills directly.
- **Every learning must land in a file.** Applied or proposed — but in a file, not just discussed.
- **Don't duplicate.** Check if the learning already exists before writing.
- **Be specific.** "Be careful with paths" is useless. "Use absolute path to pyenv Python" is useful.
- **The log distinguishes applied from proposed.** `status: "applied"` vs `status: "proposed"`.
