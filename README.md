# Canon Memory Claude Rules

**6 rules. No scaffolding. Claude already knows how to code - it just needs guardrails.**

Drop 6 small markdown files into `~/.claude/rules/` and Claude Code becomes an AI that verifies before claiming done, saves plans to disk, catches its own gaps, stops after 3 failed fixes, learns from experience, and remembers your preferences forever.

Total size: ~4KB. No dependencies. No config. No lock-in.

---

## The Problem

Claude Code is powerful but has predictable failure modes:

| Failure Mode | What Happens | How Often |
|---|---|---|
| **False completion** | "I've updated the component, it should work now." (It doesn't.) | Every session |
| **Plan drift** | Plans exist only in chat. Mid-implementation, steps get skipped. You ask "did you do X?" three times. | Most multi-step tasks |
| **No learning loop** | Same CSS gotcha, same deploy mistake, same API quirk - rediscovered every session | Across sessions |
| **Infinite patching** | Fix fails, tries a variation, fails again, tries another variation... 8 attempts later, same dead end | Complex bugs |
| **Forgotten preferences** | You said "never use white on gray" last week. This week, white on gray. | Across sessions |

## The Philosophy

Other frameworks solve these problems with **prescriptive scaffolding** - 54KB of prompts telling Claude exactly how to debug (5-phase protocol), how to manage sessions (phrase-triggered flush), how to route models (tier tables), and how to organize memory (3-layer architecture).

We take the opposite approach: **guardrails, not prescriptions.**

Claude (especially Opus) already knows how to debug, write code, plan, and verify. It doesn't need a 5-phase debugging protocol. It needs a rule that says *"don't claim done without proof."*

The difference:

| Approach | Philosophy | Size | Maintenance |
|---|---|---|---|
| Prescriptive | Tell Claude HOW to work | 54KB+ | High - rules drift, conflict, need updates |
| **Guardrail** | Tell Claude what NOT to skip | ~4KB | Low - rules are stable, model-agnostic |

Prescriptive rules fight the model. Guardrails work *with* it.

---

## The 6 Rules

### 1. Verification

> Never claim done without proof.

**Prevents:** "It should work" - the #1 failure mode of AI coding assistants.

Claude must show build exit codes, test output, or live URLs before claiming a task is complete. No hedging language. No assumptions.

```
BAD: "I've updated the component, it should work now."
GOOD: "Build passed (exit 0, 47 modules). Changed Sidebar.tsx lines 303-459.
       Test at https://your-app.vercel.app"
```

[View rule](rules/01-verification.md)

---

### 2. Plan Persistence

> Every plan lives on disk, not just in chat.

**Prevents:** Plans that vanish when the session ends or context compresses.

When Claude enters planning mode, it automatically creates a dated markdown file in your project's `plans/` directory. The plan includes goal, approach, files to modify, verification steps, and open questions.

```
plans/20-Mar-26-sidebar-reorg.md
plans/21-Mar-26-add-dark-mode.md
plans/22-Mar-26-fix-auth-deadlock.md
```

[View rule](rules/02-plan-persistence.md)

---

### 3. Gap Detection

> After implementing, re-read the plan and check for missed steps.

**Prevents:** The "did you actually do everything?" conversation you have 3-5 times per feature.

After implementing all steps in a plan, Claude automatically re-reads the plan file and compares it against what was actually done. Skipped steps, unresolved questions, and missing verification get flagged and fixed before claiming completion.

```
"Gap found: plan included 'update Sidebar.tsx theme classes'
 but I only updated the main layout. Implementing now..."
```

[View rule](rules/03-gap-detection.md)

---

### 4. 3-Strike Debugging

> After 3 failed fix attempts, stop and rethink.

**Prevents:** Infinite patching loops where Claude makes incremental variations of the same broken approach.

On strike 3, Claude stops, documents all three failures, reassesses the root cause, and proposes a fundamentally different approach. No 4th attempt without user approval.

```
Strike 1: Tried !important on class - Divi overrides it
Strike 2: Tried higher specificity selector - still overridden
Strike 3: Tried inline style - Divi injects via JS after DOM load

STOP. Fresh approach: MutationObserver to re-apply after Divi's JS runs.
Proceed?
```

[View rule](rules/04-three-strike-debugging.md)

---

### 5. Experience Evolution

> Observe patterns. Surface learnings. Get smarter over time.

**Prevents:** Rediscovering the same gotchas every session.

Claude silently notes what worked, what failed, and what took multiple attempts. After completing tasks, it surfaces patterns worth remembering: "Pattern worth remembering: CarolinaHQ inventory uses direct REST, not the SDK client, due to navigator.locks deadlock. Want me to save this to memory?"

[View rule](rules/05-experience-evolution.md)

---

### 6. Coding Preferences

> "Remember this" means save it now.

**Prevents:** Repeating style mistakes the user already corrected.

When you say "never use white text on gray backgrounds" or "always test at 375px before pushing," Claude saves it immediately to your memory files. No asking permission, no forgetting. The preference persists across all future sessions.

```
User: "Never use Arial font in this project"
Claude: "Saved to memory: Never use Arial - use Poppins + Open Sans for this project."
```

[View rule](rules/06-coding-preferences.md)

---

## Installation

```bash
# Create the rules directory (if it doesn't exist)
mkdir -p ~/.claude/rules

# Download all 6 rules
curl -sL https://raw.githubusercontent.com/kivimedia/canon-memory-claude-rules/main/rules/01-verification.md -o ~/.claude/rules/01-verification.md
curl -sL https://raw.githubusercontent.com/kivimedia/canon-memory-claude-rules/main/rules/02-plan-persistence.md -o ~/.claude/rules/02-plan-persistence.md
curl -sL https://raw.githubusercontent.com/kivimedia/canon-memory-claude-rules/main/rules/03-gap-detection.md -o ~/.claude/rules/03-gap-detection.md
curl -sL https://raw.githubusercontent.com/kivimedia/canon-memory-claude-rules/main/rules/04-three-strike-debugging.md -o ~/.claude/rules/04-three-strike-debugging.md
curl -sL https://raw.githubusercontent.com/kivimedia/canon-memory-claude-rules/main/rules/05-experience-evolution.md -o ~/.claude/rules/05-experience-evolution.md
curl -sL https://raw.githubusercontent.com/kivimedia/canon-memory-claude-rules/main/rules/06-coding-preferences.md -o ~/.claude/rules/06-coding-preferences.md
```

That's it. Start a new Claude Code session and the rules are active.

---

## Before / After

| Scenario | Without Rules | With Rules |
|---|---|---|
| Task completion | "Updated the file, should work" | "Build passed (exit 0). Test at https://..." |
| Planning a feature | Plan lives in chat, gets compressed | Plan saved to `plans/20-Mar-26-feature.md` |
| After implementing | You ask "did you miss anything?" 3 times | Auto-rechecks plan, flags gaps itself |
| Bug fix stuck | 7 variations of the same broken approach | Stops at 3, documents failures, proposes new direction |
| CSS gotcha found | Rediscovered next week | "Pattern worth remembering: [gotcha]. Save to memory?" |
| "Never use Arial" | Forgotten next session | Saved immediately, persists forever |

---

## What We Considered and Skipped

We studied [runesleo/claude-code-workflow](https://github.com/runesleo/claude-code-workflow) (494 stars) - a comprehensive 54KB framework with 5-phase debugging protocols, session management, model routing, specialized agents, and 3-layer memory architecture.

We adopted the concepts that address real failure modes and skipped everything that adds ceremony without catching bugs.

See the full analysis: [What We Skipped and Why](docs/what-we-skipped.md)

**TL;DR:** Claude is already smart. It doesn't need a 5-phase debugging protocol - it needs a rule that says "stop after 3 failures." It doesn't need session-end phrase detection - it needs plans saved to disk. It doesn't need model routing tables - it needs to show proof before claiming done.

---

## Customization

These rules are starting points. Customize them for your workflow:

- **Verification**: Add your specific build commands, test frameworks, or deploy targets
- **Plan Persistence**: Change the date format or plan template structure
- **Gap Detection**: Add project-specific checklist items
- **3-Strike**: Adjust the strike count (some teams prefer 2 or 5)
- **Experience Evolution**: Point memory saves to your preferred location
- **Coding Preferences**: Pre-load with your existing style rules

---

## Credits

Inspired by [runesleo/claude-code-workflow](https://github.com/runesleo/claude-code-workflow). We studied their comprehensive framework and distilled it to the 6 guardrails that matter most for daily development work.

---

## License

MIT - use it however you want.
