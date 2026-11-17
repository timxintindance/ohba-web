# 專案進度與架構日誌 (PROGRESS.md)

本文件為專案的統一進度追蹤看板，所有 Agent（電腦端 Antigravity / 手機端 T3 Code）完成任何任務後，皆須在最上方新增進度紀錄。

---

## 🏗️ 專案現有架構與核心盤點

### 1. 專案核心定位
- **專案名稱**：OPA SPA | 女性專屬帥哥按摩舒壓 官方線上預約系統
- **根目錄路徑**：`c:\Users\timxi\.gemini\antigravity\scratch\opa+str\opa\Opa-line-ai-bot-web`
- **架構性質**：獨立純客人端導流預約網站（無師傅排班端、無主管審核端、無外部雲端資料庫）。
- **預約出單模式**：前端選擇師傅、方案、地點與 24H 48 格開放時段後，點選預約直接透過 LINE 官方 `oaMessage` 語法跳轉喚醒 LINE App，組裝 7 行標準格式文字傳送給真人客服確認（不需電話欄位）。

### 2. 核心檔案與目錄結構
```text
Opa-line-ai-bot-web/
├── AGENTS.md               # 跨環境 AI Agent 協作規範指南 (必讀)
├── PROGRESS.md             # 本檔案：專案架構盤點與接力進度日誌
├── app.py                  # 輕量 FastAPI 後端 (提供客人端頁面與靜態資源 API)
├── run_web.bat / run_web.ps1 # 本機一鍵啟動腳本 (預設 port: 8090)
├── requirements.txt        # Python 依賴套件清單 (fastapi, uvicorn 等)
├── templates/
│   └── index.html          # 客人端單頁核心 HTML (法式輕奢沙龍風、師傅形象牆、3 步驟預訂、LINE 喚醒)
├── static/
│   ├── css/style.css       # 高奢視覺樣式 (香檳金/玫瑰粉)
│   ├── js/customer.js      # 客人端互動邏輯 (時段計算、價格動態試算、oaMessage 組裝)
│   ├── studio/             # 雙北西門私密工作室 4 張實景環境照片 (studio_1 ~ studio_4)
│   └── masters/            # 43 位精選男師相簿 (雙北 / 台中 / 高雄 / 一日男友)
└── data/
    ├── services.json       # 芳療服務療程項目與價格
    ├── pricing.json        # 計價配置 (西門工作室清潔費 $400、加時費、深夜加成、LINE ID: @thestrspa_female)
    ├── masters_profile.json# 43 位師傅身材數據與照片路徑名冊
    ├── reviews.json        # 客戶真實好評資料
    └── bookings.json       # 送單備查紀錄
```

### 3. 業務邏輯重要設定
- **師傅卡片顯示規範**：僅保留「多圖輪播相簿 + 名字 + 身高/體重/年齡 + 滿版指定預約按鈕」，無自介、無稱號、無星星評分標籤。
- **工作室照片呈現**：首頁設有「雙北西門私密工作室環境」專區，並在預約地點「雙北西門工作室」選項旁設有「查看實景照片」按鈕，支援 Lightbox 大圖全螢幕輪播。
- **送單標準格式**（7 行，不含電話）：
  ```text
  師傅：{name}
  日期：{date}
  時間：{time}
  服務城市：{city}
  服務地點：{location}
  費用：NT$ {price}
  備註：{notes}
  ```

---

## 📝 工作進度日誌 (Changelog)

> **填寫規範**：請每次完成任務後，將最新的一筆日誌加在**本區塊的最上方**。

### [2026-11-17 16:13] - 徹底根絕載入延遲：44 位師傅與 6 項服務直入客戶端內嵌與 HTML 預渲染
- **操作者**：Antigravity (電腦端)
- **本次改動內容**：
  1. **師傅名冊 100% 秒開**：將 44 位精選男師資料直接內嵌於 [`static/js/customer.js`](./static/js/customer.js) 的 `EMBEDDED_MASTERS`，初始化時以 0 延遲同步渲染師傅形象牆與指定預約下拉選單，徹底根絕網路等待導致空白的問題。
  2. **服務項目永久可見**：在 [`templates/index.html`](./templates/index.html) 的 `#services-grid` 內直接預先渲染 2 項舒緩按摩與 4 項一日男友方案，即使未連網亦完整展示。
  3. **形象牆標籤對齊名冊**：標籤顯示為「全部 (44位)」、「台北舒緩按摩 (33位)」、「台中舒緩按摩 (8位)」、「高雄舒緩按摩 (5位)」、「💖 一日男友 (6位)」，並支援複合分類過濾。
  4. 升級腳本版本至 `v=6.0`，已推送到 GitHub 遠端 [`timxintindance/ohba-web`](https://github.com/timxintindance/ohba-web)。
- **異動檔案**：
  - 更新：[`templates/index.html`](./templates/index.html)
  - 更新：[`static/js/customer.js`](./static/js/customer.js)
  - 更新：[`Opa-github-pages/index.html`](../Opa-github-pages/index.html)
  - 更新：[`Opa-github-pages/static/js/customer.js`](../Opa-github-pages/static/js/customer.js)
  - 更新：[`PROGRESS.md`](./PROGRESS.md)
- **下一步待辦 / 待討論事項**：
  - 請使用者重新整理瀏覽器驗證 44 位師傅與服務項目秒開效果。

### [2026-11-17 16:00] - 品牌名稱校正：全面由 OPA 更正為 OHBA SPA
- **操作者**：Antigravity (電腦端)
- **本次改動內容**：
  1. 依使用者指示，將全站品牌名稱全面正名為 **`OHBA SPA`**（歐爸）：
     - 網頁標題 Title、頂部奢雅導覽列 LOGO H1 與頁尾版權宣告均更新為 **`OHBA SPA`**。
     - 系統預設計價配置 [`data/pricing.json`](./data/pricing.json) 與後端 API 配置 [`app.py`](./app.py) 品牌名稱正名。
  2. 已同步推送更新至 GitHub 儲存庫 [`timxintindance/ohba-web`](https://github.com/timxintindance/ohba-web)，GitHub Pages 自動重新建置生效。
- **異動檔案**：
  - 更新：[`templates/index.html`](./templates/index.html)
  - 更新：[`data/pricing.json`](./data/pricing.json)
  - 更新：[`app.py`](./app.py)
  - 更新：[`Opa-github-pages/index.html`](../Opa-github-pages/index.html)
  - 更新：[`Opa-github-pages/data/pricing.json`](../Opa-github-pages/data/pricing.json)
  - 更新：[`PROGRESS.md`](./PROGRESS.md)
- **下一步待辦 / 待討論事項**：
  - 協助使用者確認瀏覽器端快取清除與正式網站造訪體驗。

### [2026-11-17 15:48] - 正式網站上線：完成 GitHub Pages 靜態架構建立與部署 (ohba.ohbaspa.com.tw)
- **操作者**：Antigravity (電腦端)
- **本次改動內容**：
  1. 成功建立與男客站 `str-web`（`Str-github-pages`）完全對稱的靜態專案目錄 [`Opa-github-pages`](../Opa-github-pages)。
  2. 強化 [`static/js/customer.js`](./static/js/customer.js)：新增本機 API 與線上靜態 JSON 檔案自動無縫切換機制（`pricing.json`、`services.json`、`masters_profile.json`、48 格全時段自動產生與 `reviews.json`）。
  3. 配置 `CNAME` 為 `ohba.ohbaspa.com.tw`、SEO `robots.txt` 與 `sitemap.xml`。
  4. 成功將 70 個完整網站檔案推送到 GitHub 倉庫 [`timxintindance/ohba-web`](https://github.com/timxintindance/ohba-web) 的 `main` 分支。
  5. 透過 GitHub Pages API 正式啟動託管服務，綁定自訂子網域 `ohba.ohbaspa.com.tw`。
- **異動檔案**：
  - 新增：[`CNAME`](../Opa-github-pages/CNAME) (`ohba.ohbaspa.com.tw`)
  - 新增：[`robots.txt`](../Opa-github-pages/robots.txt)、[`sitemap.xml`](../Opa-github-pages/sitemap.xml)
  - 更新：[`static/js/customer.js`](./static/js/customer.js) (靜態/API 雙模自適應)
  - 更新：[`templates/index.html`](./templates/index.html) (相對路徑化，升級 v=5.0)
  - 更新：[`PROGRESS.md`](./PROGRESS.md)
- **下一步待辦 / 待討論事項**：
  - 引導使用者於 Wix DNS 管理後台新增 CNAME 紀錄指向 `timxintindance.github.io`。

### [2026-11-17 15:35] - 同步 Google 雲端硬碟師傅名冊 (I:\我的雲端硬碟\客戶端\女客)
- **操作者**：Antigravity (電腦端)
- **本次改動內容**：
  1. 完整比對 `I:\我的雲端硬碟\客戶端\女客` 下 4 個分類資料夾（一日男友 6 人、台中 8 人、台北 33 人、高雄 5 人）：
     - **新增師傅**：新增台北師傅 **Allen**（174cm / 70kg / 30歲），照片已同步拷貝至 `static/masters/taipei/Allen.jpg`。
     - **數據更正**：更新一日男友/台北 **Wayne** 身高為 177cm（原 180cm，對齊雲端 `Wayne_177_68_21.jpg`）。
     - **相簿補齊**：為 **Circle** 補上台北工作室相簿照片 `static/masters/taipei/Circle.jpg`（支援多圖輪播）。
  2. 師傅資料庫由原本 43 位擴充至 **44 位**（台北 33 位、台中 8 位、高雄 5 位、一日男友 6 位跨區），與 Google Drive 女客名單 100% 精準對齊無遺漏。
  3. 前端 JS 版本更新為 `v=4.7`，最新 API 與頁面已即時生效。
  4. 最新公網測試網址：`https://orange-shine-appointed-expanded.trycloudflare.com`。
- **異動檔案**：
  - 新增：[`static/masters/taipei/Allen.jpg`](./static/masters/taipei/Allen.jpg)
  - 新增：[`static/masters/taipei/Circle.jpg`](./static/masters/taipei/Circle.jpg)
  - 更新：[`data/masters_profile.json`](./data/masters_profile.json)
  - 更新：[`templates/index.html`](./templates/index.html)
  - 更新：[`PROGRESS.md`](./PROGRESS.md)
  - 更新：[`tunnel_url.txt`](./tunnel_url.txt)
- **下一步待辦 / 待討論事項**：
  - 請使用者確認 Allen 新上架卡片與師傅名冊。

### [2026-11-17 15:23] - 文案調整：刪除「卸下防備，給自己一場奢寵之約」
- **操作者**：Antigravity (電腦端)
- **本次改動內容**：
  1. 依使用者需求，刪除首頁 Hero 特色區塊的大標題 `<h2>卸下防備，給自己一場奢寵之約</h2>`。
  2. 頁面視覺由英文字徽標直接銜接簡潔的師傅專業手法介紹與地點規格徽章，版面更為俐落清爽。
  3. 實測驗證：首頁 HTML 已無該標題文字，載入正常。
  4. 最新公網測試網址：`https://agreements-topic-enters-letting.trycloudflare.com`。
- **異動檔案**：
  - 更新：[`templates/index.html`](./templates/index.html)
  - 更新：[`PROGRESS.md`](./PROGRESS.md)
  - 更新：[`tunnel_url.txt`](./tunnel_url.txt)
- **下一步待辦 / 待討論事項**：
  - 請使用者確認首頁視覺排版。

### [2026-11-17 15:20] - 文案調整：全面移除「法式輕奢沙龍」
- **操作者**：Antigravity (電腦端)
- **本次改動內容**：
  1. 依使用者需求，全面去除「法式輕奢沙龍」相關文字：
     - 頂部導覽列標題下方副標：改為純粹簡潔的「女性專屬帥哥按摩舒壓」。
     - Hero 英文標籤：由 `FRENCH LUXURY SALON · PRIVATE & RELAX` 改為 `EXCLUSIVE FOR WOMEN · PRIVATE & RELAX`。
     - 系統預設配置：[`data/pricing.json`](./data/pricing.json) 與 [`app.py`](./app.py) 中的 `brand_subtitle` 更新為「女性客群專屬 · 精選帥哥芳療舒壓」。
     - 首頁 JS 引入版本升級為 `v=4.6` 防止快取。
  2. 驗證首頁與 API 回傳內容均已乾淨去除「法式輕奢沙龍」與「FRENCH LUXURY」。
  3. 最新公網測試網址：`https://unsigned-presence-advocate-feel.trycloudflare.com`。
- **異動檔案**：
  - 更新：[`templates/index.html`](./templates/index.html)
  - 更新：[`data/pricing.json`](./data/pricing.json)
  - 更新：[`app.py`](./app.py)
  - 更新：[`PROGRESS.md`](./PROGRESS.md)
  - 更新：[`tunnel_url.txt`](./tunnel_url.txt)
- **下一步待辦 / 待討論事項**：
  - 請使用者確認頂部與 Hero 區塊精簡後的視覺效果。

### [2026-11-17 15:10] - 文案調整：全面移除「私密工作室」，統一改為「工作室」
- **操作者**：Antigravity (電腦端)
- **本次改動內容**：
  1. 依使用者需求，全面去除「私密」贅字，改為乾淨俐落的「工作室」：
     - 跑馬燈：「西門工作室（清潔費 $400，無乾濕分離）」
     - Hero 規格標籤：「雙北西門工作室（查看實景環境）」
     - 實景展示區標題：「雙北西門工作室環境」與「獨立包廂」
     - 預約地點選項：「雙北西門工作室」
     - 底部宣告：「雙北西門工作室」
     - LINE 送單預約文字：「服務地點：雙北西門工作室」
  2. 同步更新檔案：[`templates/index.html`](./templates/index.html)、[`static/js/customer.js`](./static/js/customer.js)（版本升級為 `v=4.5`）、[`data/pricing.json`](./data/pricing.json)。
  3. 驗證首頁與 LINE 送單格式均無殘留「私密工作室」字眼。
  4. 最新公網測試網址：`https://raises-heater-decision-changes.trycloudflare.com`。
- **異動檔案**：
  - 更新：[`templates/index.html`](./templates/index.html)
  - 更新：[`static/js/customer.js`](./static/js/customer.js)
  - 更新：[`data/pricing.json`](./data/pricing.json)
  - 更新：[`PROGRESS.md`](./PROGRESS.md)
  - 更新：[`tunnel_url.txt`](./tunnel_url.txt)
- **下一步待辦 / 待討論事項**：
  - 請使用者確認新版文案與預約出單文字呈現。

### [2026-11-17 15:05] - 更新官方 LINE 帳號 ID 為 @973sfpws
- **操作者**：Antigravity (電腦端)
- **本次改動內容**：
  1. 更新官方 LINE ID：原 `@thestrspa_female` 全面切換為正確官方帳號 **`@973sfpws`**。
  2. 同步更新檔案：[`data/pricing.json`](./data/pricing.json)、[`app.py`](./app.py)、[`static/js/customer.js`](./static/js/customer.js) 及首頁 JS 版本號（`v=4.4` 防快取）。
  3. 驗證連線：`https://line.me/R/oaMessage/@973sfpws/?...` 可順暢喚醒 LINE App 並自動帶入 7 行預約單。
  4. 最新公網測試網址：`https://hardwood-appendix-sponsors-boys.trycloudflare.com`。
- **異動檔案**：
  - 更新：[`data/pricing.json`](./data/pricing.json)
  - 更新：[`app.py`](./app.py)
  - 更新：[`static/js/customer.js`](./static/js/customer.js)
  - 更新：[`templates/index.html`](./templates/index.html)
  - 更新：[`PROGRESS.md`](./PROGRESS.md)
  - 更新：[`tunnel_url.txt`](./tunnel_url.txt)
- **下一步待辦 / 待討論事項**：
  - 請使用者以手機點擊「送出預約、待專人確認」按鈕測試 LINE 跳轉。

### [2026-11-17 14:15] - 排除載入失敗問題並重啟 TryCloudflare 持續通道
- **操作者**：Antigravity (電腦端)
- **本次改動內容**：
  1. 根因診斷：先前對話輪次切換時穿透隧道中斷，導致外部連線回報 530 / 連線逾時無法完整載入。
  2. 建立常駐背景穿透任務 (`task-448`)，穩定穿透本機 8090 FastAPI 服務。
  3. 實測驗證：首頁 HTML (39KB)、43 位師傅 API、服務方案、CSS/JS 與工作室實景圖片均順暢秒開 (HTTP 200)。
  4. 工作室環境已確認位於「師傅形象牆」下方、「線上預約系統」上方。
  5. 最新公網測試網址：`https://habits-friendly-organizer-redeem.trycloudflare.com`。
- **異動檔案**：
  - 更新：[`PROGRESS.md`](./PROGRESS.md)
  - 更新：[`tunnel_url.txt`](./tunnel_url.txt)
- **下一步待辦 / 待討論事項**：
  - 請使用者以手機或瀏覽器開啟新網址測試工作室排版位置與預約出單流程。

### [2026-11-17 14:10] - 將工作室實景環境由頂端移至形象牆下方 (對齊 STR)
- **操作者**：Antigravity (電腦端)
- **本次改動內容**：
  1. 將「雙北西門私密工作室環境」（4 張實景照片與燈箱預覽）自頂端 Hero 下方移除，改移至「師傅形象牆 (Master Gallery)」與「線上預約系統 (Booking Section)」之間。
  2. 頁面瀏覽順序優化：客人進站先看高顏值男師陣容 -> 接續查看西門工作室隱私實景環境 -> 進行線上預約，體驗更加自然。
  3. 重新啟動 TryCloudflare 外部穿透隧道提供即時線上測試。
- **異動檔案**：
  - 更新：[`templates/index.html`](./templates/index.html)
  - 更新：[`PROGRESS.md`](./PROGRESS.md)
- **下一步待辦 / 待討論事項**：
  - 請使用者確認移動後的工作室區塊排版位置。

### [2026-11-17 14:05] - 啟用 TryCloudflare 公網安全穿透測試
- **操作者**：Antigravity (電腦端)
- **本次改動內容**：
  1. 調用本機 Cloudflared 服務建立 Quick Tunnel，穿透本地 8090 連接埠。
  2. 產出外部公網測試網址，支援手機端與外部網路隨點即測（已驗證 43 位師傅、服務清單、時段連線與 Lightbox 燈箱均正常連線）。
- **異動檔案**：
  - 更新：[`PROGRESS.md`](./PROGRESS.md)
- **下一步待辦 / 待討論事項**：
  - 待使用者線上確認排版效果，測試完畢可隨時關閉隧道或轉移部署。

### [2026-11-17 14:00] - 預約排程區塊排版對齊 STR（維持香檳金配色）
- **操作者**：Antigravity (電腦端)
- **本次改動內容**：
  1. 對照 `https://str.ohbaspa.com.tw/#booking-section` 調整 3-STEP 預約卡片排版架構：
     - **第一步**：保留「指定師傅下拉」+「服務方案卡片」+「加時加成 ADD-ON」，將服務地點獨立移至第二步，點擊按鈕為「下一步 · 選擇時間與地點」。
     - **第二步**：頂部整合「選擇時間」標題與時長金額徽章；日期橫向捲軸整合年月與「每日全時段開放預約」；時段網格寬版大按鈕化 (2~3列) 支援自動選中熱門檔期；整合「雙北西門工作室/外出服務」地點單選與外出詳細地址輸入框；備註與即時摘要小卡。
     - **第三步**：頂部設置綠色 LINE 送出大按鈕 (`送出預約、待專人確認`)；下方設置「若沒有跳轉請直接複製以下」代碼卡片、一鍵複製綠色按鈕與複製成功 Toast 提示；返回修改資料按鈕與摘要小卡。
  2. 保持 OPA 高奢法式沙龍香檳金與玫瑰暖色系不變。
  3. 同步重構 `customer.js` 的地點聯動、時段自動選中與複製 Toast 提示。
- **異動檔案**：
  - 更新：[`templates/index.html`](./templates/index.html)
  - 更新：[`static/js/customer.js`](./static/js/customer.js)
  - 更新：[`PROGRESS.md`](./PROGRESS.md)
- **下一步待辦 / 待討論事項**：
  - 待使用者確認新排版視覺與操作流暢度。

### [2026-11-17 13:50] - 本地測試站運行與功能點選驗證完成
- **操作者**：Antigravity (電腦端)
- **本次改動內容**：
  1. 啟動本機 Uvicorn 服務驗證客人端預約介面、43 位師傅相簿切換、工作室實景圖與 24H 48 個開放時段。
  2. 使用者實際點選測試完成，測試伺服器已正常關閉，本機無殘留臨時檔案。
- **異動檔案**：
  - 更新：[`PROGRESS.md`](./PROGRESS.md)
- **下一步待辦 / 待討論事項**：
  - 依使用者後續指令進行：推送到 GitHub 倉庫、轉為純靜態 GitHub Pages 部署，或進行版面細節微調。

### [2026-11-13 10:35] - 初始化跨環境 Agent 協作機制
- **操作者**：Antigravity (電腦端)
- **本次改動內容**：
  1. 建立 `AGENTS.md`：明訂跨環境（Antigravity 與 T3 Code）開工前檢查、收工後記錄及防呆安全邊界守則。
  2. 建立 `PROGRESS.md`：盤點目前專案 43 位師傅名冊、純客人端架構、LINE oaMessage 導流出單規範與核心檔案。
- **異動檔案**：
  - 新增：[`AGENTS.md`](./AGENTS.md)
  - 新增：[`PROGRESS.md`](./PROGRESS.md)
- **下一步待辦 / 待討論事項**：
  - 專案待命開發中。後續可依需求推進：推送到 GitHub 倉庫、轉為純靜態 GitHub Pages 部署，或串接至 Wix 官網。
