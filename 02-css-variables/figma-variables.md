# Figma バリアブル一覧（デザイナーからの共有想定）

Beans Coffee のFigmaファイルで定義されているバリアブルの一覧です。
色は **プリミティブ（生の値）→ セマンティック（エイリアス）** の2層構造になっています。

**命名の対応ルール**：Figmaの `コレクション/名前` → CSSの `--コレクション-名前`
（例：`color/primary` → `--color-primary`）

## palette コレクション（プリミティブ・モードなし）

生の色の値そのもの。単体では使わず、下の `color` コレクションからエイリアスとして参照される。

| Figma変数名 | 値 | CSS変数名 |
| --- | --- | --- |
| `palette/brown-50` | `#FAF6F2` | `--palette-brown-50` |
| `palette/brown-100` | `#F3ECE5` | `--palette-brown-100` |
| `palette/brown-200` | `#E8DDD3` | `--palette-brown-200` |
| `palette/brown-400` | `#C8A27A` | `--palette-brown-400` |
| `palette/brown-500` | `#6F4E37` | `--palette-brown-500` |
| `palette/brown-600` | `#4A3B2E` | `--palette-brown-600` |
| `palette/brown-700` | `#3E2F25` | `--palette-brown-700` |
| `palette/brown-800` | `#2E241C` | `--palette-brown-800` |
| `palette/brown-900` | `#221A14` | `--palette-brown-900` |
| `palette/white` | `#FFFFFF` | `--palette-white` |

## color コレクション（セマンティック・モード対応・エイリアス）

値は直接の色ではなく、上の `palette` コレクションへの**エイリアス**（参照）。
Light / Dark で参照先を切り替えることで、モードごとに違う実体を持たせている。

| Figma変数名 | Light（参照先） | Dark（参照先） | CSS変数名 |
| --- | --- | --- | --- |
| `color/primary` | → `palette/brown-500` | → `palette/brown-400` | `--color-primary` |
| `color/accent` | → `palette/brown-400` | → `palette/brown-400` | `--color-accent` |
| `color/text` | → `palette/brown-700` | → `palette/brown-100` | `--color-text` |
| `color/text-inverse` | → `palette/white` | → `palette/white` | `--color-text-inverse` |
| `color/bg` | → `palette/brown-50` | → `palette/brown-900` | `--color-bg` |
| `color/surface` | → `palette/white` | → `palette/brown-800` | `--color-surface` |
| `color/border` | → `palette/brown-200` | → `palette/brown-600` | `--color-border` |

> 💡 CSS側でのエイリアスの表現は「変数の値に別の変数の`var()`を入れる」こと。
> `--color-primary: var(--palette-brown-500);` のように書くと、Figmaのエイリアス構造をそのままCSSに持ち込める。

## spacing コレクション

| Figma変数名 | 値 | CSS変数名 |
| --- | --- | --- |
| `spacing/xs` | `8` | `--spacing-xs` |
| `spacing/sm` | `16` | `--spacing-sm` |
| `spacing/md` | `24` | `--spacing-md` |
| `spacing/lg` | `40` | `--spacing-lg` |
| `spacing/xl` | `64` | `--spacing-xl` |

## radius コレクション

| Figma変数名 | 値 | CSS変数名 |
| --- | --- | --- |
| `radius/md` | `8` | `--radius-md` |
| `radius/full` | `999` | `--radius-full` |

## font-size コレクション

| Figma変数名 | 値 | CSS変数名 |
| --- | --- | --- |
| `font-size/sm` | `14`（0.875rem） | `--font-size-sm` |
| `font-size/md` | `16`（1rem） | `--font-size-md` |
| `font-size/lg` | `24`（1.5rem） | `--font-size-lg` |
| `font-size/xl` | `40`（2.5rem） | `--font-size-xl` |

## なぜプリミティブとセマンティックを分けるか

- **プリミティブ（`palette/*`）**：デザインで使える色の全在庫。それ自体に意味はない
- **セマンティック（`color/*`）**：「どこに使うか」という役割に名前を付けたもの。実際にコンポーネントから参照するのはこちら
- こうしておくと、「ブランドカラーを少し変えたい」ときは `palette` の1色を差し替えるだけで、`color/primary` を使っている箇所すべてに伝播する。逆に「ここだけ強調したい」ときは、新しい `color/*` エイリアスを1つ足すだけで済み、生の色を増やさずに済む
