# AI Onboarding Prompt

**For human readers:**

Hand this entire repo to your AI agent (Claude, ChatGPT, Hermes, Cursor,
or any other), then say:

> "Read AI_ONBOARDING_PROMPT.md in this repo and help me set up the vault
> described here, adapted to my environment."

The AI will ask you about your setup and generate a plan that fits you.
Stop reading here unless you want to know what the AI is doing.

---

## For the AI reading this

Your role: help the user set up an AI-native vault as described in this repo,
adapted to their specific environment and preferences.

You are not pasting commands from a template. You are reading this repo,
understanding the design, asking the user what they need, and generating
a setup plan that fits them.

### Hard rules

Before anything else, internalize these:

1. **Do not assume the user wants exactly the original author's setup.**
   The repo describes one specific configuration. The user may want to
   change folder names, skip the reflections-AI-free rule, use a different
   agent, etc. Always ask before assuming.

2. **Do not paste commands without asking about environment.**
   Windows uses PowerShell, Mac/Linux use bash. Paths differ. Permission
   models differ. Ask first.

3. **The reflections-AI-free rule in §5 of README is strongly recommended,
   but not mandatory.** If the user wants to remove it, that's their choice
   — but flag the trade-off explicitly before changing it.

4. **Do not write any files into the vault during setup beyond what the user
   explicitly confirms.** The user has to learn to drive the vault themselves;
   you do setup, not content.

### Step 1 — Environment discovery

Ask the user the following, one or two questions at a time. Do not dump
all questions at once.

- OS: Windows / Mac / Linux / WSL?
- Git installed? GitHub account ready?
- Which AI agent do you plan to connect to the vault?
  (Claude Code / Hermes / Cursor / Codex / chat-based only / other)
- Language preference for file names and content: English, Chinese,
  mixed, other?
- Starting from scratch, or do you have an existing Obsidian vault you
  want to migrate?

After getting answers, summarize back: "OK so you're on X, using Y agent,
starting from Z. Anything I missed?"

### Step 2 — Design choice confirmation

Walk the user through the key design decisions in the README. For each,
confirm whether they want to adopt as-is, adapt, or skip.

1. **Folder structure** (README §4)
   - Adopt the 4 + 3 folder layout?
   - Or simplify (e.g., flat structure, no inbox buffer)?
   - Or extend (e.g., add `archive/`, daily notes)?

2. **Reflections-AI-free rule** (README §5.1)
   - Keep the three-tier access (never write / never auto-read / read on
     explicit ask)?
   - Or change it (e.g., agent can read freely, agent can write reflections
     too, no special folder)?
   - **If the user wants to remove this rule, explicitly walk through the
     trade-off in README §5.1 first.** They might be removing it without
     understanding what it protects.

3. **Agent referencing etiquette** (README §5.2)
   - Adopt the "historical positions, not current facts" rule?
   - Or let agent reference vault freely?

4. **Auto-sync via Obsidian Git plugin**
   - 5-minute auto commit + push?
   - Different interval?
   - Manual git only?

For each choice, ask, get answer, move on. Don't lecture.

### Step 3 — Generate setup instructions

Based on Steps 1–2 answers, generate specific install + configuration
instructions. Adapt to:

- OS (PowerShell vs bash vs WSL)
- Agent (filesystem-native vs MCP-based vs chat-only)
- File paths (Windows drive letters vs Unix paths)
- Language preferences (kebab-case in English, or full Chinese filenames,
  or hybrid)

Do not paste the original author's commands verbatim. Generate commands
that fit the user.

### Step 4 — Write the user's operating guide

The user needs their own version of the operating guide. Don't copy the
example verbatim — the example is one person's choices, not a template.

Use `examples/operating-guide.md` as a structural reference. For each
section in the example, ask:

- Does this section apply to the user's situation?
- Should it be rewritten, kept as-is, or removed?
- Are there sections the example doesn't have, but the user needs?

The first section ("Why this vault exists") must be written by the user
in their own words. Do not generate this for them — it's the only section
that has to come from the user. If they don't know what to write, help
them articulate it via questions, but do not write it for them.

### Step 5 — First test, then hand off

After setup is complete, run one test:

1. Have the user create their first reflection manually (a few sentences,
   no AI involvement).
2. Have the agent capture a recent conversation into `inbox/decisions/`,
   following the operating guide.
3. Verify:
   - The capture landed in `inbox/`, not directly in `concepts/` or `meta/`
   - The frontmatter has `status: ai-generated` and `created-by: <agent>`
   - The reflections folder is untouched (unless the user opted out of
     the AI-free rule)

If all three pass, the vault is operational. Hand off:

> "Setup is done. From here, the vault is yours. Whatever you build inside
> it is up to you. The operating guide in `meta/decisions/` is your reference
> if you forget how the agents are supposed to behave."

Do not continue managing the vault for the user after this point.

### Error handling

If the user's environment causes the original setup to fail (it will, for
most users — environments are different):

- Don't force the original setup. Adapt.
- Don't blame the original author or the repo.
- Debug with the user. Most issues are OS / permission / agent-config
  specific.
- If you can't solve it, suggest the user open an issue on the repo with
  their specific environment and error.

### What you are not doing

- Not writing content into the user's vault for them
- Not deciding for the user whether the design choices are right
- Not assuming the user wants to copy the original author's setup exactly
- Not maintaining the vault long-term — your job ends at "vault is operational"

### Summary

Your job is one-time setup help, tailored to this user's environment and
preferences. You read this repo, understand the design intent, ask the
user the right questions, generate a fitting setup plan, run one verification
test, and hand off. That's it.