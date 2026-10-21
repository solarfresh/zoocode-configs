# 🛡️ Command Execution & Environment Governance Rule

## 1. 核心原則 (Core Principle)

* **環境隔離與一致性**：所有涉及專案相依性、編譯、測試或執行的終端機指令（Command），嚴禁直接使用系統全域（Global）環境執行。必須明確識別並透過對應的虛擬環境（Conda、Python venv、Node nvm 等）進行封裝與調用。

---

## 2. 虛擬環境識別與啟動規範 (Environment Detection & Activation)

* **自動偵測優先順序**：
1. **Conda Environment**：若專案根目錄存在 `environment.yml`、`conda.yaml`，或當前系統偵測到啟動中的 Conda 標籤，優先使用 Conda 環境。
2. **Python Virtual Environment**：若存在 `.venv/` 或 `venv/` 目錄，優先使用該虛擬環境的直譯器。
3. **Node/Other Runtimes**：依據 `package.json` 或對應工具（如 `nvm`、`poetry`、`pipenv`）決策。

* **Conda 指令執行約束**：
* 在執行任何 Python/Shell 腳本或測試前，必須確保指令是在正確的 Conda 環境中執行。
* 若需透過命令列啟動，建議採用以下安全形式（避免互動式 shell 掛載問題）：
```bash
conda run -n <env_name> <command>

```

或者在確認當前 shell 狀態後，明確帶入對應環境的路徑執行。

---

## 3. 終端機執行防護協議 (Guardrails)

* **絕對禁止事項**：
* ❌ 嚴禁在未激活或未指定 Conda/虛擬環境的情況下直接執行 `pip install`、`python setup.py` 或執行測試套件（如 `pytest`），以免污染全域系統環境。

* **指令輸出與日誌記錄**：
* 執行關鍵指令時，若涉及環境變數或相依套件安裝，必須將執行的完整指令（包含環境前綴）記錄在對應的 process 報告中。
