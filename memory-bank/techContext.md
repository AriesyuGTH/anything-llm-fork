# 技術情境

## 使用的技術

-   **Cline AI**：作為核心的 AI 軟體工程師。
-   **模型情境協定 (MCP)**：一種用於與外部工具和服務整合的協定。MCP 伺服器提供 Cline 可以使用的工具和資源。
-   **JSON**：用於設定檔，例如 `cline_mcp_settings.json` 和 MCP 工具的參數。
-   **Markdown**：用於記憶庫檔案的格式。
-   **Shell 命令**：用於系統操作，例如建立目錄、執行安裝腳本等 (透過 `execute_command` 工具)。
-   **Node.js/NPX**：一種常見的 MCP 伺服器執行環境，如 `sequentialthinking` 伺服器 README 中所示。
-   **Docker**：另一種 MCP 伺服器部署和執行選項。

## 開發設定

-   **工作目錄**：`/workspace/anything-llm-fork`。所有相對路徑都基於此目錄。
-   **記憶庫位置**：`memory-bank/` 子目錄。
-   **MCP 設定檔**：預期為 `cline_mcp_settings.json`，位於工作目錄的根目錄下。
-   **作業系統**：Linux 6.1
-   **預設 Shell**：/bin/bash

## 技術限制

-   Cline 的記憶在會話之間重置，因此完全依賴記憶庫。
-   工具的使用是循序漸進的，一次一個工具，並等待使用者回應。
-   無法直接 `cd` 到不同目錄；需要在 `execute_command` 中使用 `cd ... && ...` 的方式。
-   對檔案系統的存取僅限於提供的工具 (`read_file`, `write_to_file`, `replace_in_file`, `list_files`)。

## 相依性

-   Cline 的運作依賴於使用者環境中可用的工具 (例如 `npx`, `docker`，如果選擇了相應的安裝方法)。
-   MCP 伺服器本身有其自身的相依性，這些相依性通常由其發行套件 (例如 npm 套件) 或 Docker 映像檔管理。

## 工具使用模式

-   **`load_mcp_documentation`**：在開始設定新的 MCP 伺服器之前呼叫，以了解一般程序。
-   **`read_file`**：用於在修改前讀取現有檔案 (例如 `cline_mcp_settings.json`) 或檢查程式碼/文件。
-   **`write_to_file`**：用於建立新檔案 (例如記憶庫檔案) 或完全覆寫現有檔案 (例如，如果 `cline_mcp_settings.json` 是全新的)。
-   **`replace_in_file`**：用於對現有檔案進行有針對性的變更 (例如，向現有的 `cline_mcp_settings.json` 新增伺服器條目)。
-   **`execute_command`**：用於執行 CLI 命令，例如 `mkdir` 來建立目錄，或 `npx ...` / `docker run ...` 來安裝/執行 MCP 伺服器。
    -   `requires_approval` 參數根據命令的潛在影響設定。安全操作 (如 `mkdir`) 可能設定為 `false`，而安裝套件等操作則設定為 `true`。
-   **`use_mcp_tool`**：在 MCP 伺服器設定並執行後，用於與其工具互動。
-   **`ask_followup_question`**：當 Cline 需要使用者提供額外資訊或澄清時使用。
-   **`attempt_completion`**：在任務完成後，用於向使用者呈現結果。
