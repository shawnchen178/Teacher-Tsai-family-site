# 公開展示 Repo 安全檢查清單

這份清單用於「只公開家人看盤網站」，避免把主程式或憑證資料外洩。

## 一、架構原則（固定）

- 主專案 `Mr-Tsai` 維持 **Private**。
- 另建一個公開展示 repo（例：`Mr-Tsai-family-site`）。
- 展示 repo 只放：
  - `family_site/index.html`
  - `family_site/data/family_list.json`
  - `family_site/data/candles/*.json`
- **禁止**主程式碼、`.pkl`、`fugle api.json`、service account JSON、`.venv` 進展示 repo。

## 二、部署前檢查（每次）

- 已在主程式完成日內掃描（`intraday_export.json` 為當日最新）。
- `export_family_site_data.py` 已成功執行。
- `family_site/data/family_list.json` 有更新時間，且筆數合理。
- 展示資料僅含：
  - 代號 / 股名 / 訊號來源 / 訊號強度
  - K 線 OHLC + 成交量
- 不含任何 token、金鑰、帳號密碼字樣。

## 三、部署後檢查（每次）

- 開啟公開網站，確認頁面可正常載入。
- 隨機點 3 檔，K 線資料存在且日期正確。
- 檢查頁面更新時間是否為當日。

## 四、資安建議（一次性）

- 若 `fugle api.json` 或其他敏感資訊曾經上過 GitHub：
  - 先 rotate 金鑰（換新 token）
  - 必要時再做歷史清理
- 公開頁面加註：
  - 「資料僅供參考，非投資建議」
  - 「資料更新時間」

## 五、登入保護注意事項（靜態站）

- 若使用 `index.html` 前端帳密登入（AUTH_USER / AUTH_PASS）：
  - 這屬於「基本門檻」，可擋一般誤入者
  - 不屬於高強度安全（懂技術者仍可逆向查看前端）
- 若需更高安全等級，建議改用：
  - Cloudflare Access / Zero Trust
  - Netlify Identity / Vercel Protected Deployment
