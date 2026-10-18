# Rule: Architecture Dispatcher Routing (架構派工路由與分流規則)

當 Router 評估 `.roo/records/task-*-dispatch-decision.md` 的產出狀態並進行任務流轉決策時，必須嚴格遵守以下路由判定與導向約束：

## 1. 任務狀態與分流判定 (Routing Conditions)
- **派工決策產出時**：當任務的多維度交叉檢核已完成，且對應的 `.roo/records/task-*-dispatch-decision.md` 已成功產出或更新時，應將控制權交回給 `plan-tracker`，由其進行全域計畫表（`task-execution-plan.md`）的後續納管與任務排程。

## 2. 任務分流對照表 (Routing Target Matrix)

| 階段 / 決策狀態 | 目標分流模式 (Custom Mode) |
| :--- | :--- |
| **已產出/更新 `task-*-dispatch-decision.md` 決策檔案** | `plan-tracker` |

## 3. Router 導向約束協議 (Router Guardrails)
- **禁止越權直接調度 Specifier**：`architecture-dispatcher` 完成決策後，Router 嚴禁直接將控制權轉交給各個 `task-specifier-*` 模式；控制權必須一律先回到 `plan-tracker`。
- **產出物完備性約束**：僅當對應的決策檔案已實質存在於 `.roo/records/` 內時，Router 才允許觸發向 `plan-tracker` 的流轉。
