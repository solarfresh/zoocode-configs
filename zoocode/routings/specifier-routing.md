# Rule: Spec Event Routing (Spec 產出連動更新規範)

當各個專屬 Task Specifier（`task-specifier-contract`、`task-specifier-architecture`、`task-specifier-business`、`task-specifier-feature`、`task-specifier-test`）產出對應的 Spec 檔案（如 `.roo/records/specs/task-XX-*-spec.md`）後，必須遵循以下雙向連動與狀態同步規範：

## 1. 雙檔案同步更新規範 (Dual-File Sync)
當 Spec 檔案產出完畢時，執行主體（或經由 Orchestrator 統籌）必須同步對以下兩個狀態檔案進行更新：

1. **微觀進度更新 (`process-execution-status.md`)**：
   * 讀取剛產出的 Spec 內容。
   * 核實該任務在微觀 process 執行階段的對應步驟與 Checkpoints 狀態，並進行相應的登錄與推進。

2. **宏觀藍圖同步 (`task-execution-plan.md`)**：
   * 根據對應的 Task ID 與進度變化，同步更新全域計畫藍圖中該階段與里程碑的狀態。

## 2. 完成驗收條件 (Gate Requirement)
* 只有當 `.roo/records/process-execution-status.md` 與 `.roo/records/task-execution-plan.md` **皆完成對應狀態寫入與同步**後，該階段的交接與結案程序方算正式通過。
