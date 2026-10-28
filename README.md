# Sol Advisor

[English](README.md) | [繁體中文（香港）](README.zh-HK.md) | [简体中文](README.zh-CN.md)

**Sol 6.1 / High runs the show. It declares a risk-gated route before task tools, keeps
solo as the default, and uses a single auxiliary only when that improves delivery.**

This GPT-6 routing edition is maintained by Chamotrans and builds on [DannyMac180/sol-advisor](https://github.com/DannyMac180/sol-advisor).
Sol Advisor is a Codex-only workflow for capability-routed software delivery. You
bring the goal and constraints; Sol owns the plan, implementation or delegation,
verification, and acceptance.

## Go deeper

Original author Daniel McAteer writes [**Attention Heads**](https://attentionheads.substack.com/?utm_source=github&utm_medium=readme&utm_campaign=sol-advisor) — deep, evidence-backed writing on AI, cognition, and agentic engineering. The **Agentic Engineering Field Notes** series is where he publishes practical advice on the craft of using AI. [Subscribe](https://attentionheads.substack.com/subscribe?utm_source=github&utm_medium=readme&utm_campaign=sol-advisor) to get new posts to your inbox.

## Quick start

If a marketplace named `sol-advisor` is already registered from another source,
remove that marketplace before adding this fork. Existing local installations can
continue using their local source.

You need a current Codex CLI or ChatGPT desktop app with plugins enabled, GPT-6.1
Sol / High for the primary session, native generic agent dispatch, and jq. GPT-6
Luna / Max, Terra 6.1, or Sol 6 / High access is needed only when the selected route
uses that model. Terra is an additional scoped implementer; Sol 6 remains the reviewer.

~~~sh
codex plugin marketplace add Chamotrans/Sol-Advisor-GPT6 --ref main
codex plugin add sol-advisor@sol-advisor
plugin_dir="$(codex plugin list --json | jq -r '.installed[] | select(.pluginId == "sol-advisor@sol-advisor") | .source.path')" && test -n "$plugin_dir" && test "$plugin_dir" != null && test -d "$plugin_dir" && test -f "$plugin_dir/scripts/install-agents.sh" && sh "$plugin_dir/scripts/install-agents.sh"
~~~

The companion installer preserves optional named-role profiles and verifies all three
exact files after installation. Generic dispatch selects explicit models and does not
depend on those roles. The installer is fail-closed: modified, unsafe, nonregular,
symlinked, unknown, or differing files are left untouched. It does not edit Codex
configuration. Start a fresh Codex task after updating the plugin.

Use this one prompt in the new task:

~~~text
Use $sol-advisor:orchestration to build this feature and verify it. Declare the selective route before task tools.
~~~

## What you do

Give Sol the outcome, constraints, and any important repository context. You do not
need to select or manage a lane; Sol records the route and owns verification and
acceptance.

## Routes

| Mode | Use it when | Delivery |
|---|---|---|
| `solo` | Default; risk is contained. | Root plans, implements, tests, and self-reviews. |
| `delegate` | A complete spec is better executed by one implementer. | Luna / Max for bounded work, Terra 6.1 for scoped code, visual, computer-use, or writing work, or Sol 6 / High for higher-risk work; root verifies. |
| `audit` | Independent final scrutiny matters more than delegation. | Root implements; a fresh read-only Sol 6 / High reviews. |
| `full` | Explicit broad or high-risk exception. | One selected implementer, root verification, and a fresh Sol 6 / High review. |

Terra 6.1 uses the exact model ID `gpt-6.1-terra` and an explicit effort supported
by the live Codex tool. No additional companion agent is required.

Solo is the default. One auxiliary is the default maximum; `full` is the explicit
exception. Sol emits a `SELECTIVE ROUTE` declaration with the mode and concise risk
rationale before the first task tool call. It can escalate only when newly observed
risk justifies it and never silently downgrades.

## What happens automatically

Sol 6.1 / High keeps architecture, decomposition, route selection, parent verification,
escalation decisions, and acceptance in the primary task. Auxiliary work substitutes
for root work; it does not duplicate it. The root inspects the complete diff and
reruns the requested checks. When the selected route includes a review, a fresh Sol 6 /
High reviewer returns ship, fix-first, or rethink; any fix requires a new review.

## Updating

Update this fork, refresh the optional companion profiles, and start a new task.
Switching the marketplace to the upstream repository replaces this edition's routing.

~~~sh
codex plugin marketplace upgrade sol-advisor
codex plugin add sol-advisor@sol-advisor
plugin_dir="$(codex plugin list --json | jq -r '.installed[] | select(.pluginId == "sol-advisor@sol-advisor") | .source.path')" && test -n "$plugin_dir" && test "$plugin_dir" != null && test -d "$plugin_dir" && test -f "$plugin_dir/scripts/install-agents.sh" && sh "$plugin_dir/scripts/install-agents.sh"
~~~

For exact spawn, runtime-evidence, sandbox, installer, and maintainer verification
details, read [advanced native operations](plugins/sol-advisor/skills/orchestration/references/operations.md).

## Privacy and contributions

Use a public handle and your GitHub-provided noreply email for commits. Before
sharing a patch, issue, screenshot, or log, remove personal paths, contact details,
client material, credentials, and session transcripts. Examples must use synthetic
data. The ignore rules help prevent accidental additions; they do not sanitize
tracked files or Git history. Public project attribution and the MIT copyright
notice are retained. See [LICENSE](LICENSE).
