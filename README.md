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
    home.html         … トップ（固定ヘッダー＋黄色ヒーロー＋白カード一覧）
    single.html       … 記事詳細（メタ情報・コメント欄なし／固定ヘッダー）
  parts/
    header.html       … 参考用ヘッダー（実際は home/single にインライン configで固定）
    footer.html       … フッター（黄色・サイト名＋コピーライト）
  styles/
    global-styles.json… 全体背景・リンク／ボタン色
  posts/
    *.html            … インタビュー記事4本（wp:html インラインHTMLデザイン）
  site.json           … サイト設定メモ
```

## 画像について

サイト上の学生イラストは、この GitHub リポジトリ（public）の `assets/` を
`https://raw.githubusercontent.com/RikitoMorikawa/WP_001/main/assets/…png`
としてホットリンク表示している。リポジトリを非公開化・削除・パス変更すると画像が表示されなくなる点に注意。正式運用時は WordPress メディアへの登録を推奨。

## メモ（プラン別の制約）

- 無料プランでは WordPress.com のサイト操作系 MCP／API が全ブロックされ、Personal 以上が必須。
- カスタムCSSファイル／オリジナルテーマは Personal では不可のため、デザインは各ブロックのインラインスタイル＋グローバルスタイルで実現。
- ヘッダーの上部固定は、テンプレートパーツの入れ子だと効かないため、home/single テンプレート直下のトップレベル `<header>`（position:sticky）として配置している。
