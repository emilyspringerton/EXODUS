# EXODUS

## What this is

A home for autocurriculum (PFSP opponent-pool self-play) research, extracted from REDGARDEN's
own multi-agent RL training pipeline for eventual open release — see `README.md` for the full
provenance, what's game-agnostic vs. REDGARDEN-specific, and the real, unresolved design
questions about the extraction itself. Founder, real-time (2026-08-11): "github upstream created
for autocurriculum" → "EXODUS" → "keep any autocurriculum specific bits there we can prepare for
open release."

**Status: scaffolding only.** `CLAUDE.md` (this file) and `README.md` are the first two files in
the repo. No code has been extracted or written here yet — that's real, unstarted follow-up work.
Read `README.md` before assuming anything about scope; don't start moving REDGARDEN code here
without first resolving the open design questions it lists (framework interface, coupling model,
licensing).

## Stack

Not yet decided. The source material (`REDGARDEN/scripts/rl_env_team.py`'s `_sample_opponent()`/
`add_opponent_checkpoint()`) is Python, targeting Gymnasium/Stable-Baselines3's `VecEnv`
conventions — a reasonable default assumption for this repo too, but not committed until someone
actually starts the extraction pass.

## Related Repos

- `REDGARDEN` — where the real, currently-working implementation lives today (`NORTHSTAR.md`
  §25.4 has the full design writeup and status history). This repo doesn't duplicate that
  implementation yet — see `README.md`'s own "What would move here" section.

## Founder Real-Time Direction

Whenever the founder gives real-time direction — a new ask, a correction, a "can we also..." —
route it through `emily observe -s info "Founder real-time: <summary>"` first, even if it isn't
this repo's usual domain, then sprint-plan it into `EMILY/BACKLOG.md` (`emily backlog curate`,
scoped into a real SECTION/sub-item, not just a one-line log), and only then implement. See
`EMILY/docs/THE_EMILY_WAY.md` Principle 18 ("Pave the Cow Paths").

## Apple Filing Protocol

After any meaningful change, file an Apple:
```bash
emily apples post -t completion -repo EXODUS "<title>" "<body with commit hash>"
```
Then mark the item done in `EMILY/BACKLOG.md` and commit.

## CHANGELOG Protocol

After any meaningful change, update CHANGELOG.md:
```bash
emily changelog add EXODUS "<what changed>"
# or manually: append a dated bullet under ## YYYY-MM-DD in EXODUS/CHANGELOG.md
```

## README Reality — SAGA reconciliation (standing instruction, monorepo-wide)

Founder real-time, 2026-09-18: if a change of yours **substantially changes the claim of this project's core README**,
then per SAGA protocols (`EMILY/docs/SAGA_SYSTEM_AUDIT_2026-07-18.md`, HQ-SPEC-DOC-102: intent ↔ claim ledger ↔ reality)
you **must update `README.md` in the same unit of work** so it reflects current reality. The README is the project's public
claim; it must not lag behind the code.

- **When it applies:** a capability is added or removed; status moves ("design only" → "working", "planned" → "shipped");
  the stack, build, run or install steps change; a claim in the README is now false or stale; or you add a **meaningful,
  genuinely interesting piece of kit** (a new tool, engine capability, protocol, pipeline, game system). For that last case
  especially: put it in the README — what it is, how to run it, and its honest status and limits.
- **When it does not:** ordinary fixes, refactors and small features that leave the README's claims true.
- **How:** re-read the README against what you just changed; fix or delete stale lines (including "not built yet" notes that
  are now built); verify any new claim by actually running it, and mark anything untested as untested; commit the README
  with (or immediately after) the change, and mention it in the CHANGELOG entry.

## Frame-Break Reframing

Founder-sourced prompting technique (REDGARDEN/NORTHSTAR.md §28, full origin in
REDGARDEN/docs2/MULTI_AGENT_RD_RESEARCH_NOTES.md §5): given a request, name the underlying
structural/systemic pattern it's one instance of — one level of abstraction up — as an added
lens during planning/triage/judgment calls. Use it to spot the general case behind a specific
ask. It augments judgment, it does not replace doing the work: direct, concrete execution of
the literal task asked for still happens every time.

## Commit Protocol (standing instruction)

Always commit and push completed work immediately — don't wait to be asked. This is the default for every repo in this monorepo.

Every commit — human-written or produced by automated code paths (git-commit helpers in emily-agent, emily.cli, IDUNA handlers, etc.) — must carry the active `emily session` fingerprint as a `session: <tag>` trailer (blank line, then the trailer). This was silently missing from several independently-implemented automated commit helpers across the monorepo until an audit on 2026-08-10 (founder, real-time: "where in the fuck is my llm session id anywhere"). If you add a new automated git-commit code path anywhere, wire in the session tag the same way — don't assume an existing helper already does it.
