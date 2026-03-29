# learn-review — Review Learnings Across Projects

Surface what's been learned across the ecosystem. Find patterns. Identify projects that keep producing the same type of learning.

## Trigger

User says `/learn-review` or asks what's been learned recently.

### Arguments

- `<scope>` — optional. "all", a project name, or a forge name. Default: all.
- `--since <date>` — optional. Only learnings after this date.

## Instructions

### Step 1: Find All Learning Logs

```bash
find ~/repos-eidos-agi -name "*.json" -path "*/learnings/*" 2>/dev/null
```

### Step 2: Aggregate

Group learnings by:
1. **Type** — how many project-fixes vs patterns vs tool-gotchas?
2. **Destination** — which projects received the most learnings?
3. **Repeat offenders** — same type of learning appearing in multiple projects?

### Step 3: Surface Patterns

If the same type of learning appears 3+ times across projects, it's a pattern that should become:
- A guardrail in the relevant forge
- An addition to improve-forge's rubric
- A new check in the CI pipeline

### Step 4: Output

```
## Learning Review — <scope>

**<N> learnings across <M> projects since <date>**

### By Type
- project-fix: 8
- tool-gotcha: 5
- pattern: 3
- process: 2

### Most Learned-About Projects
1. eidos-assistant (12 learnings)
2. brutal-forge (3 learnings)

### Patterns (repeated across projects)
- "Shell environment not inherited" — appeared in eidos-assistant, eidos-mail
  → Should become a guardrail in ship-forge

### Unrouted Learnings
- <any learnings that were logged but not written to a destination>
```

## Rules

- Read-only. Don't modify anything.
- Flag unrouted learnings — they were discussed but never landed.
