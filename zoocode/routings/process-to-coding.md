# 🚪 Phase Transition: Specs Complete -> Test-First Coding Execution (Task Executor)

## 1. 觸發情境與目標

- **前置階段**：`plan-tracker` 或 `process-tracker` 經實地掃描與檢核發現，目標任務的各維度規格檔案已完備。
- **目標階段**：由追蹤端正式將控制權交接給 **`task-executor`**，並嚴格遵循「先實作測試（Test-First）」的順序，隨後依序推進其他維度的程式碼落地；實作完成並產出過程報告後，由 **`process-tracker`** 先行確認狀態，再更新 `.roo/records/process-execution-status.md` 的執行狀態。

---

## 2. 狀態檢核與分流判定 (State Check & Routing)

請直接讀取 `.roo/records/process-execution-status.md` 並實地掃描 `.roo/records/specs/` 目錄進行狀態與檔案判定：

### 情況 A：規格尚未完備或缺少必要 Spec 檔案

- **判定條件**：經掃描發現對應的實體 Spec 檔案尚不齊全。
- **動作與分流**：
  - **嚴禁進入 Coding 實作**。
  - 將控制權回歸對應的 `task-specifier-*` 模式補齊缺漏。

### 情況 B：對應 Specs 檔案已備妥，準備進行實作（Test-First 順序）

- **判定條件**：目標任務所需的 Spec 實體檔案皆已存在。
- **實地檔案掃描與「先實作 Test」的優先序分流**：
  1. **【首要執行】若 Test Spec 已存在但測試未落地** $\rightarrow$ **強制優先**引導 `task-executor` 實作對應的測試案例與紅綠燈驗證。
  2. **若 Contract Spec 已存在但合約未實作** $\rightarrow$ 於 Test 完成或搭配後實作合約與介面結構。
  3. **若 Business Spec 已存在但邏輯未實作** $\rightarrow$ 實作領域規則與業務邏輯。
  4. **若 Architecture Spec 已存在但邊界未隔離** $\rightarrow$ 調整模組架構與依賴注入。
  5. **若 Feature Spec 已存在但功能未串接** $\rightarrow$ 進行端到端功能串接。
- **實作後狀態確認與更新**：
  - `task-executor` 完成程式碼落地並產出過程報告後，交由 **`process-tracker`** 先行確認當前執行狀態。
  - 確認無誤後，由 **`process-tracker`** 更新 `.roo/records/process-execution-status.md` 的狀態。

---

## 3. 任務分流與目標過程報告矩陣 (Routing Target Matrix & Process Outputs) (Test-First Adjusted)

| 優先實作階段 | 對應輸入 Spec 檔案 | 目標分流模式 (Custom Mode) | 目標產出過程報告 (Process Report File) | 前置依賴與順序約束 |
| --- | --- | --- | --- | --- |
| **P1 優先實作 (Test-First)** | `task-{XX}-test-spec.md` | `task-executor` | `task-{XX}-test-process.md` | **首要執行**：必須先落地測試案例以建立紅綠燈驗證機制 |
| **P2 實作** | `task-{XX}-contract-spec.md` | `task-executor` | `task-{XX}-contract-process.md` | 配合合約與介面定義進行實作 |
| **P3 實作** | `task-{XX}-business-spec.md` | `task-executor` | `task-{XX}-business-process.md` | 依賴領域規則與業務邏輯落地 |
| **P4 實作** | `task-{XX}-architecture-spec.md` | `task-executor` | `task-{XX}-architecture-process.md` | 依賴模組架構與依賴隔離 |
| **P5 實作** | `task-{XX}-feature-spec.md` | `task-executor` | `task-{XX}-feature-process.md` | 最後進行端到端功能串接 |

---

## 4. Router 導向約束協議 (Router Guardrails)

- **啟動前 Status 確認**：交接至實作或啟動流程前，必須由 `process-tracker` 先行確認 `.roo/records/process-execution-status.md` 的當前狀態與內容。
- **Test-First 強制順序**：在所有實作管線中，**必須優先處理 Test 的落地**（即先實作測試），確保紅綠燈驗證機制先行建立，後續順序則依既定架構推進。
- **無 Spec 絕對禁止 Coding**：未經實地確認 `.roo/records/specs/` 底下對應的 `task-{XX}-[type]-spec.md` 檔案已實質存在前，嚴禁直接將控制權交給 `task-executor`。
- **過程報告強制產出**：`task-executor` 執行完畢後，必須強制產出對應的 `.roo/records/processes/task-{XX}-[type]-process.md` 實體檔案。
- **實作後 Status 確認與更新者**：`task-executor` 執行完畢後，由 `process-tracker` 負責先行確認狀態，並更新 `.roo/records/process-execution-status.md` 的狀態。
