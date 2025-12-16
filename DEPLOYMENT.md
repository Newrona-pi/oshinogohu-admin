# 管理者サイトのデプロイ手順

## 概要

「推しの護符」の管理者用ページを別サイトとして分離し、セキュリティを強化しました。

### プロジェクト構成

```
c:\omairi_app\
├── omatsuri-site/      # メインサイト（ユーザー向け）
└── omatsuri-admin/     # 管理者サイト（管理者専用）
```

## セットアップ手順

### 1. 環境変数の設定

`omatsuri-admin`フォルダに`.env.local`ファイルを作成してください:

```bash
cd c:\omairi_app\omatsuri-admin
```

`.env.local`ファイルを作成し、以下の内容を記述:

```env
# Firebase Configuration (メインサイトの .env.local から値をコピー)
VITE_FIREBASE_API_KEY=<メインサイトと同じ値>
VITE_FIREBASE_AUTH_DOMAIN=<メインサイトと同じ値>
VITE_FIREBASE_PROJECT_ID=<メインサイトと同じ値>
VITE_FIREBASE_STORAGE_BUCKET=<メインサイトと同じ値>
VITE_FIREBASE_MESSAGING_SENDER_ID=<メインサイトと同じ値>
VITE_FIREBASE_APP_ID=<メインサイトと同じ値>
VITE_FIREBASE_MEASUREMENT_ID=<メインサイトと同じ値>

# Admin Password (強力なパスワードを設定)
VITE_ADMIN_PASSWORD=<安全なパスワード>
```

**重要**: `omatsuri-site\.env.local`から Firebase の設定値をコピーしてください。

### 2. ローカルでの動作確認

```bash
cd c:\omairi_app\omatsuri-admin
pnpm dev
```

ブラウザで `http://localhost:5173` にアクセスし、管理画面が正常に動作することを確認してください。

### 3. Vercelへのデプロイ

#### 3-1. GitHubリポジトリの準備（推奨）

管理者サイト専用の**プライベートリポジトリ**を作成することを推奨します:

```bash
cd c:\omairi_app\omatsuri-admin
git init
git add .
git commit -m "Initial commit: Admin site"
git remote add origin <新しいプライベートリポジトリのURL>
git push -u origin main
```

#### 3-2. Vercelでのプロジェクト作成

1. [Vercel](https://vercel.com)にログイン
2. 「New Project」をクリック
3. GitHubリポジトリを接続（または手動でデプロイ）
4. プロジェクト設定:
   - **Framework Preset**: Vite
   - **Root Directory**: `./` (リポジトリのルート)
   - **Build Command**: `pnpm build`
   - **Output Directory**: `dist`

#### 3-3. 環境変数の設定

Vercelのプロジェクト設定で、以下の環境変数を追加:

| 変数名 | 値 |
|--------|-----|
| `VITE_FIREBASE_API_KEY` | Firebaseの値 |
| `VITE_FIREBASE_AUTH_DOMAIN` | Firebaseの値 |
| `VITE_FIREBASE_PROJECT_ID` | Firebaseの値 |
| `VITE_FIREBASE_STORAGE_BUCKET` | Firebaseの値 |
| `VITE_FIREBASE_MESSAGING_SENDER_ID` | Firebaseの値 |
| `VITE_FIREBASE_APP_ID` | Firebaseの値 |
| `VITE_FIREBASE_MEASUREMENT_ID` | Firebaseの値 |
| `VITE_ADMIN_PASSWORD` | **強力なパスワード** |

**セキュリティ上の注意**:
- `VITE_ADMIN_PASSWORD`は推測されにくい強力なパスワードを設定してください
- 環境変数は絶対にGitHubにコミットしないでください

#### 3-4. デプロイ

「Deploy」ボタンをクリックしてデプロイを開始します。

デプロイが完了すると、管理者専用のURLが発行されます（例: `https://omatsuri-admin-xxxxx.vercel.app`）

### 4. セキュリティ対策

#### 必須対策

- ✅ **強力なパスワード**: 推測されにくいパスワードを設定
- ✅ **URLの秘匿**: 管理者サイトのURLは第三者に共有しない
- ✅ **HTTPS**: Vercelは自動的にHTTPSを有効化

#### 推奨対策

1. **IP制限**: Vercelの有料プランでIP制限を設定
2. **Basic認証**: Vercel の Edge Middleware で Basic 認証を追加
3. **アクセスログの監視**: 不審なアクセスがないか定期的に確認

### 5. メインサイトの変更点

メインサイト（`omatsuri-site`）から管理者関連のコードを完全に削除しました:

- ✅ `EmaAdmin.jsx`コンポーネントを削除
- ✅ `App.jsx`から管理者関連のコードを削除
- ✅ URLパラメータ（`?admin=true`）によるアクセスを削除
- ✅ キーボードショートカット（Ctrl+Shift+A）を削除

これにより、メインサイトから管理画面にアクセスする方法が完全になくなり、セキュリティが大幅に向上しました。

## トラブルシューティング

### Firebase接続エラー

- `.env.local`の環境変数が正しく設定されているか確認
- Vercelの環境変数設定を確認

### ログインできない

- `VITE_ADMIN_PASSWORD`が正しく設定されているか確認
- ブラウザのキャッシュをクリア

### ビルドエラー

```bash
cd c:\omairi_app\omatsuri-admin
pnpm install
pnpm build
```

## まとめ

✅ 管理者サイトを別プロジェクトとして完全分離
✅ メインサイトから管理者コードを完全削除
✅ セキュリティの大幅な向上
✅ 同じFirebaseプロジェクトを共有

これで、ユーザーがメインサイトから管理画面にアクセスする方法は一切なくなりました。
