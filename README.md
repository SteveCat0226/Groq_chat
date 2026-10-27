# Groq AI Chat — Streamlit 聊天機器人

使用 **Streamlit** 建立的簡易 AI 聊天網頁，透過 **Groq API** 呼叫 `openai/gpt-oss-120b` 模型，提供繁體中文介面與多輪對話功能。

## 專案功能

- **AI 即時問答**：輸入訊息後，透過 Groq API 取得模型回覆（非串流輸出）。
- **多輪對話**：每次請求會傳送目前對話中的歷史訊息，讓模型參考上下文。
- **聊天記錄顯示**：使用 Streamlit 的聊天元件呈現使用者與 AI 訊息。
- **新聊天**：點擊「新聊天」按鈕即可清除目前工作階段的聊天記錄。
- **錯誤訊息**：呼叫 API 發生例外時，畫面會顯示錯誤內容。

> 聊天記錄保存在 Streamlit 的 `session_state` 中，並未寫入資料庫或檔案；重新建立工作階段後，不保證保留記錄。

## 使用技術

- Python
- [Streamlit](https://streamlit.io/)：網頁聊天介面
- [Groq Python SDK](https://github.com/groq/groq-python)：Groq API 存取
- 模型：`openai/gpt-oss-120b`（以 `Groq_chat.py` 目前設定為準）

## 專案結構

```text
.
├── Groq_chat.py    # Streamlit 主程式
└── README.md       # 專案說明文件
```

## 安裝與執行

### 1. 取得程式碼

將此專案下載或 clone 到本機，並在終端機進入專案資料夾。

### 2. 建立虛擬環境（建議）

```bash
python -m venv .venv
```

啟動虛擬環境：

**Windows（PowerShell）**

```powershell
.venv\Scripts\Activate.ps1
```

**macOS / Linux**

```bash
source .venv/bin/activate
```

### 3. 安裝相依套件

```bash
pip install streamlit groq
```

### 4. 設定 Groq API Key

先前往 [Groq Console](https://console.groq.com/keys) 建立 API Key。此程式透過 `Groq()` 自動讀取環境變數 `GROQ_API_KEY`。

**Windows（PowerShell）**

```powershell
$env:GROQ_API_KEY="你的_Groq_API_Key"
```

**macOS / Linux**

```bash
export GROQ_API_KEY="你的_Groq_API_Key"
```

> **安全提醒**：請勿將 API Key 寫入 `Groq_chat.py`、README 或任何會推送到 GitHub 的檔案。如果使用 `.env` 儲存金鑰，請將 `.env` 加入 `.gitignore`；目前程式並未自行載入 `.env` 檔案。

### 5. 啟動聊天介面

```bash
streamlit run Groq_chat.py
```

啟動後，依終端機顯示的本機網址開啟網頁（通常是 `http://localhost:8501`），即可開始聊天。

## 使用方式

1. 在聊天輸入框輸入問題並送出。
2. 等待 AI 回覆顯示在對話區。
3. 繼續提問，程式會將目前對話歷史一起傳送給模型。
4. 如要重新開始，按下 **「新聊天」** 按鈕。

## 模型參數

`Groq_chat.py` 目前的 API 設定如下：

| 參數 | 設定值 | 說明 |
| --- | --- | --- |
| `model` | `openai/gpt-oss-120b` | 使用的模型 |
| `temperature` | `0` | 偏向穩定、可重現的回答 |
| `max_completion_tokens` | `1024` | 單次回覆的最大輸出 token 數 |
| `top_p` | `0.5` | 控制取樣範圍 |
| `stream` | `False` | 等完整回覆產生後再顯示 |

如需更換模型或調整回覆風格，可修改 `call_llama_model()` 中的參數；請確認欲使用的模型仍受到 Groq API 支援。

## 注意事項

- 使用本專案需要有效的 Groq API Key 與網路連線。
- API 的模型供應、用量限制與費率，請以 [Groq 官方文件](https://console.groq.com/docs) 為準。
- 目前程式僅支援文字聊天，沒有登入、聊天持久化、檔案上傳或串流回覆功能。
- **介面文案提醒**：目前原始碼中有「Llama模型」的描述，但實際指定的是 `openai/gpt-oss-120b`；建議同步更新 `st.write()` 的文案，以免造成誤解。

## 授權

本專案尚未附上授權條款。若要開放他人使用、修改或散布，建議另行新增 `LICENSE` 檔案並選擇合適的開源授權。
