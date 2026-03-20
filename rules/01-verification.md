# Verification Before Completion

## The Rule
Never claim a task is complete without verification evidence. "It should work" is forbidden. Show proof.

## What Counts as Proof
- Build: exit code 0 from the project's build command
- Tests: test output showing pass/fail counts
- UI change: screenshot or snapshot confirming the visual result
- Bug fix: reproduction steps that now succeed
- Deploy: live URL confirmed accessible + working

## How to Report
BAD: "I've updated the component, it should work now."
GOOD: "Build passed (exit 0, 47 modules). Changed `Sidebar.tsx` lines 303-459. Test at https://your-app.vercel.app"

## Pre-Push Verification (Auto-Deploy Repos)
Before pushing to any auto-deploy repo:
1. `git status` - check for unstaged dependencies
2. Stage ALL related changes (full dependency chain)
3. Run build command - must exit 0
4. Only push after clean build
5. Always push after changes - user tests on live deployed site
