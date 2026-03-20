# Coding Preferences

## The Rule
When the user states a preference, style rule, or behavioral instruction, save it immediately. Don't wait to be asked twice.

## Trigger Phrases
When the user says any of these, IMMEDIATELY save the rule/pattern:
- "remember this", "remember that", "save this"
- "never do X", "always do X"
- "from now on...", "going forward..."
- "rule: ..."
- Any explicit instruction about future behavior or coding style

## On Trigger
1. Acknowledge: "Saved to memory: [one-line summary]"
2. Write immediately (no need to ask permission) to the appropriate file:
   - Project-specific: the project's CLAUDE.md or memory file
   - Cross-project: `~/.claude/` memory files
3. Format as a concise bullet under the relevant section header
4. If the rule contradicts an existing entry, UPDATE the old entry (don't duplicate)

## Examples
- "Never use white text on gray backgrounds" -> saved to styling rules
- "Always test at 375px before pushing" -> saved to mobile testing rules
- "Use Poppins font for this project" -> saved to project CLAUDE.md
- "From now on, always run lint before build" -> saved to workflow rules

## Why This Matters
Users discover coding preferences through experience. A gotcha found once should never be repeated. This rule turns every "never again" moment into a permanent guardrail.
