# Cloudflare Pages デプロイ手順

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

