---
title: "Vuetify の v-data-tableのfooter(ページネーション)をカスタマイズする"
emoji: "😺"
type: "tech" # tech: 技術記事 / idea: アイデア
topics: ["Vue", "JavaScript", "Vuetify"]
published: false
---


Vuetifyの`v-data-table`のカスタマイズって、細かい修正が難しくて、調べてもなかなか出てこないですよね。
今日はそんな中でfooter部分(ページネーション)をカスタマイズする方法を紹介します。

# 環境情報

- `vue: ^2.6.14`
- `vuetify: ^2.7.2`
- `@vue/cli: ^5.0.8`



# 実装例

Vuetify2系のsampleスクリプトを例にしてみます。
https://v2.vuetifyjs.com/en/components/data-tables/


```vue
<template>
  <div>
    <v-data-table
      style="max-width: 800px; width: 90%; margin: 0 auto;"
      :headers="headers"
      :items="desserts"
      :items-per-page="5"
    ></v-data-table>
  </div>
</template>


<script>
  export default {
    name: 'Example',
    data () {
      return {
        headers: [
          {
            text: 'Dessert (100g serving)',
            align: 'start',
            sortable: false,
            value: 'name',
          },
          { text: 'Calories', value: 'calories' },
          { text: 'Fat (g)', value: 'fat' },
          { text: 'Carbs (g)', value: 'carbs' },
          { text: 'Protein (g)', value: 'protein' },
          { text: 'Iron (%)', value: 'iron' },
        ],
        desserts: [
          {
            name: 'Frozen Yogurt',
            calories: 159,
            fat: 6.0,
            carbs: 24,
            protein: 4.0,
            iron: 1,
          },
          // 以下省略
        ],
      }
    },
  }
</script>
```

カスタマイズをする前の`v-data-table`の表示は以下の通りです。

![alt text](/images/image-1.png)


# ペジネーションのテキストを変更したい

`v-data-table`のペジネーションのテキスト(defaultでは「Rows per page:」)を変更したい場合は、
`footer-props`の`items-per-page-text`にプロパティを渡します。
具体的には、以下のようになります。


```vue
<template>
  <div>
    <v-data-table
      style="max-width: 800px; width: 90%; margin: 0 auto"
      :headers="headers"
      :items="desserts"
      :items-per-page="5"
      :footer-props="{
        'items-per-page-text': 'Num to display:',
      }">
    >  
    </v-data-table>
  </div>
</template>

```
※ scriptタグは省略しています。


![alt text](/images/image-3.png)

footerの表示テキストが「Rows per page:」から「Num to display:」に変更されていますね。


# footerの配置を変更したい

`v-data-table`のfooterの配置を変更したい場合は、cssのクラスを使ってスタイルを変更します。

```vue
<template>
  <div>
    <v-data-table
      style="max-width: 800px; width: 90%; margin: 0 auto"
      :headers="headers"
      :items="desserts"
      :items-per-page="5"
      :footer-props="{
        'items-per-page-text': 'Num to display:',
      }">
    >
    </v-data-table>
  </div>
</template>

<style>
.v-data-footer {  
  display: flex;
  justify-content: center;
}
</style>
```



![alt text](/images/image-4.png)

これでfooterの配置がテーブルに対して中央寄せになりました。
また、お好みで背景色等を変更することも可能です。


```vue
<template>
  <div>
    <v-data-table
      style="max-width: 800px; width: 90%; margin: 0 auto"
      :headers="headers"
      :items="desserts"
      :items-per-page="5"
      :footer-props="{
        'items-per-page-text': 'Num to display:',
      }">
    >
    </v-data-table>
  </div>
</template>

<style>
.v-data-footer {  
  background-color: skyblue;
  display: flex;
  justify-content: center;
}
</style>
```


![alt text](/images/image-5.png)



# paginationを非表示にしたい

`v-data-table`のpaginationを非表示にしたい場合は、
`hide-default-footer`プロパティと、`disable-pagination`プロパティを使います。


```vue
<template>
  <div>
    <v-data-table
      style="max-width: 800px; width: 90%; margin: 0 auto"
      :headers="headers"
      :items="desserts"
      :items-per-page="5"
      :disable-pagination="true"
      :hide-default-footer="true"
    >
    </v-data-table>
  </div>
</template>

```



![alt text](/images/image-6.png)


paginationが無効となり、全件表示されるようになりました。
ただし、この時に`footer-props`が残っていると、paginationが表示されてしまうので注意してください。


# 結び
`v-data-table` のfooter(ページネーション)をカスタマイズする方法を紹介しました。
細かい制御が難しいと感じることもあるかもしれませんが、
なんだかんだでよく調べてみれば本当に不可能なスタイリングって実は少ないのではないかと思っています。
この記事がVue関連で悩んでいた皆様にとって、少しでもお役に立てれば幸いです。