---
name: autoresearch
license: Apache-2.0
description: "Autonomous iteration loop: modify, verify, keep/discard against any metric"
metadata:
  version: "2.2.1"
---

# Autoresearch — Autonomous Goal-directed Iteration

## Safety Invariants (all subcommands)
- Never push, publish, or deploy without explicit user approval.
- Bounded by default. Override with `Iterations: unlimited`.
- All results logged to `autoresearch/{subcommand}-{YYMMDD}-{HHMM}/` directory.
- Chain handoff via `handoff.json`. Evals reads `*-results.tsv`.

## Dispatch (bare `$autoresearch`)

Parse the invocation in this order:

| Condition | Mode |
|---|---|
| `Metric:` or `Verify:` present | **Classic** — existing metric loop, unchanged |
| Free-form natural-language goal, no metric/verify | **Plan** — derive a reviewable metric and verification command; do not start an autonomous loop yet |
| Nothing | **Setup wizard** — interactive config builder |
| `--classic` flag | Force Classic regardless of goal text |

Print a banner on every invocation: `[autoresearch] mode: classic | plan | wizard`.

## Subcommands

| Command | Does | Default Iterations |
|---|---|---|
| `$autoresearch` | Iterate against a metric: modify → verify → keep/discard | 25 |
| `$autoresearch plan` | Convert a goal into validated Scope, Metric, Verify config | N/A |
| `$autoresearch debug` | Hunt bugs: hypothesize → test → falsify → repeat | 15 |
| `$autoresearch fix` | Crush errors one-by-one until zero remain | 20 |
| `$autoresearch security` | STRIDE + OWASP audit with red-team personas | 15 |
| `$autoresearch ship` | Ship through 8 phases: checklist → dry-run → deploy → verify | N/A |
| `$autoresearch scenario` | Generate edge cases across 12 dimensions | 20 |
| `$autoresearch predict` | 5 expert personas debate before implementation | N/A |
| `$autoresearch learn` | Scout codebase → generate docs or wiki → validate → fix loop | 10 |
| `$autoresearch reason` | Adversarial debate with blind judges until convergence | 8 |
| `$autoresearch probe` | 8 personas interrogate requirements until saturation | 15 |
| `$autoresearch improve` | Research ICP challenges, discover improvements, generate PRDs | 15 |
| `$autoresearch evals` | Analyze iteration results: trends, plateaus, regressions | N/A |
| `$autoresearch regression` | Regression stability gate: baseline vs candidate, verdict STABLE/UNSTABLE | N/A |

## Universal Flags

| Flag | Applies To | Purpose |
|---|---|---|
| `Iterations: N` | All looping | Set iteration count |
| `Iterations: unlimited` | All looping | Opt-in unbounded |
| `--evals` | All looping | Mid-loop checkpoints + final summary |
| `--evals-interval N` | All looping | Override checkpoint frequency |
| `--chain <targets>` | All | Sequential handoff after completion |
| `--<subcommand>` | All | Shorthand for `--chain <subcommand>` |
| `--classic` | Bare `$autoresearch` | Force Classic metric-loop mode |

## Natural-language goals

For a plain-language goal, load `plan.md` and produce a concrete `Metric:` and `Verify:` configuration for user review. Start the Classic loop only after those fields are explicit. This package does not currently include an executable cross-subcommand orchestrator.

`references/orchestrator-routing.md` records a future routing design. Treat it as design material only: do not invoke the commands described there and do not claim that automatic goal classification, state routing, or plateau detection is available.
