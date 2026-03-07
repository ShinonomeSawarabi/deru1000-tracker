# でる1000 トラッカー — Claude 向けガイド

## プロジェクト概要

TOEIC対策問題集「でる1000問」の学習進捗を記録するシングルページWebアプリ。
ファイルは `index.html` 1本のみ。CSS・JSはすべてインライン。

## チェックコマンド

変更後は必ず以下を実行して問題がないことを確認する。

```bash
npm run check
```

個別実行も可能:

```bash
npm run lint       # HTML構造のlint (htmlhint)
npm run check:js   # JS構文チェック (Node.js vm)
```

## ファイル構成

```
index.html          ← アプリ本体（CSS・JSすべてここに）
.htmlhintrc         ← htmlhint設定
package.json        ← devDependencies (htmlhint)
CLAUDE.md           ← このファイル
.claude/
  hooks/
    session-start.sh  ← セッション開始時にnpm installを実行
  settings.json       ← フック設定
```

## index.html の構造

| 行範囲 (概算) | 内容 |
|---|---|
| `<style>` ブロック | CSS変数・レイアウト・コンポーネントスタイル |
| `<body>` | ヘッダー・セクションカード・モーダルのHTML骨格 |
| `<script>` ブロック | データ定義・ロジック・レンダリング関数 |

### データモデル

- `SECTIONS` — セクション定義配列（コード・問題範囲）
- localStorage キー: `deru1000v3`
- 保存形式: `{ records: { [sectionCode]: [{ date, err, time, timeSec, rem }] } }`

### 主要関数

| 関数 | 役割 |
|---|---|
| `render()` | 全セクションカードを再描画 |
| `saveRecord(code)` | フォーム入力をlocalStorageに保存 |
| `toggleSection(code)` | セクション開閉 |
| `exportData()` / `importData()` | JSON エクスポート・インポート |

## 変更時の注意

- `id="t-${code}"` (分) と `id="ts-${code}"` (秒) は対でセットする
- テンプレートリテラル内のHTMLはインデントに注意（不要な空白が出る）
- CSS変数 (`--accent`, `--text` 等) はすべて `:root` に定義済み
