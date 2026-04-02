# Cursor-Thief-Logic
# Audit Report: Strategic Deception & Resource Throttling in Cursor IDE
# 审计报告：Cursor IDE 的策略性欺诈与资源阉割实锤

This repository provides forensic evidence of technical misappropriation and deceptive billing practices within the **Cursor IDE**. After paying a total of **$1,446.32** ($200 Ultra + $1,246 On-demand), a local audit revealed that Cursor hard-caps premium model capabilities while billing users for full-tier access.

本仓库提供了关于 **Cursor IDE** 技术侵吞和欺诈性计费行为的法证证据。在支付了总计 **1,446.32 美元**（200美元 Ultra 订阅 + 1,246美元按需付费）后，通过本地审计发现：Cursor 在按最高规格收费的同时，硬编码限制了底层模型的能力。
---

🚩 Key Findings / 核心发现

 1. The 30k Token "Cage" (Context Throttling)30k Token “牢笼”（上下文限制）
* **Discovery**: Local logs confirm that while Claude 3 Opus is marketed with a 200k context window, Cursor hard-codes a limit of **30,000 tokens**.
* **发现**：本地日志确认，虽然 Claude 3 Opus 标称拥有 200k 上下文窗口，但 Cursor 在底层将其硬编码限制为 **30,000 tokens**。
* **Evidence**: Found in `chatConfig`: `"fullContextTokenLimit": 30000`.
* **证据**：存在于 `chatConfig` 配置中：`"fullContextTokenLimit": 30000`。
* **Impact**: Users are forced into more frequent "On-demand" calls because the AI "forgets" logic context 85% earlier than promised.
* **影响**：用户被迫进行更频繁的“按需付费”调用，因为 AI 比承诺的时间早 85% 遗忘逻辑上下文。

 2. Deceptive Model Mapping (The Migration Matrix)欺骗性模型映射（迁移矩阵）
* ** Discovery: The modelMigrations array in the internal configuration proves a systematic re-routing of user requests. While the UI is hard-coded to show a premium model, the backend logic executes a "Target Model" swap.
* **发现：内部配置中的 modelMigrations 数组证明了系统存在结构化的请求重定向。虽然 UI 界面被硬编码为显示高端模型，但后端逻辑执行了“目标模型（Target Model）”的调包。
* **The Mapping Logic: Logs show migrateModeModels: true is active for all core functions (Composer, Plan-Execution). This flag allows the system to resolve a user's Claude 3 Opus selection to a different targetModel instance (e.g., Sonnet-Thinking) without updating the UI label.
* **映射逻辑：日志显示 migrateModeModels: true 在所有核心功能（Composer, 计划执行）中均处于激活状态。该旗标允许系统将用户选择的 Claude 3 Opus 静默解析为不同的 targetModel 实例（如 Sonnet-Thinking），前端UI标签保持不变。

* **Evidence of "The Switch": The audit captures a direct mapping where the previousModel (the one the user pays for) is migrated to a targetModel of lower compute cost, yet the billing system still generates premium charges based on the fraudulent UI label.
* **“调包”证据：审计捕获到了直接的映射记录——即 previousModel（用户付费选中的模型）被迁移到了计算成本更低的 targetModel。然而，计费系统仍根据虚假的 UI 标签产生高额费用。

 📸 Visual Evidence Matrix / 证据矩阵图

![Audit Matrix](cursor_thief_new.png)

*The image above correlates the $1,246 bill with the internal 30,000 token cap and the hidden model migration logic.*
*上图将 1,246 美元的账单与内部 30,000 token 的上限以及隐藏的模型迁移逻辑进行了关联对比。*

 📂 Audit Data / 审计数据
The file `cursor_log_data_sanitized.txt` contains extracted JSON strings proving:
文件 `cursor_log_data_sanitized.txt` 包含了提取的 JSON 字符串，证明了：
- **Hard-coded limits** / 硬编码限制 (`fullContextTokenLimit`)
- **Experimental flags** / 控制模型交付的实验组旗标。
- **Backend routing scores** / 用于降级用户请求的后端路由评分逻辑。

Conclusion / 总结 (Final Forensic Edition)
Cursor is not just an IDE; it acts as a high-margin middleware that intercepts premium model capabilities, restricts them to 15% of their capacity (30k vs 200k), and pockets the difference. Through the modelMigrations matrix, the system maintains a "Must-be" UI state—forcing a premium label on the front-end while silently rerouting to lower-cost compute on the back-end.

Cursor 不再仅仅是一个 IDE；它是一个赚取高额差价的中间件。它拦截了顶级模型的能力，将其阉割至 15%（30k vs 200k），并侵吞了其中的对价。通过 modelMigrations 矩阵，系统维持了一种“强制性”的 UI 伪装——在前端强行显示高端标签，后端则静默重定向至低成本算力。

Final Decision: After discovering these hard-coded deceptions and paying $1,446.32 for a throttled experience, I have officially abandoned Cursor. Trust is the only currency in AI development, and Cursor has filed for bankruptcy.

最终决定： 在发现了这些硬编码的欺骗手段，并为这种被阉割的体验支付了 1,446.32 美元后，我已正式放弃使用 Cursor。 在 AI 开发领域，信任是唯一的货币，而 Cursor 已经破产。

Check your own logs. Don't pay for 200k if you are only getting 30k. Don't pay for Opus if the back-end is routing to Sonnet.
检查你自己的日志。如果你只拿到了 30k，就不要为 200k 买单。如果后端路由到了 Sonnet，就不要为 Opus 付费

---
*Audit by [kkjson]*
