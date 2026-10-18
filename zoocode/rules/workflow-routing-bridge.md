# Workflow Routing Bridge & Autonomous Execution Protocol

## 1. 路由規則來源 (Routing Sources)
本專案的所有自主交接與路由分流邏輯，統一由 `./roo/routings/` 目錄下的以下檔案規範：
- 任務交接協議：`./roo/routings/specifier-routing.md`
- 架構派工路由：`./roo/routings/architecture-dispatcher-routing.md`
- 計畫追蹤與相依優先度：`./roo/routings/plan-tracker-routing.md`
- 流程追蹤與狀態維護：`./roo/routings/process-tracker-routing.md`

## 2. 跨模式自主推進強制協議 (Autonomous Progression Guardrails)
當任何 Custom Mode 完成當前任務或產出對應的 Spec 檔案時，**嚴禁**出現「請問是否開始新工作或結束任務」的中斷問句。必須強制執行以下流程：

1. **自我檢核與路由判定**：
   - 依據 `./roo/routings/plan-tracker-routing.md` 的相依優先度（Pre-Check $\rightarrow$ P1 $\rightarrow$ P2 $\rightarrow$ P3 $\rightarrow$ P4 $\rightarrow$ P5）與 `./roo/routings/process-tracker-routing.md` 的狀態維護機制，自動掃描 `.roo/records/` 目錄，檢查下一個應執行的階段與目標檔案[cite: 3, 4]。
2. **產出交接與導向訊號**：
   - 在回覆的結尾，必須明確列出當前產出的檔案，並依照路由規則指示**下一個應切換的 Custom Mode** 以及**下一步要讀取的檔案路徑**。
3. **無縫銜接指令**：
   - 提供一段可以直接複製或執行的下一步指令，確保切換到下一個 Custom Mode 時，AI 能立刻讀取對應檔案並自主繼續，實現無人工斷點的管線化開發。

## 3. 標準交接輸出區塊格式 (Standard Handoff Block)
每當任務階段告一段落時，回覆結尾必須包含以下結構化區塊，供下一個模式或人工無縫接軌：

```markdown
---
### 🔄 任務交接與路由導向 (Handoff & Routing)
- **當前產出檔案**：`[列出剛剛產出的檔案路徑]`
- **微觀/宏觀同步狀態**：`[確認 task-execution-plan.md 與 process-execution-status.md 是否已更新]`[cite: 1]
- **下一步導向模式**：`[目標 Custom Mode]`
- **建議讀取檔案**：`[目標檔案路徑]`
- **無縫接軌執行指令**：
  > `@<Target-Mode> 請讀取 <Target-File> 並繼續執行下一個階段任務。`
---
```