# 🔄 Autonomous Agent Loops

[![ClawHub Skill](https://img.shields.io/badge/ClawHub-Skill-blue)](https://clawhub.ai/djc00p/skills/autonomous-agent-loops) [![Agent Skill](https://img.shields.io/badge/Agent-Skill-blue)](#) [![Automation](https://img.shields.io/badge/Automation-D54309.svg)](https://en.wikipedia.org/wiki/Automation) [![License: MIT](https://img.shields.io/badge/License-MIT-yellow.svg)](https://opensource.org/licenses/MIT)

**Autonomous Agent Loops** is a catalog of patterns for running agents in loops — enabling spec-driven development, CI/CD-style pipelines, and iterative refinement without a human in the loop between steps.

**Used correctly, this is a productivity multiplier. Used carelessly, it can drain a bank account, leak secrets, or rewrite a production codebase.**

---

## ⚠️ Security & Consent — Read First

This skill teaches unattended repository automation. It can create branches, open pull requests, retry CI failures, and merge code without a human in the loop. That power is dangerous in the wrong hands and on the wrong target.

**Before using any pattern in this skill, you MUST:**

1. **Get explicit, informed consent from the repository owner.** *"I want to automate PRs"* is not consent for *"merge my production code at 3 AM without review."*
2. **Default to dry-run.** All patterns below start in `--disable-commits` / no-write mode. Only flip the switch when you understand what each flag does.
3. **Use protected branches + least-privileged credentials.** The GitHub token this loop uses must not have admin/org-owner scopes. A scoped fine-grained PAT for one repo is the minimum.
4. **Set hard limits.** Every loop must have `--max-runs`, `--max-cost`, and `--max-duration`. Without these, a runaway loop will burn budget or destroy state.
5. **Never loop on production.** Run on a fork, a scratch repo, or a clearly-labeled branch. The main branch should be merged by a human.
6. **Audit your inputs.** Specs, directory listings, and captured diffs may contain secrets, customer data, internal paths, or proprietary code. Never pipe those into an external agent command without scrubbing them first.

> **Patterns in this skill that touch the network, filesystem, or git remotes will refuse to run unless the corresponding safety flags are explicitly set.** If a pattern silently merges or pushes, that is a bug — file an issue.

Full audit before first use: `references/security-checklist.md`

---

## 🚀 Quick Start

**Choose your pattern by complexity:**

1. **Sequential Pipeline** (simple) — chain `claude -p` calls for linear workflows
2. **Persistent REPL** (simple) — interactive sessions with history
3. **Spec-Driven Parallel** (medium) — deploy N agents from spec, manage waves
4. **PR Automation Loop** (medium) — PR creation, CI fix, auto-merge
5. **De-Sloppify Pass** (add-on) — cleanup step after any implementation
6. **RFC-Driven DAG** (advanced) — multi-unit parallel work with dependency graph

---

## 📊 Pattern Spectrum

| Pattern | Setup | Complexity | Default Mode | Best For |
|---|---|---|---|---|
| Sequential Pipeline | Bash script | Low | Dry-run | Daily tasks, scripted workflows |
| REPL | Node/CLI | Low | Read-only | Interactive development |
| Parallel Agents | Agent loop | Medium | Dry-run | Content generation, spec variations |
| PR Loop | Shell script | Medium | `--disable-commits` | Iterative multi-day projects |
| De-Sloppify | Add-on to any | Optional | Dry-run | Quality cleanup after implementation |
| DAG Orchestration | Python/Node | High | Dry-run | Large features, parallel units, merge coordination |

---

## 🧭 Decision Matrix

```text
Is this a single focused change?
├─ Yes → Sequential Pipeline
└─ No → Do you have a spec/RFC?
         ├─ Yes → Do you need parallel work?
         │        ├─ Yes → DAG Orchestration
         │        └─ No → PR Automation Loop
         └─ No → Do you need many variations?
                  ├─ Yes → Parallel Agents + Spec
                  └─ No → Sequential Pipeline + De-Sloppify
```

---

## 📐 Key Principles

1. **Isolation** — each loop iteration gets fresh context, no bleed-through
2. **Context Persistence** — use files (`SHARED_TASK_NOTES.md`) to bridge iterations
3. **Exit Conditions** — always set max-runs, max-cost, max-duration, or a completion signal
4. **No Blind Retries** — capture error context for the next iteration, and redact secrets first
5. **Separate Concerns** — different loop patterns for different problem sizes
6. **Default Deny** — network writes, git pushes, and merges require explicit opt-in flags

---

## 🚫 Anti-Patterns

| Anti-pattern | Why it hurts |
|---|---|
| Infinite loops without exit conditions | Runaway budget and state destruction |
| No context bridge between iterations | Each iteration re-derives what the last one learned |
| Retrying the same failure without capturing error context | Same loop, same failure, same cost |
| Negative instructions instead of cleanup passes | The model needs something to fix, not something to avoid |
| All agents in one context window | The reviewer should never be the author |
| Ignoring file overlap in parallel work | Concurrent agents write the same file |
| Auto-merge without explicit human approval on every merge | Unreviewed code reaches the main branch |
| Skipping the security checklist before first use | The most dangerous pattern is the one nobody audited |

---

## 📚 References

* `references/security-checklist.md` — **read before any first use**
* `references/sequential-pipeline.md` — basic `claude -p` loops with examples
* `references/persistent-repl.md` — session persistence and secrets-handling rules
* `references/parallel-agents.md` — spec-driven deployment with worktree isolation
* `references/pr-automation.md` — continuous PR loop with consent gates and CI gates
* `references/de-sloppify.md` — quality cleanup pattern
* `references/dag-orchestration.md` — RFC-driven multi-unit coordination and merge-conflict recovery

---

## 📦 Install

```bash
clawhub install autonomous-agent-loops
```

Or clone this repo and add the skill folder to your agent's skills path.

### Repository layout

```
SKILL.md                              # entry point, pattern spectrum, decision matrix
references/
  security-checklist.md               # pre-flight audit — read first
  sequential-pipeline.md              # basic claude -p loops
  persistent-repl.md                  # session persistence + secrets handling
  parallel-agents.md                  # spec-driven deployment, worktree isolation
  pr-automation.md                    # PR loop with consent and CI gates
  de-sloppify.md                      # quality cleanup pattern
  dag-orchestration.md                # RFC-driven coordination + conflict recovery
LICENSE                               # MIT-0 / MIT
README.md                             # this file
```

### Versions

Releases track the published ClawHub versions.

| Version | ClawHub |
| --- | --- |
| 1.0.2 | [autonomous-agent-loops v1.0.2](https://clawhub.ai/djc00p/skills/autonomous-agent-loops) |

---

**Adapted from [everything-claude-code](https://github.com/affanm/everything-claude-code) by [@affaan-m](https://github.com/affaan-m) (MIT).**

**Published by [@djc00p](https://github.com/djc00p)**
