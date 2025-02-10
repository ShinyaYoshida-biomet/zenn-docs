---
title: "Routing Comparison: React VS Next.js / Vue.js VS Nuxt.js"
emoji: "📋"
type: "tech" # tech: technical article
topics: ["javascript", "Vue", "Nuxt", "React", "Next"]
published: true
---

React.js and Vue.js are highly regarded frameworks in the developer community for their attractiveness and functionality. However, transitioning between these frameworks can sometimes lead to confusion and bugs due to differences in routing. Developers familiar with Vue.js may find themselves spending time understanding the routing syntax and paradigms when they switch to React.js, and vice versa.

Additionally, these frameworks have specialized frameworks, Nuxt.js for Vue.js and Next.js for React.js, which can also cause issues during transitions.

In this article, I will create a quick reference table comparing the routing differences between React.js and Next.js, and Vue.js and Nuxt.js, to help developers avoid confusion during transitions and for my own reference.

# Definitions

- **Programmatic Navigation**: A method of navigating pages within code. It dynamically changes routes using JavaScript.
- **Declarative Navigation**: A method of specifying page navigation directly within JSX or templates.

※ These definitions are based on the usage in the official documentation of Vue Router and React Router ([reference](https://router.vuejs.org/guide/essentials/navigation.html#Programmatic-Navigation)).

# React.js VS Next.js

## Comparison Table

|                        | React.js                   | Next.js                                       |
|------------------------|----------------------------|-----------------------------------------------|
| **Programmatic Navigation** | ・Use the `useNavigate()` hook <br> ・Set up with `createXXXRouter` from `react-router-dom` | Use the `useRouter()` hook <br> ・Define components in the `pages` directory |
| **Declarative Navigation**  | `<Link>` from `react-router-dom`           | `<Link>` from `next/link`                     |

***Note***
- This assumes the use of React hooks within function components, not class components.
- `react-router-dom` is assumed to be version 6 or later.
- For Next.js, it is assumed that components are defined in the `pages` directory ([Next.js v13 and later use the app directory](https://nextjs.org/blog/next-13-4)).
- Both Programmatic Navigation and Declarative Navigation have several alternative methods, but this article lists the most common ones.

## Examples

### Programmatic Navigation

#### React.js
```jsx
import { useNavigate } from 'react-router-dom';

function Page() {
  // Get the navigate function using the useNavigate hook
  const navigate = useNavigate();

  // Function to navigate to the home page
  function goToHome() {
    navigate('/');  // Specify the path to navigate
  }

  // Execute goToHome on button click
  return <button onClick={goToHome}>Go to Home</button>;
}
```

#### Next.js
```jsx
import { useRouter } from 'next/router';

function Page() {
  // Get the router object using the useRouter hook
  const router = useRouter();

  // Function to navigate to the home page
  function goToHome() {
    router.push('/');  // Specify the path to navigate. Note the use of the push method
  }

  // Execute goToHome on button click
  return <button onClick={goToHome}>Go to Home</button>;
}
```

Note that you need to call `router.push` instead of just `router`.

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

Note that the property to specify the destination in the Link element is `to`.

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

Note that the property to specify the destination in the Link element is `href`.

## Reference Links
- [react-router-dom documentation useNavigate](https://reactrouter.com/en/main/hooks/use-navigate)
- [react-router-dom documentation Link](https://reactrouter.com/en/main/components/link)
- [next.js documentation useRouter](https://nextjs.org/docs/pages/api-reference/functions/use-router)
- [next.js documentation Link](https://nextjs.org/docs/pages/api-reference/components/link)

# Vue.js VS Nuxt.js

|                        | Vue.js                                       | Nuxt.js                                        |
|------------------------|----------------------------------------------|------------------------------------------------|
| **Programmatic Navigation** | ・`this.$router.push()`<br> ・Set up in `router/index.js` | ・`this.$router.push()`<br> Define components in the `pages` directory |
| **Declarative Navigation**  | `<router-link>` |`<NuxtLink>` |

***Note***
- This assumes Vue 2.x. For information on the Composition API, please refer to another article ([reference](https://zenn.dev/fuqda/articles/2781d2f5196a4c)).

## Examples

### Programmatic Navigation

The code is almost the same, but in Vue.js, you set it up in `router/index.js`, while in Nuxt.js, you define components in the `pages` directory.

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

### Declarative Navigation

#### Vue.js
```vue
<template>
  <nav>
    <router-link to="/">Home</router-link>
  </nav>
</template>
```

If Vue Router is integrated into the project, there is no need to import `<router-link>`.

#### Nuxt.js
```vue
<template>
  <nav>
    <NuxtLink to="/">Home</NuxtLink>
  </nav>
</template>
```

Similarly, `<NuxtLink>` is included in Nuxt, so there is no need to import it with other components. Convenient!

## Reference Links
- [Vue Router documentation](https://router.vuejs.org/guide/essentials/navigation.html)
- [Nuxt.js Routing documentation](https://nuxt.com/docs/getting-started/routing)

# Conclusion
Routing is a core part of front-end development. To get a good start when learning a new framework or to avoid confusion caused by differences between frameworks in various projects, I hope this article will be helpful to someone.
