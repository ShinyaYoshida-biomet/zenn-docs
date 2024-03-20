---
title: "React VS Next.js / Vue.js VS Nuxt.js のルーティング比較"
emoji: "📋"
type: "tech" # tech: 技術記事
topics: ["javascript", "Vue", "Nuxt", "React", "Next"]
published: true
---

React.jsとVue.jsは、それぞれの魅力と機能性の高さにより、開発者コミュニティ内で高い評価を受けているフレームワークです。
しかしこれらのフレームワーク間での移行は、時にルーティングの違いにより、混乱やバグを招く場合があります。
Vue.jsに慣れ親しんだ開発者がReact.jsに触れた時、またはその逆の場合には、
ルーティングの構文やパラダイムの違いにより、時間を取られることがあります。

さらに、これらのフレームワークはそれぞれ、Vue.jsのためのNuxt.js、
React.jsのためのNext.jsという特化したフレームワークがあるため、
これらの間の移行でも同様にトラブルが発生する可能性があるかもしれません。

本記事では、フレームワーク間の移行作業や新規学習者が混乱ルーティングの実装で混乱することを防ぐために、
そして何より私自身の備忘録のために、
React.jsとNext.js、Vue.jsとNuxt.jsのルーティングの違いについての早見表をまとめたいと思います。


# 言葉の定義

- **Programmatic Navigation** : コード内でページ遷移を行う方法。JavaScriptを使用して動的にルートを変更する。
- **Declarative Navigation** : JSX内またはテンプレートの中で直接ページ遷移を指定する方法。

※ Vue RouterおよびReact Routerの公式ドキュメントにおける言葉の用法を参照に、本記事で用いる定義を設けました([参考](https://router.vuejs.org/guide/essentials/navigation.html#Programmatic-Navigation))。


# React.js VS Next.js

## 比較表

|                        | React.js                   | Next.js                                       |
|------------------------|----------------------------|-----------------------------------------------|
| **Programmatic Navigation** | ・`useNavigate()` hookを利用する <br> ・`react-router-dom`の `createXXXRouter`で設定が必要 | `useRouter()` hookを利用する <br> ・`pages`ディレクトリにコンポーネントを定義 |
| **Declarative Navigation**  | `<Link>` from `react-router-dom`           | `<Link>` from `next/link`                     |

***注意***
- ※ クラスコンポーネントではなく、関数コンポーネントの中でreact hooksを使うことを前提としています。
- `react-router-dom` はversion6以降を前提としています。
- ※ next.jsの場合、`pages`ディレクトリにコンポーネントを定義することを前提としています([Next.js v13以降はappディレクトリ](https://nextjs.org/blog/next-13-4))。
- Programmatic Navigation / Declarative Navigation共に、代替となる方法がいくつか存在しますが、本記事ではその中でも一般的と思われる方法を記載しています。



## 例

### Programmatic Navigation

#### React.js
```jsx
import { useNavigate } from 'react-router-dom';

function Page() {
  // useNavigateフックを使ってnavigate関数を取得
  const navigate = useNavigate();

  // ホームページに遷移する関数
  function goToHome() {
    navigate('/');  // パスを指定して遷移
  }

  // ボタンクリックでgoToHomeを実行
  return <button onClick={goToHome}>Go to Home</button>;
}
```


#### Next.js
```jsx
import { useRouter } from 'next/router';

function Page() {
  // useRouterフックを使ってrouterオブジェクトを取得
  const router = useRouter();

  // ホームページに遷移する関数
  function goToHome() {
    router.push('/');  // パスを指定して遷移。pushメソッドを呼び出すことに注意
  }

  // ボタンクリックでgoToHomeを実行
  return <button onClick={goToHome}>Go to Home</button>;
}
```

単なるrouterではなく`router.push`を呼び出すことに注意してください。

### Declarative Navigation

#### React.js
```jsx
import { Link } from 'react-router-dom';

function Page() {
  return (
    <nav>
      <Link to="/">Home</Link>
    </nav>
  );
}
```

Linkエレメントにて遷移先を指定するプロパティは`to`であることにご注意ください。

#### Next.js
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

Linkエレメントにて遷移先を指定するプロパティは`href`であることにご注意ください。


## 各種参考リンク
- [react-router-domドキュメント useNavigate](https://reactrouter.com/en/main/hooks/use-navigate)
- [react-router-domドキュメント Link](https://reactrouter.com/en/main/components/link)
- [next.jsドキュメント useRouter](https://nextjs.org/docs/pages/api-reference/functions/use-router)
- [next.jsドキュメント Link](https://nextjs.org/docs/pages/api-reference/components/link)

# Vue.js VS Nuxt.js

|                        | Vue.js                                       | Nuxt.js                                        |
|------------------------|----------------------------------------------|------------------------------------------------|
| **Programmatic Navigation** | ・`this.$router.push()`<br> ・`router/index.js`内で設定 | ・`this.$router.push()`<br>　・`pages`ディレクトリにコンポーネントを定義 |
| **Declarative Navigation**  | `<router-link>` |`<NuxtLink>` |


***注意***
- ※ Vueは2系を前提としております。Composition APIの仕様については別の記事をご参照ください([参考](https://zenn.dev/fuqda/articles/2781d2f5196a4c))。


## 例

### Programmatic Navigation

コードとしてはほぼ同じですが、
Vue.jsの場合は`router/index.js`内で設定を行い、
Nuxt.jsの場合は`pages`ディレクトリにコンポーネントを定義することが必要です。


#### Vue.js
```vue
<template>
  <button @click="goToHome">
    Home
  </button>
</template>

<script>
  export default {
    methods: {
      goToHome() {
        this.$router.push('/');
      }
    }
  }
</script>
```

#### Nuxt.js
```vue
<template>
  <button @click="goToHome">
    Home
  </button>
</template>

<script>
  export default {
    methods: {
      goToHome() {
        this.$router.push('/');
      }
    }
  }
</script>
```


### Declaration Navigation

#### Vue.js
```vue
<template>
  <nav>
    <router-link to="/">Home</router-link>
  </nav>
</template>
```

`<router-link>` はVue Routerがプロジェクトに組み込まれていれば、import不要です。

#### Nuxt.js
```vue
<template>
  <nav>
    <NuxtLink to="/">Home</NuxtLink>
  </nav>
</template>
```

`<NuxtLink>`もはNuxtに含まれているため、
`<router-link>`と同様に、他のコンポーネントと一緒にimportする必要はありません。
便利ですね！



## 各種参考リンク
- [Vue Routerドキュメント](https://router.vuejs.org/guide/essentials/navigation.html)
- [Nuxt.js Routing関連ドキュメント](https://nuxt.com/docs/getting-started/routing)


# 結び
ルーティングはフロントエンド開発の核心的な部分です。
新しいフレームワークを学ぶ際に良いスタートダッシュを切るため、
もしくは様々な案件を経験する中で生じるフレームワークの違いによる混乱を避けるためにも、
本記事がだれかのために役立つことを願っています。


