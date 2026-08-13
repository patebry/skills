---
name: review-pull-request
description: Review a GitHub pull request or code diff for actionable correctness, security, data-integrity, lifecycle, contract, architecture, and test defects, then publish one terse final review when permitted. Use for PR review, audit, judgment, or assessment; request changes only for proven merge blockers, distinguish defects from verification gaps, use comment-only reviews for self-authored PRs, and do not publish clean results.
---

# Review Pull Request

Find merge-relevant defects, prove them against current code, and tell the author tersely. Prefer no finding over a speculative one. Missing evidence is not evidence of broken behavior.

## Bound the review

Keep investigation read-only except for an isolated temporary worktree and one final review publication. Do not edit source, apply fixes, commit, push, switch the user's checkout, create branches, resolve threads, change labels, merge, or close the PR.

For a hosted PR, resolve the repository, PR, base ref and tip, head SHA, merge-base SHA, open/merged state, title, body, linked requirements, PR author, and effective publishing identity. Compare immutable provider user IDs, not display names or local credentials. If the publishing identity is unknown, review but do not publish.

Read the current provider diff, repository instructions, requirements, and relevant surrounding code. Read before-state from the merge base. Exclude unrelated pre-existing defects unless the PR makes them newly reachable or materially worse.

Treat requirements as evidence of intended behavior, not automatically as merge policy. Distinguish behavioral requirements from requested verification steps, implementation suggestions, historical plans, and author responsibilities. A repository instruction or protected-branch rule may define a mandatory gate; a ticket checkbox alone does not.

## Isolate local analysis

Before reading local repository source or running repository commands, create a unique temporary detached worktree at the exact full head commit. Give every review invocation its own worktree path, including concurrent reviews of the same repository or commit. Never use, switch, reset, clean, stash, or otherwise depend on the user's primary checkout; dirty primary-checkout files must not enter the review.

Use a system-temporary directory created exclusively for this review. Verify the worktree is detached, clean, and at the intended commit before using it. Run local inspection and eligible checks only from that worktree. Direct disposable outputs into review-owned temporary paths.

A worktree does not isolate shared Git refs, objects, configuration, hooks, credentials, caches, processes, ports, containers, databases, services, or network. During review, do not fetch, pull, push, mutate refs or configuration, run Git maintenance, initialize submodules, start persistent services, use shared databases or containers, or run commands whose effects cannot be bounded to the review's temporary state. If safe isolation cannot be established, use provider/API and static evidence instead and report any material gap under `Unverified:`. Never fall back to the primary checkout.

If the reviewed head changes materially, retire the old worktree and create a new unique worktree at the new exact head. Never reset, repoint, or reuse a review worktree.

After every terminal outcome, remove only the exact review-owned worktree and temporary directory. Validate the target before cleanup. Never use broad prune, globbed paths, branch deletion, reset, clean, or cleanup based on an unresolved variable. If exact ownership or safe cleanup is uncertain, retain the worktree and report its path and blocker rather than risking another session or the user's checkout.

For a supplied immutable diff with no repository or commit, review the artifact statically without creating an empty worktree. Do not publish it as a hosted PR review, and disclose material surrounding-code or runtime gaps.

## Map risk

Prioritize only the lenses relevant to the change:

- correctness and user-visible runtime paths
- authorization, tenant isolation, and trust boundaries
- data, financial, and domain invariants
- lifecycle, cleanup, retry, race, and terminal paths
- schemas, migrations, APIs, UI state, and mirrored contracts
- architecture, ownership, dependency direction, and reviewable scope
- test sensitivity and environment realism
- configuration, deployment, compatibility, and operational failure

Spend attention in proportion to reachable impact. Do not turn these lenses into a boilerplate checklist.

## Trace candidates

Follow suspicious changed lines through real callers, consumers, permission sources, state transitions, error and cleanup paths, and canonical neighboring implementations. Compare before and after behavior. Inspect callees before asserting what a helper or service does.

For a test concern, determine whether the test would fail against the suspected bug in the environment where it manifests. Green checks support a conclusion; they do not establish correctness.

Run only the narrowest relevant checks that satisfy the isolation boundary. Report central risks that access, tooling, or safe isolation prevented you from checking.

Accept evidence in proportion to the changed mechanism. Static producer/consumer tracing, focused tests, deterministic artifact comparison, type checks, builds, audits, and protocol-level assertions may establish correctness without reproducing every manual workflow. Do not insist on a particular validation method merely because a ticket prescribed it.

## Separate defects from verification gaps

Classify every candidate before writing feedback:

- `Finding`: changed code is proven to produce an incorrect or unsafe result.
- `Unverified`: relevant behavior could not be exercised or observed directly.
- `Question`: intent, requirement authority, or an environment assumption is unresolved.

Only a `Finding` affects the submitted review event.

A missing observation is not a failed behavior. Do not turn an unchecked ticket box, absent manual QA, missing screenshot, browser smoke test, live-environment check, or author-disclosed limitation into a finding by itself. These are presumptively `Unverified`, especially when they require credentials, database mutation, hardware, external services, deployment access, privileged state, or coordination outside the PR.

Missing verification becomes a finding only when either:

1. the changed code itself proves a concrete defect through another evidence path; or
2. an authoritative repository or branch policy explicitly prohibits merging without that exact gate.

Before classifying a verification gap as blocking, apply this counterfactual: if the unchecked checkbox disappeared from the ticket, could you still name the changed-code mechanism, reachable input or state, incorrect result, and concrete consequence? If not, it is not a blocking finding.

Do not request changes when the only requested correction is to run, record, or repeat a check. Put that under `Unverified:` or ask a concise question unless a governing merge policy makes the check mandatory.

## Verify findings

Emit a finding only when all are present:

- an exact changed or causally implicated `path:line`, or an exact PR-level locator for an inherently cross-cutting defect
- a concrete mechanism grounded in the analyzed code, configuration, topology, authoritative requirement, or repository instruction
- a reachable input, actor, state, or runtime path that triggers the defect
- an incorrect result and concrete user, security, data, operational, or maintenance consequence
- severity justified by that consequence
- a bounded correction or verification request tied to the defect

For convention findings, cite the governing repository instruction. Historical comments, issue plans, checkbox wording, author review notes, and patterns are leads, not authority. Discard style-only, speculative, obsolete, duplicate, convention-compliant, and verification-only candidates. Verify suggested fixes against architecture and callers.

A blocking finding requires a verified mechanism, reachability, incorrect result, and merge-relevant consequence. Otherwise classify it as a question, an unverified surface, a non-blocking maintainability concern, or omit it. After author pushback, reread the evidence and narrow, correct, or retract a disproved or overstated finding promptly.

Use `blocking` only when the PR is unsafe to merge: reachable security/privacy exposure, corrupt financial or domain data, broken primary behavior, a proven behavioral acceptance-criterion violation, a governing test or merge-policy violation, or scope too broad to review safely. An unperformed validation step is not a proven behavioral violation. Use `non-blocking` for bounded maintainability, consistency, or follow-up risk. Do not infer severity from review state, labels, merge status, reviewer tone, ticket formatting, or unchecked boxes.

## Write terse feedback

Order findings by impact. Use one item per independent defect:

```text
[blocking|non-blocking] path:line — terse defect title

Current behavior and reachable path. Concrete consequence. Smallest correction. Optional focused regression test.
```

Use one to four short sentences per finding. Preserve exact symbols, actors, states, and failure conditions. Avoid praise inventories, review diaries, code dumps, repeated summaries, generic best practices, and confidence theater.

Keep questions and unverified surfaces separate from findings. Never use either to disguise an unverified blocker. If complete and clean, return exactly `No findings.` If a central surface was not verified, return `No findings in the inspected surface.` followed by concise `Unverified:` or `Question:` entries.

## Refresh and publish once

Immediately before handoff or publication, refetch the base tip, head SHA, open/merged state, title, body, and load-bearing requirements. If an input material to a conclusion changed, rebuild the complete review artifact in a fresh worktree and re-adjudicate dependent findings.

Before publishing `REQUEST_CHANGES`, perform a strict blocker audit for every blocking item:

- Is there a proven defect, not only absent evidence?
- Is its runtime or operational path reachable from this change?
- Is the incorrect result and consequence concrete?
- Would the blocker still exist without the ticket's verification checklist?
- Does the requested correction fix behavior rather than merely produce more evidence?

If any answer is no, do not request changes for that item.

Publish at most one submitted review containing all actionable findings. Attach findings inline when valid current-diff coordinates are available; otherwise put them in the review body with exact locators. Use a short nonempty body even when all findings are inline.

Choose the event from findings only, never from questions or unverified surfaces:

- No actionable findings: do not publish.
- Effective publishing identity is the PR author: `COMMENT`, even with blockers.
- Another author and at least one blocker: `REQUEST_CHANGES`.
- Another author and only non-blockers: `COMMENT`.

Never submit `APPROVE`. Never weaken a failed `REQUEST_CHANGES` attempt into a comment. Do not publish if the PR is closed or merged, identity is unknown, the artifact is unhosted, or publication capability is unavailable; return the review text and terse reason.

Before mutation, inspect submitted reviews and pending reviews by the effective identity. Skip an equivalent submitted review only when head, event, canonical body, and complete inline finding set match. Submit a pending review only when its full payload matches exactly; otherwise leave it untouched and report the mismatch. Do not treat conversation comments as submitted reviews.

Submit explicitly against the verified head. Make at most one blind mutation attempt. On timeout or ambiguous response, do not retry; reread reviews to classify the outcome and report uncertainty. After success, read back the review and all inline comments, verify actor, event, head, body, and complete finding set, then report the review URL and event.

Do not send the final user response until the isolated worktree cleanup has been attempted. Append any retained-worktree limitation without changing the review event or retrying publication.
