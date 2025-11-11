# Cloudflare Pages デプロイ手順

## 方法3: GitHub Actionsで自動デプロイ（推奨・現在設定済み）

このリポジトリには、`main` への push をトリガーに Cloudflare Pages へ自動デプロイするワークフロー（`.github/workflows/deploy-cloudflare-pages.yml`）が含まれています。静的ファイルを安全に抽出して `dist/` に集約し、Cloudflare Pages のプロジェクト `sai-llc` に配備します。

### 事前準備（1回のみ）
- Cloudflare Pages プロジェクトを作成
  - プロジェクト名: `sai-llc`
  - Framework: なし（ビルド不要）
- GitHub Secrets を設定（Repository > Settings > Secrets and variables > Actions）
  - `CLOUDFLARE_ACCOUNT_ID`: Cloudflare アカウントID
  - `CLOUDFLARE_API_TOKEN`: Pages へのデプロイ権限を持つ API Token（Account: Cloudflare Pages - Edit 権限が必要）

### デプロイの流れ
1. `main` に push
2. GitHub Actions が起動し、リポジトリ直下の静的ファイルを `dist/` にコピー（`.github` やドキュメントは除外）
3. `dist/` を Cloudflare Pages（`sai-llc`）へアップロード

### 手動実行
- GitHub の Actions タブから当該ワークフローを選び、「Run workflow」で手動実行も可能です。

### トラブルシュート
- 403/認証エラー: Secrets の `CLOUDFLARE_ACCOUNT_ID` / `CLOUDFLARE_API_TOKEN` が正しいか確認
- プロジェクト名エラー: Pages 側のプロジェクトが `sai-llc` か確認
- 配備内容が空/不足: ワークフローの「Prepare dist」ステップの除外設定やファイル配置を確認

## 方法1: Cloudflare Dashboardから直接アップロード（最も簡単・推奨）

### 手順

1. [Cloudflare Dashboard](https://dash.cloudflare.com/)にログイン
2. 左メニューから「Workers & Pages」を選択
3. 「Pages」タブをクリック
4. 「プロジェクトを作成」ボタンをクリック
5. 「直接アップロード」を選択
6. プロジェクト名を入力（既に作成済み: `sai-llc`）
7. このフォルダ内のすべてのファイルを選択してZIPに圧縮
   ```bash
   # 現在のディレクトリのすべてのファイルをZIPに圧縮
   zip -r deploy.zip . -x "*.git*" "*.md" "sai-company-website.html"
   ```
   （`index.html`、すべての画像ファイルを含める）
8. ZIPファイルをアップロード
9. 「デプロイ」をクリック

## 方法2: Wrangler CLIを使用（今後の更新時におすすめ）

### 現在のプロジェクト情報
- プロジェクト名: `sai-llc`（Dashboardで確認してください）

### 手順

1. プロジェクト名を確認
   ```bash
   wrangler pages project list
   ```
   これで現在のアカウントにあるすべてのプロジェクトが表示されます。

2. プロジェクトをデプロイ
   ```bash
   cd /Users/ryosukeoyama/Documents/saillc
   wrangler pages deploy . --project-name=sai-llc --branch=main
   ```
   
   **注意**: もしプロジェクトが見つからない場合は、Dashboardで正確なプロジェクト名を確認してください。

## デプロイ後の確認

デプロイが完了すると、`https://sai-llc.pages.dev` のURLでアクセスできます。

## カスタムドメインの設定（オプション）

1. Cloudflare Dashboardの「Workers & Pages」→「Pages」→プロジェクトを選択
2. 「カスタムドメイン」タブをクリック
3. ドメイン名を入力して設定

## 注意事項

- すべての画像ファイル（`sai_logo.png`、`sai-hero-bg2.png`、`bizdev.png`、`mk.png`、`sys.png`、`wave.jpg`）が同じディレクトリに含まれていることを確認してください
- `index.html`がルートディレクトリに存在する必要があります

