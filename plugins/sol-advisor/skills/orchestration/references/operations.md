# Native operations

This is the maintainer and operator reference for Sol Advisor's native agent
workflow. Keep the README user-facing; use this page when installing, delegating,
inspecting routing, or validating a release.

## Default native dispatch contract

The parent is `gpt-6.1-sol` / High; implementers are `gpt-6-luna`, `gpt-6.1-terra`,
or `gpt-6-sol`. Audit/full reviewers remain `gpt-6-sol` / High.
Use the currently exposed generic `collaboration.spawn_agent` contract.
For bounded implementation, pass these explicit fields:

~~~text
agent_type: default
model: gpt-6-luna
reasoning_effort: max
fork_turns: none
~~~

For scoped code fixes, visual reasoning, computer use, or writing from supplied
material, Terra 6.1 is another implementer choice. For example:

~~~text
agent_type: default
model: gpt-6.1-terra
reasoning_effort: high
fork_turns: none
~~~

The 2026-10-27 Codex native-tool snapshot exposes `low`, `medium`, `high`, `xhigh`,
`max`, and `ultra` for `gpt-6.1-terra`. Select an explicit effort supported by the
current tool; the example does not impose High. The
[API model page](https://developers.openai.com/api/docs/models/gpt-6.1-terra)
has a different effort set and must not override the Codex dispatch schema.

For judgment-heavy implementation or a fresh audit/full reviewer, pass:

~~~text
agent_type: default
model: gpt-6-sol
reasoning_effort: high
fork_turns: none
~~~

Supply the complete worker packet or read-only reviewer prompt from role-contracts.md.
The explicit generic model and effort controls are the supported dispatch contract;
do not depend on named-role discovery. Require public spawn metadata to support and
confirm the selected model and effort. If public metadata omits a runtime field, the
local inspector may fill that field; a conflict stops the lane.

## Optional named-role compatibility

Installed TOMLs preserve these profiles for compatible native hosts:

The identifier `sol_advisor_terra_implementer` and `--check-role terra` are retained
only for compatibility with existing native role discovery. This role runs
`gpt-6-sol` / High; it does not select `gpt-6.1-terra`. Invoke Terra 6.1 through
generic dispatch with its exact model ID; no new companion profile is required.

| Role type | Model | Effort | Use |
|---|---|---|---|
| sol_advisor_luna_implementer | gpt-6-luna | max | Delegate/full bounded routine implementation |
| sol_advisor_terra_implementer | gpt-6-sol | high | Delegate/full judgment-heavy or high-risk implementation |
| sol_advisor_sol_reviewer | gpt-6-sol | high | Audit/full fresh review; requests read-only sandbox |

Use a named role only when its public metadata agrees with the intended model and
effort. A stale public role definition, including a Terra-named role still advertising
an older model, is unusable even if the local TOML is newer. Do not override a named
role's contradictory metadata. Return to the supported generic dispatch above when
its public contract is available; never claim that a stale named role ran GPT-6.

## Selective route declaration, preflight, and caching

The primary session should be `gpt-6.1-sol` / High. Check observable parent metadata.
If it conflicts, stop delegation. If it is unavailable, disclose the limit and continue
safe solo work without claiming confirmed primary routing. Companion installation is
separate from task routing because plugin installation does not register user-owned TOMLs.

At installation or update time, run the repository-relative installer and its exactness
check:

~~~sh
sh plugins/sol-advisor/scripts/install-agents.sh
sh plugins/sol-advisor/scripts/install-agents.sh --check
~~~

When operating from an installed skill, resolve the same script relative to this
reference's parent skill:

~~~sh
skill_dir=<directory-containing-this-SKILL.md>
installer="$skill_dir/../../scripts/install-agents.sh"
sh "$installer" --check
~~~

The installer is fail-closed and performs its own post-install exactness check. It
recognizes only byte-exact historical templates, including the shipped v0.2.0, v0.5.0, and v0.6.0 profiles. Modified/unsafe/nonregular/
symlinked/conflicting destinations remain refusals, and all mutations are preflighted.

The root emits one machine-auditable declaration before its first task tool call:

~~~text
SELECTIVE ROUTE
mode: solo | delegate | audit | full
risk: <concise, task-specific rationale>
~~~

Solo is the default. One auxiliary is the default maximum; full is an explicit broad
or high-risk exception. The root may emit a later declaration only to escalate when
newly observed risk justifies it. It records that evidence and never silently
downgrades.

The existing --check flag verifies all three compatibility profiles. For generic
dispatch, public model/effort availability and the exact spawn fields are the task
preflight; the installer is not a prerequisite. If a named-role compatibility path is
selected, check only its selected profiles; every check is non-mutating and fail-closed:

| Route | Required companion checks |
|---|---|
| solo | None |
| delegate (Luna) | `--check --check-role luna` |
| delegate (Sol 6) | `--check --check-role terra` |
| audit | `--check --check-role sol` |
| full (Luna) | `--check --check-role luna --check-role sol` |
| full (Sol 6) | `--check --check-role terra --check-role sol` |

For example:

~~~sh
sh plugins/sol-advisor/scripts/install-agents.sh --check --check-role luna
sh plugins/sol-advisor/scripts/install-agents.sh --check --check-role sol
~~~

Unknown or missing role arguments fail before any destination mutation. A selective
check ignores unselected role destinations, while the all-role --check behavior
remains unchanged. Cache successful checks only for the task; never carry them across
later tasks, installation/update, or routing/configuration changes.

Luna / Max is for bounded, fully specified work. Terra 6.1 is an optional scoped
implementer with a live-supported effort. Sol 6 / High is selected for
judgment-heavy, high-risk, context-heavy, or wide-blast-radius work. A Luna result
may justify a declared Sol 6 escalation only when it shows newly observed risk. One
corrected Luna attempt is reserved for a specification error and is not a prerequisite
for Sol 6.

If public runtime metadata omits model or effort, use the local inspector below for
those omitted fields only. Do not use it to replace available public evidence.

## Runtime routing evidence

The public spawn/details record is authoritative for the selected model and effort.
For generic dispatch the role is `default`; optional named roles require an exact
public role match. When model or effort is omitted, resolve the helper relative to the
installed skill and inspect the exact native thread ID:

~~~sh
skill_dir=<directory-containing-this-SKILL.md>
runtime_inspector="$skill_dir/../../scripts/inspect-agent-runtime.sh"
sh "$runtime_inspector" <native-subagent-thread-id>
~~~

For a disposable fixture or non-default session root:

~~~sh
sh "$runtime_inspector" --sessions-dir /absolute/path/to/sessions <native-subagent-thread-id>
~~~

The helper searches one exact rollout filename suffix and emits only allowlisted
routing fields. It refuses invalid IDs, zero/multiple matches, missing fields, or
conflicting model/effort/sandbox/permission/working-directory values. It never prints
prompts, messages, environment variables, tokens, configuration, or arbitrary rollout
payloads.

Accepted routing is Luna / max or Terra 6.1 / a selected live-supported effort for
bounded delegate/full implementation, Sol 6 / high
for higher-risk delegate/full implementation, and Sol 6 / high for audit/full review.
If public and local evidence both exist, they must agree. The local inspector is not a
model-selection fallback.

## Read-only reviewer interpretation

The optional reviewer TOML requests sandbox_mode = read-only; generic dispatch may
inherit a broader host sandbox. Capture the observed sandbox policy type and
permission profile type from public metadata or the inspector:

- Observed read-only sandbox: isolation is enforced.
- Broader host policy: continue only when hard isolation is not required, the prompt
  forbids edits, and the parent captures exact before/after repository and artifact
  state. Report the broader policy and profile as residual risk.
- Unobservable isolation, required hard isolation, or any mutation: stop the review and
  do not claim read-only isolation.

A reviewer returns exactly ship, fix-first, or rethink. A fix invalidates the prior
verdict; parent verification and a new fresh review are required.

## Worker packet and parent acceptance

Every Luna 6, Terra 6.1, or Sol 6 prompt uses the five-part packet in role-contracts.md:

- OBJECTIVE
- FILES AND OWNERSHIP
- INTERFACES
- CONSTRAINTS
- VERIFICATION

It must also request the structured implementation report. The parent owns architecture,
complete diff inspection, verification reruns, correction/escalation decisions, and
acceptance. Worker claims never replace direct inspection.

In solo, the root plans, implements, tests, and self-reviews with no auxiliary. In
delegate, one selected Luna 6, Terra 6.1, or Sol 6 implementer completes the spec and the root
verifies with no fresh reviewer. In audit, the root implements and verifies, then a
fresh Sol 6 reviewer reviews. In full, one selected implementer completes the spec, the
root verifies, and a fresh Sol 6 reviewer reviews. Auxiliary work substitutes for root
work; it does not duplicate it. A reviewer never fixes its own findings.

## Maintainer verification

From the repository root, run:

~~~sh
sh plugins/sol-advisor/scripts/verify.sh
git diff --check
git status --short
git diff --stat
~~~

The verifier covers the v0.6.1 manifest, exact three-role TOMLs, selective-routing
contracts, concise README journey, absence of retired workflow references, installer
safety fixtures, Luna and generic Terra runtime evidence, JSON/TOML validity, and shell syntax.
