# 3-Strike Debugging

## The Rule
If a fix fails 3 consecutive times, stop patching and escalate. No 4th attempt without user approval.

## The Protocol
When a fix attempt fails:
- **Strike 1**: Note what was tried and why it failed. Try a different approach.
- **Strike 2**: Note the second failure. Reassess the root cause - is the diagnosis correct?
- **Strike 3**: STOP. Do not attempt another fix.

## On Strike 3 - Escalation
1. Enter planning mode automatically
2. Present a clear summary:
   - What was tried (all 3 attempts)
   - What failed and why (specific errors)
   - What the actual root cause likely is (revised diagnosis)
3. Propose a fresh approach that is fundamentally different from the 3 failed attempts
4. Wait for user approval before proceeding

## Why This Works
Claude tends to make incremental variations of the same failed approach. Forcing a full stop after 3 failures breaks the pattern and forces a rethink. The user gets visibility into what was tried, and the fresh approach avoids the same dead end.
