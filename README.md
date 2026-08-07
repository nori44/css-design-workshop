# 実践パート 進行ガイド

架空のコーヒースタンド「Beans Coffee」のLPを題材に、3つの実践を通しで行います。

## 必要な環境

- テキストエディタ（VS Code 推奨）
- モダンブラウザ（Chrome / Firefox / Safari / Edge の最新版）
- **Git と Node.js（実践①のみ）**：実践①はPostCSSでビルドする現場寄りの構成にしたため、事前に環境構築パート（講義の最後・10分）で `git clone` と `npm install` を済ませます
- 実践②・③はビルド不要。`index.html` をブラウザで開くだけ、または Markdown を読むだけで進められます

## リポジトリの取得（環境構築パートで実施）

```bash
git clone <配布予定のGitHubリポジトリURL>
cd css-design-workshop/01-file-split
npm install
```

`npm install` は実践①のフォルダでのみ必要です（`postcss` / `postcss-cli` / `postcss-import` を使うため）。

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
