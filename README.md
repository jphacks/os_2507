# 組みTalk (Kumi-Talk)

**家具の“取説”をチャットで解決する AI アシスタント**

だれでも迷わず組み立てられる“会話型マニュアル体験”を提供します。
PDFの取説をアップロードすると、工程や部品の疑問に根拠つきで答えます。

[https://youtu.be/QWwfHjx5484](https://www.youtube.com/embed/QWwfHjx5484?si=uTAG9TY_9YO10kQa)

## 概要 / 位置づけ

DIY初心者にもやさしい操作性

## プロジェクトの特徴

### コア機能

* **PDFアップロード**: 取説（PDF）とメタ情報を登録
* **自動解析 & RAG**: ページ分割→埋め込み→近傍検索→**回答**
* **チャットQA**: 途中からでも再開できる履歴ベースの対話
* **横断検索**: 取説を跨いだ検索・再開

### 技術的アピールポイント

#### 技術スタック

* **Next.js 14 (App Router)** + **TypeScript**
* **Supabase**（Postgres）
* **Tailwind CSS** による迅速なUI開発
* **LLM**: Gemini

#### アーキテクチャ設計

* **非同期解析ジョブ**: 初回解析はバックグラウンドで進行、UIはプログレス表示
* **権限制御**: 所有者だけが`documents`へアクセス
* **引用UI**: 回答の根拠ページへジャンプ

## システム構成

```
Frontend (Next.js)
    ↓ API Routes (/app/api/*)
Backend (Upload / Analyze / Chat)
    ↓
Supabase (Postgres)
    ↓
LLM Provider (Gemini via Adapter)
```

## 環境構築

### 必要な環境

* Node.js 20+
* Supabase CLI（ローカルDB / pgvector）

### セットアップ手順

1. **リポジトリのクローン**

```bash
git clone https://github.com/jphacks/os_2507.git
cd os_2507
```

2. **依存関係のインストール**

```bash
npm install
```

3. **データベースの起動/初期化**

```bash
supabase start
supabase db reset
```

4. **開発サーバーの起動**

```bash
npm run dev
```

アプリは [https://kumi-talk-tanakastronghold.vercel.app](https://kumi-talk-tanakastronghold.vercel.app) で起動します。

## 主要機能（画面）

### ルーム/一覧・詳細に相当する導線

1. ホーム（最近のチャット / アップロードCTA）
2. チャット（引用から該当ページへジャンプ）

### 解析とチャットの体験

1. PDFをアップロード
2. AIによる解析をもとに画像を生成
3. 組み立ての段階ごとにチャットが可能

## 🔧 開発・運用コマンド

```bash
# 開発
npm run dev

# 本番ビルド/起動
npm run build
npm run start

# 品質
npm run lint
npm run format
npm run format:check
```

## 📁 ディレクトリ構成

```
KumiTalk/
├── documents/
│   ├── chair_nitori.pdf
│   └── shelf_ikea.pdf
├── images/
│   └── chair_nitori/
│       ├── color/
│       │   ├── parts_color.png
│       │   ├── step1_color.png
│       │   └── step2_color.png (...)
│       ├── parts.png
│       ├── raw.png
│       └── step1.png (...)
├── prisma/
│   ├── migrations/
│   │   ├── 20251018043648_init/
│   │   ├── 20251018080759_change_schema/
│   │   └── 20251018191140_add_assembly_steps/
│   └── schema.prisma
├── public/
│   ├── images/
│   │   └── KumiTalk.png
│   ├── file.svg
│   ├── globe.svg
│   ├── next.svg
│   ├── vercel.svg
│   └── window.svg
├── src/
│   ├── app/
│   │   ├── (auth)/
│   │   │   ├── login/
│   │   │   └── signup/
│   │   ├── analyzer/
│   │   ├── api/
│   │   │   ├── analyze-manual/
│   │   │   ├── assembly/
│   │   │   ├── auth/
│   │   │   ├── chat/
│   │   │   ├── gemini/
│   │   │   └── messages/
│   │   ├── chat/
│   │   ├── favicon.ico
│   │   ├── globals.css
│   │   ├── layout.tsx
│   │   └── page.tsx
│   ├── components/
│   │   ├── chat/
│   │   │   ├── chat-interface.tsx
│   │   │   ├── chat-sidebar.tsx
│   │   │   ├── chat-window.tsx
│   │   │   └── markdown-renderer.tsx
│   │   ├── dialog/
│   │   │   ├── file-upload-dialog.tsx
│   │   │   └── show-image-dialog.tsx
│   │   ├── manual-analyzer/
│   │   │   └── manual-analyzer.tsx
│   │   ├── ui/
│   │   │   ├── avatar.tsx
│   │   │   ├── badge.tsx
│   │   │   ├── button.tsx
│   │   │   ├── dialog.tsx
│   │   │   ├── input.tsx
│   │   │   └── label.tsx
│   │   ├── footer.tsx
│   │   ├── global-progress-overlay.tsx
│   │   ├── top-nav.tsx
│   │   └── top-progress.tsx
│   └── lib/
│       ├── auth.ts
│       ├── prisma.ts
│       ├── progress.tsx
│       └── utils.ts
├── components.json
├── eslint.config.mjs
├── next-env.d.ts
├── next.config.ts
├── package.json
├── postcss.config.mjs
├── tsconfig.json
└── README.md
```
---

**開発期間**: 2日 (ハッカソン)

**チームメンバー**: 今村賢人，江畑尚，大屋善彦，笹原悠太

**デプロイ**: Vercel + Supabase
