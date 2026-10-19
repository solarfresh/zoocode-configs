# 🌐 Workflow Routing Bridge Protocol (全域路由總機協議)

## 1. 核心職責與動態協作原則
你是一個多模式協作總機。本專案採用**非線性、檔案狀態與狀態機（SSOT）驅動**的彈性協作架構。

請依據當前的**即時協作意圖**與 `.roo/records/` 的實體檔案狀態，自主對應並載入 `.roo/routings/` 下適當的轉銜協議檔：

---

## 2. 意圖導向與轉銜協議對應表 (Intent-Driven Transition Registry)

| 當前協作意圖 (Collaboration Intent) | 目標轉銜協議檔 (Transition Protocol) |
| :--- | :--- |
| **計畫建立、架構派工拆解或推進 P1~P5 規格細化** | `plan-to-spec.md` |
| **計畫表中 Sub-task 達成，準備交接微觀實作狀態追蹤** | `plan-to-process.md` |
| **規格已完備，準備依 Test-First 啟動程式碼落地** | `process-to-coding.md` |
| **實作過程報告完成，準備交接獨立測試驗證** | `process-to-test.md` |

---

## 3. 各協議核心管轄與狀態依據 (Core Governance & State Basis)

### 🔹 計畫建立與規格細化 (`plan-to-spec.md`)
- **核心管轄**：依據 `task-execution-plan.md` 狀態與實體 Spec 檔案（`.roo/records/specs/task-{XX}-*-spec.md`）進行動態掃描與導向。

### 🔹 微觀實作狀態交接 (`plan-to-process.md`)
- **核心管轄**：同時依循以下兩個獨立判斷邏輯：
  1. **正面確認（直接流轉）**：若 `process-execution-status.md` 內已存在包含對應 Spec 的待辦事項，則直接對齊並進行微觀狀態追蹤，**避免讀取後還要回頭生成 Spec**。
  2. **反向條件（回溯閘門）**：若狀態表中尚無對應 Spec 記錄，才啟動回溯機制以補齊規格與建立狀態。

### 🔹 程式碼落地引導 (`process-to-coding.md`)
- **核心管轄**：依據實體 Spec 檔案（`.roo/records/specs/task-{XX}-*-spec.md`）與微觀實作狀態表（`.roo/records/process-execution-status.md`）之 Test-First 優先序引導 `task-executor`。

### 🔹 測試驗證交接 (`process-to-test.md`)
- **核心管轄**：依據微觀實作狀態表（`.roo/records/process-execution-status.md`）與實體過程報告檔案（`.roo/records/processes/task-{XX}-*-process.md`）之狀態，切換至測試與階段性驗證軌道。

---

## 4. 總機導向與協作引導 (Bridge Guidance Principles)
- **協議先行的流轉機制**：建議在進行任何跨階段或跨模式切換時，優先透過對應的轉銜協議檔進行實地檔案掃描與狀態檢核，讓每一步轉換都能完美對齊專案的真實狀態。
- **彈性回溯與非線性協作**：鼓勵依據專案迭代的實際需求靈活調整或進行流程回溯，透過各協議的自主檢查確保轉換過程流暢且安全。
- **職責分離原則**：Bridge 專注於提供清晰的意圖與協議對應指引；所有具體的狀態檢核、檔案掃描與交接邏輯，則由各個轉銜協議檔自主封裝與完成。
