# 🚪 Phase Transition: Macro Plan (Sub-task Complete) -> Process Pipeline

## 1. 觸發情境與目標

* **前置階段**：`plan-tracker` 經實地掃描與檢核發現，`task-execution-plan.md` 中的**其中一項（或多項）Sub-task 已標記為 `Completed**`，且對應的規格檔案已產出。
* **目標階段**：由 `plan-tracker` 將控制權部分或全面移交給 **`process-tracker`**，初始化或更新微觀實作進度表（`.roo/records/process-execution-status.md`），準備由 `task-executor` 進行對應項目的程式碼落地。

---

## 2. 狀態檢核與分流判定 (State Check & Routing)

請直接讀取 `.roo/records/task-execution-plan.md` 與 `.roo/records/specs/` 目錄進行出口閘門判定：

### 情況 A：所有 Sub-task 均尚未完成（無任何 Completed 項目）

* **判定條件**：計畫表中的相關 Sub-task 皆處於 `Pending` 或 `In Progress`，尚未有任何一項達到 `Completed`。
* **動作與分流**：
* 繼續留在規格細化階段，依據 `plan-to-spec.md` 推進對應的 `task-specifier-*` 模式。
* **嚴禁過早移交至 `process-tracker**`。



### 情況 B：至少有一項 Sub-task 標記為 `Completed`（閘門通過）

* **判定條件**：`task-execution-plan.md` 中已經有至少一項 Sub-task 宣告完成，且對應的實體 Spec 檔案已備妥。
* **動作與分流**：
1. 允許將控制權交接至 **`process-tracker`**。
2. 提示 `process-tracker` 讀取已完成項目對應的 Specs，並於 `.roo/records/process-execution-status.md` 建立或對應更新該項目的微觀實作追蹤。



---

## 3. 轉銜交接對照表 (Hand-off Matrix)

| 階段 / 輸入檔案狀態 | 目標交接模式 (Custom Mode) | 目標觸發動作 / 產出 | 前置依賴與閘門約束 |
| --- | --- | --- | --- |
| **至少一項 Sub-task 標註為 `Completed**` | `process-tracker` | 初始化或更新 `process-execution-status.md` 中對應的實作項目 | 必須對應已產出實體檔案的 Spec 項目 |
| **微觀實作項目已建立與排序** | `task-executor` | 開始撰寫程式碼並產出 `task-{XX}-[type]-process.md` | 必須由 `process-tracker` 完成實作任務分發 |

---

## 4. Router 導向約束協議 (Router Guardrails)

* **部分推進原則**：允許非同步、階段性地推進，只要有特定 Sub-task 達成 `Completed`，即可解鎖對應的實作管線。
* **實作狀態隔離原則**：`process-tracker` 僅負責微觀實作進度（`process-execution-status.md`）的排程與追蹤；驗證與測試（Testing/Verification）由獨立機制把關，不混入純實作狀態中。
* **單一真理來源約束 (SSOT)**：轉銜判定必須 strictly 依據 `.roo/records/task-execution-plan.md` 的 Sub-task 狀態標記以及 `.roo/records/specs/` 內的真實檔案，確保狀態切換的冪等性與明確性。
