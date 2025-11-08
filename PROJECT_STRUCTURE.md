# プロジェクト構造ドキュメント / Project Structure Documentation

このドキュメントでは、Open ChatGPT Atlasプロジェクトの各ファイルとフォルダーの役割を説明します。

## ルートディレクトリのファイル

### 設定ファイル

- **`package.json`** - プロジェクトのメタデータと依存関係を定義するNode.jsプロジェクトファイル
  - Chrome拡張機能のビルド設定が含まれる
  - React、AI SDK、Anthropic/Google/OpenAIのSDKなどの依存関係を管理

- **`package-lock.json`** - 依存関係の正確なバージョンをロックするファイル

- **`tsconfig.json`** - TypeScriptコンパイラの設定ファイル
  - 型チェックとトランスパイルの設定

- **`tsconfig.node.json`** - Node.js環境用のTypeScript設定

- **`vite.config.ts`** - Viteビルドツールの設定
  - Chrome拡張機能のビルドプロセスを定義

- **`.gitignore`** - Gitで追跡しないファイルとフォルダーを指定

- **`.env.example`** - 環境変数のサンプルファイル
  - APIキーなどの設定例を提供

### 拡張機能のコアファイル

- **`manifest.json`** - Chrome拡張機能のマニフェストファイル（Manifest V3）
  - 拡張機能の権限、バックグラウンドスクリプト、コンテンツスクリプトなどを定義

- **`background.ts`** - Chrome拡張機能のバックグラウンドサービスワーカー
  - 拡張機能のバックグラウンド処理を担当

- **`content.ts`** - コンテンツスクリプト
  - ウェブページに注入されるスクリプト
  - ページの自動化や情報抽出を実行

### UIコンポーネント

- **`sidepanel.tsx`** - サイドパネルのメインReactコンポーネント
  - チャットインターフェースのエントリーポイント

- **`sidepanel.html`** - サイドパネルのHTMLテンプレート

- **`sidepanel.css`** - サイドパネルのスタイルシート

- **`settings.tsx`** - 設定画面のReactコンポーネント
  - APIキーなどの設定を管理

- **`settings.html`** - 設定画面のHTMLテンプレート

- **`settings.css`** - 設定画面のスタイルシート

### 型定義とユーティリティ

- **`types.ts`** - TypeScriptの型定義
  - プロジェクト全体で使用される共通の型を定義

- **`tools.ts`** - AIツールの定義
  - Gemini Computer UseやComposioツールのインターフェース

### ドキュメント

- **`README.md`** - プロジェクトの概要、セットアップ手順、使用方法
  - Chrome拡張機能版とElectronアプリ版の両方の説明

- **`FAQ.md`** - よくある質問と簡易トラブルシューティング

- **`TROUBLESHOOTING.md`** - 詳細なトラブルシューティングガイド

### アセット

- **`atlas.gif`** - デモアニメーション
  - READMEで使用されるプロジェクトのデモ

## フォルダー構造

### `icons/`
Chrome拡張機能のアイコンを格納
- 拡張機能のツールバーアイコン
- 複数のサイズ（16x16, 48x48, 128x128など）

### `assets/`
プロジェクトの画像やその他のアセットファイル
- バナー画像（`banner.png`）
- その他のUI要素

### `electron-browser/`
Electronベースのデスクトップアプリケーション版

このフォルダーには、スタンドアロンのデスクトップブラウザアプリケーションが含まれています。

#### 構造:

```
electron-browser/
├── src/
│   ├── main/              # Electronメインプロセス
│   │   ├── index.ts       # アプリケーションのエントリーポイント
│   │   ├── window-manager.ts    # ウィンドウ管理
│   │   ├── browser-manager.ts   # ブラウザビューの管理
│   │   ├── ipc-handlers.ts      # プロセス間通信ハンドラー
│   │   └── computer-use-service.ts  # ブラウザ自動化サービス
│   │
│   ├── renderer/          # Electronレンダラープロセス（UI）
│   │   ├── components/    # Reactコンポーネント
│   │   │   ├── ChatInterface.tsx    # チャットUI
│   │   │   ├── NavBar.tsx           # ナビゲーションバー
│   │   │   ├── Settings.tsx         # 設定画面
│   │   │   └── ToolCallComponent.tsx # ツール呼び出し表示
│   │   ├── services/      # サービスレイヤー
│   │   │   ├── gemini-service.ts       # Gemini API統合
│   │   │   ├── chat-with-tools.ts      # ツール付きチャット
│   │   │   ├── computer-use-service.ts # Computer Use機能
│   │   │   └── tool-router-service.ts  # Composioツールルーター
│   │   ├── App.tsx        # メインアプリコンポーネント
│   │   ├── index.tsx      # レンダラーエントリーポイント
│   │   ├── styles.css     # グローバルスタイル
│   │   └── types.ts       # 型定義
│   │
│   ├── preload/           # プリロードスクリプト
│   │   └── index.ts       # セキュアなAPIブリッジ
│   │
│   └── constants/         # 定数定義
│       ├── index.ts       # 一般的な定数
│       └── systemPrompts.ts  # AIシステムプロンプト
│
├── package.json           # Electronプロジェクトの設定
├── vite.config.ts         # Electronアプリのビルド設定
├── tsconfig.json          # TypeScript設定
├── tsconfig.node.json     # Node.js用TypeScript設定
└── index.html             # レンダラープロセスのHTML
```

#### 主要な機能:

1. **メインプロセス (`src/main/`)**
   - Electronアプリケーションのライフサイクル管理
   - ブラウザウィンドウの作成と管理
   - IPCを通じたレンダラーとの通信
   - ブラウザ自動化の実装

2. **レンダラープロセス (`src/renderer/`)**
   - ReactベースのUI
   - チャットインターフェース
   - AI統合とツール呼び出し
   - ユーザー設定管理

3. **プリロードスクリプト (`src/preload/`)**
   - メインプロセスとレンダラープロセス間の安全なブリッジ
   - コンテキスト分離を維持しながらAPIを公開

### `.git/`
Gitバージョン管理システムのデータ
- コミット履歴、ブランチ、タグなどを保存

## アーキテクチャの概要

### Chrome拡張機能版
1. **Background Service Worker** (`background.ts`) - バックグラウンド処理
2. **Content Script** (`content.ts`) - ページ操作
3. **Sidepanel UI** (`sidepanel.tsx`) - チャットインターフェース
4. **Settings UI** (`settings.tsx`) - 設定管理

### Electronデスクトップアプリ版
1. **Main Process** - アプリケーション管理とブラウザ制御
2. **Renderer Process** - UIとユーザーインタラクション
3. **Preload Scripts** - セキュアな通信ブリッジ
4. **IPC Communication** - プロセス間のデータ交換

## 主要な技術スタック

- **フロントエンド**: React 18, TypeScript
- **ビルドツール**: Vite
- **AI SDK**: Vercel AI SDK
- **AI プロバイダー**:
  - Google Gemini (Computer Use)
  - Composio (Tool Router)
- **デスクトップ**: Electron 28
- **UI**: カスタムCSS、React Markdown

## 開発ワークフロー

1. **Chrome拡張機能**:
   ```bash
   npm run dev      # 開発モード
   npm run build    # プロダクションビルド
   ```

2. **Electronアプリ**:
   ```bash
   cd electron-browser
   npm run dev      # 開発モード
   npm run build    # ビルド
   npm run start    # アプリ起動
   ```

## 重要な設計決定

1. **Manifest V3**: 最新のChrome拡張機能標準を使用
2. **Direct API Calls**: バックエンド不要、全てのAPI呼び出しは拡張機能/アプリから直接実行
3. **Dual Mode**: ブラウザツールモードとツールルーターモードの2つのモードをサポート
4. **Cross-Platform**: Chrome拡張機能版とElectronデスクトップアプリ版の両方を提供
5. **TypeScript**: 型安全性のためプロジェクト全体でTypeScriptを使用
