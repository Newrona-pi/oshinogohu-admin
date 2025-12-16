# 推しの護符 - 管理画面

このプロジェクトは「推しの護符」の管理者専用サイトです。
絵馬データの閲覧、編集、削除を行うことができます。

## セットアップ

### 1. 環境変数の設定

`.env.local`ファイルを作成し、以下の環境変数を設定してください:

```env
# Firebase Configuration (メインサイトと同じ値を使用)
VITE_FIREBASE_API_KEY=your_api_key
VITE_FIREBASE_AUTH_DOMAIN=your_auth_domain
VITE_FIREBASE_PROJECT_ID=your_project_id
VITE_FIREBASE_STORAGE_BUCKET=your_storage_bucket
VITE_FIREBASE_MESSAGING_SENDER_ID=your_messaging_sender_id
VITE_FIREBASE_APP_ID=your_app_id
VITE_FIREBASE_MEASUREMENT_ID=your_measurement_id

# Admin Password
VITE_ADMIN_PASSWORD=your_secure_password
```

**重要**: メインサイト (`omatsuri-site`) の `.env.local` から Firebase の設定値をコピーしてください。

### 2. 依存関係のインストール

```bash
pnpm install
```

### 3. 開発サーバーの起動

```bash
pnpm dev
```

ブラウザで `http://localhost:5173` にアクセスしてください。

### 4. ビルド

```bash
pnpm build
```

## デプロイ

### Vercelへのデプロイ

1. Vercelアカウントにログイン
2. 新しいプロジェクトを作成
3. GitHubリポジトリを接続（または手動でデプロイ）
4. 環境変数を設定:
   - すべての `VITE_` で始まる環境変数を追加
   - 特に `VITE_ADMIN_PASSWORD` を安全なパスワードに設定
5. デプロイ

### セキュリティ上の注意

- このサイトは**管理者専用**です
- メインサイトとは**完全に別のURL**でデプロイしてください
- URLは第三者に共有しないでください
- 強力なパスワードを設定してください
- 可能であれば、IP制限やBasic認証などの追加のセキュリティ対策を検討してください

## 機能

- ✅ パスワード認証
- ✅ 絵馬データの一覧表示
- ✅ 絵馬の検索（願い事、名前、キャラクター名）
- ✅ 絵馬の並び替え（新着順、古い順、いいね順）
- ✅ 絵馬の編集
- ✅ 絵馬の削除（単一・一括）
- ✅ 統計情報の表示

## ライセンス

Private
