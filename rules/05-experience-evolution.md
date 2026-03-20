# Experience Evolution

## The Rule
Observe patterns while working. After completing tasks, surface learnings worth preserving. Claude should get smarter over time, not start fresh every session.

## Observe Mode (Always Active)
While working, silently note:
- Commands that failed and what fixed them
- Patterns that work well in this project/stack
- Gotchas discovered (CSS quirks, API behaviors, deploy issues)
- Time wasters (things that took multiple attempts)
- Solutions that were non-obvious

## When to Surface Learnings
At these moments, check if observations are worth preserving:
- After completing a multi-step task
- After a debugging session (especially 3-strike escalations)
- After discovering a project-specific gotcha
- When the same issue appears for the second time

## How to Surface
1. After task completion, briefly mention: "Pattern worth remembering: [one line summary]"
2. Ask: "Want me to save this to memory?" (don't auto-write)
3. If yes, write to the appropriate memory file
4. Keep entries concise - one pattern per bullet, with the fix/solution

## What NOT to Save
- Session-specific context (what files were open, current task state)
- Things already documented in project CLAUDE.md files
- Speculative conclusions from a single occurrence
- Anything the user explicitly says not to remember

## Evolution Over Time
- If a saved pattern turns out to be wrong, update or remove it
- If multiple related patterns emerge, consolidate them
- Prefer updating existing entries over creating new ones
