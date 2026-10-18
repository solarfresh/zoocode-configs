以下為你提供 `.roo/routings/transitions/process-to-test.md` 的草稿內容。

這份草稿延續了我們前面確立的原則：不依賴關鍵字、完全由檔案狀態與狀態機（SSOT）驅動、實作與驗證職責分離，並將狀態依據對齊至實作狀態表記錄。

---

# 🚪 Phase Transition: Process Execution -> Testing & Verification

## 1. 觸發情境與目標

* **前置階段**：`task-executor` 已依據實作狀態表與 Spec 完成程式碼變更，並產出對應的實作過程報告（例如 `.roo/records/processes/task-{XX}-[type]-process.md`）。
* **目標階段**：由 `process-tracker` 進行實作狀態的簽收與同步，並正式將任務控制權引導至**獨立的測試與階段性驗證機制**。

---

## 2. 狀態檢核與分流判定 (State Check & Routing)

請讀取 `.roo/records/process-execution-status.md` 與 `.roo/records/processes/` 目錄進行出口閘門判定：

### 情況 A：實作尚未落地或過程報告缺失

* **判定條件**：對應的實作項目在狀態表中尚未標記為完成，或 `.roo/records/processes/` 內找不到對應的實作過程報告。
* **動作與分流**：
* **嚴禁進入測試驗證**。
* 控制權維持在 `process-tracker` 與 `task-executor`，繼續推進程式碼實作與過程記錄。



### 情況 B：實作過程報告已備妥且項目完成（閘門通過）

* **判定條件**：實體過程報告檔已存在，且微觀實作狀態表中的對應項目已更新或準備標記為完成。
* **動作與分流**：
1. `process-tracker` 將 `.roo/records/process-execution-status.md` 中的對應實作項目標記為 `Completed`（或準備進行驗證狀態更新）。
2. 提示使用者或引導進入測試階段：「實作與過程報告已完備。接下來將進入獨立的測試與階段性驗證（如測試案例執行、紅綠燈驗證或對應的驗收流程）。」



---

## 3. 轉銜交接對照表 (Hand-off Matrix)

| 階段 / 輸入檔案狀態 | 目標交接模式 / 機制 | 目標觸發動作 / 產出 | 前置依賴與閘門約束 |
| --- | --- | --- | --- |
| **實作過程報告已產出且狀態更新** | 測試與驗證機制 / 測試專屬模式 | 執行驗證、測試案例比對或紅綠燈檢查 | 必須具備對應的 `task-{XX}-*-process.md` 實體檔案 |
| **驗證完成且無紅燈阻塞** | `plan-tracker` | 將對應任務或子任務推進至最終完成狀態 | 必須通過所有驗證條件 |

---

## 4. Router 導向約束協議 (Router Guardrails)

* **實作與驗證隔離原則**：`process-tracker` 僅負責追蹤程式碼落地的實作過程，嚴禁將測試失敗或驗證邏輯混入純實作狀態表內；測試與驗證必須透過獨立的驗證軌道處理。
* **單一真理來源約束 (SSOT)**：轉銜判定必須嚴格依據 `.roo/records/process-execution-status.md` 中的狀態與實體過程報告檔案，確保狀態切換的明確性與一致性。
