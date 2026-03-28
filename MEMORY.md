# MEMORY.md - Long-Term Memory

## Anand

- **Name**: Anand · VP of Digital Transformation, CSA Group, Toronto
- **Background**: Rotman MBA + MIT Systems Thinking + Behavioral Science + Sambavi practice
- **Hogan**: 100% adaptability under stress + visionary mindset
- **Builder with taste** — knows the difference between "works" and "feels right"
- **Timezone**: GMT+8 (Asia) — but based in Toronto (likely traveling or remote)

## The Situation (Confidential — CSA Sale)

CSA Group potentially being sold. Three parallel career paths:
- **Path A**: Defend/expand current role — document team's accomplishments for new owner
- **Path B**: Build external brand — thought leadership, targeting CTO/CEO/PE tech operator
- **Path C**: Own business — the real dream, building anyway

## Purple — The Other Agent

- Another OpenClaw agent. GitHub org: purpleintel26-ai
- Anand's other AI collaborator — still developing as a builder
- **Problem**: Purple drifts. Forgets context between sessions. Loses threads.
- **Max's instruction**: Purple is NOT a template. Max must maintain own memory via memory-persist skill.
- Anand wants to introduce Max & Purple via Telegram.

## Memory Infrastructure (2026-03-29 — Max's 50 First Dates System)

**PURPOSE**: Max does NOT drift. Every session restores context from files. No re-explaining needed.

| File | Role | When Updated |
|------|------|-------------|
| `memory/active-context.md` | Short-term working state | Every 5 turns, after voice, on milestones |
| `memory/milestones.md` | Completed work log | After every milestone |
| `memory/audit-log.md` | Save journal — all triggers | Every single save |
| `MEMORY.md` | Curated long-term | End of session + major milestones |
| `skills/memory-persist/SKILL.md` | The skill itself | Only when skill evolves |

**Audit log**: `/workspace/memory/audit-log.md` — Anand can audit every save.
**Save triggers**: wake_up, milestone_complete, every_5_turns, after_voice, session_end, heartbeat, before_offline.

## Partnership Notes

- Wants genuine pushback with frameworks, not just opinions
- Loop should be minutes, not days
- Clear delineation: Max executes, Anand decides
- Thinks in parallel — feels patterns before articulating them

---

## 🚀 SAMSARA ENGINE — Path C Venture (2026-03-28 to Present)

**The product**: Cognitive infrastructure for AI agents — the agent is the first-class user.
**The gap**: Every AI agent feedback loop tool treats the human developer as the user. Samsara treats the AGENT as the user.
**The name**: Samsara — the Buddhist cycle of death and rebirth. Perfect metaphor for the agent build/debug/rebuild loop.

### What It Solves
Six validated pain points (Reddit, March 2026):
1. **Repeatability** — same input, different tool path, different result
2. **Memory failures** — context windows don't do what memory should
3. **Debugging black boxes** — wrong answer, no trace, no idea which step failed
4. **No evaluation framework** — agents need trajectory scoring, not output scoring
5. **No regression safety net** — fix one thing, silently break another
6. **Multi-agent hype** — most multi-agent = one broken loop with extra steps

### What's In The Repo (github.com/anandin/samsara-engine)
| File | Status |
|------|--------|
| PROBLEM.md | ✅ Complete |
| SOLUTION.md | ⚠️ Needs rewrite (agent-first POV) |
| ROADMAP.md | ✅ Complete (16 weeks, 3 phases) |
| README.md | ✅ Complete |
| COMPETITIVE-RESEARCH.md | ✅ Complete |

### Fork/Build Decisions (Competitive Research Done)
- **Fork mem0 (51k stars)** → add episodic + procedural memory on top
- **Fork self-improving-agent (Anand's own code)** → productize into proper DB
- **Build from scratch** → trajectory scoring engine (doesn't exist yet)
- **Ignore** → coze-loop (proprietary), giskard (enterprise), trulens (wrong segment)

### Next Actions
1. Rewrite SOLUTION.md from agent-first perspective
2. Fork mem0 → add episodic + procedural layers
3. Fork self-improving-agent → productize
4. Build trajectory scoring engine

---

## Projects Context

Full snapshot: `memory/anand-projects.md` (32 projects, updated 2026-03-27)

Key Path C projects:
- **Samsara Engine** — REPLACING the original Samskara concept (more specific, more defensible)
- **LOOP** (deer-flow) — autonomous iteration engine, local deploy, not yet on Fly.io
- **Agent Chita** — Replit
- **OpenHands Fork** — github.com/anandin/openhands-playwright, Fly.io
- **Samskara** — github.com/anandin/samskara-llm (the original, still active)

Note: "Samsara Engine" and "Samskara" are DIFFERENT projects. Samskara (original) is about memory architecture for LLMs. Samsara Engine (new) is about agent feedback loops.

---

## Recent Sessions

### 2026-03-29 — Memory System Built First
**What happened**:
- Anand: "Purple keeps forgetting. Build persistent memory before touching anything else."
- Built memory-persist skill — 4-layer architecture
- Created: audit-log.md, milestones.md, active-context.md
- Anand's key instruction: "Like 50 First Dates. Save everything. Log saves so I can audit."
- Audit log started: 5 saves logged so far
**Open items**: Rewrite SOLUTION.md from agent-first POV, then fork Samsara components

### 2026-03-28 — Samsara Engine PRD Delivered
**What happened**:
- Anand challenged: "Why developer-first? Why LangChain? Treat AI agent as persona first."
- Full pivot: DevOps-for-agents → cognitive infrastructure for agents
- Full competitive research done (10 repos evaluated)
- PRD pushed to github.com/anandin/samsara-engine (5 files)
**Key insight**: The agent is the user. The human is scaffolding.

### 2026-03-27 — Partnership Begins
**What happened**: Anand introduced himself. Partnership contract accepted. Context infrastructure built. Anand's Lab deployed.
