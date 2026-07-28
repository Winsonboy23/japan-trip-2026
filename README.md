# 日本行程互動表（大阪・金澤・高山・京都 2026）

7 人、3 組動線的唯讀行程網站。可依組別（七撰 / 王家）切換只看自己的行程，點與點之間顯示交通工具與花費時間。行程內容寫死在原始碼裡，純靜態、無後端、無任何資料儲存行為。

只有 Winson＋Maggie 兩人的行程預設隱藏，連表頭的日期區間、動線數、進出點與天數編號都跟著收成 10/12 – 10/19 的版本；點標題的「大阪」輸入密碼 `0420` 才會還原成完整的 10/10 – 10/27（重新整理後回到隱藏狀態）。**這只是畫面上的遮蔽**：密碼與行程都寫在 `index.html` 裡，任何人看原始碼都拿得到，不要當成真正的保護。

## 要改行程內容

打開 `index.html`，找到中間標了 `行程內容寫死在這裡` 的 `SEED` 陣列，直接改資料後 `git push`，Zeabur 會自動重新部署。不需要資料庫或後端。

## 檔案

- `index.html` — 整個網站（單檔，含樣式與邏輯，字型走 Google Fonts CDN）

## 推上 GitHub

在本機的專案資料夾裡（把 `index.html` 與 `README.md` 放進去）：

```bash
git init
git add .
git commit -m "init: 日本行程互動表"
git branch -M main
git remote add origin https://github.com/<你的帳號>/<repo 名稱>.git
git push -u origin main
```

## 部署到 Zeabur（含自動部署 = CI/CD）

1. 進 <https://zeabur.com> → 建立專案 → **Deploy service → GitHub** → 選這個 repo。
2. Zeabur 會偵測到純靜態網站（有 `index.html`、無 `package.json`），用 Caddy 直接服務。
3. 部署完成後可到 **Networking / Domains** 綁 `.zeabur.app` 網域或自訂網域。
4. 之後每次 `git push` 到 `main`，Zeabur 會自動重建並重新部署 — 這就是 CI/CD。

### 若沒被自動判定為靜態
到該 service 的 **Settings → Build**，把類型改為 **Static**、輸出目錄設為根目錄 `.`（或 `/`）後重新部署。

### 可選的 Caddy 功能（放在專案根目錄即生效）
- `404.html` — 自訂 404 頁
- `_redirects` — 轉址規則
- `_headers` — 自訂 HTTP headers（語法近似 Netlify）

Zeabur 會自動封鎖 `.git`、`node_modules` 等敏感路徑，不用額外處理。

## 本機預覽

直接用瀏覽器打開 `index.html` 即可；或起一個簡易伺服器：

```bash
python3 -m http.server 8080
# 然後開 http://localhost:8080
```
