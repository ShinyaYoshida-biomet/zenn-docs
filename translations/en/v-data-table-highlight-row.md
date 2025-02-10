---
title: "How to Highlight Rows with Specific Values in v-data-table"
emoji: "📊"
type: "tech" # tech: technical article / idea: idea article
topics: ["Vue", "JavaScript", "Vuetify", "UI"]
published: true
---

I couldn't find a perfect article in Japanese, so I'm leaving a memo as a reminder.
Similar articles include:

- [Change the background color of odd rows in Vuetify's v-data-table](https://qiita.com/chanchanko/items/ea918401e608d3fc4d13)
- [Import CSV into v-data-table and change the color of items with blank fields](https://teratail.com/questions/278428)

# Environment Information

- `vue: ^2.6.14`
- `vuetify: ^2.7.2`
- `@vue/cli: ^5.0.8`

# Implementation Example

Let's use a sample script from Vuetify 2.x as an example.
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
          // ...other items...
        ],
      }
    },
  }
</script>
```

Before customization, the `v-data-table` looks like this:

![alt text](/images/image-1.png)

Suppose you want to highlight rows with high calories (300kcal or more) in red. You can achieve this using `v-slot` as follows:

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

<!-- script is unchanged, so omitted -->

<style>
.highlight-row {
  /* Customize the color as you like. */
  background-color: #f26651;
}
</style>
```

![alt text](/images/image-2.png)

You can now highlight high-calorie desserts in red.

# Why This Method Works

Using `v-slot`, you can override the default rendering logic. This means that the default rows (`<tr>` elements) and their child elements (`<td>` elements) rendered by `v-data-table` are replaced by the elements defined within `<template v-slot:item="{ item }">`.

Therefore, by specifying `:class="{ 'highlight-row': item.calories > 300 }"` on the `<tr>` element, the `highlight-row` class (which colors the background red) is applied only to rows with calories greater than 300.
