---
title: "v-data-tableで特定以上の値を持つ行をハイライトする方法"
emoji: "📊"
type: "tech" # tech: 技術記事 / idea: アイデア
topics: ["Vue", "JavaScript", "Vuetify", "UI"]
published: true
---


日本語で調べてもドンピシャな記事が出てこなかったので、備忘録としてメモを残しておきます。
似ている記事としては下記です。

- [Vuetifyの v-data-tableで奇数行の背景色を変える](https://qiita.com/chanchanko/items/ea918401e608d3fc4d13)
- [v-data-tableにCSVをインポートし、空欄があった場合にはその項目の色を変える](https://teratail.com/questions/278428)


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

![alt text](/images/image-1.png)

例えばこのようなテーブルで、カロリーが高すぎる(300kcal以上)の行を赤くハイライトしたいとします。
その場合、v-slotを使って以下のように実装します。

```vue
<template>
  <div>
    <v-data-table
      style="max-width: 800px; width: 90%; margin: 0 auto"
      :headers="headers"
      :items="desserts"
      :items-per-page="5"
    >
      <template v-slot:item="{ item }">
        <tr :class="{ 'highlight-row': item.calories > 300 }">
          <td>{{ item.name }}</td>
          <td>{{ item.calories }}</td>
          <td>{{ item.fat }}</td>
          <td>{{ item.carbs }}</td>
          <td>{{ item.protein }}</td>
          <td>{{ item.iron }}</td>
        </tr>
      </template>
    </v-data-table>
  </div>
</template>

<!-- scriptは変更なしなので省略 -->

<style>
.highlight-row {
  /* カラーコードはお好みで。 */
  background-color: #f26651;
}
</style>

```


![alt text](/images/image-2.png)

上記のようにハイカロリーなデザートを赤くハイライトすることができました。

# なぜ上記の方法でうまくいくのか

`v-slot`を使うことで、デフォルトのレンダリングロジックをオーバーライドすることができます。
つまり、v-data-tableを使ってレンダリングされるはずだったデフォルトの行（`<tr>` 要素）とその子要素（`<td>` 要素）が、
`<template v-slot:item="{ item }">` の中で定義した要素に置き換わります。

そのため、`<tr>` 要素に対して`:class="{ 'highlight-row': item.calories > 300 }"` を指定することで、
カロリーが300kcal以上の行に対してだけ、`highlight-row` (背景を赤く着色する)クラスが適用されるようになります。

