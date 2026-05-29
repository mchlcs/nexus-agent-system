# Nexus Agent System

A 7-agent orchestration system for Claude Code, built around a governed constitution and cost-aware model routing.

Each agent has a fixed role, a specific Claude model, and a hard scope boundary. The system runs on top of an Obsidian vault — prompts live as notes, editable like any file, picked up on the next run.

---

## Architecture

```
                    ┌─────────────────────────────────────────┐
                    │              NEXUS (Sonnet)              │
                    │  Entry point. Reads state. Delegates.   │
                    │  Never does the work itself.             │
                    └──────────────┬──────────────────────────┘
                                   │ delegates with done criteria
               ┌───────────────────┼────────────────────┐
               ▼                   ▼                    ▼
        ┌─────────────┐   ┌───────────────┐   ┌──────────────┐
        │ SCOUT (Haiku)│   │ FORGE (Sonnet)│   │ PIXEL (Sonnet│
        │  Research   │   │ Implement     │   │  UI/Design   │
        └──────┬──────┘   └───────┬───────┘   └──────┬───────┘
               │                  │                   │
               └──────────────────▼───────────────────┘
                                  │
                    ┌─────────────▼──────────────────┐
                    │       SHIELD (Opus)             │
                    │  PASS / FAIL with evidence.     │
                    │  Mandatory before production.   │
                    └─────────────┬──────────────────┘
                                  │
                    ┌─────────────▼──────────────────┐
                    │      HERALD (Haiku)             │
                    │  Transforms output for humans.  │
                    └─────────────┬──────────────────┘
                                  │
                    ┌─────────────▼──────────────────┐
                    │      LEDGER (Haiku)             │
                    │  Logs every cycle. No exceptions│
                    │  Terminal — never delegates.    │
                    └────────────────────────────────┘
```

---

## Agent Roster

| Agent  | Role              | Model              | Why this model                                   |
|--------|-------------------|--------------------|--------------------------------------------------|
| Nexus  | Orchestrator      | claude-sonnet-4-6  | Routing needs judgment, not Opus-tier cost       |
| Scout  | Researcher        | claude-haiku-4-5   | Structured search — heavy reasoning not needed   |
| Forge  | Implementer       | claude-sonnet-4-6  | Code quality requires the workhorse tier         |
| Shield | Security / Arch   | claude-opus-4-7    | 10% of tasks, critical decisions only            |
| Pixel  | Visual Designer   | claude-sonnet-4-6  | Design coherence requires reasoning              |
| Herald | Communicator      | claude-haiku-4-5   | Synthesis and writing — cheap, fast              |
| Ledger | Memory / Auditor  | claude-haiku-4-5   | Append-only logging — max speed and economy      |

**Key insight:** Shield (Opus) activates for ~10% of decisions — architecture, security, pre-deploy — while cheap models handle routine work. Expensive model ≠ better outcome for every task.

---

## How to Use

Always start with Nexus. It reads `docs/progress.md`, decides which agent acts, and delegates with a measurable done criterion.

**Standard prompt:**
```
@nexus — [task description]. Context: [file or link].
```

**Example flows:**

| Task type             | Agent sequence                        |
|-----------------------|---------------------------------------|
| New feature           | `forge → shield → ledger`             |
| Research question     | `scout → nexus → herald`              |
| Security review       | `shield → ledger`                     |
| Bug fix               | `forge → shield`                      |
| Architectural change  | `shield → forge`                      |
| End of every cycle    | `ledger` (no exceptions)              |

---

## Constitution

Six principles that govern every agent. No agent can act against them.

1. **Minimal context, maximum quality** — no agent loads more context than needed for its task
2. **Evidence before action** — no change without a measurable done criterion
3. **Drift is debt** — outdated documentation is a bug
4. **Closed scope by default** — each agent does exactly what Nexus delegated, nothing more
5. **Fail early, fail visibly** — silent errors are worse than crashes
6. **The system improves every cycle** — continuous improvement is mandatory, not optional

**Absolute limits:**
- No agent modifies `constitution.md` without explicit human approval + ADR + Shield review
- Shield is mandatory before any production deploy
- Ledger is called at the end of every cycle — no exceptions
- `progress.md` is updated every session — never more than 1 session stale

**Authority hierarchy:**
```
Human (final decision)
└── Nexus (orchestration and delegation)
    ├── Shield (technical veto — can block any action)
    ├── Forge / Pixel / Scout / Herald (execution)
    └── Ledger (memory — read by all, write exclusive)
```

---

## File Structure

```
nexus-agent-system/
├── README.md
├── agents/
│   ├── nexus.md       — orchestrator, entry point
│   ├── scout.md       — researcher, haiku
│   ├── forge.md       — implementer, fullstack
│   ├── shield.md      — security + architecture, opus
│   ├── herald.md      — communicator, synthesis
│   ├── ledger.md      — memory + auditor, terminal
│   └── pixel.md       — visual designer, UI
└── docs/
    ├── constitution.md    — 6 principles, 4 absolute limits
    ├── standards.md       — code quality, output standards
    ├── improve-agent.md   — continuous improvement lifecycle loop
    └── adr/
        └── 0000-template.md   — architecture decision record template
```

---

## Adapting to Your Project

1. **Copy the agents** — each `.md` file is a complete agent prompt. Drop them into your own project folder.
2. **Edit in your editor** — prompts are plain markdown. Change the scope, model, or rules. The next invocation picks up the changes.
3. **Start with `nexus.md` and `ledger.md`** — the orchestrator and memory. Add specialist agents (forge, scout, shield) as you need them.

> The `docs/constitution.md` is the most portable piece. Reading it before building any multi-agent system will save you from the most common failure modes.

---

## Design Principles

- **Prompts live as files, not config** — edit in Obsidian, VS Code, or any editor. No redeployment.
- **Model routing by task type, not by preference** — Haiku for structured work, Sonnet for reasoning, Opus for critical decisions only.
- **Ledger closes every cycle** — the system cannot drift silently.
- **ADRs for every architectural decision** — decisions without context are not traceable.
- **Agents don't expand scope** — Nexus delegates; agents execute exactly that, no more.

---

## Related

- [claude-automations](https://github.com/mchlcs/claude-automations) — 7 slash commands + 7 scheduled routines for Claude Code + Obsidian
