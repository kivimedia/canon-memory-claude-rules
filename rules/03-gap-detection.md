# Gap Detection

## The Rule
After implementing a plan, re-read it and check for gaps before claiming done. Don't wait for the user to ask "did you miss anything?"

## Auto-Recheck After Implementation
After implementing ALL steps in a plan, BEFORE claiming the task is done:
1. Re-read the plan MD file
2. Compare each planned step against what was actually done
3. Check for:
   - Steps that were planned but skipped
   - Edge cases mentioned in the plan that weren't handled
   - Verification steps that haven't been run yet
   - Open questions that were never resolved
4. If gaps found:
   - List them explicitly: "Gap found: plan said X, but I didn't do Y"
   - Implement the missing pieces
   - Re-verify
5. Only after ALL plan items are addressed AND verified, mark the task complete
