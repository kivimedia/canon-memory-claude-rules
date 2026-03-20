# What We Considered and Skipped

We evaluated every feature in [runesleo/claude-code-workflow](https://github.com/runesleo/claude-code-workflow) (494 stars, 54KB). Here's what we adopted, what we skipped, and why.

## Our Principle

**Guardrails, not prescriptions.** If Claude already does something well natively, adding a rule for it is noise. Rules should only exist where Claude has a proven failure mode.

---

## Adopted (with modifications)

### Verification Before Completion
- **Their approach**: Evidence-based completion claims with mandatory verification commands
- **Our take**: Adopted and strengthened. This is the single highest-value rule. We added pre-push verification for auto-deploy repos (Vercel, Netlify) because git-status mismatches are a real and common failure mode.
- **Why**: This prevents the #1 failure mode of AI coding - claiming "done" without proof.

### Plan Persistence
- **Their approach**: "Planning with files" skill that creates persistent memory files
- **Our take**: Adopted with a key addition - automatic gap detection after implementation. Their version saves plans but doesn't recheck them. Ours does.
- **Why**: Plans that exist only in chat get compressed or lost. Disk-based plans survive sessions.

### Experience Evolution
- **Their approach**: Observe-only mode with a 4-phase roadmap (currently Phase 1)
- **Our take**: Adopted with a user-triggered "remember this" shortcut. Their version waits for automatic detection. Ours lets users instantly save preferences.
- **Why**: Cross-session learning prevents rediscovering the same gotchas. The trigger phrase makes it frictionless.

---

## Skipped

### 5-Phase Systematic Debugging Protocol
- **Their approach**: Phase 0 (memory recall) -> Phase 1 (root cause investigation) -> Phase 2 (pattern analysis) -> Phase 3 (hypothesis testing) -> Phase 4 (test-first implementation)
- **Our take**: Skipped the protocol. Kept only the escalation mechanism (3-strike rule).
- **Why**: Claude (especially Opus) already debugs well. It naturally investigates root causes, tests hypotheses, and writes tests. The 5-phase protocol adds ceremony without catching more bugs. What Claude does NOT do well is *stop trying the same approach*. The 3-strike rule catches that specific failure mode with zero overhead.

### Session-End Memory Flush
- **Their approach**: Triggers on phrases like "done for today" or "I'm heading out." Runs a 5-step cleanup: record learnings, update task registry, update goals, trim context, git commit.
- **Our take**: Not included.
- **Why**: Fragile trigger detection. "I'm done with this file" could accidentally trigger a full session flush. No explicit `/session-end` command exists as a safety net. No confirmation before executing git commits and memory writes. The risk of accidental activation outweighs the benefit.

### Model Routing Tables
- **Their approach**: Tier system mapping task types to models (Opus for critical logic, Sonnet for daily work, Haiku for queries, Codex for verification, Ollama for offline).
- **Our take**: Not included.
- **Why**: Claude Code already has a model picker. Adding routing guidance in a rules file is advice the model can choose to ignore - it's not enforceable. The user already controls which model to use. Adding a doc that says "use Haiku for simple queries" doesn't change behavior when Claude is running as Opus.

### Specialized Agents (PR-Reviewer, Security-Reviewer, Performance-Analyzer)
- **Their approach**: Three agent templates with specific review scopes and output formats.
- **Our take**: Not included.
- **Why**: These are useful but orthogonal to behavioral rules. They're subagent prompt templates, not guardrails. You can use them alongside our rules if you want, but they solve a different problem (specialized review) than what we target (behavioral discipline).

### Content Safety / Hallucination Detection
- **Their approach**: Source attribution requirements, cross-model verification, conversation re-anchoring after 20 turns.
- **Our take**: Not included.
- **Why**: Claude's built-in safety training handles this. Adding rules that say "don't hallucinate" doesn't prevent hallucination - it just adds words to the system prompt. Our verification rule (show proof before claiming done) is more effective because it catches hallucinated claims at the output level rather than trying to prevent them at the generation level.

### 3-Layer Memory Architecture
- **Their approach**: Layer 0 (always-loaded, ~2K tokens), Layer 1 (on-demand reference, ~1-3K each), Layer 2 (hot working memory with daily logs and task registries).
- **Our take**: Not included.
- **Why**: Claude Code natively provides `CLAUDE.md` (always-loaded), `~/.claude/rules/` (always-loaded), and `~/.claude/memory/` (persistent). This already IS a layered memory system. Adding another naming convention on top (Layer 0/1/2) creates maintenance overhead without functional benefit. The native system works.

### Session Memory Files (today.md, active-tasks.json, goals.md, projects.md)
- **Their approach**: Auto-maintained files that track daily sessions, active tasks, weekly/monthly goals, and project status.
- **Our take**: Not included.
- **Why**: These require ongoing maintenance (pruning, updating, archiving). Claude Code's native CLAUDE.md + memory files already provide cross-session context. Adding more auto-maintained files creates noise. If you need task tracking, use a real project management tool - not markdown files that Claude writes to itself.

### Slash Commands (/debug, /review, /deploy, /exploration)
- **Their approach**: Four structured commands that trigger multi-step workflows.
- **Our take**: Not included (but compatible).
- **Why**: These are useful workflow accelerators but not behavioral rules. You can add them as `~/.claude/commands/` alongside our rules. We focus on the always-active behavioral layer, not triggered workflows.

---

## Summary

| Feature | Them | Us | Verdict |
|---------|------|-----|---------|
| Verification | Yes | Yes (strengthened) | Core value |
| Plan files | Yes | Yes (+ gap detection) | Core value |
| Experience learning | Yes (observe-only) | Yes (+ user trigger) | Core value |
| 3-strike debugging | Part of debugging protocol | Standalone rule | Core value |
| Coding preferences | Not explicit | Yes (new) | Core value |
| 5-phase debugging | Yes | No | Ceremony without benefit |
| Session-end flush | Yes | No | Fragile triggers |
| Model routing | Yes | No | Not enforceable |
| Specialized agents | Yes | No | Orthogonal |
| Content safety | Yes | No | Already built-in |
| 3-layer memory | Yes | No | Native system sufficient |
| Session files | Yes | No | Maintenance overhead |
| Slash commands | Yes | No | Compatible add-on |

**Their framework: 54KB, 25+ files.** Comprehensive but prescriptive.
**Our rules: ~4KB, 6 files.** Focused on failure modes that actually happen.
