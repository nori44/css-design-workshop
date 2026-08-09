# 実践パート 進行ガイド

架空のコーヒースタンド「Beans Coffee」のLPを題材に、3つの実践を通しで行います。

## 必要な環境

- テキストエディタ（VS Code 推奨）
- モダンブラウザ（Chrome / Firefox / Safari / Edge の最新版）
- **Git と Node.js（実践①②）**：実践①②はPostCSSでビルドする現場寄りの構成にしたため、`git clone` と `npm install` が必要です。**このリポジトリはnpm workspacesを使っているので、ルートで`npm install`を1回実行するだけで①②両方の準備が整います**
- 実践③はビルド不要。Markdown を読むだけで進められます

## リポジトリの取得と準備（環境構築パートで実施）

```bash
git clone https://github.com/nori44/css-design-workshop.git
cd css-design-workshop
npm install
```

`npm install` はここ（リポジトリのルート）で1回実行するだけでOKです。`01-file-split` と `02-css-variables` 両方の依存関係（`postcss` / `postcss-cli` / `postcss-import`）がまとめてインストールされます。

## フォルダ構成

`index.html` を開くと、各実践の start / goal / README へのリンク一覧を見られます。

```
css-design-workshop/
├── package.json          ルートのビルドコマンド（build:01 / build:02）
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

## ビルドコマンド（実践①②共通）

`npm install` はルートで1回だけですが、**ビルドは実践ごとに別コマンド**です。ルートにいる状態でどちらでも実行できます。

```bash
npm run build:01   # 実践①（01-file-split）をビルド
npm run build:02   # 実践②（02-css-variables）をビルド
```

各フォルダに `cd` してから `npm run build` を実行する、これまで通りのやり方も引き続き使えます（`01-README.md` / `02-README.md` に記載）。
