---
date: 2026-05-19
type: operating-guide
status: reviewed
audience: future-self + my-agents
review-checkpoint: 2026-08-19
---

# Vault Operating Guide

> **Note for readers of obsidian-knowledge-vault:**
> This is *my* operating guide as of 2026-05-19. Treat it as a structural
> reference, not a template to copy. Sections marked `[personal context]`
> are specific to me — you'll have your own equivalents. Sections marked
> `[generic pattern]` are likely applicable to most setups but you should
> still adapt the wording.
>
> The first section in particular MUST be written in your own words.
> Don't copy mine.

This document has three readers: future-me, my agents (currently Hermes
and Claude via MCP when connected). Agents must read this before operating
the vault.

---

## 1. Why this vault exists `[personal context — rewrite for yourself]`

This vault is my knowledge body and my trail.

As long as AI tools exist, this content does not disappear — future AI
systems that come in can use what's accumulated here. AI is the tool that
changes; the vault is the spine that doesn't.

AI can help me organize facts and draft material, but reflection and value
judgments are mine. Three years from now I want to see how I actually
thought and how I changed — not an AI-curated version of it.

> **For readers:** Replace this entire section with your own reason. If
> you can't articulate why you're building this in your own words, the
> rest of the guide is built on sand. Don't skip.

---

## 2. Folder structure and responsibilities `[generic pattern — adapt as needed]`

```
vault/
├── sources/                  Raw materials (clippings, papers, articles)
├── inbox/                    Buffer for AI drafts when unsure where they belong
│   ├── concepts/
│   ├── decisions/
│   └── questions/
├── concepts/                 Evergreen notes, reusable across projects
├── projects/                 Project narratives (one folder per project)
└── meta/
    ├── decisions/            Decision log
    ├── reflections/          Personal reflections (boundary rules in §3)
    └── open-questions/       Unresolved questions
```

> **For readers:** You can rename folders, split or merge layers, but the
> distinction between "AI-touchable" and "human-only" (your equivalent
> of reflections/) should be preserved. See §3.

---

## 3. Agent read/write permissions `[generic pattern — keep the spirit, adapt details]`

| Folder | Read | Write |
|---|---|---|
| `sources/` | ✅ default-read | ✅ writable |
| `inbox/` | ✅ default-read | ✅ writable |
| `concepts/` | ✅ default-read | ✅ writable |
| `projects/` | ✅ default-read | ✅ writable |
| `meta/decisions/` | ✅ default-read | ✅ writable |
| `meta/open-questions/` | ✅ default-read | ✅ writable |
| `meta/reflections/` | 🔒 read only on explicit request | ❌ never write |

### Reflections-specific hard rules

- **Never write**: agents do not create, modify, or append files in
  `meta/reflections/` under any circumstance.
- **Never auto-read**: agents do not scan or index this folder when loading
  default context for a conversation.
- **Read on explicit request only**: when the user says things like
  "look at my reflections about X", "given my June reflection",
  "did I write about Y before?" — the agent may read the relevant file.
- **Cite explicitly**: when reading reflections content, the agent must
  cite the source file in its response, not blend the content into
  generic advice.

> **For readers:** This rule is the most important design in the whole
> guide. If you remove it, please first read README §5.1 in this repo
> and understand what you're giving up. Then decide.

---

## 4. Frontmatter convention `[generic pattern — adopt as-is or change keys]`

**For files an agent writes**, frontmatter must include:

```yaml
---
date: YYYY-MM-DD
status: ai-generated
created-by: <agent-name>          # e.g. hermes, claude
source-session: <session-id>      # so it's traceable to the conversation
---
```

**For files written or edited by you**:

```yaml
---
date: YYYY-MM-DD
status: reviewed
---
```

Both statuses can coexist in the same folder. The frontmatter is what
tells you (and future you) what was AI-generated vs. human-reviewed.
You can audit this any time.

---

## 5. Write triggers `[generic pattern — adapt heuristics]`

Two trigger types:

**Explicit triggers** — agent writes when the user says:
- "summarize into inbox"
- "write into concepts"
- "capture this conversation"
- "session done"

**Heuristic triggers** — agent should *ask* (not write) when the
conversation shows:
- Decision keywords ("I'm going with...", "let's choose...", "the
  direction is...")
- An insight surfaces ("oh that's actually...")
- An unresolved question is raised but not answered
- The conversation has been long (~10+ turns) and converging on a topic

**Boundary**: the agent decides "is this worth asking the user about?"
but does NOT decide "is this important enough to write?" — the user
decides the latter.

Casual chat, technical Q&A, and conversations without decision character
should NOT be captured.

---

## 6. How agents should reference vault content `[generic pattern — keep the spirit]`

**✅ Reference past notes as historical positions:**

> "Your 2026-05-19 decision says X. Does that still apply?"
> "`concepts/state-persistence.md` from August says Y. Same context here?"

**❌ Don't treat past notes as current facts:**

> "Your view is X."
> "You believe Y."

**✅ Challenge vault content during brainstorming:**

> "Your vault references state-persistence only in the Harness context.
> If you step outside Harness, what's not covered yet?"

**Core principle**: the agent reads the vault for **grounding** (factual
context about my work), not **anchoring** (locking me to past positions).
Grounded but not anchored.

---

## 7. Review checkpoint `[generic pattern — set your own date]`

`[personal context: my checkpoint is 2026-08-19, three months after
vault setup. Yours should be 2-3 months after your start date.]`

At review time, ask:
- Does the graph have actual density (concepts cross-linked, not just
  isolated nodes)?
- Can the time axis (decisions + reflections) tell a coherent story I
  can read back?
- Are the agents following this guide in practice? Which rules turn out
  to be useless, which ones are insufficient?
- Has the reflections folder accumulated, or is it sitting empty?

**If not meeting expectations**: change the guide, change the workflow.
Don't try to change your writing habits to fit the guide.

---

## 8. How this guide evolves `[generic pattern]`

- Changes to this guide are themselves decisions → log them in
  `meta/decisions/` and link back here
- Don't overwrite history of this file; git keeps versions
- If major revision after a checkpoint, version the file
  (`operating-guide-v2.md`, keep v1 as archive)