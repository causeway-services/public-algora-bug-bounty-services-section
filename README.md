# Algora bounties

<!-- 561-group-standards:badges:start -->
[![Causeway Services](https://img.shields.io/badge/561%20Group-Causeway%20Services-115e69)](https://github.com/causeway-services)
[![License: Apache 2.0](https://img.shields.io/badge/license-Apache%202.0-115e69)](LICENSE)
[![Indigenous data: CARE](https://img.shields.io/badge/Indigenous%20data-CARE-4b6f44)](https://github.com/causeway-services/.github/blob/main/GOVERNANCE.md)
[![Tips welcome](https://img.shields.io/badge/tips-welcome-d4a017)](https://github.com/561-group/.github/blob/main/TIPS.md)
<!-- 561-group-standards:badges:end -->

This is one Node program for working through Algora bounties. It discovers and
ranks public bounties, keeps an evidence journal, can prepare a disposable
repository workspace, applies a caller-supplied patch, runs a caller-supplied
test command, compares the result with the baseline, and accepts an explicit
remote-execution adapter. It never claims a bounty or posts a comment.

`src/` is organized by ordinary responsibilities, not pseudo-services:

- `bounty.mjs` holds the `Bounty` and `Attempt` data classes plus Algora comment parsing.
- `github.mjs` fetches and normalizes GitHub issues and comments.
- `selection.mjs` makes the deliberately simple, explainable ranking.
- `repository.mjs`, `validation.mjs`, and `solve.mjs` are the patch-work path; a
  caller supplies the patch author, so model credentials and policy stay outside.
- `github-actions.mjs` dispatches a caller-selected existing Actions workflow.
- `outcomes.mjs` records content-addressed outcomes and finds repeated patterns.
- `platforms.mjs` reads the other platforms that fund GitHub issues (Opire, TaskBounty, Agent
  Exchange, Agent Bounties on Base) into the same `Bounty`, with `platform` naming where the
  reward is posted; a platform that fails is reported without failing the rest.
- `selection.mjs` also refuses, whatever it pays, a task that asks the solver for its own system
  prompt or hidden instructions.
- `patch-rewards.mjs` assesses won, merged work against Google's OSS Patch Rewards scope (Tier 1,
  security rationale, one month to one year after merge); it never submits a claim.
- `algora.mjs` composes those functions for callers; it has no hidden loop.

`scripts/` contains only command-line entry points.

```sh
# List current bounties as JSON. GH_TOKEN raises GitHub's rate limit.
GH_TOKEN=... npm run list -- --limit 20

# List the same bounties in priority order.
GH_TOKEN=... npm run plan -- --minimum-reward 25

# Include other platforms' funded issues in the same ranking.
GH_TOKEN=... npm run plan -- --platforms opire,taskbounty,agentexchange,agent-bounties

# Review repeated wins, losses, or failures in a local JSONL outcome journal.
npm run history -- .algora

npm run check
```

---

Executable code of this section (`scripts/, src/, test/`) lives in [red-cup-engineering/public-algora-bug-bounty-services-section](https://github.com/red-cup-engineering/public-algora-bug-bounty-services-section) at the same relative paths; run it with `SECTION_ROOT` pointing at this checkout.
