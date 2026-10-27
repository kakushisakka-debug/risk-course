# 💰 理財目標規劃師

以現值（PV）、終值（FV）、每期付款（PMT）為核心的理財試算網站。純靜態、無需建置，所有計算都在瀏覽器端完成。

## 功能
- 理財目錄：退休金、購屋頭期款、子女教育金、緊急預備金、大額目標、貸款還款分析。
- 每個目標提供：目標金額、預估達成金額、每月所需 PMT、達成率、走勢圖與調整建議。
- PV·FV·PMT 計算機：任選一個未知數求解。
- 支援深色模式與手機版面。

## 專案結構
```
finance-planner/
├── public/
│   ├── index.html      # 整個網站（HTML + CSS + JS）
│   └── _headers        # Cloudflare 安全標頭與快取設定（選用）
├── .github/workflows/
│   └── deploy.yml      # GitHub Actions 自動部署（選用）
├── wrangler.toml       # Cloudflare Pages 設定
├── package.json
├── .gitignore
├── LICENSE
└── README.md
```

## 本機預覽
```bash
npm install
npm run dev        # 開啟 http://localhost:8788
```
或直接用瀏覽器開啟 `public/index.html`。

## 上傳到 GitHub
```bash
cd finance-planner
git init -b main
git add .
git commit -m "Initial commit"
git remote add origin https://github.com/<你的帳號>/finance-planner.git
git push -u origin main
```
（請先在 GitHub 建立一個空的 repository，不要勾選 README 或 .gitignore。）

## 部署到 Cloudflare Pages

### 方式 A：Cloudflare 連接 GitHub（最簡單，推薦）
1. Cloudflare Dashboard → Workers & Pages → Create → Pages → Connect to Git。
2. 選擇 `finance-planner` repository。
3. Framework preset 選 `None`；Build command 留空；Build output directory 填 `public`。
4. Save and Deploy。之後每次 push 到 `main` 都會自動更新。
5. 這種方式請刪除 `.github/workflows/deploy.yml`，避免重複部署。

### 方式 B：GitHub Actions 自動部署
1. 先建立 Pages 專案（擇一）：
   - 指令：`npx wrangler pages project create finance-planner --production-branch=main`
   - 或在 Dashboard 以 Direct Upload 建立同名專案。
2. 到 Cloudflare 建立 API Token（權限：Account → Cloudflare Pages → Edit），並複製 Account ID。
3. 在 GitHub repository → Settings → Secrets and variables → Actions，新增：
   - `CLOUDFLARE_API_TOKEN`
   - `CLOUDFLARE_ACCOUNT_ID`
4. push 到 `main` 即自動部署。若你的專案名稱不是 `finance-planner`，請同步修改 `deploy.yml`、`package.json` 與 `wrangler.toml`。

### 方式 C：手動指令部署
```bash
npm install
npx wrangler login
npm run deploy
```

部署完成後會得到 `https://finance-planner.pages.dev`，可在 Pages 專案的 Custom domains 綁定自己的網域。

## 計算公式（月複利、期末投入）
- `FV = PV·(1+r)^n + PMT·((1+r)^n − 1) / r`
- `PMT = (FV − PV·(1+r)^n)·r / ((1+r)^n − 1)`
- 貸款：`PMT = PV·r / (1 − (1+r)^−n)`

其中 r 為月利率、n 為月數。

## 免責聲明
本工具僅供試算與教育用途，報酬率與通膨皆為假設值，不構成任何投資或財務建議。

## 授權
MIT License
