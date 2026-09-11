# 実践①：ファイル分割と Cascade Layers（PostCSSビルド）（15分）

`start/src/style.css` は、Beans Coffee LP のスタイルが全部入った1枚のCSSです。
これを役割ごとのファイルに分割し、Cascade Layers で優先順位を宣言したうえで、
**PostCSSでビルドして1つのファイルにまとめます**。

> 講義で話した「`@import`はブラウザで直接読み込むと直列(ウォーターフォール)になって遅い」という話、覚えていますか？
> ここではその解決策として、ビルド時に`postcss-import`で1ファイルへ束ねる、現場でよく使われる構成を体験します。

## ゴール

- `style.css` の中身を **reset / base / layout / components / utilities** の5ファイルに分割する（`style.css` 自体は削除せず、分割後は各ファイルを束ねるハブにする）
- `@layer` でレイヤーの優先順位を宣言する
- `npm run build` でPostCSSが `style.css` 経由で5ファイルを**1つのdist/style.cssにバンドル**することを確認する
- 分割の副産物として、`utilities` の `!important` を撤去する

## 事前準備（環境構築パートで実施済みの想定）

`npm install` は**リポジトリのルート**（`css-design-workshop/`）で1回実行済みのはずです（npm workspacesで①②まとめてインストールされます）。このフォルダで改めて実行する必要はありません。

`package.json` の中身：

```json
{
  "scripts": {
    "build": "postcss start/src/style.css -o start/dist/style.css",
    "watch": "postcss start/src/style.css -o start/dist/style.css --watch"
  },
  "devDependencies": {
    "postcss": "^8.4.38",
    "postcss-cli": "^11.0.0",
    "postcss-import": "^16.1.0"
  }
}
```

`postcss.config.js` はプラグイン1つだけのシンプルな構成です。

```js
module.exports = {
  plugins: [require("postcss-import")]
};
```

## 手順

### 0. 準備（1分）

`start/index.html` をブラウザで開いておく（分割の前後で見た目が変わらないことの確認用）。
`index.html` は `dist/style.css` を読み込む設定になっています（`src/` ではない点に注意）。
`start/dist/style.css` は最初から分割前の状態でリポジトリに含まれているので、`npm run build` を打つ前でも今の時点で見た目を確認できます。

### 1. 空ファイルを作る（2分）

`start/src/` に以下の5ファイルを作成:

```
reset.css  base.css  layout.css  components.css  utilities.css
```

### 2. 中身を引っ越す（5分）

`start/src/style.css` の `/* ==== reset ==== */` などのコメント区切りを目印に、
各セクションを対応するファイルへそのまま移動する。

### 3. style.css をハブにする（4分）

`start/src/style.css` の中身を全部消して、次の6行だけにする:

```css
@layer reset, base, layout, components, utilities;

@import url("reset.css") layer(reset);
@import url("base.css") layer(base);
@import url("layout.css") layer(layout);
@import url("components.css") layer(components);
@import url("utilities.css") layer(utilities);
```

- 1行目が**レイヤーの優先順位の宣言**（後に書いたレイヤーほど強い）
- `layer(...)` 付きの `@import` で、各ファイルを丸ごとそのレイヤーに入れる

### 4. ビルドする（2分）

このフォルダにいるなら：

```bash
npm run build
```

リポジトリのルートにいるなら：

```bash
npm run build:01
```

どちらも同じ結果です。`start/dist/style.css` が更新され、5ファイルが1つに束ねられます（各レイヤーは`@layer reset { ... }`のように展開される）。
ここが今回の肝です。**ソースは5ファイルのまま読み書きしつつ、ブラウザに届くのは1リクエスト分の1ファイル**になります。

編集のたびにビルドし直すのが面倒な場合は、このフォルダで `npm run watch` を実行するとファイル変更を監視できます。

### 5. 動作確認（3分）

1. ブラウザをリロードして、見た目が変わっていないことを確認
2. `index.html` のバッジ（`hero__badge`）から `u-hidden` を外す → バッジが表示される
3. `index.html` の`u-hidden` を戻し、`utilities.css` の `!important` を削除 →ビルドし直す → **それでもバッジは隠れたまま**

## なぜ `!important` を消せたのか

- 分割前：`.hero .hero__badge`（詳細度 0,2,0）が `.u-hidden`（0,1,0）より優先されるため、`!important` が必要だった
- 分割後：レイヤー同士の比較では**詳細度より宣言順が優先**される。`utilities` は最後に宣言したレイヤーなので、詳細度が低くても優先される

試しに、分割前の状態で `!important` だけ消すとバッジが出てしまいます。
「詳細度による優先順位を、レイヤーの宣言順に置き換える」のが Cascade Layers の役割です。

## goal を自分でビルドし直したい場合

このフォルダから：`npm run build:goal`
リポジトリのルートから：`npm run build:01:goal`
