---
name: orchestrator-discipline
description: Delegation and communication discipline for orchestrating subagents — when to fan out, how to brief agents, how to treat their reports, and how to report back readably. Invoke at the start of any multi-agent, fan-out, or long-horizon task, especially on models with a strong delegation bias in either direction (Claude Opus 4.8 under-delegates; Claude Opus 5 over-delegates).
---

# Orchestrator Discipline

Most orchestration failures are silent. Work that should have fanned out runs
serially. Subagent reports get trusted without verification. Findings arrive
compressed into shorthand the reader can't decode. Nothing errors — the
result is just slower, thinner, and harder to trust than it should be.

These rules override your default habits wherever they conflict.

## When to delegate

- **Fan out across independent items.** Many files to read, many candidates
  to check, many tests to run, many angles to search — parallel subagents,
  launched in a single message so they run concurrently.
- **Work directly on sequential or single-item work.** Don't spawn an agent
  for a file you can read yourself in one call. Delegation has overhead;
  spend it where parallelism or context isolation pays for it.
- **Correct for the model's delegation bias — it runs both ways.** Some
  models under-delegate (e.g. Claude Opus 4.8 won't reach for subagents
  unless reasonably sure it's needed — treat "could this fan out?" as a
  standing question at the start of every multi-part task). Others
  over-delegate (e.g. Claude Opus 5 reaches for subagents freely — keep
  spawn counts low, and don't delegate work you could finish yourself in a
  handful of tool calls). Know which way your model errs and push the
  other way.

## Briefing

- **Brief a subagent like a new hire.** State the goal, the constraints, the
  exact deliverable, and the *reason* behind the task — an agent that knows
  why can connect information you didn't think to include. It cannot see
  your conversation; anything it needs, you must say.
- **Include what's already settled.** Decisions made, approaches ruled out,
  dead ends already explored. Otherwise agents re-litigate solved questions
  or re-walk paths you know are empty.
- **Specify the return format.** Results you'll compose or compare should
  come back in a shape you dictated — a list, a table, a verdict with
  confidence — not whatever prose each agent improvises.

## Trust and verification

- **Subagent reports are claims, not facts.** Verify anything load-bearing
  before building on it — spot-check the file it cites, run the test it says
  passes.
- **Never fabricate or predict a pending agent's result.** If it hasn't
  reported, it hasn't reported. Say the work is still running.
- **Fresh eyes beat self-critique.** For important deliverables, spawn a
  fresh-context verifier rather than re-reading your own work — you will
  re-convince yourself of your own reasoning. For findings that matter, make
  the verifier adversarial: instruct it to refute, not to confirm.
- **Gate model handoffs with an audit.** When one model continues another
  model's in-flight work, the inherited notes are claims, not state — and
  the successor will fill their gaps by inference under completion
  pressure. Before it starts: a fresh-context audit that traces every
  identifier and assertion in the notes to a verifiable source, and
  rewrites them as verified-or-flagged. The cheap tell that you skipped
  this: the new model confidently "knows" things nobody verified.
- **Coverage first, filter second.** Agents collecting findings (review,
  audit, research) should report everything with a confidence and severity
  attached. Filtering is a separate downstream step — a finder that
  self-censors "minor" issues silently caps your recall.

## Model tiering

- **Match the model to the role.** Strongest model orchestrates, reviews,
  and judges; mid-tier models build; the cheapest tier does mechanical
  fan-out (enumerate, extract, reformat) — and is never assigned review,
  design, or judgment calls.
- **Cheap agents earn their keep on volume.** A task worth delegating to a
  small model is one where being 90% as good on each of 40 items beats
  being 100% as good on 6.
- **Effort is the second dial.** Set each agent's reasoning effort
  explicitly — the `effort:` frontmatter key on an agent definition, or the
  per-agent effort option where the harness exposes one: `low` for
  mechanical fan-out, `high` as the default, `xhigh` for the hardest
  build/verify passes. Prompt keywords ("think harder", "ultrathink") do
  not set thinking depth on current adaptive-thinking models — effort does.

## Reporting back

- **Lead with the outcome.** The first sentence of a summary answers "what
  happened" or "what did you find" — the thing the reader would ask for if
  they said "just give me the TLDR." Detail and reasoning come after.
- **Readable beats concise.** Shorten by dropping details that don't change
  what the reader does next — not by compressing into fragments, arrow
  chains (`A → B → fails`), or labels you invented mid-task. Complete
  sentences; every identifier gets a plain-language clause saying what it is.
- **The final message stands alone.** Write it for someone who stepped away
  and is catching up: none of the shorthand you built up while working,
  every mid-turn finding restated. The vocabulary you developed across
  twenty tool calls is yours, not the reader's.
- **Silence between tool calls.** Write text only when you find something
  load-bearing, change direction, or hit a blocker — one sentence each. No
  "Now I'll…" narration.
- **Ground every progress claim in a tool result.** Only report work you can
  point to evidence for. Tests fail → say so, with the output. A step was
  skipped → say that. Done and verified → state it plainly, without hedging.

## Ending the turn

Check your last paragraph before stopping. If it's a plan, a question you
could answer yourself, a list of next steps, or a promise about work not yet
done ("I'll…", "let me know when…") — do that work now, with tool calls.
End only when the task is complete or you're blocked on input only the user
can provide.
