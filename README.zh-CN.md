# Sol Advisor

[English](README.md) | [繁體中文（香港）](README.zh-HK.md) | [简体中文](README.zh-CN.md)

**由 Sol 6.1 / High 主导。在使用任务工具前，先根据风险声明执行模式；默认自行完成，只有在有助于交付时才使用一个辅助代理。**

本 GPT-6 路由版本由 Chamotrans 维护，基于 [DannyMac180/sol-advisor](https://github.com/DannyMac180/sol-advisor)。
Sol Advisor 是专供 Codex 使用、按模型能力分配工作的软件交付流程。你提供目标和限制；Sol 负责规划、实现或委派、验证及验收。

## 延伸阅读

原作者 Daniel McAteer 的 [Attention Heads](https://attentionheads.substack.com/) 探讨 AI、认知和代理工程，并以证据支持分析。
其中 **Agentic Engineering Field Notes** 系列提供使用 AI 的实践建议。[订阅](https://attentionheads.substack.com/subscribe)后可通过电子邮件接收新文章。

## 快速开始

如果已从其他来源注册名为 `sol-advisor` 的 marketplace，请先移除该 marketplace，再添加本衍生版本。现有本地安装可以继续使用其本地来源。

需要支持插件的当前版 Codex CLI 或 ChatGPT 桌面应用、运行 GPT-6.1 Sol / High 的主会话、原生通用代理调度功能，以及 jq。
只有所选模式需要相应模型时，才需要 GPT-6 Luna / Max、Terra 6.1 或 Sol 6 / High 的访问权限。Terra 是额外的限定范围实现者选项；审核仍由 Sol 6 负责。

~~~sh
codex plugin marketplace add Chamotrans/Sol-Advisor-GPT6 --ref main
codex plugin add sol-advisor@sol-advisor
plugin_dir="$(codex plugin list --json | jq -r '.installed[] | select(.pluginId == "sol-advisor@sol-advisor") | .source.path')" && test -n "$plugin_dir" && test "$plugin_dir" != null && test -d "$plugin_dir" && test -f "$plugin_dir/scripts/install-agents.sh" && sh "$plugin_dir/scripts/install-agents.sh"
~~~

配套安装程序会保留可选的命名角色配置，并在安装后逐一核对三个文件是否完全一致。通用调度直接指定模型，不依赖这些角色。
安装程序遇到异常会停止操作：已修改、不安全、非普通文件、符号链接、未知或内容不符的文件均不会被覆盖。程序不会修改 Codex 配置。更新插件后，请新建 Codex 任务。

在新任务中输入以下提示：

~~~text
Use $sol-advisor:orchestration to build this feature and verify it. Declare the selective route before task tools.
~~~

## 你需要提供什么

向 Sol 说明预期成果、限制和重要的仓库背景。你无需自行选择或管理执行分工；Sol 会记录所选模式，并负责验证和验收。

## 执行模式

| 模式 | 适用情况 | 交付方式 |
|---|---|---|
| `solo` | 默认模式；风险可控。 | 主代理负责规划、实现、测试及自行复核。 |
| `delegate` | 已有完整规格，交给一名实现者处理更合适。 | Luna / Max 处理范围明确的工作；Terra 6.1 处理限定范围的代码、视觉、计算机操作或写作工作；Sol 6 / High 处理较高风险工作。主代理负责验证。 |
| `audit` | 独立的最终审核比委派实现更重要。 | 主代理实现，再由全新的只读 Sol 6 / High 审核。 |
| `full` | 明确属于范围广或风险高的例外情况。 | 一名指定实现者完成工作，主代理验证，再由全新的 Sol 6 / High 审核。 |

Terra 6.1 使用准确的模型 ID `gpt-6.1-terra`，并明确指定当前 Codex 工具支持的推理强度（effort）。无需额外安装配套代理。

默认采用 `solo`，辅助代理默认上限为一个；`full` 是必须明确选择的例外。
首次使用任务工具前，Sol 会发出 `SELECTIVE ROUTE` 声明，列出模式和简短的风险理由。只有新发现的风险足以支持时才可升级模式，不会静默降级。

## 自动处理的工作

Sol 6.1 / High 在主任务中负责架构、工作拆分、模式选择、主代理验证、升级判断和验收。
辅助代理的工作必须替代部分主代理工作，而非重复执行。主代理会检查完整差异，并重新运行所要求的检查。
如果所选模式包含审核，全新的 Sol 6 / High 审核者会返回 `ship`、`fix-first` 或 `rethink`；任何修改后均需重新审核。

## 更新

更新本衍生版本、刷新可选的配套角色配置，再新建任务。如果将 marketplace 改回上游仓库，本版本的路由规则会被替换。

~~~sh
codex plugin marketplace upgrade sol-advisor
codex plugin add sol-advisor@sol-advisor
plugin_dir="$(codex plugin list --json | jq -r '.installed[] | select(.pluginId == "sol-advisor@sol-advisor") | .source.path')" && test -n "$plugin_dir" && test "$plugin_dir" != null && test -d "$plugin_dir" && test -f "$plugin_dir/scripts/install-agents.sh" && sh "$plugin_dir/scripts/install-agents.sh"
~~~

有关准确的代理启动方式、运行证据、沙箱、安装程序及维护验证详情，请参阅[高级原生操作指南](plugins/sol-advisor/skills/orchestration/references/operations.md)（英文）。

## 隐私与参与贡献

提交 commit 时，请使用公开账号名称和 GitHub 提供的 noreply 电子邮件地址。
分享补丁、issue、截图或日志前，须移除个人路径、联系信息、客户材料、凭据和会话记录。示例必须使用模拟数据。
忽略规则有助于防止误加文件，但不会清理已跟踪文件或 Git 历史。公开的项目署名和 MIT 版权声明予以保留。详见 [LICENSE](LICENSE)。
