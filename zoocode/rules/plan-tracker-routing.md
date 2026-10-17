# Rule: Plan Tracker Routing (計畫追蹤路由與分流規則)

當 Router 解析 `.roo/records/task-execution-plan.md` 並進行任務分流與路由決策時，必須嚴格遵守以下路由判定與導向約束：

## 1. 任務狀態與分流判定 (Routing Conditions)
- **無待處理進度時**：若經解析發現目前沒有要推進的新任務或待處理進度，應將路由直接交由 `process-tracker`，由其接手現有流程狀態的維護與對齊。
- **需要技術剖析與派工決策時**：當任務屬於新建立、結構複雜，或在 `.roo/records/task-execution-plan.md` 中尚未包含對應的調度與拆解細節時，應優先路由至 `architecture-dispatcher` 進行細節剖析與結構化拆解。
- **有明確 Specifier 執行任務時**：當任務在 `.roo/records/task-execution-plan.md` 中已存在明確的細項目標與執行順序時，啟動分流機制，將路由嚴格對應至專屬的 Specifier 模式。

## 2. 任務分流對照表 (Routing Target Matrix)

| 任務核心目標特徵 / 階段 | 目標分流模式 (Custom Mode) |
| :--- | :--- |
| **多維度交叉檢核、相依性剖析與派工決策產出** | `architecture-dispatcher` |
| **介面合約與邊界協議** | `task-specifier-contract` |
| **系統架構、依賴與隔離性實作** | `task-specifier-architecture` |
| **領域規則與狀態轉移邏輯** | `task-specifier-business` |
| **端到端系統功能與整合** | `task-specifier-feature` |
| **測試案例撰寫** | `task-specifier-test` |

## 3. Router 導向約束協議 (Router Guardrails)
- **前置依賴約束**：未經技術剖析與細節納管的任務，禁止直接路由至各個專屬的 Specifier 模式。
- **單一真理來源約束 (SSOT)**：Router 進行路由判定時，其狀態依據與導向依據必須完全來自 `.roo/records/task-execution-plan.md` 本身，確保流程的冪等性與唯一性。
