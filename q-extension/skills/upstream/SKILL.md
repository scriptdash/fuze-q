---
name: upstream
description: Turn session friction and the project's recorded deviations into upstream PRs against the repos that own the rules — q's own, or a third-party extension's. Use when friction with a q skill, one of q's rules, or an extension's surfaces, when upstream candidates were flagged this session or have accumulated in the project's rulings, or to change how the workflow works.
---

# Upstream

Never edit q or an installed extension in place. The next install overwrites the change, and no other project ever sees it. Open a PR against the repo that owns the rule instead.

Payload docs don't name q's own repo, its practices or its layout. This skill must: a PR needs a destination, and a change set landing there has to satisfy whatever that repo checks (exception: @lab43/q conventions/extensions.md, Which rules ship).

Follow the run contract — `${CLAUDE_PLUGIN_ROOT}/references/run-contract.md`.

## Step 1: Gather candidates

1. The prompt — the user may name the improvement outright.
2. This session's history — friction with a q skill, one of q's rules, or an extension's (an instruction that misfired, a gap, a wrong assumption), and any upstream candidates flagged earlier in the session.
3. The project's marked overrides — grep `q-docs/conventions/` for "(overrides:", dropping those that target the project's own docs.
4. The project's unmarked elaborations — read the project docs whose filenames match one of q's conventions or an installed extension's, for rulings that build on a rule there rather than contradict it.

Partition the candidates by destination: skill friction and "(overrides: @lab43/q …)" targets belong to the q repo, `Lab43/q`; a candidate targeting an extension's doc belongs to that extension's repo, read from `repository` in `node_modules/<extension>/package.json`. An extension with no repository recorded can't be PRed — carry its candidates to the report for the user to deliver by hand.

## Step 2: Converge with the user

Qualify the candidates against q's documentation policy (see: @lab43/q conventions/documentation.md). An override or elaboration made for project-specific reasons doesn't qualify. Don't disqualify one for adopting an alternative the target doc records as rejected — that is evidence against the rejection, and the candidate becomes revisiting it. Check each destination's PR history too — search open and closed PRs per candidate (`gh pr list --repo <owner>/<repo> --state all --search "<topic>"`), reading a hit's diff when its description doesn't settle the overlap: a candidate an open PR already covers is recommended defer, and one already proposed and closed without merging qualifies only with evidence the earlier PR lacked. Without `gh` (see: @lab43/q conventions/pull-requests.md, Opening), skip the PR-history search, and say in the report that it was skipped. Non-qualifiers are dropped without discussion and surface only in the report.

Present the qualifiers grouped by destination — for each, the proposed change and the evidence behind it — and collect a ruling on each (AskUserQuestion), in conversational mode (see: ${CLAUDE_PLUGIN_ROOT}/references/run-contract.md, Collaboration modes). In the same batch, ask which review mode — local or ship — the deliveries run under, every destination PR and the project-side deletions alike (see: ${CLAUDE_PLUGIN_ROOT}/references/run-contract.md, Review modes). Discuss a candidate only where its ruling calls for it: the user pushes back, asks, or raises an alternative. The rulings:

- **Ship** — joins the change set the remaining steps carry to its destination's PR.
- **Defer** — stays recorded in the project, a candidate for a later run.
- **Remove** — for an override the review turns against: the upstream rule holds up and the deviation was the mistake. Step 3 deletes it from the project's docs.

## Step 3: Make the changes

The Step 2 rulings are the agreement — work each destination autonomously.

First the Remove rulings. They ship to this project as a PR of their own, separate from every destination PR, under the run's review mode. Branch for them (see: ${CLAUDE_PLUGIN_ROOT}/references/run-contract.md, The delivery branch). Delete each override per the documentation policy (see: @lab43/q conventions/documentation.md), committing in ship mode, then validate the deletions (see: ${CLAUDE_PLUGIN_ROOT}/references/run-contract.md, Validation) with the **correctness** and **conventions** lenses. In local mode, run the local gate over them (see: ${CLAUDE_PLUGIN_ROOT}/references/run-contract.md, The local gate).

Then, for each destination with shipped candidates:

1. **Clone and branch**: clone fresh into a temporary directory outside the project (`git clone https://github.com/<owner>/<repo>.git`) and branch.
2. **Read the checkout's briefing** — `CLAUDE.md` — first and follow it. It governs how the change is made.
3. **Apply the change set** for the destination. In the q repo, run `npm run check`. Leave every `version` untouched, in whichever manifests carry one — releasing is the maintainer's act, not the PR's.
4. **Commit**, in ship mode — the review history stays inspectable in git.
5. **Adversarial review**: validate the change set (see: ${CLAUDE_PLUGIN_ROOT}/references/run-contract.md, Validation) with the **correctness** and **conventions** lenses, run against the checkout. Name q's own rules as the substitute grounding surface in both lenses' launches — every destination shares them, extensions being authored in projects that use q (source: @lab43/q conventions/extensions.md). The checkout's own recorded deviations win where they speak. This project's project-tier rulings never apply.
6. **The local gate**: in local mode, run it over the checkout's diff, committing onto its branch (see: ${CLAUDE_PLUGIN_ROOT}/references/run-contract.md, The local gate). The gate's own reviewer pass takes the same substitute grounding as item 5's.

## Step 4: Open the PRs

Push each destination branch and open its PR against the default branch (see: @lab43/q conventions/pull-requests.md, Opening) — the Step 2 agreement covers these PRs. Author the body and diff comments per the PR-authoring rules (see: @lab43/q conventions/pull-requests.md); the destination's own PR conventions and template win where they speak (source: @lab43/q conventions/pull-requests.md). Carry the provoking context from this session in the body — the PR is the paper trail, so provenance belongs there, never in the conventions prose (source: @lab43/q conventions/conventions.md, Conventions docs). If pushing to a repo is denied, fall back to a fork with `gh repo fork` and say so. Without `gh`, carry that destination's change set to the report for the user to deliver by hand.

Push the project branch carrying any Remove rulings and open its PR against this project's default branch, authored per the PR-authoring rules and this project's own conventions (see: @lab43/q conventions/pull-requests.md).

## Step 5: Report

Per destination: the PR link; what shipped, what was deferred or dropped and why; any candidate undeliverable for lack of a recorded repository. Include the project-side PR from any Remove rulings. And any project deviation a PR would resolve — leave it recorded, untouched: it comes out only after the change ships in a pin update, and the PR may be rejected.
