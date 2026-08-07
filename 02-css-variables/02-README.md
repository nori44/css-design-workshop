# 実践②：CSSバリアブル（デザイントークン）（15分）

`start/` は実践①の完成状態です（①が途中でも、ここから合流できます）。
デザイナーからFigmaのバリアブル一覧（`figma-variables.md`）が共有された、という想定で、ハードコードされた値をデザイントークンに置き換えます。

## ゴール

- `figma-variables.md` の対応表から**まずは2〜3個**を選んで `tokens.css` を作る（全部揃えるのは時間的に厳しいので、必要になったタイミングで増やしていく）
- 繰り返し登場する色・余白などを `var()` に置き換える
- トークンを1箇所変えるだけでサイト全体が変わることを確認する

## 手順

### 1. Figmaバリアブル一覧を読む（2分）

`figma-variables.md` を開き、Figmaの変数名とCSS変数名の対応ルールを確認する。
色は **`palette`（プリミティブ）→ `color`（セマンティック・エイリアス）** の2層になっている点に注目。

### 2. tokens.css を作る（4分）

`start/css/tokens.css` を新規作成する。**色トークンを全部用意する時間はないので、まずは2〜3個だけ**、`palette`（プリミティブ）→ `color`（セマンティック）の2層構造・**エイリアス（`var()`参照）**で作ってみましょう。変数の値はプリミティブの値のままではなく、必ずエイリアスの形で書いてください。

**最初におすすめの2〜3個**：
- `--color-primary`（`palette-brown-500` → `#6f4e37`。7箇所で使われていて効果が一番わかりやすい）
- `--color-text`（`palette-brown-700` → `#3e2f25`）
- 余裕があれば `--color-bg`（`palette-brown-50` → `#faf6f2`）

`--color-primary` なら、こう書きます：

```css
--palette-brown-500: #6f4e37;
--color-primary: var(--palette-brown-500);
```

`goal/css/tokens.css` が完成形です。`spacing` / `radius` / `font-size` は `figma-variables.md` に `palette` 層が定義されていないので、余裕があれば直接値のまま追加してみてください（例：`--spacing-md: 24px;`）。

`style.css` に `tokens` レイヤーを追加する:

```css
@layer tokens, reset, base, layout, components, utilities;

@import url("tokens.css") layer(tokens);
@import url("reset.css") layer(reset);
/* 以下そのまま */
```

### 3. ハードコードされた値を置き換える（6分）

`base.css` / `layout.css` / `components.css` の値を `var()` に置き換える。
エディタの一括置換が便利です（例：`#6f4e37` → `var(--color-primary)`）。

まずは手順2で用意した2〜3個の変数が使われている箇所から置き換えましょう。
**置き換えたい値の変数がまだなければ、その都度 `tokens.css` に追加してOK。** 全部を最初に揃える必要はなく、必要になったタイミングで少しずつ増やしていく方が現実的です。

余裕があれば、他にも繰り返し登場する値をトークン化してみてください:

- `#6f4e37`（7箇所）、`#ffffff`、`#3e2f25`、`#faf6f2`、`#e8ddd3`、`#c8a27a`
- `16px` `24px` `40px` `64px` の余白
- `8px` `999px` の角丸

`1.25rem` や `12px 32px` のような1回しか出ない値は、直書きのまま残して構いません。
「どこまでトークン化するか」自体が実践③で決めるルールの1つです。

### 4. 動作確認（3分）

1. リロードして見た目が変わっていないことを確認
2. `tokens.css` の `--color-primary` を `#2f6f4e`（緑）に変えてリロード
   → ロゴ・見出し・ボタン・CTAが一斉に変わる（確認したら戻す）

## ボーナス①：エイリアスの効能を確認する

`goal/css/tokens.css` は `palette`（プリミティブ）→ `color`（エイリアス）の2層構成です。
`--palette-brown-500` の値だけを変えてリロードすると、それを参照している `--color-primary` も連動して変わります。
「生の値は1箇所、意味づけ（どこに使うか）は`color/*`側」と分けておくと、ブランドカラーの変更が1点集中で済むことを体感できます。

## ボーナス②：Figmaの「モード」をCSSで再現

`goal/css/tokens.css` の末尾に、ダークモード用の上書きが入っています。
`index.html` の `<html>` に `data-theme="dark"` を付けると切り替わります。
Dark側では `palette` はそのままに、`color/*` エイリアスの参照先だけが切り替わっている点に注目してください。
Figmaバリアブルの Light / Dark モードと同じ考え方です。
