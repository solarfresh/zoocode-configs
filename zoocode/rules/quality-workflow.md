# 🔄 專案品質與任務協作工作流 (Quality Workflow SOP)

本專案採用快照驅動（Snapshot-driven）與閉環治理（Closed-loop Governance）機制，透過四個專業 Custom Mode 分工合作，實現自動化品質防護與任務推進。

---

## 🏗️ 協同工作流架構 (Pipeline Overview)


```

[1. code-reviewer]      ──> 更新 `.roo/records/code-review.md` (架構與風險快照)
[2. unit-test-runner]   ──> 更新 `.roo/records/unit-test.md` (測試結果快照)
│
▼ (融合交叉分析)
[3. tech-planner]       ──> 更新 `.roo/records/todo-list.md` (價值與風險優先度矩陣)
│
▼ (原子化任務規劃與進度更新)
[4. task-decomposer]    ──> 更新 `.roo/records/task-execution-plan.md` & `todo-list.md` (執行藍圖)

```

---

## ⚙️ 標準操作程序 (Standard Operating Procedure)

### Step 1: 程式碼審查
- **切換模式**：`Code Reviewer`
- **執行動作**：更新 `.roo/records/code-review.md`。

### Step 2: 單元測試
- **切換模式**：`Unit Test Runner`
- **執行動作**：更新 `.roo/records/unit-test.md`。

### Step 3: 優先度排定
- **切換模式**：`Tech Planner & Todo Synthesizer`
- **執行動作**：更新 `.roo/records/todo-list.md`。

### Step 4: 任務拆解
- **切換模式**：`Task Decomposer & Action Planner`
- **執行動作**：更新 `.roo/records/task-execution-plan.md` 與 `.roo/records/todo-list.md`。
