---
title: "Understanding the Differences Between MUI Wrapper Components (Box, Container, Paper, Stack)"
emoji: "🎁"
type: "tech" # tech: technical article / idea: idea article
topics: ["React", "MUI", "TypeScript", "JavaScript", "Frontend"]
published: true
---

When developing with React.js, you may often find yourself relying on the beauty and convenience of MUI. Among the frequently used components in MUI are the wrapper components like `Box`, `Container`, `Paper`, and `Stack`.

These components are designed for different purposes. However, I realized that I hadn't been using them with a clear understanding of their differences. It's time to understand the differences between `Box`, `Container`, `Paper`, and `Stack`.

This article aims to organize the differences between MUI wrapper components as a reminder for myself.

:::message
This article is intended for frontend engineers developing with React.js and MUI (formerly Material-UI).
:::

# Box

First, let's look at the `Box` component. Here's what the [MUI official page](https://mui.com/material-ui/react-box/) says:

> The Box component differs from other containers available in Material UI in that its usage is intended to be multipurpose and open-ended, just like a `<div>`. Components like Container, Stack and Paper, by contrast, feature usage-specific props that make them ideal for certain use cases: Container for main layout orientation, Stack for one-dimensional layouts, and Paper for elevated surfaces.

In other words, `Box` is a multipurpose component intended to be used like a `<div>`. It allows for flexible styling, unlike `Container`, `Stack`, and `Paper`, which have specific use-case properties.

This flexibility is due to the abundance of properties. You can directly specify styling properties such as margin, padding, typography, color, layout, and positioning.

```tsx
<Box backgroundColor="rgba(0, 0, 0, 0.1)" margin="60px 0">
  <div style={{ backgroundColor: "rgba(255, 255, 0, 0.3)", width: "40%", height: "50px" }}>Box Content</div>
  <div style={{ backgroundColor: "rgba(255, 255, 0, 0.3)", width: "70%", height: "50px" }}>Box Content</div>
  <div style={{ backgroundColor: "rgba(255, 255, 0, 0.3)", width: "100%", height: "50px" }}>Box Content</div>
</Box>
```

![alt text](/images/doc9/image9-2.png)

You can also style using the `sx` property. However, using direct properties allows for shorthand like `mb` (margin-bottom), making the code more concise.

Another flexibility point is the `component` property. By specifying the `component` property, you can render different HTML elements.

```tsx
<Box component="span"/>
<Box component="form">
```

✅ `Box` is a component that allows for flexible styling with abundant properties.
✅ `Box` can be used like a `<div>`.
✅ `Box` can render different HTML elements by specifying the `component` property.

# Container

Next, let's look at `Container`. Here's the [official description](https://mui.com/material-ui/react-container/):

> The container centers your content horizontally. It's the most basic layout element.

This means that the `Container` centers its content horizontally.

As mentioned in the Box description:

> Container, Stack, and Paper have specific use-case properties.

For `Container`, the specific use case is **centering content horizontally**.

![alt text](/images/doc9/image9-1.png)

Think of it as applying `<div style="margin: 0 auto;">` to the content inside the `Container`.

※ This is just an image. The behavior may differ when using `Container` compared to writing the above div element directly.

You can control the width of the centered content using the `maxWidth` property.

```jsx
<Container maxWidth="sm"></Container>
```

✅ `Container` is a component for centering content.
✅ `Container` can control the width of its content using the `maxWidth` property.

# Paper

Next is the [Paper component](https://mui.com/material-ui/react-paper/).

> The Paper component is a container for displaying content on an elevated surface.

The specific use case for `Paper` is **displaying content on an elevated surface**.

```jsx
<Paper sx={{ backgroundColor: "rgba(0, 0, 0, 0.1)", marginY: "60px" }}>
  <div style={{ backgroundColor: "rgba(255, 0, 0, 0.3)", width: "40%", height: "50px" }}>Paper Content</div>
  <div style={{ backgroundColor: "rgba(255, 0, 0, 0.3)", width: "70%", height: "50px" }}>Paper Content</div>
  <div style={{ backgroundColor: "rgba(255, 0, 0, 0.3)", width: "100%", height: "50px" }}>Paper Content</div>
</Paper>
```

![alt text](/images/doc9/image9-3.png)

You can see that the corners are rounded, and there is a shadow-like decoration. Think of `Paper` as a `div` with `border-radius` and `box-shadow` applied.

※ This is just an image. The behavior may differ when using `Paper` compared to writing the above div element directly.

✅ `Paper` is a component for displaying content on an elevated surface.
✅ `Paper` is similar to a `div` with `border-radius` and `box-shadow` applied.

# Stack

Finally, let's look at the [Stack component](https://mui.com/material-ui/react-stack/).

> Stack is a container component for arranging elements vertically or horizontally.

`Box` allows for flexible styling (grid layout or complex two-dimensional layout), while **`Stack` is specialized for one-dimensional layouts**.

For simple layouts like the one below, using `Stack` can make the code more concise.

```jsx
<h1>Example using Box</h1>
<Box display="flex" flexDirection="column">
  <Box mb={2} bgcolor="lightblue" p={2}>Item 1</Box>
  <Box mb={2} bgcolor="lightgreen" p={2}>Item 2</Box>
  <Box bgcolor="lightcoral" p={2}>Item 3</Box>
</Box>

<h1>Example using Stack</h1>
<Stack spacing={2}>
  <Box bgcolor="lightblue" p={2}>Item 1</Box>
  <Box bgcolor="lightgreen" p={2}>Item 2</Box>
  <Box bgcolor="lightcoral" p={2}>Item 3</Box>
</Stack>
```

![alt text](/images/doc9/image9-4.png)

As shown in the image above, you can achieve the same layout using `Box` + `flexDirection` and `Stack`.

You can also specify the direction of the layout using the `direction` property and display dividers between elements using the `divider` property (very convenient).

```tsx
<h1>Stack / row direction</h1>
<Stack spacing={2} direction="row">
  <Box bgcolor="lightblue" p={2}>Item 1</Box>
  <Box bgcolor="lightgreen" p={2}>Item 2</Box>
  <Box bgcolor="lightcoral" p={2}>Item 3</Box>
</Stack>

<h1>Stack / divider</h1>
<Stack spacing={2} divider={<Divider orientation="horizontal" flexItem />}>
  <Box bgcolor="lightblue" p={2}>Item 1</Box>
  <Box bgcolor="lightgreen" p={2}>Item 2</Box>
  <Box bgcolor="lightcoral" p={2}>Item 3</Box>
</Stack>
```

![alt text](/images/doc9/image9-5.png)

✅ `Stack` is a component for one-dimensional layouts.
✅ `Stack` can specify the layout direction using the `direction` property.
✅ `Stack` can display dividers between elements using the `divider` property.

# Grid (Added on 2024/04/12)

Another wrapper component in MUI is `Grid`. As the name suggests, `Grid` is a component for grid layouts. While `Container` is for centering content and `Stack` is for one-dimensional layouts, `Grid` is for two-dimensional layouts.

For specific usage and code examples, refer to the [official documentation](https://mui.com/material-ui/react-grid/). (I plan to write an article about `Grid` soon.)

*(Added on 2024/04/12)*

# Summary

I have organized the differences between MUI wrapper components (Box, Container, Paper, Stack).

✅ Finally, here is a summary table of their features:

| Component Name | Features and Overview |
|----------------|------------------------|
| Box            | A multipurpose component that allows for flexible styling. Can be used like a `<div>`. |
| Container      | Used for defining the main layout of a page. Suitable for centering content and controlling maximum width. |
| Paper          | A component for displaying content on an elevated surface with decorations like `border-radius` and `box-shadow`. |
| Stack          | A component for one-dimensional layouts. Creates vertical or horizontal linear stacks. |

Understanding this can help you write simpler code using `Container`, `Paper`, or `Stack` where you previously used `Box`, or leverage the flexibility of `Box` for complex layouts.

I hope this article serves as a reminder for myself and helps others. Please bookmark it for future reference.
