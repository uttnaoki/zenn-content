---
title: '個人サイトをGoogle検索から辿れるようにする'
emoji: '🔍'
type: 'tech'
topics: ['nextjs', 'vercel', 'seo', 'claude']
published: true
---

## はじめに

Next.js + Vercelで個人サイト（ボードゲームイベントのカレンダーサイト）を作ったものの、Google検索に出てこない状態でした。（[記事](https://zenn.dev/uttnaoki/articles/249da7e82cb25c)）

対象のサイト: https://boardgame-calendar.vercel.app/

このサイトをGoogle検索で表示できるようにします。
手順として、Claudeから以下の対応を提案してもらいました。

1. **Google Search Consoleに登録する** — サイトの存在をGoogleに知らせる
2. **SEO関連のメタデータを整備する** — サイトマップ、robots.txt、title/description、OGタグ、JSON-LD構造化データの追加

## 1. Google Search Consoleへの登録

### プロパティの追加

[Google Search Console](https://search.google.com/search-console) にアクセスして、プロパティを追加します。

Vercelのサブドメイン（`xxx.vercel.app`）を使っている場合、DNS認証ができないので **「URLプレフィックス」** を選択します。

入力欄に `https://boardgame-calendar.vercel.app` を入力して「続行」。

![URLプレフィックスを選択してURLを入力](/images/2026-02-14-nextjs-vercel-seo/gsc-url-prefix.png)

### 所有権の確認

確認方法はいくつかありますが、今回は **HTMLタグ方式** を使いました。
Search Consoleで表示されるメタタグを、Next.jsの `layout.tsx` に追加します。

![HTMLタグ方式の確認画面](/images/2026-02-14-nextjs-vercel-seo/gsc-html-tag-method.png)

```tsx:app/layout.tsx
export const metadata: Metadata = {
  // ...
  verification: {
    google: "ここにSearch Consoleで表示されたコード",
  },
};
```

Next.jsの `metadata` オブジェクトに `verification.google` を設定すると、自動的に `<meta name="google-site-verification" content="..." />` が出力されます。

## 2. SEO関連のメタデータ整備

### sitemap.xml と robots.txt

Googleのクローラーがサイトの構造を把握するために必要なファイルです。

最初は Next.js の `app/sitemap.ts` や `app/robots.ts` で動的に生成しようとしましたが、ビルド時にエラーが発生したため、`public/` に静的ファイルとして配置しました。

```xml:public/sitemap.xml
<?xml version="1.0" encoding="UTF-8"?>
<urlset xmlns="http://www.sitemaps.org/schemas/sitemap/0.9">
  <url>
    <loc>https://boardgame-calendar.vercel.app</loc>
    <changefreq>daily</changefreq>
    <priority>1.0</priority>
  </url>
</urlset>
```

```txt:public/robots.txt
User-agent: *
Allow: /
Disallow: /api/
Disallow: /admin/

Sitemap: https://boardgame-calendar.vercel.app/sitemap.xml
```

デプロイ後、Search Consoleの「サイトマップ」メニューから `sitemap.xml` を送信します。

### メタデータ（title / description）

Googleは `<title>` と `<meta description>` を検索結果の表示に使います。狙いたい検索キーワードをここに含めるのと良いそうです。

```tsx:app/layout.tsx
export const metadata: Metadata = {
  title: "福岡ボードゲームカレンダー | 福岡のボードゲーム会・イベント情報",
  description:
    "福岡のボードゲーム会・ボードゲームイベントの開催情報をカレンダー形式で表示。福岡で開催されるボードゲーム会の日程をまとめてチェックできます。",
};
```

## 結果

これらの対応を行った結果、「福岡ボードゲームカレンダー」で検索するとGoogle検索に表示されるようになりました。

![Google検索結果に表示された状態](/images/2026-02-14-nextjs-vercel-seo/google-search-result.png)

ただ、「Vercel」の表示はドメインを用意しないと変更できないかもしれません。

## まとめ

Google Search Consoleへの登録とメタデータを整理し、
Vercelで公開した個人サイトをGoogleで検索できるようになりました。

Google Search Consoleの使い方はわかっていませんでしたが、
キャプチャをClaudeに添付することでコンソール上の操作を教えてもらえ、また、ソースコードの修正をやってくれました。
