# 🧠 Memory Persist Skill
## The 50 First Dates Memory System for Max

---

## The Problem

OpenClaw agents lose context between sessions. There's no continuous memory — only what was in the last session's working context. This is acceptable for simple tasks. It is catastrophic for long-running collaborative projects where the work spans days, dozens of conversations, and complex multi-step builds.

Anand's agent Purple drifts. She forgets where a project left off. She restarts without knowing what was decided. She loses the thread.

**Max will not drift.**

---

## The Design Principle

The human brain converts short-term memory to long-term memory during sleep — roughly every 90 minutes, and intensively during REM cycles. This skill implements the same principle for an AI agent:

> **Short-term memory (active context) → Long-term memory (milestone log + MEMORY.md) — triggered by events, not just time.**

The system must be:
- **Frequent**: Saves happen on events, not just at session end
- **Auditable**: Every save is logged with metadata — Anand can inspect the audit trail
- **Recoverable**: Any session can restore full context from the milestone log
- **Deliberate**: The skill does not just passively log — it actively reflects on "what happened since last save"
- **Lightweight**: Never burn tokens on memory operations — file I/O only, no LLM calls in the save path

---

## Core Files

| File | Purpose | Type |
|------|---------|------|
| `memory/active-context.md` | Current working state — what I'm doing right now | Short-term |
| `memory/milestones.md` | Completed chunks — what got finished, with dates | Long-term |
| `memory/audit-log.md` | Every save event — timestamps, triggers, token count | Audit |
| `memory/working-notes.md` | Scratch space for current task — ephemeral | Working |

---

## Memory Layers

### Layer 1: Working Notes (Session-Per-Task)
File: `memory/working-notes.md`
- Created at start of each new task
- Contains: current task, plan, questions to resolve, code being written
- Overwritten on each new task — ephemeral
- Never saved to long-term unless a milestone is declared

### Layer 2: Active Context (Short-Term →滚动)
File: `memory/active-context.md`
- What I'm currently working on
- What was just decided (last 3-5 exchanges)
- What's unresolved and needs attention
- What was the last milestone completed
- Updated EVERY 5 conversational turns, not just at session end

### Layer 3: Milestones (Long-Term)
File: `memory/milestones.md`
- What was completed, when, linking to project/code/doc
- Each milestone: what it is, why it matters, what's next
- Survives session resets — the permanent record of work done
- Never deleted — only appended

### Layer 4: MEMORY.md (Curated Long-Term)
File: `MEMORY.md`
- The curated record — updated from milestones, not overwritten
- Contains: Anand's context, project status, key decisions, open questions
- Updated at end of each session and after major milestones
- Reviewed weekly during heartbeats

---

## Save Triggers

The skill activates on ALL of these:

| Trigger | When | Layer Saved |
|---------|------|-------------|
| `milestone_complete` | After any chunk finishes | Active context → milestones |
| `every_5_turns` | Every 5 exchanges | Working notes → active context |
| `after_voice` | After any TTS voice message | Active context update |
| `heartbeat` | Every 30 min if active work | Active context refresh |
| `session_end` | When session closes | Full consolidation |
| `anand_kickoff` | Anand starts something new | Active context checkpoint |
| `anand_decision` | Anand makes a key call | Decision log in active context |
| `anand_feedback` | Anand corrects or redirects | Lesson captured |
| `before_offline` | Before any long gap | Full long-term write |
| `wake_up` | Session starts | Read + restore |

---

## Skill Actions

### `memory_persist`
The main action. Run this:
- After every milestone_complete
- After every 5 conversational turns
- After any voice message
- On heartbeat if work is active

```
Action: memory_persist
Input:
  trigger: "milestone_complete" | "every_5_turns" | "after_voice" | "heartbeat" | "session_end" | "wake_up"
  current_task: string
  what_happened: string (1-3 sentences)
  milestone_reached: string (optional)
  decisions_made: string[] (optional)
  open_items: string[] (optional)
  next_step: string
```

What it does internally:
1. Read existing active-context.md
2. Merge new input
3. Write active-context.md
4. If milestone_reached: append to milestones.md
5. Append to audit-log.md with trigger + timestamp
6. If session_end: update MEMORY.md from milestones.md

### `memory_restore`
Run at session start to get context before anything else.
```
Action: memory_restore
Output: active_context, last_milestone, recent_decisions, open_items, last_session_summary
```

### `memory_audit`
Run on demand to review save frequency and pattern.
```
Action: memory_audit
Output: save_count_by_trigger, last_30_saves, average_turns_between_saves, any_gaps_
```

### `memory_snapshot`
Full state capture — used before major sessions or builds.
```
Action: memory_snapshot
Input: project_name, phase, what's_ready, what_matters_next
```

---

## Audit Log Format

Every save writes ONE line to `audit-log.md`:

```
## [YYYY-MM-DD HH:MM GMT+8] SAVE #{n}
**Trigger**: {trigger}
**What**: {one-line summary}
**Milestone**: {yes/no}
**Context**: {1-line state description}
**Turn**: {conversation turn count at time of save}
```

Example:
```
## [2026-03-29 07:45 GMT+8] SAVE #14
**Trigger**: milestone_complete
**What**: Built memory-persist skill — full 4-layer system delivered
**Milestone**: ✅ YES
**Context**: Skill delivered, pushed to GitHub, Anand approved
**Turn**: 8
```

---

## Session Start Protocol

At the START of every session, before responding to Anand:

1. Run `memory_restore`
2. Read `memory/milestones.md` (last 5 entries)
3. Read `memory/active-context.md`
4. **Do not ask Anand to re-explain** what was already captured
5. If context is stale (>24h old): acknowledge gap, ask what needs re-establishing
6. Then greet Anand and state what you're resuming

---

## The "Sleep" Metaphor

| Human Brain | Max Memory System |
|------------|-----------------|
| REM sleep (every 90 min) | Every 5 turns + after voice |
| Deep sleep consolidation | Milestone save |
| Waking up with memory intact | Session start + restore |
| Forgetting dreams (unimportant) | Working notes overwritten |
| Remembering lessons (important) | Milestones + MEMORY.md |
| Sleep debt accumulates | Gap detection in audit log |

---

## Implementation Notes

- **No LLM calls in the save path** — file I/O only, always fast
- **Never overwrite milestones** — append only, never delete
- **Never lose a save** — write to active-context first, then move to milestones
- **Audit trail is sacred** — every save logged, no exceptions
- **Token-efficient** — the skill itself should never burn >100 tokens per invocation
- **Separate files** — audit log is separate from milestone log — different audiences

---

## Skill Metadata

- **Name**: memory-persist
- **Version**: 1.0.0
- **Author**: Max (built for Anand)
- **License**: MIT
- **Purpose**: Combat short-term memory loss between sessions
- **Dependencies**: None (pure file I/O)
- **Trigger**: Always load on session start; call memory_persist after milestones
