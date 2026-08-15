# mc_project CSSコーディング規約

| | |
| --- | --- |
| 版 | v0.1 |
| 決定日 | 2026年8月8日 |
| 決めた人 | micco|
| 見直しタイミング | 案件の節目ごと／新メンバー参加時 |

> この規約は「一度作って終わり」ではありません。
> 運用して合わなかったルールは、理由を添えて更新してください。

## 1. 命名規則

**決めたこと**：
- UIコンポーネントのクラス命名にはBEM（`Block__Element--Modifier`）を厳密に適用する
  - なぜ：クラス名の衝突と「どこで使われているかわからない」問題を排除するため
- Elementのネスト表現は禁止（`.card__header__title` ✗ → `.card__title` ○）
- Modifierの単独使用は禁止。必ず基底クラスと併記する（`class="button button--primary"`）
- Block名はコンポーネントのファイル名と一致させる（`.card` → `card.css`）
- 状態変化は `is-` プレフィックス（`.is-open`）。JS用フックは `js-` プレフィックスとし、CSSでの装飾を禁止する

**なぜ**：
担当者ごとの独自な命名を減らし、記述ルールを統一することで、後から見ても構造や役割が分かりやすく、可読性・保守性を高められるため。

**例**：

```css
/* ○ 良い例 */

.card {
}

.card__title {
}

.card__image {
}

/* BlockのModifier */
.card--featured {
}

/* ElementのModifier */
.card__title--large {
}

/* 状態 */
.card.is-active {
}


/* ✗ 避ける例 */

/* Elementをネストしている */
.card__header__title {
}

/* BEM以外の独自ルール */
.cardTitle {
}

/* IDセレクタで装飾 */
#card {
}

/* JS用フックで装飾 */
.js-modal {
  display: none;
}
```

```html
<!-- ○ 良い例：BlockのModifier -->
<div class="card card--featured">
  <h2 class="card__title">タイトル</h2>
</div>

<!-- ○ 良い例：ElementのModifier -->
<div class="card">
  <h2 class="card__title card__title--large">タイトル</h2>
</div>

<!-- ✗ 避ける例：Modifierを単独で使用 -->
<div class="card--featured">
  <h2 class="card__title--large">タイトル</h2>
</div>
```

## 2. ファイル構成と Cascade Layers

**レイヤーの構成順**：
- Cascade Layers は `reset, base, components, utilities` の4層を基本とし、必要になったら追加してよい

```css
@layer reset, base, components, utilities;
```

**各レイヤーの役割**：
| レイヤー | 役割 |
| --- | --- |
| reset | ブラウザデフォルトスタイルのリセット・正規化 |
| base | body, a, img, 見出しなどHTML要素の基本スタイル |
| components | BEMで定義するUIコンポーネント |
| utilities | .u-hidden など単一目的の補助クラス |

**ファイル分割の方針**：
- `components` は原則としてBlock単位でファイルを分ける（`.card` → `card.css`）
- `reset`、`base`、`utilities` は役割ごとにファイルをまとめる
- ページ固有のスタイルが増えた場合は、必要に応じてページ単位でファイルを分ける
- 行数だけを基準にせず、目的のスタイルを探しやすい構成を優先する

**なぜ**：
最初から細かく分割しすぎると管理が煩雑になるため、基本構成はシンプルにし、ページ数や記述量の増加に応じて必要な単位で分割することで、可読性・保守性を維持しやすくするため。

## 3. デザイントークンの運用

**トークン化の基準**：
- **2回以上使う値はトークン化する**（1回目は直書きでよい）
  - なぜ：使うか分からない値まで先回りでトークン化すると、トークン一覧が育ちすぎるため
- トークン名はFigmaと多少ずれてもよい。ただし対応表をREADMEに残す

**Figmaとの対応ルール**：
- FigmaのVariables・StylesとCSSのデザイントークンは、できるだけ意味が対応する名前にする
- FigmaとCSSで名称を完全に一致させることは必須としない
- 名称が異なる場合は、対応関係をREADMEに記載する
- Figmaに定義されている値をすべてトークン化するのではなく、実装上必要な値だけをCSS側で管理する

| Figma | CSS |
| --- | --- |
| Primary / Blue | --color-primary |
| Spacing / Medium | --spacing-md |

**追加・変更のフロー**：
- 新しいトークンを追加する前に、既存トークンで対応できないか確認する
- 既存トークンを変更する場合は、使用箇所への影響を確認する
- Figmaとの対応関係が変わった場合はREADMEを更新する

## 4. 禁止・非推奨パターン

| パターン | 扱い（禁止/非推奨） | 理由・例外条件 |
| --- | --- | --- |
| `!important` | 禁止 | 例外なし。必要になったらレイヤー構成を見直す |
| IDセレクタでの装飾 | 禁止 | 詳細度が高く、クラスベースのスタイルを上書きしづらくなるため。ID自体はアンカーリンクや要素の識別など、CSS以外の用途では使用可。 |
|  HTMLの `style` 属性（インラインスタイル） | 禁止 | スタイルがHTML側に分散し、CSSから検索・管理しづらくなるため。また、Cascade Layersやデザイントークンによる一元管理を損なうため。 |

## 5. ツールによる強制

**使うツール**：
- Prettier は必須とする（フォーマットの議論を消すため）

**強制の範囲**：
- Stylelint は導入したい人が設定案を持って提案してから
  - Stylelintは現時点では必須としない。
  必須にすることで記述の統一は期待できるが、ルールを細かく設定しすぎると実装の自由度が下がったり、例外対応がしづらくなる可能性があるため、導入したい人が具体的な設定案を持って提案し、その内容を確認した上で判断する。

## 未決事項

（決めきれなかった論点をここに残す。次の見直しで扱う）
