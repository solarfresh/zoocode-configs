# Rule: Specifier Routing (Task Specifiers 路由與交接規則)

當各個專屬 Task Specifier（`task-specifier-contract`、`task-specifier-architecture`、`task-specifier-business`、`task-specifier-feature`、`task-specifier-test`）完成對應的 Spec 檔案產出後，必須遵循以下路由交接規範：

## 1. 任務交接協議
1. **交接辦結**：當專屬 Spec 檔案（如 `.roo/records/tasks/task-XX-*-spec.md`）產出後，即完成該 Specifier 之階段性任務。
2. **檔案移交**：將該 Spec 檔案的路徑與產出狀態移交給 `process-tracker`，由其自主接手後續的狀態追蹤與登錄作業，Specifier 不得越權干涉。
