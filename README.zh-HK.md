# Sol Advisor

[English](README.md) | [繁體中文（香港）](README.zh-HK.md) | [简体中文](README.zh-CN.md)

**由 Sol 6.1 / High 主導。在使用任務工具前，先按風險宣告執行模式；預設自行完成，只有在有助交付時才使用一個輔助代理。**

本 GPT-6 路由版本由 Chamotrans 維護，以 [DannyMac180/sol-advisor](https://github.com/DannyMac180/sol-advisor) 為基礎。
Sol Advisor 是專供 Codex 使用、按模型能力分配工作的軟件交付流程。你提供目標及限制；Sol 負責規劃、實作或委派、驗證及驗收。

## 延伸閱讀

原作者 Daniel McAteer 的 [Attention Heads](https://attentionheads.substack.com/) 探討 AI、認知及代理工程，並以證據支持分析。
其中 **Agentic Engineering Field Notes** 系列提供使用 AI 的實務建議。[訂閱](https://attentionheads.substack.com/subscribe)可透過電郵接收新文章。

## 快速開始

若已從其他來源註冊名為 `sol-advisor` 的 marketplace，請先移除該 marketplace，再加入本衍生版本。現有本機安裝可繼續使用其本機來源。

需要支援插件的現行 Codex CLI 或 ChatGPT 桌面應用程式、以 GPT-6.1 Sol / High 執行的主工作階段、原生通用代理調度功能，以及 jq。
只有所選模式需要相應模型時，才須具備 GPT-6 Luna / Max、Terra 6.1 或 Sol 6 / High 的使用權限。Terra 是額外的限定範圍實作者選項；審核仍由 Sol 6 負責。

~~~sh
codex plugin marketplace add Chamotrans/Sol-Advisor-GPT6 --ref main
codex plugin add sol-advisor@sol-advisor
plugin_dir="$(codex plugin list --json | jq -r '.installed[] | select(.pluginId == "sol-advisor@sol-advisor") | .source.path')" && test -n "$plugin_dir" && test "$plugin_dir" != null && test -d "$plugin_dir" && test -f "$plugin_dir/scripts/install-agents.sh" && sh "$plugin_dir/scripts/install-agents.sh"
~~~

配套安裝程式會保留供選用的具名角色設定，並在安裝後逐一核對三個檔案是否完全一致。通用調度直接指定模型，不依賴這些角色。
安裝程式遇到異常會停止操作：經修改、不安全、非一般檔案、符號連結、未知或內容不符的檔案均不會被覆寫。程式不會修改 Codex 設定。更新插件後，請開啟新的 Codex 任務。

在新任務輸入以下提示：

~~~text
Use $sol-advisor:orchestration to build this feature and verify it. Declare the selective route before task tools.
~~~

## 你需要提供甚麼

向 Sol 說明預期成果、限制及重要的儲存庫背景。你毋須自行選擇或管理執行分工；Sol 會記錄所選模式，並負責驗證及驗收。

## 執行模式

| 模式 | 適用情況 | 交付方式 |
|---|---|---|
| `solo` | 預設模式；風險可控。 | 主代理負責規劃、實作、測試及自行覆核。 |
| `delegate` | 已有完整規格，交由一名實作者處理更合適。 | Luna / Max 處理範圍明確的工作；Terra 6.1 處理限定範圍的程式、視覺、電腦操作或寫作工作；Sol 6 / High 處理較高風險工作。主代理負責驗證。 |
| `audit` | 獨立的最終審核比委派實作更重要。 | 主代理實作，再由全新的唯讀 Sol 6 / High 審核。 |
| `full` | 明確屬於範圍廣或風險高的例外情況。 | 一名指定實作者完成工作，主代理驗證，再由全新的 Sol 6 / High 審核。 |

Terra 6.1 使用確切模型 ID `gpt-6.1-terra`，並明確指定當前 Codex 工具支援的推理強度（effort）。毋須額外安裝配套代理。

預設採用 `solo`，輔助代理預設上限為一個；`full` 是須明確選用的例外。
在首次使用任務工具前，Sol 會發出 `SELECTIVE ROUTE` 宣告，列明模式及簡短風險理由。只有新發現的風險足以支持時才可升級模式，不會默默降級。

## 自動處理的工作

Sol 6.1 / High 在主任務中負責架構、工作拆分、模式選擇、主代理驗證、升級判斷及驗收。
輔助代理的工作須取代部分主代理工作，而非重複執行。主代理會檢查完整差異，並重新執行所要求的檢查。
若所選模式包含審核，全新的 Sol 6 / High 審核者會回覆 `ship`、`fix-first` 或 `rethink`；任何修正後均須重新審核。

## 更新

更新本衍生版本、重新整理供選用的配套角色設定，再開啟新任務。若將 marketplace 改回上游儲存庫，本版本的路由規則會被取代。

~~~sh
codex plugin marketplace upgrade sol-advisor
codex plugin add sol-advisor@sol-advisor
plugin_dir="$(codex plugin list --json | jq -r '.installed[] | select(.pluginId == "sol-advisor@sol-advisor") | .source.path')" && test -n "$plugin_dir" && test "$plugin_dir" != null && test -d "$plugin_dir" && test -f "$plugin_dir/scripts/install-agents.sh" && sh "$plugin_dir/scripts/install-agents.sh"
~~~

如需確切的代理啟動方式、執行證據、沙盒、安裝程式及維護驗證詳情，請參閱[進階原生操作指南](plugins/sol-advisor/skills/orchestration/references/operations.md)（英文）。

## 私隱與參與貢獻

提交 commit 時，請使用公開帳號名稱及 GitHub 提供的 noreply 電郵地址。
分享修補檔、issue、截圖或日誌前，須移除個人路徑、聯絡資料、客戶材料、憑證及工作階段逐字紀錄。範例必須使用模擬資料。
忽略規則有助防止誤加檔案，但不會清理已追蹤檔案或 Git 歷史。公開的專案署名及 MIT 版權聲明會予保留。詳見 [LICENSE](LICENSE)。
