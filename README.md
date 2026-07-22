# orchestrator-discipline

A Claude Code skill that teaches the *orchestration* half of working with
subagents: when to fan out, how to brief agents, how much to trust their
reports, and how to report the results back so a human can actually read
them.

## Why

There's a small ecosystem of skills that carry frontier-model rigor down to
the models you run every day — [fable-discipline](https://github.com/assafkip/fable-discipline),
[fablize](https://github.com/fivetaku/fablize), and
[Iwo's Rigor Pack](https://www.iwoszapar.com/tools/rigor-pack) all focus on
*verification procedure*: run what you build, refute your own work, see
things through. They're good. Install one.

This skill covers what they don't: the **delegation layer**. Multi-agent
work fails differently than solo work — and it fails silently:

- **Under-delegation.** Capable models (Claude Opus 4.8 in particular) are
  conservative about spawning subagents. Work that should fan out across
  ten parallel agents runs serially in one context instead. Nothing errors;
  it's just slow and shallow.
- **Thin briefs.** A subagent can't see your conversation. Briefed with a
  bare task and no context, it re-litigates settled decisions and re-walks
  dead ends you already explored.
- **Misplaced trust.** Subagent reports get treated as facts. A hallucinated
  "all tests pass" propagates upward unchecked.
- **Unreadable rollups.** After forty tool calls the orchestrator compresses
  everything into arrow chains and invented shorthand the reader can't
  decode. The work was fine; the report loses it.

Each of these has a discipline that prevents it. That's what's in the skill.

## What's inside

One skill, six sections:

| Section | The rule in one line |
|---|---|
| **When to delegate** | Fan out across independent items in one message; work directly on sequential/single-item tasks; treat "could this fan out?" as an opening question, not a fallback |
| **Briefing** | Brief like a new hire — goal, constraints, deliverable shape, and the *why*; include what's already settled; dictate the return format |
| **Trust & verification** | Reports are claims, not facts; never predict a pending result; fresh-context adversarial verifiers over self-critique; coverage first, filter downstream |
| **Model tiering** | Strongest model orchestrates and judges; mid-tier builds; cheapest tier does mechanical fan-out and never review or design |
| **Reporting back** | Lead with the outcome; readable beats concise; the final message stands alone; silence between tool calls; every claim grounded in a tool result |
| **Ending the turn** | If your last paragraph is a promise about work not yet done, do the work |

## Install

As a plugin (recommended — one command):

```
/plugin marketplace add dgaidula/orchestrator-discipline
/plugin install orchestrator-discipline
```

Or manually, as a personal skill:

```sh
git clone https://github.com/dgaidula/orchestrator-discipline
cp -r orchestrator-discipline/skills/orchestrator-discipline ~/.claude/skills/
```

## Use

Invoke explicitly at the start of multi-agent work:

```
/orchestrator-discipline
```

Or let Claude load it on its own — the skill's description triggers on
multi-agent, fan-out, and long-horizon tasks.

Works alongside the verification-focused packs above; there's no overlap by
design. Written for Claude Code, but the disciplines are harness-agnostic —
nothing in the skill depends on a specific tool set.

## License

MIT © Dan Gaidula
