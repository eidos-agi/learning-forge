---
title: "learning-forge: Turn Experience Into Improvement"
type: "vision"
date: "2026-03-29"
---

## North Star

Every session produces learnings. Most die in the chat. learning-forge captures them, classifies them, and routes each one to the project, forge, or guardrail that needs to hear it.

The pattern: something happened → we learned something → that learning improves a specific thing → we record it where it will be found next time.

## Why This Exists

AI agents repeat mistakes across sessions. A bug found on Monday is re-introduced on Wednesday because the learning wasn't recorded where the next agent would see it. Humans have the same problem — lessons from one project don't transfer to the next.

learning-forge closes this loop. It takes raw learnings (like "macOS apps don't inherit shell environment") and routes them to:
- The project that needs the fix (eidos-assistant CLAUDE.md: "use absolute Python paths")
- The forge that should teach this (improve-forge: "check for environment gaps in health checks")  
- A guardrail if it's a constraint ("never shell out to python3 via PATH from a .app")
- omni if it's general knowledge worth remembering

## What It Does

1. **Capture** — extract learnings from a session (conversation, debug log, post-mortem)
2. **Classify** — each learning is about: a project, a forge, a pattern, a tool, or a principle
3. **Route** — write the learning where it will be found: CLAUDE.md, guardrails, visionlog goals, improve-forge snapshots, omni
4. **Verify** — confirm the learning was written to the right place

## What It Is Not

- Not a knowledge base (that's omni)
- Not a task tracker (that's ike)
- Not a decision log (that's visionlog ADRs)
- It's the bridge between experience and all of those systems
