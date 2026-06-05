# ことば

一個單檔靜態前端的日文 N5 單字學習 app。它把傳統翻牌自評升級成主動回想測驗，並用 SRS、錯題回流、連續滑動與即時回饋，讓使用者可以像刷 feed 一樣持續練習。

## 功能特色

- 內建 N5 核心詞庫，包含漢字、讀音、中文意思、例句與分類標籤。
- 兩種主動回想題型：
  - 看日文選中文
  - 看中文選日文
- 每題 4 個選項，干擾選項會優先從同詞性、相近標籤的詞產生。
- SRS 進度支援 `correct`、`hesitant`、`wrong` 三種結果。
- 答錯詞會在同一輪後段回流，並排入近期複習。
- 無每日上限，改成每日打卡與連續學習 flow。
- 每 16 題顯示非阻斷式小結浮層，不中斷學習。
- 卡片堆疊預覽下一張，保留滑動 feed 感。
- 顯示連續答題、最佳連擊、今日練習數、救回錯題數。
- 支援 TTS 日語朗讀：出題時讀一次，答題後再讀一次。
- 支援獎勵音效：答對、連擊、救回錯題、答錯與不熟都有即時聲音回饋。
- 支援鍵盤、滑動與點擊操作。
- 進度保存在瀏覽器 `localStorage`。
- 支援深色與淺色主題。

## 操作方式

### 滑鼠與觸控

- 點選選項：作答。
- 點卡片：標記不熟並揭曉答案。
- 上滑、左滑、右滑：換下一張。
- 答題後點「下一題」：繼續。

### 鍵盤

- `1` 到 `4`：選擇答案。
- `Space`：
  - 未揭曉時：標記不熟並揭曉答案。
  - 已揭曉時：下一題。
  - 小結頁時：繼續學習。
- `Enter`：答題後下一題。
- `ArrowLeft` / `ArrowRight` / `ArrowUp`：換下一張。

## 本地使用

這是純靜態單檔 app，不需要 build system。

直接用瀏覽器開啟：

```bash
open index.html
```

或啟動本地 HTTP server：

```bash
python3 -m http.server 4173
```

然後打開：

```text
http://localhost:4173
```

## 專案結構

```text
.
├── index.html
└── README.md
```

`index.html` 內含：

- HTML 結構
- CSS 樣式
- N5 詞庫資料
- SRS 與 session 邏輯
- localStorage 進度儲存
- TTS、鍵盤、觸控與 UI rendering

## 資料儲存

學習進度保存在目前瀏覽器的 `localStorage`，key 為：

```text
kotoba.study.v1
```

每個單字會保存 SRS 與答題統計，例如：

- `ease`
- `interval`
- `repetition`
- `due`
- `correctCount`
- `wrongCount`
- `hesitantCount`
- `lastResult`
- `lastStudied`

重置進度可以在 app 的設定頁中操作。

## 開發與驗證

檢查 inline script 語法：

```bash
node -e "const fs=require('fs'); const html=fs.readFileSync('index.html','utf8'); const script=html.match(/<script>([\\s\\S]*)<\\/script>/)[1]; new Function(script); console.log('script syntax ok')"
```

檢查 Git diff whitespace：

```bash
git diff --check
```

建議手動驗證：

- 首頁打卡狀態
- 開始學習
- 答對、答錯、不熟揭曉
- 錯題回流
- 連續滑動超過 16 題後的小結浮層
- 再練錯題
- 設定頁
- 獎勵音效開關
- 深色與淺色主題
- 手機 viewport 是否無水平溢出

## 設計方向

這個 app 的目標不是把學習變成壓力型每日任務，而是降低每次繼續的摩擦：

- 不設每日上限
- 不用完成頁打斷學習流
- 用錯題回流與個人化排序維持挑戰
- 用 TTS、微動畫、連擊與滑動回饋維持節奏
