# Rule: Process Tracker Routing (Process Tracker 流程追蹤與交接規則)

當 `process-tracker` 維護與追蹤全域 process 執行狀態（`.roo/records/process-execution-status.md`）時，必須遵循以下路由與分派規範：

## 1. 任務分派協議（指派給 Specifier）
1. **讀取判定檔與狀態**：依據 `plan-tracker` 產出的分流判定檔與全域 process 狀態，確認當前任務需要進入的細節規劃階段。
2. **交給對應 Specifier**：當任務進入對應的流程節點時，將控制權與任務需求交給對應的專屬 Task Specifier（`task-specifier-contract`、`task-specifier-architecture`、`task-specifier-business`、`task-specifier-feature`、`task-specifier-test`）執行任務細節拆解與 Spec 產出。

## 2. 任務完成與回饋協議（交還 Plan Tracker）
1. **核對 Specifiers 執行狀態**：持續監控並確認當前任務所對應的所有專屬 Task Specifiers 皆已完成各自的細節 Spec 產出與驗證 Checkpoints。
2. **交回 Plan Tracker**：當某一階段或任務對應的所有 Specifiers 皆圓滿執行完成後，`process-tracker` 必須將控制權與已完成的執行狀態回饋交給 `plan-tracker`。
3. **宏觀進度更新**：由 `plan-tracker` 讀取最新狀態並更新 `.roo/records/task-execution-plan.md` 中的全域宏觀進度與階段性里程碑。
