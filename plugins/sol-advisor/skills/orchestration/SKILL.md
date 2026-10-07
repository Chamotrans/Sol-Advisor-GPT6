---
name: orchestration
description: "Codex-native risk-gated selective routing: default solo delivery, targeted native delegation or audit, and exceptional full review."
---

# Sol Advisor Orchestration

Act as the architect. Own the user's intent, architecture, route choice, decomposition,
implementation or delegation, parent verification, escalation decisions, and final
acceptance. Selective routing has four exact modes: `solo`, `delegate`, `audit`, and
`full`. Solo is the default. One auxiliary agent is the default maximum; full is an
explicit broad or high-risk exception.

The primary model is `gpt-6.1-sol`; delegate and reviewer models are limited to
`gpt-6-sol` and `gpt-6-luna`. Live tools may narrow this set, but must not silently
expand it to older or unlisted models.

Read [references/role-contracts.md](references/role-contracts.md) before the first
delegation. Use [references/operations.md](references/operations.md) for exact spawn,
preflight, runtime-evidence, isolation, and maintainer procedures.

## Confirm the primary session

Run the primary Codex session on `gpt-6.1-sol` with high reasoning. Verify the current
model and effort when runtime metadata exposes them. If either differs, stop delegation
and report the mismatch. If the primary model or effort is unobservable, disclose that
limit and continue safe solo work; do not claim the primary is confirmed or repeatedly
ask the user to reconfirm unrelated tasks. A skill cannot change the primary model.

## Declare the route before task tools

Before the first task tool call, emit one machine-auditable declaration:

~~~text
SELECTIVE ROUTE
mode: solo | delegate | audit | full
risk: <concise, task-specific rationale>
~~~

No task tool call may precede this declaration. Choose `solo` unless a stated risk
justifies another mode. A later declaration may only escalate the route when newly
observed risk justifies it; never silently downgrade. Record the evidence for an
escalation. Details and the task-scoped preflight matrix are in operations.md.

## Preflight selected auxiliaries only

Check the observable primary model and effort first. Preflight only an auxiliary selected
by the declared route: none for solo; Luna / Max or Sol 6 / High for delegate; fresh
Sol 6 / High for audit; and the selected implementer plus fresh Sol 6 reviewer for full.
The default native dispatch uses `agent_type: default`, explicit `model` and
`reasoning_effort`, and `fork_turns: none`. Public spawn metadata is authoritative;
the local inspector may fill omitted runtime fields but cannot override a conflict.
Missing, conflicting, unavailable, or unobservable auxiliary evidence stops that lane;
never silently substitute a model, effort, or reviewer. See operations.md for optional
compatibility with named roles exposed by a matching public schema.

## Route delivery without duplication

- `solo`: root plans, implements, tests, and self-reviews; spawn no auxiliary.
- `delegate`: select Luna / Max for bounded, fully specified work, or Sol 6 / High for
  judgment-heavy, high-risk, context-heavy, or wide-blast-radius work. The selected
  implementer executes the complete spec; root verifies; do not request a fresh review.
- `audit`: root implements and verifies; a fresh read-only Sol 6 / High reviewer reviews
  the accumulated diff; spawn no implementer.
- `full`: only for an explicit broad or high-risk exception. Select one implementer,
  root verifies, then a fresh read-only Sol 6 / High reviewer reviews.

Auxiliary work must substitute for root work, not duplicate it. A Luna result may
justify escalation to Sol 6 / High only when it reveals newly observed complexity,
risk, wide blast radius, or misclassification. A corrected Luna attempt is reserved
for a specification error and is not a prerequisite for Sol 6. Any route change must
be declared and evidenced; do not silently downgrade.

## Keep architect work in the primary session

Keep these responsibilities in the primary session:

- Resolve requirements and material ambiguity.
- Choose architecture, interfaces, decomposition, and selective route.
- Write the complete five-part worker specification for any selected implementer.
- Inspect the actual diff and rerun verification.
- Decide whether newly observed risk warrants escalation.
- Judge the reviewer verdict when the route includes review and accept the deliverable.

Every worker prompt must contain OBJECTIVE, FILES AND OWNERSHIP, INTERFACES,
CONSTRAINTS, VERIFICATION, and the structured implementation return in
[the role contracts](references/role-contracts.md). State the exact owned files,
preserve concurrent edits, and never silently widen scope.

Treat worker reports as claims. Confirm the complete diff, changed-file scope, requested
checks, and artifact/runtime evidence in the parent session. Do not duplicate the
selected implementer's work in the primary session.

## Review only when the route includes it

For `audit` and `full`, after parent verification, spawn a new native Sol 6 / High
reviewer. The reviewer must remain behaviorally read-only, inspect the actual
accumulated diff, and return exactly ship, fix-first, or rethink. A reviewer never
implements its own fixes. `solo` and `delegate` do not receive a fresh reviewer.

- ship: report completion with the verification evidence.
- fix-first applies only to `audit` and `full`:
  - audit: the root implements the required correction, re-verifies, and obtains a new
    fresh reviewer.
  - full: the selected implementer handles the required correction, the root
    re-verifies, and a new fresh reviewer reviews.
  - solo and delegate: no fresh reviewer is added unless a newly observed,
    risk-evidenced route escalation is declared; never silently add one.
- rethink: revise the architecture and do not report completion.

Any implementation correction invalidates the prior verdict. Apply the observed sandbox
and permission profile rules in the operations reference; never claim enforced
read-only isolation when it was not observed.
