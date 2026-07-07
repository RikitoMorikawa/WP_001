# CLAUDE.md — 学生寮インタビューサイト 作業ガイド

このリポジトリは WordPress.com 上の「学生寮インタビュー」サイトのソース書き出し＋作業手順。
サイトの実体は WordPress.com の DB 側にあり、操作は **WordPress.com MCP** 経由で行う。

## 基本情報

| 項目 | 値 |
|---|---|
| 公開URL | https://studentsdormitory.wordpress.com/ |
| blog_id（`wpcom_site` に使う） | `235419237` |
| アカウント | 3165071@gmail.com（wp3165071 / 森川力斗） |
| プラン | Personal（¥4,800/年）※MCP操作には必須 |
| テーマ | Twenty Twenty-Four（ブロックテーマ / FSE） |
| カテゴリー「インタビュー」 | term id `11788` |
| アイキャッチ用メディア | 男子 `student-boy`=ID `73` / 女子 `student-girl`=ID `74` |
| 記事 | post id `8`(田中) `23`(佐藤) `24`(鈴木) `25`(山田) |
| 雛形（下書き） | post id `61`（本文は `wp-source/posts/_TEMPLATE-blocks.html`） |
| GitHub | RikitoMorikawa/WP_001（public, main）。gh は RikitoMorikawa でログイン済み |

## MCP ツールの使い分け

- `wpcom-mcp-content-authoring` … 投稿・固定ページ・メディア・カテゴリー/タグ・パターン（`list`→`describe`→`execute`）
- `wpcom-mcp-site-editing` … テンプレート / テンプレートパーツ / グローバルスタイル
- `wpcom-mcp-site` … サイト設定・テーマ・公開（launch）・ユーザー
- `wpcom-mcp-site-editor-context` … テーマのプリセット（色・フォント・スペーシング）参照
- 書き込み系（`*.create/update/delete`, `settings.update`, `manage-site.launch` 等）は必ず **`user_confirmed: true`**。

## ハマりどころ（重要）

- **無料プランは MCP のサイト操作系が全ブロック**（読み取りすら不可）。Personal 以上が必須。
- **日本語文字列はパラメータに UTF-8 で直接書く**。`\u` エスケープは誤変換しやすい（実際に 暗/暮・慎/慣 でミス）。
- **色は直接 hex 指定**（例 `#FFD029`）。テーマの accent プリセットを global-styles で差し替えても保存されなかったため、プリセットに頼らない。
- **`has-border-color` を1辺だけの border に付けない**。全辺 solid＋既定幅＋currentColor で「両端に黒い縦線」が出る。1辺だけなら inline で `border-bottom:2px solid #...` 等、または枠線なし。
- **画像 `post-featured-image` は `aspectRatio` を使うとカード幅いっぱいに広がる**。`contentSize` を指定した group で包んでサイズを抑える（現状 170px）。
- **コアの Button ブロックは手書きだと検証に落ちる**（「復旧を試みる」）。バッジ/タグは Button ではなく段落で作る。
- **`media.create` は base64 必須**でURL取り込み不可。数万文字の base64 を正確に渡せないため、**アイキャッチ用画像はユーザーが管理画面でメディアにアップロード**する運用。
- **パターン登録（patterns.create）は MCP に無い**。「パターンを作成」は編集画面での手動操作。
- **パーマリンク構造は簡易サイトで固定**（`/年/月/日/スラッグ/`）。`/id` 単独は不可（Businessが必要）。URLをIDっぽくするには**スラッグを数字にする**（例 `/2026/07/06/8/`）。
- **サブドメイン名の変更は MCP 不可**（ダッシュボードの手動操作）。**公開（launch）は `manage-site.launch` で可能**。
- 大きいテンプレート更新で稀に **502（プロキシ）**。軽い読み取りで復旧確認 → リトライ。

## トップ一覧の仕組み

`home` テンプレートは動的 **Query Loop**（`inherit:false` / postType post / 新しい順）。
**公開した投稿は自動でカードに並ぶ。** カードのサムネイル＝各投稿の**アイキャッチ画像**、説明文＝**「抜粋(Excerpt)」欄**。

## 新しいインタビュー記事を追加する流れ

1. 投稿一覧で**雛形（id 61）を「コピー」**（複製は下書きで、アイキャッチ・抜粋も引き継ぐ）
2. 本文の `【】` を差し替え（名前 / 学部・学年 / 寮 / タグ / リード / Q&A / 寮情報）
3. **男子/女子**に合わせて、本文イラストの画像URL＋**アイキャッチ**（男子73/女子74）を選択
4. **「抜粋」欄にリード文**を入れる（＝カードに表示される説明文。空だと本文先頭を自動で拾って崩れる）
5. **スラッグ**を数字/短い英字に（URLをキレイに。任意）
6. カテゴリー **「インタビュー」** にチェック（任意）
7. **公開** → トップの先頭に顔付きカードで自動追加、クリックで詳細へ

MCP から記事を作る場合：`posts.create`（`status:"draft"` 既定）。本文は Gutenberg ブロックマークアップ。`featured_media` にメディアID、`excerpt` にリード文、`slug` に数字、`categories:[11788]`。

## デザイン（配色・構造）

- メイン黄色 `#FFD029` / 濃い黄（テキスト）`#B8860B` / ページ背景 `#FFFBEA` / 淡い黄ボックス `#FFF9E0` / 枠 `#FFE066`
- ヘッダー/フッターは黄色帯（枠線なし）。ヘッダーは home/single 直下の top-level `<header>` に `position:sticky` で上部固定（パーツ入れ子だと効かない）。
- 記事本文（4本）は `wp:html` のインラインHTML（黄色カードデザイン）。雛形は標準ブロック版（GUI編集しやすい）。

## リポジトリ同期

サイト側（テンプレート・投稿）を変更したら、`wp-source/` の該当ファイルを現在の内容に更新してコミットする。
- テンプレ/パーツ … `wp-source/templates/*.html`, `wp-source/parts/*.html`
- グローバルスタイル … `wp-source/styles/global-styles.json`
- 記事 … `wp-source/posts/*.html`（本文のみ。アイキャッチ・スラッグ・抜粋はメタ情報でファイル外→ `site.json` に記録）
- 設定メモ … `site.json`

## 未対応・検討中（TODO）

- 公開中の複製「id 78」の扱い（実記事化 / 下書き / 削除）
- リード文を1回書くだけにする write-once 構成（本文リード欄を post-excerpt ブロックにして「抜粋」欄と一元化）
- 本文内画像を GitHub ホットリンクから WordPress メディアへ寄せる（GitHub 依存の解消）
