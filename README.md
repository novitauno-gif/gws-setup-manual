# Google Workspace CLI（gws）セットアップマニュアル

## 概要

**Google Workspace CLI（gws）** は、Google Cloud のコマンドラインツールです。ターミナルから直接 Google Drive、Gmail、Calendar などの Workspace リソースにアクセス可能です。

### gws vs Antigravity

| 項目 | gws | Antigravity |
|:---|:---|:---|
| **実行環境** | ターミナル（ネイティブ） | ブラウザ拡張 |
| **速度** | 高速 | やや遅い |
| **安定性** | Google公式 | サードパーティ |
| **メンテナンス** | Google が管理 | 自分で管理 |
| **自動化** | スクリプト化可能 | 困難 |

---

## 必要な環境

- **macOS** （Intel / Apple Silicon 対応）
- **Homebrew** （パッケージマネージャー）
- **Node.js / npm** （バージョン 18以上推奨）
- **Google アカウント** （novita.uno@gmail.com）
- **GCP プロジェクト** （または新規作成）

---

## インストール手順

### Step 1: Xcode ライセンス同意

```bash
sudo xcodebuild -license accept

