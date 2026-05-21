# Obsidian Knowledge Vault

A personal knowledge vault for Obsidian, designed to work with AI agents
(Claude, Hermes, others) without losing the human owner in the process.

This repo is a setup reference, not a tutorial. See §1 before reading on.

---

## §1 — This is not a tutorial

This document describes how I set up my vault for the AI x Web3 School cohort.
It is not a step-by-step install guide.

Your environment is different from mine:
- Different OS (I use Windows, you might use Mac / Linux / WSL)
- Different AI agent (I use Hermes Agent + Claude; you might use Claude Code, Cursor, Codex, etc.)
- Different existing tooling, different starting point

Rather than try to write a tutorial that covers every case (impossible, and will
go stale within months), this repo is designed for a different workflow:

> Hand this repo to your AI agent. The agent reads
> `AI_ONBOARDING_PROMPT.md`, asks you about your environment and choices,
> then generates a setup plan that fits you.

The README explains what this vault is and the design decisions behind it.
The AI prompt handles the actual installation, adapted to you.

This way:
- You don't get stuck on instructions that don't match your setup
- I don't have to maintain instructions for environments I don't use
- The design choices are explicit and you can disagree with any of them

---

## §2 — Why this exists

Background: I spend a lot of time building agent infrastructure (Harness — an
agent framework; AIP — an agent identity protocol deployed on 0G mainnet).

A pattern I noticed over the past six months: when I work with AI agents
intensively, my own thinking starts to evaporate into the conversation logs.
Three months later I can remember roughly what I decided, but not why —
the reasoning happened in a chat that's no longer in front of me.

When the AI x Web3 School cohort started, I wanted to fix this before
12 weeks of new material made it worse. Specifically:

- Cohort content, agent conversations, and my own builder thinking should
  accumulate in one place
- The system should be portable — not locked to any specific AI vendor
- Agents should help me organize, but not become the source of truth
  for my own reflection

This repo is the result. It's not the "best practice" — it's the design
that fits my use case. The repo exists so other people can see the
decisions and adapt them, not copy them.

---

## §3 — Stack

| Tool | Role | Why |
|---|---|---|
| Obsidian | Editor + graph view | Local markdown, no vendor lock-in |
| Git (private repo) | Version history + portability | Plain files, works with any agent |
| Obsidian Git plugin | Auto commit + push | Saves me from manual git |
| Hermes Agent | Self-hosted agent with filesystem access | Reads/writes vault directly, no MCP needed |
| Your AI agent | Reads operating guide, follows rules | Plug whichever you prefer |

The choice of Obsidian + plain markdown + git is intentional: if any of these
tools disappear, the vault still works. Markdown files don't go away.

---

## §4 — Vault structure

```
vault/
├── sources/                  Raw inputs — clippings, articles, papers
├── inbox/                    AI-generated drafts buffer
│   ├── concepts/
│   ├── decisions/
│   └── questions/
├── concepts/                 Evergreen notes (reviewed, cross-project)
├── projects/                 Project narratives (one folder per project)
└── meta/
    ├── decisions/            Decision log
    ├── reflections/          Personal reflections (see §5 for boundary)
    └── open-questions/       Unresolved questions
```

Two folders behave differently from the rest:

- `inbox/` — buffer for AI-generated content. Used when you (or the agent)
  aren't sure where something belongs yet. Promote to `concepts/`,
  `meta/decisions/`, etc. when the content matures.
- `meta/reflections/` — has special access rules. See §5.

This structure is roughly a hybrid of Evergreen Notes (Matuschak) and
PARA (Forte). It's not the only valid layout — Karpathy's LLM Wiki pattern
is another option, Johnny Decimal is another. The structure matters less
than the discipline around it.

---

## §5 — Operating Guide (the core design)

The vault has an operating guide that the AI agent reads before doing anything.
Full version in `examples/operating-guide.md`. Two design decisions are worth
explaining in detail.

### 5.1 — Reflections folder is mostly off-limits to agents

`meta/reflections/` has three access rules:

- **Never write** — agents do not create, modify, or append files here
- **Never read by default** — agents do not scan or index this folder when
  loading context for a conversation
- **Read only when explicitly asked** — if I say "look at my reflections about
  X" or "given my reflection from June, what do you think", the agent reads
  the relevant file. Otherwise, untouched.

Why this matters: when you talk to an AI agent constantly, your reasoning
gets co-produced with the AI. That's fine for most things — except for
the layer where you reflect on your own choices, values, and direction.
If the agent reads and references your reflections in normal conversation,
your own reflections start mirroring the agent's framing. After six months
of this, you can't tell what's "you" anymore.

So reflections stay AI-untouched by default. It's a small constraint that
costs little, but preserves something that's hard to recover once lost.

### 5.2 — Agents reference past notes as historical positions, not current facts

When the agent quotes vault content back to me, it should say:

> "Your 2026-05-19 decision was X. Does that still apply?"

Not:

> "Your view is X."

The first treats the vault as a record of past decisions. The second treats
the vault as a fixed model of who I am, which it shouldn't be.

This rule matters in brainstorming: I want the agent to actively challenge
what's in the vault, not anchor on it. The vault is grounding (gives the
agent context about my work), not anchoring (it shouldn't lock me into
past positions).

Full guide: [`examples/operating-guide.md`](examples/operating-guide.md)

---

## §6 — Setup outline

Not install instructions. A high-level outline of what needs to happen:

1. Set up an Obsidian vault somewhere on your filesystem
2. Initialize a git repo (private, on GitHub or anywhere else)
3. Install Obsidian Git plugin, configure auto commit-and-sync
4. Write your own version of the operating guide (use
   `examples/operating-guide.md` as starting point — don't copy mine verbatim,
   your context is different)
5. Connect your AI agent to the vault. For filesystem-native agents (Hermes,
   Claude Code), point them at the vault directory. For chat-only agents
   (Claude.ai, ChatGPT), you'll need MCP or manual paste.
6. Test: ask your agent to summarize a conversation into `inbox/`, verify
   it followed the operating guide.

For OS-specific commands, agent-specific configuration, and debugging your
particular environment: hand this repo to your AI agent and follow
`AI_ONBOARDING_PROMPT.md`.

---

## §7 — Pitfalls I hit

Things that wasted my time during setup, in case you hit them too:

- **Windows D-drive ACL permissions**: if you create a folder via File
  Explorer with UAC elevation, the folder becomes owned by Administrator
  and non-admin processes can't write to it. Symptom: Obsidian fails to
  create `.obsidian/`, error "EPERM operation not permitted". Fix: `icacls`
  to grant your user account ownership of the parent directory once,
  then never touch admin shell for vault operations again.

- **Agent stale state after self-update**: my agent (Hermes) auto-updated
  in the middle of a session. The new version reported `Connection error`
  on every API call, even though the underlying provider was healthy and
  my API key was valid (verified by direct curl). Restarting via a known-good
  launch script fixed it. Lesson: when an agent behaves oddly right after
  an update, force a clean process restart before debugging anything else.

- **Provider model list desync**: my agent's UI showed `deepseek-v4-pro` as
  default model, but the provider's actual available model list (returned
  by API) didn't include that ID. Requests failed silently as connection
  errors. Lesson: after major agent or provider upgrades, verify the default
  model actually exists in the current model list before debugging deeper.

- **Decision log discipline is harder than vault setup**: I almost skipped
  writing decision logs because "the AI conversation already covered it".
  Three days later, I had to reconstruct one decision from chat history,
  because nothing was in the vault. The vault is only useful if you actually
  commit decisions to it — the operating guide and folder structure don't
  enforce this, you do.

---

## §8 — If you want to talk

If you're setting up something similar — or you've made different design
choices for the same problems — open an issue. I'm interested in how others
handle the trade-offs in §5 (especially the AI-free baseline), and in
operating guide patterns for multi-agent setups.

---

## Repo layout

```
vault-for-agents/
├── README.md                   This file
├── AI_ONBOARDING_PROMPT.md     Hand to your AI for setup help
└── examples/
    └── operating-guide.md      My actual operating guide (with notes)
```
