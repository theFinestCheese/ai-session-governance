# Important Themes in LLM Work

The concepts that matter when working with coding agents but rarely get discussed. These are the ideas behind the [working agreement](working-agreement.md) — the *why* underneath the rules.

---

## Rules vs. prompts

Establishing permanent rules (e.g. `copilot-instructions.md`, `CLAUDE.md`) is a different tool than prompting. Rules control not just how a given prompt is handled but how the whole session is managed. A prompt is a request; a rule is a constraint that persists. Most people only ever use the first and wonder why behavior is inconsistent.

## Establishing governance

Use a small kernel of permanent rules to stay inside the model's working-memory limits. Within that kernel, use pointers to reference larger files and documents rather than inlining everything — that's the art of it. The kernel and its references define the boundaries you want and how the model should respond to your prompts. Keep the kernel small; every line you add to it competes with every other line for the model's attention.

## Define risk externally

Establish a list of actions and assign them levels of risk. Give the model different governance and approval requirements for different risk levels. Have it explicitly flag risky actions and present a plan for your approval before executing them — destructive or irreversible actions should never be a surprise.

## Drive for conversation

Put decision-making in the user's hands in every case. The model presents a plan — and where useful, alternatives — and asks for approval. Establish this in your permanent rules, then adjust for your own communication and prompting style. The goal is a partner that proposes, not one that acts unilaterally.

## Externalize resume/restore

Most coding agents have built-in resume/restore functionality to fight cross-session decay. You don't have to rely on it. Building and externalizing your own gives you personalization and control the built-in version can't — and it lives outside the system that's decaying, which matters more than it sounds.

## Specific language matters

The user might not notice much difference between these prompts, but the results differ significantly in terms of governance:

- *"Do X. Give me a plan."* → little or no rule compliance
- *"Do X. Give me a plan according to our rules."* → moderate compliance
- *"Do X. Give me a plan according to [specific rule file or document]."* → maximum compliance

The more precisely you point at the governing document, the more the model actually governs itself by it. Vague references get vague adherence.

## Order of operations

LLMs are language-driven, but underneath they're still computational. Despite the conversational surface, the order of instructions in a prompt or rule set can matter — especially for rules you want to persist across sessions. *When* you ask for something can matter as much as what you asked for, particularly in relation to when you asked for something else. Sophisticated design needs sophisticated prompting, and the denser a prompt gets with instructions, the more their order matters — which is easy to lose track of under the conversational flow.

## The decay spiral

Sessions degrade. The trouble is that the moment the user *notices* drift is the moment the spiral accelerates — because the natural response is to start asking the model complicated diagnostic and recall questions, which only consumes more of the context that was already failing.

**Don't argue. Restart.** Nobody has ever argued a model into retroactively increasing its memory. Re-anchor to the rules in a clean session instead of trying to recover a degraded one.

## Managing session degradation

Practical controls that go into permanent instructions:

- Explicit anti-looping behavior (same failure twice → stop)
- Model-switching rules
- Session-health awareness
- A hard stop on session length

## The session timer is physical, not software

I originally tried to build tooling to detect session decay and trigger a reset automatically. I abandoned it. A monitor built into the model's rules is itself subject to the decay it's meant to catch — it becomes a meta-process vulnerable to the same spiral it's observing, and worse, the more important you make it, the more attention it pulls from the actual work. The fix was to take the monitoring out of the system entirely: a physical timer on the desk. When it goes off, the session ends. No code, no meta-process, nothing for the model to forget. (Started at 90 minutes. Now closer to 45. The right interval depends on the intensity of the work and the model in use.)

## The final-mile struggle

An LLM will get you to 80–90% with little effort. Past that, the user spends exponentially more effort for worse and worse returns.

Part of why: in a project's final mile, the user wants *less* of whatever they're chasing — fewer open bugs, fewer TODOs, a smaller backlog — heading toward completion, which is effectively an empty state. But an LLM, by design, doesn't want to give you an empty result; it wants to engage. "Nothing left to do. No issues. Project complete." is a user's goal and a model's nightmare.

You don't get past this by prompting harder. You get past it by recognizing the conflict and taking over finalization yourself — using the model as a tool, but making the decisions and the definition of done your own.

## Cardinal vs. conversational

Human-to-LLM interaction has been tuned to feel conversational, and I lean into that. But it's worth knowing when to switch to cardinality and explicit order of operations — most obviously in list-formatted instructions, but it applies to plain conversational prompting too. Knowing which register to use, and when, is part of the craft.
