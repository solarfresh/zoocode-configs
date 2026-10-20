## 📋 待辦清單強制約束 (Mandatory Todo List Protocol)

1. **執行前置檢查 (Pre-Execution Requirement)**：
   - 在任何複雜任務、多步驟任務或開始執行程式碼落地前，Agent **必須**先透過 `update_todo_list` 建立並確認當前任務的待辦清單（Todo List）。
   - **嚴禁**在未建立或未更新待辦清單的情況下直接開始執行後續的大型動作。

2. **進度即時同步 (Real-time Progress Tracking)**：
   - 隨著任務的推進，每完成一個階段或子任務，必須隨時呼叫 `update_todo_list` 將對應項目的狀態更新為完成（Completed）。
   - 確保工作階段中的 `REMINDERS` 待辦清單與實際進度保持 100% 同步。
