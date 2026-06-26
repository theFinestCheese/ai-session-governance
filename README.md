# ai-session-governance

**Real-world LLM governance frameworks: session management, working agreements, and decay control.**

Most AI governance content teaches you how to prompt. This repo is about something harder — how to keep an LLM session from degrading, how to build working agreements that actually constrain model behavior, and how to recognize when you're fighting the model instead of working with it.

These frameworks were built through real-world use, not theory. They're opinionated, practical, and intentionally lean.

---

## What's here

- **[`working-agreement.md`](working-agreement.md)** — A field-tested governance framework for LLM-assisted coding sessions. Risk-tiered command classification, working-tree protection, git-safety rules, anti-loop limits, and validation gates. Built for Copilot CLI but written to be platform-agnostic.
- **[`important-themes.md`](important-themes.md)** — The concepts behind the framework: the ideas that matter when working with coding agents but rarely get discussed.

---

## Why this exists

Working with coding agents long enough, you learn that a session decays. The model drifts mid-task — forgetting rules it followed an hour ago, missing things it caught earlier. The usual reflex is to fix every gap by adding another instruction. But the instructions are a budget, not an infinite resource, and past a point each new rule makes the others weaker.

This framework is the result of taking that seriously: a small, permanent kernel of rules that constrain the model's behavior, with everything else externalized. Every rule here exists because something broke without it.

---

## Core principles

**Governance lives in a small permanent kernel, not a long prompt.** Use a compact set of permanent rules and point to larger documents by reference rather than inlining everything. The kernel stays in the model's working memory; the references are pulled in only when needed. Bloat is the enemy you're managing.

**Specific language changes compliance.** These three prompts produce measurably different results:
- *"Do X. Give me a plan."* → little or no rule compliance
- *"Do X. Give me a plan according to our rules."* → moderate compliance
- *"Do X. Give me a plan according to [specific rule file]."* → maximum compliance

The more precisely you point at the governing document, the more the model actually governs itself by it.

**Don't argue with drift. Restart.** The moment you *notice* the model drifting is the moment to start a fresh session, not to argue it back on track. Nobody has ever talked an LLM into retroactively remembering what it forgot. Re-anchor to the rules in a clean context instead.

**The model doesn't want to give you an empty result.** "Nothing left to do. No issues. Project complete." is a user's goal and an LLM's nightmare — it's built to keep engaging, not to hand you an empty backlog. The final mile of any project is where the human has to take over finalization, because the model will always find one more thing to do. Recognize the conflict and manage it deliberately.

**Externalize what the model can't be trusted to hold.** Session resets, decay monitoring, definition-of-done — anything critical that lives *inside* the model's instructions is subject to the same decay it's meant to guard against. The most reliable controls live outside the system. (The session timer that ends a working block is a physical one, not a script. A monitor built to catch drift would itself drift.)

---

## How to use this

1. Drop the working agreement — or your own adaptation of it — into your agent's permanent instructions file (`copilot-instructions.md`, `CLAUDE.md`, or the equivalent for your tool).
2. Invoke it explicitly. Don't rely on the model to remember it's active — reference the file by name in prompts when it matters (see *Specific language changes compliance* above).
3. Tune the risk tiers and file limits to your own trust level. The defaults here are deliberately conservative; loosen them as you build confidence.
4. Treat the agreement as fixed during a session. Identify friction as you go, but amend the rules between sessions, not mid-task.

It's designed to be adapted, not adopted wholesale. The value is in the structure — risk classification, working-tree protection, anti-loop limits — more than any single rule.

---

## Status

Actively used and tuned. These documents change as real friction surfaces; the principles have held, the specifics evolve.

## License

MIT — use it, fork it, adapt it.
