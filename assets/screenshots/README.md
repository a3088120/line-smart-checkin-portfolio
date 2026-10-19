# 實際操作截圖清單

本目錄收錄 n8n 編輯器與 LINE 操作的實際截圖。它們是個別操作畫面，不是完整系統驗收證明。畫面中的遮蔽方式及刻意保留的部分姓氏、時間、地址／地圖展示欄位已由專案負責人確認可公開。

| 檔案 | 畫面內容 | 類型 |
| --- | --- | --- |
| `employee-checkin-workflow.png` | 員工打卡完整 n8n Workflow | 實際 n8n 編輯器 |
| `employee-checkin-logic.png` | 打卡類型判斷、暫存狀態及寫入／回覆分支 | 實際 n8n 編輯器 |
| `employee-photo-processing.png` | 照片上傳、正式紀錄確認及失敗處理 | 實際 n8n 編輯器 |
| `line-checkin-photo-prompt.png` | 定位後提示員工於 90 秒內拍照 | 實際 LINE 操作 |
| `line-checkin-success.png` | 員工照片打卡成功回覆 | 實際 LINE 操作 |
| `line-checkin-photo-timeout.png` | 照片超過 90 秒後，提示打卡尚未完成並要求重新定位 | 實際 LINE 操作 |
| `manager-query-workflow.png` | 主管身份驗證、出勤資料查詢及回覆流程 | 實際 n8n 編輯器 |
| `line-manager-attendance-query.png` | LINE 主管查詢出勤卡片，保留部分姓氏作為展示 | 實際 LINE 操作 |

`assets/architecture.svg` 與 `assets/workflow-diagram.svg` 是根據程式及文件繪製的系統架構／流程示意圖，存放於本目錄的上一層 `assets/`，不是實際操作截圖。
