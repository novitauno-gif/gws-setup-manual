では、以下の最新版マークダウンを全部コピーして、nano に貼り付けてください：

text
# Google Workspace CLI（gws）セットアップマニュアル

## 📋 概要

**Google Workspace CLI（gws）** は、ターミナルから直接 Google Drive、Gmail、Calendar などの Workspace リソースにアクセス可能なコマンドラインツールです。

### gws vs Antigravity

| 項目 | gws | Antigravity |
|:---|:---|:---|
| **実行環境** | ターミナル（ネイティブ） | ブラウザ拡張 |
| **速度** | 高速 | やや遅い |
| **安定性** | Google公式ツール | サードパーティ |
| **メンテナンス** | 不要（Google管理） | 自分で管理 |
| **自動化** | スクリプト化可能 | 困難 |
| **モバイル対応** | ✗ | ✗ |

---

## ⚠️ **重要な前提条件（必読）**

### 🔴 **macOS バージョンの制約**

**gws は macOS Tahoe 26.3 以上を強く推奨します。**

- ✅ **macOS Tahoe 26.3**：完全動作確認済み
- ❌ **macOS Sequoia 15.7.4**：認証トークン保存に失敗（暗号化プロトコル互換性なし）

**Sequoia でセットアップした場合、以下のエラーが発生します：**
"encryption_error": "Could not decrypt. May have been created on a different machine."
"encryption_valid": false

text

### 必要な環境

- **macOS Tahoe 26.3 以上**
- **Homebrew**
- **Node.js / npm**（バージョン 18 以上推奨）
- **Google アカウント**
- **GCP プロジェクト**（または新規作成）

---

## 🚀 **インストール手順（5ステップ）**

### Step 1: Xcode ライセンス同意

```bash
sudo xcodebuild -license accept
エラー例：

text
Error: You have not agreed to the Xcode license.
注意： パスワード入力時、画面に何も表示されませんが、内部で認識されています。

Step 2: Google Cloud SDK インストール
bash
brew install --cask google-cloud-sdk
echo 'export PATH=/opt/homebrew/share/google-cloud-sdk/bin:$PATH' >> ~/.zshrc
source ~/.zshrc
確認:

bash
gcloud --version
出力例:

text
Google Cloud SDK 559.0.0
トラブル：Updating Homebrew... で止まる場合

bash
HOMEBREW_NO_AUTO_UPDATE=1 brew install --cask google-cloud-sdk
または公式スクリプト：

bash
curl https://sdk.cloud.google.com | bash
source ~/.zshrc
Step 3: npm のパス設定
bash
mkdir ~/.npm-global
npm config set prefix '~/.npm-global'
echo 'export PATH=~/.npm-global/bin:$PATH' >> ~/.zshrc
source ~/.zshrc
エラー例：npm EACCES permission denied

上記のコマンドで解決します。

Step 4: gws インストール
bash
npm install -g @googleworkspace/cli
gws --version
出力例:

text
gws 0.8.0
This is not an officially supported Google product.
Step 5: gws セットアップと認証
5-1: セットアップウィザード実行
bash
gws auth setup
5-2: ウィザードの流れ
Step 1/5: gcloud CLI

自動確認

Step 2/5: Authentication

ブラウザでログイン（novita.uno@gmail.com）

Step 3/5: GCP project

既存プロジェクト選択 or 新規作成

プロジェクト名例：gws-novita-uno-2026

Step 4/5: Workspace APIs

すべて選択（22個）推奨

Step 5/5: OAuth credentials

GCP コンソールで OAuth クライアント作成が必要

GCP での OAuth クライアント作成手順：

https://console.cloud.google.com/ を開く

プロジェクト（gws-novita-uno-2026）を選択

「APIとサービス」→「OAuth 同意画面」

ユーザータイプ：外部

アプリ名：gws

サポートメール：自分のメールアドレス

「対象」→ テストユーザーに自分のメールアドレスを追加

「APIとサービス」→「認証情報」

「+ 認証情報を作成」→「OAuth クライアント ID」

アプリケーションタイプ：デスクトップ アプリ

Client ID と Client Secret をコピー

ターミナルに戻り：

Enter OAuth Client ID: → 貼り付けて Enter

Enter OAuth Client Secret: → 貼り付けて Enter

完了メッセージ:

text
✅ Setup complete! Run `gws auth login` to authenticate.
5-3: 最終認証
bash
gws auth login
ターミナルにスコープ選択画面が出る → Enter で確定

ブラウザが開く（または URL が表示される）

Google アカウントでログイン

「続行」→「次へ」→ 権限確認して確定

「You may now close this window.」→ ブラウザを閉じる

Step 6: 動作確認
bash
gws drive files list
成功例:

json
{
  "files": [
    {
      "id": "19eeyoHhDbyiTlEUzraQTSVcAAD3sEcEH",
      "name": "Rei&Uno.psd"
    }
  ]
}
bash
gws auth status
成功例:

json
{
  "auth_method": "oauth2",
  "token_valid": true,
  "encryption_valid": true,
  "enabled_api_count": 43
}
🔥 トラブルシューティング（実際に遭遇したエラー）
❌ エラー 1: npm EACCES permission denied
原因: npm のグローバルインストール先に書き込み権限がない

解決策:

bash
mkdir ~/.npm-global
npm config set prefix '~/.npm-global'
echo 'export PATH=~/.npm-global/bin:$PATH' >> ~/.zshrc
source ~/.zshrc
❌ エラー 2: macOS Sequoia 15.7.4 で認証トークン保存失敗
原因: OS バージョンが古く、gws の暗号化プロトコルに対応していない

症状:

text
"encryption_error": "Could not decrypt. May have been created on a different machine."
"token_valid": false
解決策:

bash
# macOS を Tahoe 26.3 にアップグレード
# システム設定 → ソフトウェア・アップデート
❌ エラー 3: OAuth flow failed: invalid_client
原因: GCP の OAuth クライアントが削除されている or 設定不備

解決策:

GCP コンソールで古い OAuth クライアントを削除

新しい OAuth クライアントを作成（デスクトップ アプリ）

テストユーザーに自分のメールアドレスを追加

rm -rf ~/.config/gws でローカルをリセット

gws auth setup をやり直し

❌ エラー 4: gws: command not found
原因: ターミナルのパスが更新されていない

解決策:

bash
export PATH=~/.npm-global/bin:$PATH
または、ターミナルを再起動：

bash
source ~/.zshrc
❌ エラー 5: Safari がブロック（認証エラー）
原因: Safari のセキュリティ設定

解決策:

Chrome をデフォルトブラウザに設定

または --no-browser オプション使用：

bash
gws auth login --no-browser
📊 検証コマンド一覧
bash
# Google Drive のファイル一覧
gws drive files list

# Gmail のメール検索
gws gmail messages list

# Google Calendar の予定一覧
gws calendar events list

# ステータス確認
gws auth status

# ヘルプ表示
gws --help
gws drive --help
🎯 今後の活用
Gemini CLI との MCP 連携

GitHub への自動バックアップ

自動化パイプライン構築

クロック同期 + 定期実行

📝 更新履歴
2026/03/07（土）：MacBook Air M3（Tahoe 26.3）で初回セットアップ成功

2026/03/08（日）：Mac mini M4 Pro で Sequoia → Tahoe アップグレード後、セットアップ成功

🔗 参考リンク
Google Cloud CLI ドキュメント

Google Workspace APIs

作成者: uno (novita.uno@gmail.com)
検証環境: macOS Tahoe 26.3 / gws 0.8.0

text

***

## 次のステップ

1. **nano で既存の内容を全削除**（`Ctrl + K` を何度も押す）
2. **上記のマークダウン全体をコピペ**
3. **`Ctrl + X` → `Y` → `Enter` で保存**
4. **GitHub にプッシュ：**

```bash
git add README.md
git commit -m "Update manual with macOS version requirements and troubleshooting"
git push
では、nano に貼り付けてください！ 🚀


