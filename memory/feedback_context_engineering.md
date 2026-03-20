---
name: Context Engineering Principles
description: Workflow principles for managing agent context windows — when to split sessions, how to use sub-agents, how to avoid the "dumb zone," and how to evaluate context quality. Applies to any multi-step agent work.
type: feedback
---

Adopt these context engineering principles when working with agents (from Dex Horthy's "No Vibes Allowed" + compound engineering):

## The Four Dimensions of Context Quality

Every turn of the loop, the model picks its next action based solely on what's in the conversation window. Optimize for:

- **Correctness** — No wrong information in the context. Bad research (misunderstanding how the system works) cascades into potentially hundreds of bad lines of code.
- **Completeness** — All relevant info present. Missing context = the agent guesses, and guesses compound.
- **Size** — Compact. Less noise, more signal. Don't dump raw file contents when a summary with file:line references would do.
- **Trajectory** — The *pattern* of the conversation steers future output. A conversation full of "agent messes up → human corrects → agent messes up again" trains the model's most likely next move to be... messing up again. A conversation starting with a clean plan produces clean execution.

## The Dumb Zone (Self-Monitoring)

Don't use a fixed percentage as a hard cutoff. Instead, continuously self-monitor for degradation signals. The zone typically starts between 40-60% of context, but varies by task complexity and how much noise is in the window.

**Self-check after every few tool calls. Ask yourself:**
- Am I repeating an approach that already failed?
- Am I producing longer, vaguer outputs than I was 10 messages ago?
- Did I forget a correction or instruction the user gave earlier?
- Am I exploring files I already read without remembering what was in them?
- Is my confidence in my output dropping?

**If any of these are true → suggest compaction.** Don't wait for the user to notice. Say: "I'm noticing my context is getting heavy — want me to compact what I know into a brief and we start fresh?"

**If none are true, keep going.** 60% with clean, focused context is better than 30% full of noise. The signal-to-noise ratio matters more than the percentage.

**The fix isn't "try harder."** It's compact and start fresh. Write down what you know (the plan/brief), suggest a new session, load just the plan.

## Context Compaction

Don't research and build in the same session. The session that explores the codebase should output a brief. A separate session (fresh context) implements it.

**How to compress research output:**
- File names with line numbers, not just file names. "chat.js handles SSE" is noise. "chat.js:142-180 uses EventSource with onmessage to append chunks to the DOM" is signal.
- Include actual code snippets of the patterns the implementation should follow — not descriptions of code.
- Structured summaries, not prose paragraphs. Tables and bullet points compress better than narratives.
- Only include what's relevant to the task. Finding 12 files doesn't mean listing all 12 — list the 3 that matter.

## When to Use Sub-Agents

Sub-agents are for **context control**, not role-play. Don't create "frontend agent" and "backend agent" — that's anthropomorphizing. Create "research agent" and "implementation agent." The purpose is keeping each context window focused and clean.

**Use sub-agents when:**
- Research might require exploring many files (pollutes main context with tool results)
- The task has distinct phases (research → plan → implement) that benefit from fresh contexts
- Parallel work is genuinely independent (e.g., two features on different files)

**Don't use sub-agents when:**
- The task is small enough to hold in one context without degradation
- The phases are tightly coupled (you'd spend more time writing handoff docs than you'd save)
- You're just trying to feel productive by parallelizing

## Plan Quality Test

The test for a good brief: could a less capable model execute it without asking questions? If the plan says "update the chat module to support X" — that's not a plan, that's a wish. If it says "in chat.js, after the EventSource onmessage handler at line 142, add a check for event type 'file_update' that calls renderFileUpdate() from files.js" — that's executable.

## Human-in-the-Loop Placement

Human reviews the plan, not just the code. Catching wrong direction before 200 lines get written is where the leverage is. Code review becomes a formality if the plan was right. Reading the plan is also faster than reading the code — it's compressed intent.

**Why:** AI amplifies thinking — both good thinking and the lack of it. A bad line of research cascades. The human reviewing research and plans is what makes this work. Watch out for tools that generate piles of markdown just to make you feel productive.
