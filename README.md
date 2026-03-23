
一個示範如何將網頁改造為「AI Agent 可操作」的完整範例，使用 Google Chrome 的 WebMCP 標準。

📖 這是什麼？
這個範例是一個虛構的咖啡廳點餐系統，展示如何透過 WebMCP（Web Model Context Protocol） 讓 AI Agent 直接呼叫網頁功能，而不是靠截圖來猜測要點哪個按鈕。

網頁包含哪些功能？
| 分頁     | 功能元件                           |
| ------ | ------------------------------ |
| 🍽️ 菜單 | 搜尋輸入框、分類下拉選單、品項卡片、數量加減按鈕、加入購物車 |
| 📅 訂位  | 姓名／電話／日期輸入框、人數下拉選單、備註欄、確認按鈕    |
| 🛒 購物車 | 品項清單、合計金額、清空購物車、送出訂單           |


AI Agent 可呼叫的 6 個工具
| 工具名稱             | 說明                          |
| ---------------- | --------------------------- |
| switch_tab       | 切換分頁（menu／reservation／cart） |
| search_menu      | 依關鍵字或分類搜尋菜單品項               |
| add_to_cart      | 將指定品項加入購物車                  |
| view_cart        | 查看購物車內容與合計金額                |
| clear_cart       | 清空購物車                       |
| make_reservation | 填寫並送出訂位資料                   |


## 必要條件
需要 Chrome Canary 146+ 並開啟 chrome://flags/#enable-webmcp-testing

⚠️ 一般 Chrome／Edge／Firefox 無法使用 WebMCP 工具呼叫功能（但網頁 UI 仍可正常操作）

建議工具（擇一）
| 用途               | 工具                                        |
| ---------------- | ----------------------------------------- |
| 瀏覽器內測試 WebMCP 工具 | Model Context Tool Inspector（Chrome 擴充功能） |
| 用 AI 驅動工具呼叫      | Cursor 或 Claude Code + webmcp-server 橋接器  |

🚀 快速開始
Step 1：開啟網頁
將 webmcp-demo.html 下載到本機，使用 Chrome 開啟。

需使用本機伺服器開啟（避免瀏覽器安全限制）：
### Python 3
python -m http.server 8080

### viusal studio code
Live Server插件

🤖 如何讓 AI Agent 操作這個網頁
方法一：Model Context Tool Inspector（推薦新手）
在 Chrome Web Store 搜尋並安裝 "Model Context Tool Inspector"

開啟 webmcp-demo.html，點擊右上角擴充功能圖示

側邊欄會自動列出 6 個已註冊的工具與參數說明

手動測試（不需要 AI）：

選擇工具 → 填入 JSON 參數 → 點擊 Execute。例如：

json
工具：switch_tab
參數：{ "tab": "reservation" }
執行後網頁會切換到訂位分頁。

用 Gemini AI 驅動（需要 API Key）：

在擴充功能設定填入 Google AI Studio 的 API Key

側邊欄輸入自然語言，Gemini 會自動選擇並呼叫對應工具

方法二：Claude Code
bash
# 一行指令加入 MCP server
claude mcp add webmcp-server -- npx webmcp-server
加入後在對話中即可直接操作網頁工具。

🧪 完整測試 Prompt
複製以下內容貼入任何支援 WebMCP 的 Agent，可驗證全部 6 個工具：

text
請依照下列步驟操作 Brew Lab Café 網頁：

1. 切換到菜單分頁
2. 搜尋「咖啡」分類的所有品項，告訴我有哪些
3. 把「拿鐵咖啡」加入購物車 2 杯，再加入「卡布奇諾」1 杯
4. 查看購物車，告訴我目前總金額是多少
5. 切換到訂位分頁，用以下資料訂位：
   - 姓名：王小明
   - 電話：0912-345-678
   - 日期：2026-04-05
   - 人數：2 人
   - 備註：希望靠窗座位
6. 最後清空購物車

預期結果：
每一步操作後右下角 Console 出現對應工具呼叫記錄
購物車最終清空，訂位欄位填入完成並顯示成功訊息


頂部 WebMCP 狀態徽章
狀態	說明
🟢 WebMCP 已啟用	環境正確，6 個工具已全部註冊
🟠 不支援	瀏覽器不支援，UI 仍可手動操作
❓ 常見問題

Q：右上角顯示橘色「不支援」
→ 確認使用的是 Chrome 146+，並已開啟 chrome://flags/#enable-webmcp-testing。

Q：工具呼叫後網頁沒有反應
→ 查看右下角 Log Console 是否有紀錄。有紀錄但 UI 沒更新，請重新整理頁面後重試。

Q：這個網頁可以部署到正式環境嗎？
→ WebMCP 目前仍是實驗性草案，不建議用於生產環境，適合學習與概念驗證使用。

# 參考資料
https://github.com/GoogleChromeLabs/webmcp-tools


