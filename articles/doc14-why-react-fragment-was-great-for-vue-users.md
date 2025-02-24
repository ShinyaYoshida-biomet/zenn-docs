---
title: "Reactを知ってしまった僕がVueに戻れなくなった理由1.~ 直感的なFragmentの挙動 ~"
emoji: "⚛️"
type: "tech" # tech: 技術記事 / idea: アイデア
topics: ["Vue.js", "React.js", "フロントエンド", "React.Fragment"]
published: true
---

# はじめに
少し自分語り。
かれこれFrontend開発に触り始めてから4~5年が経ちます。
最初は社内企業のMVPを作成するために、Vue.js + Vuetifyを使って簡単なWebアプリケーションを作成するところから始まりました。
1年くらいはVue.jsを使い続けていましたが、最近はもっぱらReact.js派です。

Frontend開発のイロハがわかっていない(特にHTML,CSSの基本的なマークアップができない)時期に、
Vue.js + Vuetifyは非常に魅力的でした。
`v-select`や`v-text-field`などのtagを使うだけで、簡単にフォームを作成できたため、
短時間でMVPを作成しなければならない私のような初心者には非常にありがたかったです。

しかし、一度React.jsを触ってしまうと、Vue.jsによるコンポーネントの書き方が少し違和感を感じるようになりました。
特に、React.jsの`Fragment`とVue.jsの`template`の違いについて。
どちらも、追加のDOM要素を生成せずに複数のHTML要素をグループ化できるという点で共通しています。
しかし、細かい挙動が異なっており、数年触り続けた結果、React.jsの`Fragment`の方が使いやすいと感じ、Vue.jsに二度と戻れない体となってしまいました。

主に大きく2点に分けて、React.jsの`Fragment`とVue.jsの`template`の違いについて書いていきたいと思います。



# コンポーネント全体の条件付きrendering

Vue.jsでコンポーネントの中身全体を条件付きrenderingしたいとします。
直感的にはrootのtemplateタグにv-ifを使えば良いと考える開発者が多いと思います。

```vue
<template v-if="condition">
  <h1>
    Hello World!
  </h1>
</template>

<script setup lang="ts">
import { ref } from 'vue';

const condition = false;
</script>
```

そこで上記のようにコンポーネント全体を条件付きrenderingすると、どうなるでしょうか。


![alt text](/images/doc14/image.png)


`condition` 変数の値に寄らずに、`Hello World!`が表示されてしまいます。
これはrootの `<template>` タグが Vue Loader によってコンパイルされる際にv-ifが無視され、常にコンポーネント全体がレンダリングされてしまうためです。

[stackoverflow](https://stackoverflow.com/questions/47459404/v-if-on-a-component-template-tag)でも同様の質問がされています。

ですので、もしこのコンポーネントの中身全体を条件付きrenderingしたい場合は、
下記のようにrootの下にwrapperを追加する必要があります。

```vue
<template>
  <template v-if="condition">
    <h1>
      Hello World!
    </h1>
  </template>  
</template>

<script setup lang="ts">
import { ref } from 'vue';

const condition = false;
</script>
```

直感的ではない上に、一手間加える必要があるため、Vue.jsの`template`タグを使ったコンポーネント全体の条件付きrenderingは少し使いづらいと感じます。

一方で、React.jsの`Fragment`は、コンポーネント全体を条件付きrenderingする際に非常に直感的です(React.jsの`Fragment`が <></> で省略して表現できることについてここでは詳細に触れません)。

```jsx
export default function App() {
  const condition = false;
  return (
    condition && (
      <>
        <h1>Hello World</h1>
      </>
    )
  );
}
```



# Multi-rootの要素を返すコンポーネント

Vue.jsのコンポーネント内では複数の子要素を直接返すことができず、rootの下に`<div>`や別の`template`などのwrapperを追加する必要があります。

```vue
<template>
  <div>
    <h1>
      Hello World!
    </h1>
    <p>
      This is a paragraph.
    </p>
  </div>
</template>
```

一方で、React.jsの`Fragment`は複数の子要素を直接返すことができます。

```jsx
export default function App() {
  const condition = true;
  return (
    condition && (
      <>
        <h1>Hello World</h1>
        <h1>This is a paragraph.</h1>
      </>
    )
  );
}
```




![alt text](/images/doc14/image-1.png)



# 最後に

細かい挙動の差分ではありますが、いざspeed感を持って実装しないといけない局面で、
これらの微妙な差分は開発効率に大きな影響を及ぼすことがあり、侮れないです。
あと、地味に「コンポーネント全体の条件付きrendering」などの挙動は知ってないとデバックが難しいという意味でもVue.jsの`template`タグは使いづらいと感じます。


「Multi-rootの要素を返すコンポーネント」についても同様です。

**※ Vue.3系からは、`<template>`タグの中に複数の子要素を返すことができるようになりました。**
参考: [official migration document](https://v3-migration.vuejs.org/new/fragments#:~:text=In%20Vue%203%2C%20components%20now%20have%20official%20support%20for%20multi%2Droot%20node%20components%2C%20i.e.%2C%20fragments!)

Vueは明らかにReactを意識してアップデートを重ねており、年々改善されていることは認めます。
しかし、React.jsの直感的な使いやすさには敵わないというのが私の個人的な見解です。
というのも、`Fragment`に限らず多くの点でReact.jsの方が使いやすいと感じるからです。

今後機会がありましたら、Reactを知ってしまった僕がVueに戻れなくなった理由について別の観点から記事を追加したいと思います。