# Family Site (Static Deploy)

這個資料夾可直接部署到 GitHub Pages / Cloudflare Pages。

## 每日更新流程（手動）

1. 先在主程式完成日內掃描（更新 `intraday_export.json`）。
2. 執行資料匯出：

```bash
./.venv/bin/python export_family_site_data.py
```

3. 會產生：
   - `family_site/data/family_list.json`
   - `family_site/data/candles/<sid>.json`
4. 將 `family_site/` 推上 GitHub（或你的靜態託管平台）。

## 一鍵部署到 GitHub Pages（推薦）

在專案根目錄執行：

```bash
./deploy_family_site.sh
```

這個腳本會：

1. 先跑資料匯出（更新 `family_site/data/*`）
2. 建立 `gh-pages` 工作樹
3. 同步 `family_site/` 內容
4. 自動 commit 並 push 到 `gh-pages`

第一次使用請到 GitHub 倉庫設定：

- `Settings` -> `Pages`
- Source 選 `Deploy from a branch`
- Branch 選 `gh-pages`，Folder 選 `/ (root)`

## 安全版：部署到「獨立公開 repo」（不碰主程式）

如果主 repo 是私有、你只想公開家人看盤頁面，請使用：

```bash
./deploy_family_site_public.sh <公開repo-url> [branch]
```

範例：

```bash
./deploy_family_site_public.sh https://github.com/yourname/Mr-Tsai-family-site.git main
```

這個腳本會：

1. 先匯出 `family_site/data/*`
2. 做基本敏感字串掃描
3. 建立臨時獨立 git 倉庫（只含 `family_site` 內容）
4. 強制推送到你指定的公開 repo 分支

詳見安全檢查清單：`family_site/SECURITY_CHECKLIST.md`

## 本地預覽

```bash
python3 -m http.server 8080 --directory family_site
```

打開 `http://127.0.0.1:8080`。

## 家人登入帳密

目前 `family_site/index.html` 內建預設帳密：

- 帳號：`family`
- 密碼：`tsai1234`

如需修改，請編輯 `index.html` 內這兩行：

- `const AUTH_USER = "...";`
- `const AUTH_PASS = "...";`
