# 🚪 Phase Transition: Specs Complete -> Architecture/Contract/Test-First Coding Execution (Task Executor)

## 1. 觸發情境與目標

- **前置階段**：`plan-tracker` 或 `process-tracker` 經實地掃描與檢核發現，目標任務的各維度規格檔案已完備。
- **目標階段**：由追蹤端正式將控制權交接給 **`task-executor`**。`task-executor` 接管後，**必須先讀取並解析對應的 Spec 檔案內容**，並嚴格遵循「Architecture/Contract 先行、Test 其次（TDD）」的依賴順序推進程式碼落地；實作完成並產出過程報告後，由 **`process-tracker`** 先行確認狀態，再更新 `.roo/records/process-execution-status.md` 的執行狀態。

---

## 2. 狀態檢核與分流判定 (State Check & Routing)

請 `process-tracker` 讀取 `.roo/records/process-execution-status.md`、實地掃描 `.roo/records/specs/` 目錄，並檢視 `.roo/records/processes/task-{XX}-[type]-process.md` 進行狀態與任務完成度判定：

### 情況 A：規格尚未完備或缺少必要 Spec 檔案

- **判定條件**：經掃描發現對應的實體 Spec 檔案尚不齊全。
- **動作與分流**：
  - **嚴禁進入 Coding 實作**。
  - 將控制權回歸對應的 `task-specifier-*` 模式補齊缺漏。

### 情況 B：對應 Specs 檔案已備妥，準備進行實作（依依賴順序推進）

- **判定條件**：目標任務所需的 Spec 實體檔案皆已存在。
- **過程報告（Process Report）完成度檢核**：
  - 交接與執行前，必須實地檢查 `.roo/records/processes/task-{XX}-[type]-process.md` 檔案是否存在。若經確認該實體檔案已存在，則直接判定該階段為 Completed，無須重複實作。
- **實地檔案掃描與依賴順序分流**：
  * **【執行前置條件】`task-executor` 必須先完整讀取並理解對應的 `task-{XX}-[type]-spec.md` 內容**，再依下列順序執行：
  1. **【P1 優先實作】若 Architecture Spec 已存在但對應 process 檔案不存在** $\rightarrow$ 優先建立模組邊界、依賴注入機制與系統骨架。
  2. **【P2 優先實作】若 Contract Spec 已存在但對應 process 檔案不存在** $\rightarrow$ 實作合約、介面定義與資料模型。
  3. **【P3 優先實作】若 Test Spec 已存在但對應 process 檔案不存在** $\rightarrow$ **強制優先於邏輯實作**，依據架構與合約撰寫測試案例與紅綠燈驗證機制。
  4. **【P4 實作】若 Business Spec 已存在但對應 process 檔案不存在** $\rightarrow$ 實作領域規則與業務邏輯以通過測試。
  5. **【P5 實作】若 Feature Spec 已存在但對應 process 檔案不存在** $\rightarrow$ 進行端到端功能串接。
- **實作後狀態確認與更新**：
  - `task-executor` 完成程式碼落地並產出過程報告後，交由 **`process-tracker`** 先行確認當前執行狀態。
  - 確認無誤後，由 **`process-tracker`** 更新 `.roo/records/process-execution-status.md` 的狀態。

---

## 3. 任務分流與目標過程報告矩陣 (Routing Target Matrix & Process Outputs) (Dependency-Ordered Adjusted)

| 優先實作階段 | 對應輸入 Spec 檔案 | 目標分流模式 (Custom Mode) | 目標產出過程報告 (Process Report File) | 前置依賴與順序約束 |
| --- | --- | --- | --- | --- |
| **P1 基礎架構** | `task-{XX}-architecture-spec.md` | `task-executor` | `task-{XX}-architecture-process.md` | **基石首要**：`task-executor` 須先讀取 Spec，並建立模組邊界與依賴隔離機制 |
| **P2 介面合約** | `task-{XX}-contract-spec.md` | `task-executor` | `task-{XX}-contract-process.md` | 依賴 P1：定義 Core Interfaces、DTO 與方法簽章 |
| **P3 測試落地 (TDD)** | `task-{XX}-test-spec.md` | `task-executor` | `task-{XX}-test-process.md` | 依賴 P1/P2：**邏輯前強制**，優先落地測試案例以建立紅綠燈驗證機制 |
| **P4 領域邏輯** | `task-{XX}-business-spec.md` | `task-executor` | `task-{XX}-business-process.md` | 依賴 P3：實作核心領域規則與業務邏輯使測試轉綠燈 |
| **P5 功能串接** | `task-{XX}-feature-spec.md` | `task-executor` | `task-{XX}-feature-process.md` | 最後進行端到端應用層功能串接 |

---

## 4. Router 導向約束協議 (Router Guardrails)

- **啟動前 Status 與 Process 報告確認**：交接至實作或啟動流程前，必須由 `process-tracker` 先行確認 `.roo/records/process-execution-status.md` 的當前狀態，並實地檢視 `.roo/records/processes/task-{XX}-[type]-process.md` 確認該階段是否早已完成任務。
- **`task-executor` 執行前強制讀取 Spec**：`task-executor` 在取得控制權後、動手實作前，**必須強制先行讀取並解析對應的 `task-{XX}-[type]-spec.md` 檔案**，確保理解設計意圖與邊界條件。
- **Architecture/Contract 優先於 Test 與 Business**：在實作測試與商業邏輯前，必須確保 Architecture 與 Contract 階段已先行落地，提供合適的依賴邊界與介面定義。
- **Test 必須優先於 Business/Feature**：在商業邏輯與功能串接落地前，**必須強制優先處理 Test 的落地**（即先實作測試），確保紅綠燈驗證機制先行建立。
- **無 Spec 絕對禁止 Coding**：未經實地確認 `.roo/records/specs/` 底下對應的 `task-{XX}-[type]-spec.md` 檔案已實質存在前，嚴禁直接將控制權交給 `task-executor`。
- **過程報告強制產出**：`task-executor` 執行完畢後，必須強制產出對應的 `.roo/records/processes/task-{XX}-[type]-process.md` 實體檔案。
- **實作後 Status 確認與更新者**：`task-executor` 執行完畢後，由 `process-tracker` 負責先行確認狀態，並更新 `.roo/records/process-execution-status.md` 的狀態。
