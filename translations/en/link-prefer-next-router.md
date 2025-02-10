---
title: "3 Reasons Why Next.js <Link> Tag Should Be Preferred Over useRouter"
emoji: "🔗"
type: "tech" # tech: technical article / idea: idea article
topics: ["JavaScript", "React", "Next"]
published: true
---

# Introduction

When navigating pages in Next.js, you have two options: using the `<Link>` tag or the `useRouter` hook.

Both methods can execute page navigation without any issues, so you might have wondered which one to use.

The `useRouter` hook should be preferred in cases where you need to navigate based on specific conditions or pass parameters during navigation.

In other cases, it is recommended to use the `<Link>` tag. This article explains three reasons why the `<Link>` tag should be preferred over `useRouter`.

※ Note that this article assumes the use of the Pages Router in Next.js, not the App Router introduced in Next.js v13. Therefore, when using the `useRouter` hook, import it from `next/router` instead of `next/navigation`.

# 1. Prefetching

The `<Link>` tag has a prefetching feature that loads the destination page before the user clicks the link. This reduces the time it takes for the destination page to appear.

Here is a quote and translation from the [Next.js official documentation](https://nextjs.org/docs/pages/api-reference/components/link):

> <Link> is a React component that extends the HTML <a> element to provide prefetching and client-side navigation between routes. It is the primary way to navigate between routes in Next.js.

> <Link> is a React component that extends the HTML <a> element to provide prefetching and client-side navigation between routes. It is the primary way to navigate between routes in Next.js.

**Supplement**

> Any <Link /> in the viewport (initially or through scroll) will be prefetched by default (including the corresponding data) for pages using Static Generation. The corresponding data for server-rendered routes is fetched only when the <Link /> is clicked.

For statically generated pages, all `<Link>` elements in the viewport are prefetched by default, while for server-rendered pages, the corresponding data is fetched only when the `<Link>` is clicked.

# 2. SEO

The `<Link>` tag is also effective for SEO.

In a [discussion on vercel/next.js](https://github.com/vercel/next.js/discussions/62496), the following comment was made:

> Yes, router.push('/example') does perform client-side navigation, but it doesn't create a <a> tag, so your links won't be detected by crawlers, which is inherently bad for SEO.

`router.push('/example')` performs client-side navigation but does not create an `<a>` tag, so the links are not detected by crawlers, which is inherently bad for SEO.

On the other hand, the `<Link>` tag creates an `<a>` tag, so the links are detected by crawlers. This ensures that the site's structure is correctly understood by crawlers, making the `<Link>` tag preferable from an SEO perspective.

# 3. Simpler Code

The `<Link>` tag allows you to specify the destination path directly, reducing the amount of code needed compared to using the `useRouter` hook, which requires instantiating the router.

## Example Using `<Link>` Tag

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

## Example Using `useRouter`

```jsx
import { useRouter } from 'next/router';

function Page() {
  // Get the router object using the useRouter hook
  const router = useRouter();
  function goToHome() {
    router.push('/');
  }
  return <button onClick={goToHome}>Home</button>;
}
```

# Conclusion

The `<Link>` tag offers benefits such as prefetching, SEO, and simpler code, making it the recommended choice unless there are specific reasons to use `useRouter`.

It might be a good idea to review your project and see if there are any instances where `useRouter` can be replaced with the `<Link>` tag.
