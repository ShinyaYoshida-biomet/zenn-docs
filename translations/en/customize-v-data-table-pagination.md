---
title: "Customizing the Footer (Pagination) of Vuetify's v-data-table"
emoji: "😺"
type: "tech" # tech: technical article / idea: idea article
topics: ["Vue", "JavaScript", "Vuetify"]
published: true
---

Customizing Vuetify's `v-data-table` can be tricky, especially when it comes to making fine adjustments. It's often hard to find the right information. Today, I'll introduce how to customize the footer (pagination) of the `v-data-table`.

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

# Changing the Pagination Text

To change the pagination text (default is "Rows per page:") in the `v-data-table`, pass a property to `footer-props` called `items-per-page-text`. Specifically, it looks like this:

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
※ The script tag is omitted.

![alt text](/images/image-3.png)

The footer text has changed from "Rows per page:" to "Num to display:".

# Changing the Footer Alignment

To change the alignment of the footer in the `v-data-table`, use CSS classes to style it.

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

Now the footer is centered relative to the table. You can also change the background color or other styles as you like.

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

# Hiding Pagination

To hide the pagination in the `v-data-table`, use the `hide-default-footer` and `disable-pagination` properties.

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

Pagination is now disabled, and all items are displayed. Note that if `footer-props` is still present, pagination will be displayed, so be careful.

# Conclusion
I introduced how to customize the footer (pagination) of the `v-data-table`. While it may seem challenging to control the details, with some research, you'll find that there are very few styling options that are truly impossible. I hope this article helps those struggling with Vue-related issues.
