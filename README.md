# EXODUS

Autocurriculum research, extracted for eventual open release.

## What this is

A home for the PFSP (Prioritized Fictitious Self-Play) opponent-pool autocurriculum work
originally built inside [REDGARDEN](../REDGARDEN)'s multi-agent RL training pipeline
(`NORTHSTAR.md` §25.4). The technique itself — sample a training opponent from a pool of past
self-play checkpoints plus a fixed heuristic baseline, biased via `(1 - win_rate)^p` toward
whichever opponent the current policy is currently losing to most — isn't specific to REDGARDEN's
own game content. This repo exists to hold the general, game-agnostic pieces so they can be
studied, reused, and eventually published independently of REDGARDEN's own (currently private)
codebase.

Founder, real-time (2026-08-11): "github upstream created for autocurriculum" → "EXODUS" → "keep
any autocurriculum specific bits there we can prepare for open release."

## Status: repo scaffolding only, no code yet

This is a fresh, empty repo as of 2026-08-11 — `CLAUDE.md` and this `README.md` are the first two
files. The actual extraction (deciding what's genuinely game-agnostic vs. entangled with
REDGARDEN's own simulation internals, and moving/adapting that code here) is real, unstarted
follow-up work, not attempted in this pass. See "What would move here" below for the honest
current picture of what exists today and where.

## Provenance — what exists today, and where it actually lives right now

Nothing has been copied or moved into this repo yet. For context, the real, currently-working
implementation lives in REDGARDEN as of 2026-08-11:

- **The core sampling algorithm** — `_sample_opponent()` in `REDGARDEN/scripts/rl_env_team.py`.
  PFSP weighting (`(1 - win_rate)^PFSP_SHARPNESS`, Beta(1,1)-smoothed win rate, a minimum-weight
  floor so a fully-solved opponent never drops out of rotation entirely). This is the most
  self-contained, genuinely game-agnostic piece — it operates on win/loss counts and a bounded
  checkpoint pool, with no REDGARDEN-specific knowledge baked in.
- **Pool management** — `add_opponent_checkpoint()` / `MAX_CHECKPOINT_OPPONENTS` eviction, same
  file. Also fairly game-agnostic — a small population cap plus oldest-first eviction.
- **The C-level plumbing it depends on** — `sim_step_team_vs_actions()` / `sim_get_obs_team_any()`
  in `REDGARDEN/apps/arena_training/src/headless.c`. This half is NOT game-agnostic — it's wired
  directly into REDGARDEN's own arena simulation (`packages/simulation/arena_game.c`), and
  wouldn't move here as-is. A real open-release version would need a clean interface boundary
  (something like "an environment that can accept externally-computed opponent actions and
  report team-relative observations") that REDGARDEN's own environment implements, rather than
  porting REDGARDEN's simulation code itself into this repo.
- **The live-deployment consumer** — `team_rl_engage_nudge()` in
  `REDGARDEN/apps/arena_bot/src/main.c`. Entirely REDGARDEN-specific (reads REDGARDEN's own wire
  protocol), stays there.

## What would move here (real, unscoped design questions — not resolved by this doc)

- Does this repo target a specific RL framework's `VecEnv`-style interface (e.g. Gymnasium/SB3,
  matching what REDGARDEN's own `ArenaTeamVecEnv` already uses), or something more abstract?
- Does REDGARDEN keep its own copy and this repo becomes the canonical upstream REDGARDEN
  vendors/imports, or does REDGARDEN's own `rl_env_team.py` get refactored to import from here
  directly (real coupling/versioning tradeoffs either way)?
- License and any real IP-scoping pass needed before code lands here, separate from the technical
  extraction itself.

None of this is decided. Flagged honestly as open, not silently resolved by whoever writes the
first real commit here.

## Related

- `REDGARDEN/NORTHSTAR.md` §25.4 — the full design writeup and honest status history (the
  critical training-pipeline bug fix, the noisy-gestalt run, this repo's own creation) live there
  for now.
- `REDGARDEN/docs2/MULTI_AGENT_RD_RESEARCH_NOTES.md` — source research notes this whole thread
  traces back to.
