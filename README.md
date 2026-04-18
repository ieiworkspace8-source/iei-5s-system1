# IEI 5S AI 智慧評分系統 — 部署說明

## 系統架構

```
網頁 (Vercel 免費託管)
  └── Firebase Firestore  → 儲存帳號 / 線別 / 稽核紀錄
  └── Firebase Storage    → 儲存標準照圖片
```

---

## 步驟一：建立 Firebase 專案（免費）

1. 前往 https://console.firebase.google.com/
2. 點擊「新增專案」，輸入專案名稱（如：iei-5s-system）
3. 不需要啟用 Google Analytics，直接建立

---

## 步驟二：啟用 Firestore 資料庫

1. 左側選單 → 建構 → **Firestore Database**
2. 點擊「建立資料庫」
3. 選擇「**在測試模式下啟動**」（之後可設定安全規則）
4. 選擇離您最近的伺服器位置（建議選 `asia-east1` 台灣）

---

## 步驟三：啟用 Firebase Storage

1. 左側選單 → 建構 → **Storage**
2. 點擊「開始使用」
3. 選擇「**在測試模式下啟動**」
4. 選擇相同的伺服器位置

---

## 步驟四：取得 Firebase 設定金鑰

1. Firebase 主控台 → 左上角齒輪 → **專案設定**
2. 向下捲動到「您的應用程式」區塊
3. 點擊「</>」（Web）圖示
4. 輸入應用程式暱稱（如：5s-web），點擊「註冊應用程式」
5. 複製 `firebaseConfig` 的內容，貼入 `firebase-config.js`

範例（您的金鑰會不同）：
```js
export const firebaseConfig = {
  apiKey:            "AIzaSyAbc123...",
  authDomain:        "iei-5s-system.firebaseapp.com",
  projectId:         "iei-5s-system",
  storageBucket:     "iei-5s-system.appspot.com",
  messagingSenderId: "123456789",
  appId:             "1:123456789:web:abc123"
};
```

---

## 步驟五：上傳至 Vercel（免費）

### 方法 A：直接拖曳上傳（最簡單）

1. 前往 https://vercel.com/ → 免費註冊
2. 點擊「Add New → Project」
3. 選擇「**Upload**（上傳）」
4. 將整個 `5s-system` 資料夾拖曳上去
5. 點擊「Deploy」
6. 約 30 秒後即可取得網址，如：`https://iei-5s.vercel.app`

### 方法 B：透過 GitHub（推薦，可自動更新）

1. 在 GitHub 建立新 Repository，上傳這兩個檔案
2. Vercel 連結 GitHub，選擇該 Repository
3. 之後只要更新 GitHub，網站自動更新

---

## 步驟六：設定 Firebase 安全規則（建議）

進入 Firebase 控制台 → Firestore → 規則，貼入以下內容：

```
rules_version = '2';
service cloud.firestore {
  match /databases/{database}/documents {
    match /{document=**} {
      allow read, write: if true;  // 測試期間開放
    }
  }
}
```

正式上線後建議改為需要驗證才能存取。

---

## 使用說明

| 功能 | 說明 |
|------|------|
| 預設帳號 | admin / 1234（首次部署自動建立） |
| 新增使用者 | 後台管理 → 帳號管理 |
| 新增線別 | 後台管理 → 線別管理（上傳標準照） |
| AI 評分 | 選擇線別 → 上傳現況照 → 開始評分 |
| 匯出 PDF | 評分後點擊「匯出 PDF 並下載」 |
| 稽核紀錄 | 後台管理 → 稽核紀錄（可匯出 CSV） |

---

## 費用說明

| 服務 | 免費額度 | 說明 |
|------|----------|------|
| Vercel | 完全免費 | 無限靜態網頁 |
| Firebase Firestore | 每日 5萬次讀、2萬次寫 | 一般使用量足夠 |
| Firebase Storage | 5GB 儲存、每月 1GB 下載 | 標準照圖片用 |

> 一個 50 人部門每天使用，月費估計 **$0 元**。

---

## 常見問題

**Q: 資料庫連線失敗怎麼辦？**
A: 確認 `firebase-config.js` 的金鑰正確，且 Firestore/Storage 已在測試模式啟用。

**Q: 圖片上傳後顯示不出來？**
A: 確認 Firebase Storage 規則是否允許讀取。

**Q: 要如何讓全部門同仁使用？**
A: 將 Vercel 提供的網址（如 `https://iei-5s.vercel.app`）傳給大家，加入書籤即可，無需安裝任何軟體。

---

## 檔案清單

```
5s-system/
  ├── index.html          ← 主程式（勿修改，除非需要客製）
  ├── firebase-config.js  ← ★ 填入您的 Firebase 設定
  └── README.md           ← 本說明文件
```
