# 🚪 Phase Transition: Macro Plan & Dispatch -> Spec Pipeline

## 1. 觸發情境與目標

* **前置階段**：`plan-tracker` 解析全域執行計畫表（`.roo/records/task-execution-plan.md`）。
* **目標階段**：依據計畫表內容與實體檔案狀態，動態判定並路由至「無待處理進度」、「架構派工拆解（Dispatch）」或「依序推進與獨立 Spec 產出的規格細化階段（Spec Pipeline）」。

---

## 2. 狀態檢核與分流判定 (State Check & Routing)

請直接讀取 `.roo/records/task-execution-plan.md` 並實地掃描 `.roo/records/specs/` 目錄進行狀態與檔案判定：

### 情況 A：無待處理進度（無新任務或無可推進項目）

* **判定條件**：經解析發現目前沒有要推進的新任務或待處理進度。
* **動作與分流**：
  * 將路由直接交由 **`process-tracker`**，由其接手現有流程狀態的維護與對齊。

### 情況 B：需要技術剖析與派工決策（無 Sub-task 或 Dispatch Decision 尚不存在）

* **判定條件**：任務屬新建立、結構複雜，或對應的實體檔案 `.roo/records/specs/task-{XX}-dispatch-decision.md` 尚不存在。
* **動作與分流**：
  1. 優先路由至 **`architecture-dispatcher`** 進行多維度交叉檢核與派工決策產出。
  2. 產出或更新決策檔案：`.roo/records/specs/task-{XX}-dispatch-decision.md`。
  3. **防呆約束**：`architecture-dispatcher` 產出決策後，控制權必須先交回 `plan-tracker` 更新計畫表並建立 Sub-tasks，**嚴禁越權直接調度 Specifier**。

### 情況 C：Sub-task 為 `Pending` 且派工決策已存在（依序列化與 P4/P5 獨立 Spec 撰寫導向）

* **判定條件**：`task-execution-plan.md` 中 Sub-task 為 `Pending`，且 `.roo/records/specs/task-{XX}-dispatch-decision.md` 已實質存在。
* **實地檔案掃描與相依順序分流**：
  1. **若不存在 `task-{XX}-contract-spec.md`** $\rightarrow$ 導向 **`task-specifier-contract`** (P1)
  2. **若已存在 Contract 但不存在 `task-{XX}-business-spec.md`** $\rightarrow$ 導向 **`task-specifier-business`** (P2)
  3. **若已存在前兩者但不存在 `task-{XX}-architecture-spec.md`** $\rightarrow$ 導向 **`task-specifier-architecture`** (P3)
  4. **若前三者（Contract, Business, Architecture）皆已完備**：
     * **若 `task-{XX}-feature-spec.md` 與 `task-{XX}-test-spec.md` 尚未雙雙完備** $\rightarrow$ 允許依據需求**獨立導向** `task-specifier-feature` (P4) 或 `task-specifier-test` (P5) 進行規格撰寫。
  5. **若上述五項 Spec 檔案皆已完備** $\rightarrow$ 導向 **`plan-tracker`** 將該主任務標記為 `Completed`

---

## 3. 任務分流與目標檔案矩陣 (Routing Target Matrix)

| 相依優先度 | 規格撰寫範疇 (Spec Focus Scope) | 目標分流模式 (Custom Mode) | 目標產出檔案格式 (Target Spec File) | 前置依賴約束 |
| :--- | :--- | :--- | :--- | :--- |
| **Pre-Check** | 多維度交叉檢核、相依性剖析與派工決策規劃 | `architecture-dispatcher` | `task-{XX}-dispatch-decision.md` | N/A（起始派工） |
| **P1** | API 合約與 Payload Schema 規格定義 | `task-specifier-contract` | `task-{XX}-contract-spec.md` | 必須先於 Business 與 Architecture 規格 |
| **P2** | 領域規則、業務邏輯與狀態轉移規格定義 | `task-specifier-business` | `task-{XX}-business-spec.md` | 必須依賴已產出之 `task-{XX}-contract-spec.md` |
| **P3** | 系統架構、模組邊界與隔離性規格定義 | `task-specifier-architecture` | `task-{XX}-architecture-spec.md` | 必須依賴已產出之 Contract 與 Business 規格 |
| **P4 (Independent)** | 功能行為與端到端串接規格定義 | `task-specifier-feature` | `task-{XX}-feature-spec.md` | 必須依賴 P1~P3 規格，可與 P5 獨立撰寫 |
| **P5 (Independent)** | 測試案例與驗收標準規格定義 | `task-specifier-test` | `task-{XX}-test-spec.md` | 必須依賴 P1~P3 規格，可與 P4 獨立撰寫 |

---

## 4. Router 導向約束協議 (Router Guardrails)

* **前置序列化與後段獨立**：P1 至 P3（Contract $\rightarrow$ Business $\rightarrow$ Architecture）維護嚴格的規格產出順序；當 P1~P3 完備後，P4 (Feature) 與 P5 (Test) 的**規格撰寫**允許獨立推進，互不阻塞。
* **嚴禁越權實作**：本轉銜協議僅限於產出與維護 `.roo/records/specs/` 下的規格文件，**嚴禁任何 Specifier 模式直接撰寫專案原始碼或執行程式**。
* **前置依賴與檔案約束**：未經架構分派器（`architecture-dispatcher`）產出對應的 `task-{XX}-dispatch-decision.md` 判定檔與進行細節納管的任務，禁止直接路由至各個專屬的 Specifier 模式。
* **單一真理來源約束 (SSOT)**：Router 進行路由判定時，其狀態依據與導向依據必須完全來自 `.roo/records/task-execution-plan.md` 與實體檔案掃描結果，確保流程的冪等性與唯一性。
