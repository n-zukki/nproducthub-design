# nproducthub-design

トークン（CSS 変数）と汎用クラスの CSS。ビルドは要らず、CSS をそのまま配る。業務を知らない共通の見た目だけを置く。Web 専用。

## 入れ方

npm レジストリには公開しない。git のタグを指定して入れる。

```bash
npm i github:n-zukki/nproducthub-design#v0.2.0
```

`package.json` には `"nproducthub-design": "github:n-zukki/nproducthub-design#v0.2.0"` と、タグ固定で書く。lockfile にはタグが指すコミットが記録される。

## 読み込み方

入口は 2 つ。

| 読み込み | 中身 |
| --- | --- |
| `nproducthub-design` または `nproducthub-design/index.css` | 全部入り（トークン、土台、汎用クラス、ユーティリティ） |
| `nproducthub-design/tokens.css` | トークン（`:root` の CSS 変数）だけ |

アプリの入口の CSS から読む。アプリ独自の CSS は、パッケージのあとに読み込む。

```css
@import 'nproducthub-design/index.css';
@import './app/index.css';
```

Next.js なら、ルートのレイアウトで `import 'nproducthub-design/index.css';` を、独自の CSS の前に書く。

全部入りの読み込み順は `tokens` → `base` → `layout` → `button` → `badge` → `notice` → `form` → `table` → `tabs` → `state` → `utilities`。同じ詳細度で勝ち負けが決まる箇所があるので、並べ替えない。新しいファイルは後ろに足す。

## トークン

すべて `tokens.css` の `:root` にある。色・余白・角丸・影・文字サイズ・重なり順は、直書きせずトークンを経由する。

| 種類 | 変数 |
| --- | --- |
| 地・面・線・文字 | `--c-bg` `--c-surface` `--c-surface-muted` `--c-border` `--c-border-strong` `--c-text` `--c-text-muted` |
| ブランド | `--c-brand`（装飾用の明るい青）`--c-focus` `--c-primary` `--c-primary-hover` `--c-primary-soft` `--c-primary-border` `--c-on-primary` |
| 塗り・アバター | `--c-fill` `--c-avatar-1` 〜 `--c-avatar-6` `--c-skeleton` |
| 状態 | `--c-success` `--c-warning` `--c-danger`（それぞれ `-soft` `-border` あり） |
| 形・影 | `--radius-sm` `--radius` `--shadow` |
| 余白 | `--space-1`（4px）〜 `--space-6`（32px） |
| 文字 | `--font-sans` `--fs-xxs` `--fs-xs` `--fs-sm` `--fs-md` `--fs-lg` `--fs-xl` `--fs-2xl` `--fs-3xl` `--fs-4xl` `--fs-display` |
| 重なり順 | `--z-raised` `--z-sticky` `--z-floating` `--z-header` `--z-toast` |

## ブランド色の差し替え

パッケージのあとに読み込む自分の CSS で、`:root` の変数を上書きする。クラスは触らない。

```css
:root {
  --c-brand: #…;
  --c-primary: #…;
  --c-primary-hover: #…;
  --c-primary-soft: #…;
  --c-primary-border: #…;
  --c-focus: #…;
  --c-on-primary: #ffffff;
}
```

- `--c-primary` は、文字や塗りボタンに使う。白（`--c-on-primary`）との比が 4.5:1 以上になる濃い色にする。
- `--c-focus` は、白地での比が 3:1 以上になる色にする。
- `--c-brand` は装飾用で、文字やフォーカスの輪には使わない。
- システム専用の色（業務の意味を持つ色）は、パッケージに足さず、各システムの CSS で別の変数として定義する。

## クラス名の規則

`block__element` と `block--modifier`（BEM）。例：`.btn` `.btn--primary` `.card__header` `.field__label`。

主なクラス：`.btn` `.badge` `.notice` `.field` `.input` `.select` `.textarea` `.choice` `.table` `.table-wrap` `.card` `.segmented` `.day-tabs` `.state` `.skeleton` `.page` `.page-header` `.stack` `.row` `.fields-2` `.fields-3`、ユーティリティの `.muted` `.small` `.tabular` `.visually-hidden`。

## ブレークポイント

CSS 変数にできないので、値を直接書く。

- 狭い幅：`@media (max-width: 599px)`
- 広い幅：`@media (min-width: 600px)`
- コンテナクエリ：`900px`

## 版の上げ方

1. このリポジトリで CSS を直す。見た目を変えるときも、トークン・クラスの名前は、使う側に影響するので慎重に変える。
2. `package.json` の `version` を上げる。名前の変更や削除は大きい版（破壊的変更）、追加は中、直しは小。
3. `main` へ PR で入れたあと、`git tag vX.Y.Z` を打って push する。タグは動かさない。
4. 使う側は、依存の `#vX.Y.Z` を書き換えて `npm i` し、lockfile を更新する。

## 約束

- 業務を知らない値だけを置く。特定のシステムのためのトークンやクラスは足さない。
- ビルドなし。CSS をそのまま配る。
