# SamAlive 調色解析與調色實驗器

人像調色的學習專案，包含兩個純前端網頁：

- **調色解析**（`index.html`）：整理自 JustYøu 頻道的影片〈SamAlive 攝影調色全面解析〉，拆解七個風格特點、八個實作步驟，附 Lab 膚色檢查器與顏色分級互動示範。
- **調色實驗器**（`lab/`）：網頁版的簡易調色軟體，可讀取 JPG、PNG、WebP 與相機 RAW 檔，即時顯示直方圖、波形與向量示波器。

原影片：<https://www.youtube.com/watch?v=MW_fqTK-eow>。本專案的文字是重新整理改寫的教學內容，不含影片逐字稿；影片與其調色風格的權利屬於原作者。

## 調色實驗器功能

- 基本調整：白平衡（含白平衡吸管、灰色世界自動白平衡）、曝光、對比、高光、陰影、白色、黑色、自然飽和度、飽和度
- 曲線：RGB 與紅、綠、藍單通道，快捷「黑色缺失」「黑白皆缺失」
- 混色器：8 個色相的色相、飽和度、明亮度
- 顏色分級：陰影、中間調、高光三個色輪，加上混合與平衡
- 效果：暈影、顆粒
- 示波器：RGB 直方圖（含裁切比例）、明度波形與 RGB 分量、向量示波器（含膚色線）
- Lab 取樣器：顯示 L、a、b 並判斷膚色偏紅或偏黃，可釘選 4 個取樣點
- 原圖對比、分割對比、復原與重做
- 預設：內建 4 組（含 SamAlive 風格），可儲存自訂預設，並匯出、匯入 `.json` 預設檔
- 匯出：原始解析度 JPG
- RAW：由 LibRaw 解碼為 16 位元，以半浮點紋理處理

所有運算都在瀏覽器內完成，照片不會上傳到任何伺服器。

## 本機執行

需要透過本機伺服器開啟（直接雙擊 HTML 無法載入 RAW 解碼器的背景執行緒）：

```bash
npx serve .
```

再用瀏覽器打開顯示的網址。瀏覽器需支援 WebGL 2（新版 Chrome、Edge、Firefox、Safari 皆可）。

## 部署

這是純靜態網站，不需要後端。把整個資料夾部署到任何靜態空間即可，例如 GitHub Pages、Vercel、Netlify、Cloudflare Pages。

- 不需要設定 COOP／COEP 標頭（使用的是單執行緒版本的 LibRaw）
- `.wasm` 檔以 `application/wasm` 類型送出效能較好；上述平台預設都會這樣做

## 加入教學頁的截圖

把截圖放在 `screenshots/` 資料夾，檔名用影片時間的秒數：

| 影片時間 | 檔名 |
|---|---|
| 00:58 | `shot-58.jpg` |
| 04:36 | `shot-276.jpg` |
| 12:39 | `shot-759.jpg` |

支援 `.jpg`、`.png`、`.webp`。頁面載入時會自動顯示；沒有檔案的欄位會顯示佔位框，點擊可以暫時預覽本機圖片（重新整理後消失）。

所有截圖欄位的秒數：58、82、127、149、184、211、227、276、337、454、582、621、649、711、759。

## 預設檔格式

```json
{
  "app": "調色實驗器",
  "version": 1,
  "name": "我的預設",
  "params": { "temp": 6, "exposure": 0.1, "contrast": 28, "curve": { "rgb": [[0, 34], [255, 250]] } }
}
```

`params` 未列出的欄位會使用預設值，超出範圍的數值會自動限制在範圍內。

## 目錄結構

```
index.html                 調色解析教學頁
screenshots/               教學頁截圖（自行加入）
lab/index.html             調色實驗器
lab/vendor/libraw/         LibRaw WebAssembly 解碼器與授權條款
```

## 第三方元件

- **libraw-wasm-nothread** 1.6.0-nothread.1（ISC 授權）：`lab/vendor/libraw/worker.js`、`libraw.wasm`，取自 npm 套件。授權見 `LICENSE-libraw-wasm-nothread.txt`。
- **LibRaw**（LGPL 2.1 或 CDDL 1.0 擇一）：編譯於 `libraw.wasm` 內。授權條款見 `LibRaw-COPYRIGHT.txt`、`LibRaw-LICENSE.LGPL.txt`、`LibRaw-LICENSE.CDDL.txt`。原始碼：<https://github.com/LibRaw/LibRaw>
- 字型：Noto Serif TC、Noto Sans TC、IBM Plex Mono，經 Google Fonts 載入。
