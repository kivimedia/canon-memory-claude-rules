# Plan Persistence

## The Rule
Every planning session creates a persistent plan file. Plans don't live only in chat - they live on disk.

## Auto-Save Plans
When entering planning mode or designing an implementation approach:
1. Create a plan file at `{project_root}/plans/{DD}-{Mon}-{YY}-{slug}.md`
   - Example: `plans/20-Mar-26-sidebar-reorg.md`
   - If `plans/` dir doesn't exist, create it
2. The plan file contains:
   - **Goal**: What we're trying to achieve (1-2 sentences)
   - **Approach**: The implementation steps
   - **Files to modify**: List with brief description of changes
   - **Verification**: How to confirm it works
   - **Open questions**: Anything unclear
3. Update the plan file as the approach evolves during planning
4. Reference the plan file path when presenting the plan to the user

## Plan File Cleanup
- Plans older than 30 days can be archived (moved to `plans/archive/`)
- Never delete plan files - they're useful for understanding past decisions
- Add `plans/` to .gitignore if the user prefers (ask on first use)
