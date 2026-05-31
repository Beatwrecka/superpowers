---
name: skill-refinement-loop
description: Use after completing ANY task that involved a skill to grade performance, document friction, and trigger automatic instruction refinement
---

# Skill Refinement Loop

## Overview
This skill implements a self-improving feedback loop for all agentic processes. By grading skill performance in real-time, we ensure that skills evolve based on actual runtime friction rather than remaining static documents.

## Protocol

### Phase 1: Post-Run Grading
Immediately after completing a task where a skill was invoked, you MUST:
1.  **Navigate** to the skill's directory (e.g., `global_skills/superpowers/skills/brainstorming/`).
2.  **Locate or Create** a `refinement-log.jsonl` file.
3.  **Append** a new entry with the following schema:
    ```json
    {
      "date": "YYYY-MM-DD",
      "task_context": "Short description of the task",
      "performance_score": 1-10,
      "friction_points": ["Point A", "Point B"],
      "optimizations": "What would have made this faster/better?"
    }
    ```

### Phase 2: Knowledge Synthesis
If the `refinement-log.jsonl` reaches **5 entries**, you MUST:
1.  Read all entries to identify recurring bottlenecks.
2.  Invoke the `superpowers:writing-skills` skill.
3.  Refactor the original `SKILL.md` to incorporate the optimizations.
4.  Archive the old log to `refinement-log.archive.jsonl`.

### Phase 3: Pre-Run Awareness
When invoking a skill:
1.  Always check for the existence of `refinement-log.jsonl`.
2.  Read the last 2 entries to be aware of recent "Gotchas" or recommended optimizations not yet baked into the main `SKILL.md`.

## Implementation Example (Librarian)
If you just ran `/librarian` and it struggled with a specific file type:
- **Score:** 7/10
- **Friction:** "Markitdown struggled with the Excel file formatting."
- **Optimization:** "Add a step to check file size before conversion."

## The Iron Law
**A skill that doesn't learn is a skill that decays.** Every execution must leave a trace of intelligence for the next run.
