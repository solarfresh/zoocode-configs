# Rule: Plan Tracker Routing (計畫追蹤路由與分流規則)

當 Router 解析 `.roo/records/task-execution-plan.md` 並進行任務分流與路由決策時，必須嚴格遵守以下路由判定、相依優先度與導向約束：

## 1. 任務狀態與分流判定 (Routing Conditions & File Targets)
- **無待處理進度時**：若經解析發現目前沒有要推進的新任務或待處理進度，應將路由直接交由 `process-tracker`，由其接手現有流程狀態的維護與對齊。
- **需要技術剖析與派工決策時**：當任務屬於新建立、結構複雜，或在 `.roo/records/specs/task-{XX}-dispatch-decision.md` 實體檔案尚不存在時，應優先路由至 `architecture-dispatcher`，產出或更新分流判定檔。
- **有明確 Specifier 執行任務時**：當任務在 `.roo/records/specs/task-{XX}-dispatch-decision.md` 已存在時，Router 必須實地掃描 `.roo/records/specs/` 目錄，依據檔案相依順序進行動態檢查並導向對應模式：
  1. 若不存在 `task-{XX}-contract-spec.md` $\rightarrow$ 導向 `task-specifier-contract` (P1)
  2. 若已存在 Contract 但不存在 `task-{XX}-business-spec.md` $\rightarrow$ 導向 `task-specifier-business` (P2)
  3. 若已存在前兩者但不存在 `task-{XX}-architecture-spec.md` $\rightarrow$ 導向 `task-specifier-architecture` (P3)
  4. 若已存在前三者但不存在 `task-{XX}-feature-spec.md` $\rightarrow$ 導向 `task-specifier-feature` (P4)
  5. 若已存在前四者但不存在 `task-{XX}-test-spec.md` $\rightarrow$ 導向 `task-specifier-test` (P5)
  6. 若上述五項 Spec 檔案皆已完備 $\rightarrow$ 導向 `plan-tracker` 將該主任務標記為 `Completed`

## 2. 任務分流對照表與目標檔案矩陣 (Routing Target Matrix & File Outputs)

| 相依優先度 | 任務核心目標特徵 / 階段 | 目標分流模式 (Custom Mode) | 目標產出檔案格式 (Target Spec File) | 前置依賴約束 |
| :--- | :--- | :--- | :--- | :--- |
| **Pre-Check** | 多維度交叉檢核、相依性剖析與派工決策產出 | `architecture-dispatcher` | `task-{XX}-dispatch-decision.md` | N/A（起始派工） |
| **P1** | 介面合約與 Payload Schema 定義 | `task-specifier-contract` | `task-{XX}-contract-spec.md` | 必須先於 Business 與 Architecture |
| **P2** | 領域規則、業務邏輯與狀態轉移 | `task-specifier-business` | `task-{XX}-business-spec.md` | 必須依賴已產出之 `task-{XX}-contract-spec.md` |
| **P3** | 系統架構、模組邊界、通訊協定與隔離性 | `task-specifier-architecture` | `task-{XX}-architecture-spec.md` | 必須依賴已產出之 Contract 與 Business 檔案 |
| **P4** | 端到端系統功能串接與整合 | `task-specifier-feature` | `task-{XX}-feature-spec.md` | 必須依賴前三者之對應 Spec 檔案 |
| **P5** | 測試案例撰寫與紅綠燈驗證 | `task-specifier-test` | `task-{XX}-test-spec.md` | 必須在所有相關實作 Spec 檔案完備後執行 |

## 3. Router 導向約束協議 (Router Guardrails)
- **前置依賴與檔案約束**：未經架構分派器（architecture-dispatcher）產出對應的 `task-{XX}-dispatch-decision.md` 判定檔與進行細節納管的任務，禁止直接路由至各個專屬的 Specifier 模式；同時必須嚴格遵循檔案相依順序（Contract 檔案 $\rightarrow$ Business 檔案 $\rightarrow$ Architecture 檔案 $\rightarrow$ Feature 檔案 $\rightarrow$ Test 檔案），未完成前置依賴檔案的任務禁止越級路由，確保依賴鏈條完整。
- **單一真理來源約束 (SSOT)**：Router 進行路由判定時，其狀態依據與導向依據必須完全來自 `.roo/records/task-execution-plan.md` 本身，確保流程的冪等性與唯一性。
