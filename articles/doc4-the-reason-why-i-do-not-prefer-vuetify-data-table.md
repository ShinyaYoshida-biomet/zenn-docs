---
title: "Vuetifyのv-data-tableがなぜ使いにくいか、言語化してみた"
emoji: "📊"
type: "tech" # tech: 技術記事 / idea: アイデア
topics: ["Vue", "JavaScript", "Vuetify", "UI"]
published: true
---



# 本記事について
VuetifyはVue.jsのためのデザインフレームワークです。


# 環境情報

- `vue: ^2.6.14`
- `vuetify: ^2.7.2`
- `@vue/cli: ^5.0.8`



# とりあえず使ってみる
Vuetify2系のsampleスクリプトをそのままUIで表示してみます。
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
    name: 'HelloWorld',
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
          {
            name: 'Ice cream sandwich',
            calories: 237,
            fat: 9.0,
            carbs: 37,
            protein: 4.3,
            iron: 1,
          },
          {
            name: 'Eclair',
            calories: 262,
            fat: 16.0,
            carbs: 23,
            protein: 6.0,
            iron: 7,
          },
          {
            name: 'Cupcake',
            calories: 305,
            fat: 3.7,
            carbs: 67,
            protein: 4.3,
            iron: 8,
          },
          {
            name: 'Gingerbread',
            calories: 356,
            fat: 16.0,
            carbs: 49,
            protein: 3.9,
            iron: 16,
          },
          {
            name: 'Jelly bean',
            calories: 375,
            fat: 0.0,
            carbs: 94,
            protein: 0.0,
            iron: 0,
          },
          {
            name: 'Lollipop',
            calories: 392,
            fat: 0.2,
            carbs: 98,
            protein: 0,
            iron: 2,
          },
          {
            name: 'Honeycomb',
            calories: 408,
            fat: 3.2,
            carbs: 87,
            protein: 6.5,
            iron: 45,
          },
          {
            name: 'Donut',
            calories: 452,
            fat: 25.0,
            carbs: 51,
            protein: 4.9,
            iron: 22,
          },
          {
            name: 'KitKat',
            calories: 518,
            fat: 26.0,
            carbs: 65,
            protein: 7,
            iron: 6,
          },
        ],
      }
    },
  }
</script>
```


# 何が問題なのか

主にデザインの制御がしにくいことが原因。
何が原因で制御が難しくなっているか。その原因をbreak downし、言語化していきます。


## カスタマイズのために、子エレメントを定義する必要がある


### 行ごとのスタイルについて



### ツールチップによる省略表示について


## propertyによる制御とstyleによる制御が混在している


### テーブルのサイズ(幅/高さ)について

テーブルサイズの調整は、テーブルの導入を決めた時点で避けては通れない基本的なタスクになる。
しかし、その基本的なタスクを処理するのにも、Vuetifyの`v-data-table`は直感的ではないため、時間をロスする人がいると思われる。

例えば、tableが画面(もしくは親コンポーネント)全体に占める割合を80%かつ、heightを300pxにしたいとする。
この時、`v-data-table`には `height` プロパティは存在しているが、`width` プロパティが存在しない(😱 驚愕)。
table自体の幅を変更したいならば、一度親要素のwidthを変更するか、別にstyleを当てる必要がある。

すなわち、

```vue
<template>
  <div>
    <v-data-table
      class="table-desserts"
      :headers="headers"
      :items="desserts"
      :items-per-page="5"
      :height="300"      
    ></v-data-table>
  </div>
</template>

<style scoped>
.table-desserts {
  width: 80%;
}
</style>
```
もしくは、

```vue
<template>
  <div>
    <v-data-table
      style="width: 80%;"
      :headers="headers"
      :items="desserts"
      :items-per-page="5"
      :height="300"      
    ></v-data-table>
  </div>
</template>
```

といった書き方になる。
本来あるコンポーネントの高さ(`height`)と幅(`width`)は同様の方法で指定できて欲しいのに、
`v-data-table`ではそうなっていない。直感的でない上に不便極まりない。

別に最初からclassを定義して(`height`と`width`)指定するか、`v-data-table`内でstyle定義すればいいじゃんという意見もあるかもしれないが、
それならば`height`と言うまどろっこしいプロパティを `v-data-table` から消してほしい。


### カラムごとの幅について


# SOLIDの原則から考えみる

## 単一責任の原則に反している(SOLIDのS)

- headerのスタイルとbodyのスタイルが分離されていない（SOLID)
- paginationのスタイル


## 単一責任の原則に反している(SOLIDのI)

# 申し送り事項
- Vuetify3系ならもっとマシかもしれない。筆者はVuetify3系に移行する前にVue自体卒業し、React + MUIに染まってしまった。
- MUIに慣れてしまったので、Vuetifyのデザインが使いにくいと感じることが多いだけかもしれない。
  - もしVuetifyに慣れ親しんだ体になったらVuetifyの方が使いやすいのかもしれない。



# 結び

直感的でないと感じる項目の数だけ、Google検索の犠牲者が増える。
例えば、「v-data-table 幅 カスタマイズ」とか「v-data-table 行 色指定」とか。
さらにこれらの検索結果の上位の記事が必ずしも解決策を導いてくれるとは限らず、
一人で泥沼にハマったり上司ともども頭を悩ませたりした人は、私だけではないと思う。

Vuetifyのデザインは、デフォルトのデザインが美しいと言われているが、
その美しさの分デザインのカスタマイズが難しいという棘を持つバラのようなデザインフレームワークだと思う。
そのバラの棘を折る勇気と根気を持つ人には、Vuetifyは最高のデザインフレームワークかもしれない。
そうではない開発者は、棘がないけれど美しく咲きうる(カスタマイズ可能な)チューリップのようなフレームワーク(MUIやmantine等)を、
あなたのプロジェクトという庭に植えてみてはいかがだろうか。