# 実践パート 進行ガイド

架空のコーヒースタンド「Beans Coffee」のLPを題材に、3つの実践を通しで行います。

## 必要な環境

- テキストエディタ（VS Code 推奨）
- モダンブラウザ（Chrome / Firefox / Safari / Edge の最新版）
- **Git と Node.js（実践①②）**：実践①②はPostCSSでビルドする現場寄りの構成にしたため、`git clone` と `npm install` が必要です。実践①分は環境構築パート（講義の最後・10分）で事前に済ませます。実践②分は実践②に入るタイミングでそのフォルダに移動して各自 `npm install` してください
- 実践③はビルド不要。Markdown を読むだけで進められます

## リポジトリの取得（環境構築パートで実施）

```bash
git clone https://github.com/nori44/css-design-workshop.git
```

## フォルダ構成

`index.html` を開くと、各実践の start / goal / README へのリンク一覧を見られます。

```
css-design-workshop/
├── index.html            実践一覧（各start/goal/READMEへのリンク）
├── 01-file-split/        実践①（15分）：1枚のCSSを分割し、Cascade Layersで優先順位を管理する
│   ├── start/            ここを編集する
│   └── goal/             完成例（詰まったら見る）
├── 02-css-variables/     実践②（15分）：Figmaのバリアブル表に沿ってデザイントークン化する
│   ├── figma-variables.md
│   ├── start/            実践①のgoalと同じ状態から始める
│   └── goal/
└── 03-coding-guidelines/ 実践③（25分）：AI生成の規約たたき台2案を比較し、自分の規約を作る
    ├── drafts/           たたき台A案・B案
    ├── prompt.md         たたき台を生成したプロンプト（持ち帰り用）
    ├── worksheet.md      比較ワークシート
    └── template.md       規約テンプレート
```

## 進め方

1. 各実践フォルダの `0N-README.md`（`01-README.md` など）に手順とタイムボックスが書いてあります
2. `start/` を自分で編集し、詰まったら `goal/` を見てください
3. 途中参加・途中脱落しても大丈夫なように、実践②は実践①の完成状態から始まります

## 実践①②の準備

```bash
cd css-design-workshop/01-file-split
npm install
```

```bash
cd css-design-workshop/02-css-variables
npm install
```

実践①と実践②では、各フォルダに移動してから
`npm install` を実行してください（`postcss` / `postcss-cli` / `postcss-import` を使うため）。実践②分は実践②に入ってから `cd ../02-css-variables && npm install` します。