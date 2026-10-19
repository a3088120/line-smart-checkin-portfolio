# 系統架構

## 元件責任

| 元件 | 責任 | 程式／文件依據 |
| --- | --- | --- |
| LINE Messaging API | 接收使用者訊息、位置及照片事件，並傳送流程回覆。 | n8n Webhook、HTTP Request 節點 |
| n8n 主流程 | 驗證事件類型並分流至打卡或查詢子流程。 | `01_Main_Workflow` 匯出中的 Webhook、Switch、IF、Execute Workflow 節點 |
| n8n 員工打卡 | 整理事件、查詢員工／地點／班別及既有紀錄、判斷打卡狀態並處理寫入與回覆。 | `02_Employee_Checkin` 匯出及 JavaScript Code Nodes |
| n8n 主管查詢 | 讀取主管名單、檢查權限、查詢並整理出勤結果。 | `03_Manager_Query` 匯出中的主管權限驗證與結果整理節點 |
| Google Sheets | 保存人員、主管、地點、班別、出勤與照片處理狀態。 | Workflow 的 Google Sheets 節點及 Data Contract |
| Google Drive | 保存打卡照片；查詢流程使用檔案識別資訊組合受限存取連結。 | Workflow 的 Google Drive 節點與主管查詢程式 |
| GAS | 提供管理 API、驗證 LINE 身分與角色、讀寫管理資料及呼叫 Geocoding API。 | `管理介面後端.gs`、`自動抓取經緯度.gs` |
| Cloudflare Pages／Functions | 託管 LIFF 前端，並提供同源 API 閘道至 GAS。 | hosting 專案的 `public/index.html`、`functions/api/[[path]].js` |
| Google Maps Geocoding API | 將管理介面輸入的地址轉換成座標。 | GAS Script Properties 讀取 API Key 後在伺服器端呼叫 |

## 資料流

![系統架構圖](../assets/architecture.svg)

### 員工打卡

LINE Webhook → n8n 主流程分流 → 員工子流程查詢人員、地點、班別與當日紀錄 → 驗證位置並判斷狀態 → Google Sheets／Drive 寫入 → LINE 回覆。

### 主管查詢

LINE Webhook → n8n 主流程辨識查詢 → 主管子流程比對啟用主管 → 通過後讀取出勤資料並整理 → LINE 回覆；未通過則回覆權限不足。

### 管理介面

LINE LIFF → Cloudflare Pages 前端 → 同源 Pages Function 白名單 API → GAS Web App 驗證共享密鑰及 LINE ID Token／角色 → Google Sheets。地址轉座標時由 GAS 伺服器端呼叫 Google Maps Geocoding API。

## 信任邊界

- LINE Channel Access Token 與 n8n Google 憑證應配置在 n8n Credentials；不需在作品集重現。
- 管理介面的共享密鑰只存在 Pages Secret 與 GAS Script Properties，不送到瀏覽器。
- GAS 重新驗證 LINE ID Token 與角色，Pages Function 的同源檢查不取代後端授權。
- 照片使用 Google Drive 權限控制；工作流程文件明確避免為回覆而開啟公開分享。

## 架構圖說明

`assets/architecture.svg` 是根據本機 Workflow、GAS、Pages Function 與設計文件重繪的架構示意，不是 n8n 畫面截圖。圖中不含正式網址、識別碼或憑證值。
