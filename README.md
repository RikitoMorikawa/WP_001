# 学生寮インタビュー — WordPress サイトソース

学生寮に住む学生へのインタビュー記事サイト（黄色デザイン）のソース一式。

- **公開URL**: https://studentsdormitory.wordpress.com/
- **プラットフォーム**: WordPress.com（Personalプラン / Twenty Twenty-Four ブロックテーマ）
- **ベースカラー**: 黄色 `#FFD029`（背景 `#FFFBEA` / 濃い黄 `#B8860B`）

## 構成

WordPress.com のサイトエディター（テンプレート／パーツ／グローバルスタイル）と投稿を、ブロックマークアップとしてエクスポートしたもの。テーマ本体は未改変で、すべて DB 上のユーザーカスタマイズ。

```
assets/
  student-boy.png     … 男子学生イラスト（田中・鈴木さんに使用）
  student-girl.png    … 女子学生イラスト（佐藤・山田さんに使用）
wp-source/
  templates/
    home.html         … トップ（固定ヘッダー＋黄色ヒーロー＋動的クエリループのカード一覧）
    single.html       … 記事詳細（メタ情報・コメント欄なし／固定ヘッダー）
  parts/
    header.html       … 参考用ヘッダー（実際は home/single にインライン configで固定・枠線なし）
    footer.html       … フッター（黄色・サイト名＋コピーライト・枠線なし）
  styles/
    global-styles.json… 全体背景・リンク／ボタン色
  posts/
    08-…〜25-….html   … インタビュー記事4本（wp:html インラインHTMLデザイン）
    _TEMPLATE.html    … 貼り付け用ひな型（Custom HTML 版）
    _TEMPLATE-blocks.html … 標準ブロック版ひな型（下書き post 61 の本文。複製 or パターン化して使用）
  site.json           … サイト設定メモ（投稿ID・スラッグ・アイキャッチmedia ID 等）
```

## トップ一覧の仕組み

`home.html` は動的な **Query Loop**（`inherit:false` / postType post / 新しい順）。**公開した投稿は自動でカードに並ぶ**。カードのサムネイルは各投稿の**アイキャッチ画像（post-featured-image）**、説明文は**「抜粋(Excerpt)」欄**を表示する。新規投稿はアイキャッチと抜粋を設定するとカードが整う。詳細URLはスラッグを数字IDにして末尾をIDにしている（例 `/2026/07/06/8/`）。

## 画像について

学生イラストは2系統で使用している。

- **記事本文内**の図：この GitHub リポジトリ（public）の `assets/*.png` を `https://raw.githubusercontent.com/RikitoMorikawa/WP_001/main/assets/…png` として**ホットリンク表示**。リポジトリを非公開化・削除・パス変更すると表示されなくなる点に注意。
- **トップのカードのサムネイル**：同じイラストを **WordPress メディアにも登録**し（`student-boy`=ID 73 / `student-girl`=ID 74）、各投稿の**アイキャッチ画像**として設定。カードは post-featured-image でこれを表示。

正式運用では本文内の画像もメディア（WordPress ホスト）に寄せると GitHub 依存がなくなる。

## メモ（プラン別の制約）

- 無料プランでは WordPress.com のサイト操作系 MCP／API が全ブロックされ、Personal 以上が必須。
- カスタムCSSファイル／オリジナルテーマは Personal では不可のため、デザインは各ブロックのインラインスタイル＋グローバルスタイルで実現。
- ヘッダーの上部固定は、テンプレートパーツの入れ子だと効かないため、home/single テンプレート直下のトップレベル `<header>`（position:sticky）として配置している。
