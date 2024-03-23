---
title: "Next.jsの`<Link>`タグが`useRouter`より優先されるべき3つの理由"
emoji: "🔗"
type: "tech" # tech: 技術記事 / idea: アイデア
topics: ["JavaScript", "React", "Next"]
published: true
---


# はじめに

Next.jsでページ遷移を行う際に、候補となるのが`<Link>`タグと`useRouter`フックの利用です。


上記のどちらを用いても、ページ遷移自体は問題なく実行できるため、
`<Link>`タグと`useRouter`フックのどちらを使うかについて、迷ったことのある方もいるかもしれません。


`useRouter`が優先されるべきケースとしては、特定の条件下でページ遷移を行いたい場合や、
ページ遷移を行う際にパラメータを渡したい場合などが挙げられます。

一方それ以外の場合は、`<Link>`タグを使うことが推奨されます。
本記事では、`<Link>`タグが`useRouter`より優先されるべき3つの理由について解説します。


※ Next.js v13以降はappディレクトリにコンポーネントを配置する方法もありますが、本記事ではPages Routerを前提としています。
※ そのため、`useRouter`フックを使う場合は、`next/navigation`ではなく `next/router`をインポートしてください。

# 1. プリフェッチ機能

`<Link>`タグは、遷移先のページをプリフェッチする機能を持っています。
プリフェッチとは、ユーザーがリンクをクリックする前に、遷移先のページを事前に読み込むことです。
これにより、遷移先のページが表示されるまでの時間を短縮することができます。

[Next.jsの公式ドキュメント](https://nextjs.org/docs/pages/api-reference/components/link)の引用と和訳を置いておきます。

> <Link> is a React component that extends the HTML <a> element to provide prefetching and client-side navigation between routes. It is the primary way to navigate between routes in Next.js.

> <Link> は、HTMLの <a> 要素を拡張したReactコンポーネントであり、ルート間のプリフェッチとクライアントサイドナビゲーションを提供します。これは、Next.jsでルート間を移動する主要な方法です。


**補足**

> Any <Link /> in the viewport (initially or through scroll) will be prefetched by default (including the corresponding data) for pages using Static Generation. The corresponding data for server-rendered routes is fetched only when the <Link /> is clicked.


静的ビルドされたページでは、ビューポート内にあるすべての`<Link>`はデフォルトでプリフェッチされる一方で、
サーバーサイドレンダリングされたページでは、`<Link>`がクリックされた時にのみ、対応するデータが取得されるようです。

# 2. SEO対策

`<Link>`タグは、SEO対策にも有効です。

[vercel/next.jsのDiscussions](https://github.com/vercel/next.js/discussions/62496)に下記のようなコメントがありました。

> Yes, router.push('/example') does perform client-side navigation, but it doesn't create a <a> tag, so your links won't be detected by crawlers, which is inherently bad for SEO.

`router.push('/example')`はクライアントサイドのナビゲーションを行いますが、`<a>`タグを作成しないため、
クローラーによってリンクが検出されません。これはSEOにとって悪影響を及ぼします。

一方で`<Link>`タグは`<a>`タグを作成するため、クローラーによってリンクが検出されます。
これにより、サイトの構造がクローラーによって正しく理解されるため、SEO対策上の観点から`Link`タグの使用が推奨されます。


# 3. コードがシンプルになる

`<Link>`タグは、遷移先のパスを指定するだけで遷移を行うことができるので、
useRouterのインスタンス化を含むコードの記述量が少なくなり、シンプルになります。


## `<Link>`タグを使う例


```jsx
import Link from 'next/link';

function Page() {
  return (
    <nav>
      <Link href="/">Home</Link>
    </nav>
  );
}
```

## `useRouter`を使用する例
```jsx
import { useRouter } from 'next/router';

function Page() {
  // useRouterフックを使ってrouterオブジェクトを取得
  const router = useRouter();
  function goToHome() {
    router.push('/');
  }
  return <button onClick={goToHome}>Home</button>;
}
```

# まとめ

`<Link>`タグは、プリフェッチ機能、SEO対策、コードのシンプル化といったメリットがあり、
特別な理由がない限り、`<Link>`タグを使うことが推奨されます。

`Link`タグで置き換え可能な`useRouter` の利用がないか、
折を見て一度プロジェクトを見直してみると良いかもしれません。
