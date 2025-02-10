---
title: "5 Useful Regex Searches for Frontend Development"
emoji: "🔍"
type: "tech" # tech: technical article / idea: idea article
topics: ["Regex", "Tailwind", "React", "JavaScript", "VSCode"]
published: true
---

When working on frontend development, project files tend to increase. You often want to find files that contain specific code.

Using regular expressions for search can make this easier. This article introduces tips for searching within files or across projects, especially for those developing with JavaScript and TypeScript.

✅ Using regex in VSCode can be very convenient.

![Image-1](/images/doc8/doc8-1.png)

# 1. Find Specific Module Imports

```regex
from\s+.*module-name
```

For example, if you want to search for files using the MUI (formerly Material-UI) library in React:

```ts
import { YYY } from '@mui/material/XXX';
```

Use the following regex to search for such import statements:

```regex
from\s+.*mui
```

![Image-2](/images/doc8/doc8-2.png)

:::message
- `\s` represents whitespace characters.
- `+` means the preceding character appears one or more times.
- `.*` represents any string.
:::

# 2. Find `let` Declarations

In JavaScript and TypeScript, using `const` for variable declarations is often recommended to prevent reassignment.

If you want to search for places where `let` is used:

```ts
array.map((item) => {
  let name = item.name;
  console.log(name);
})
```

Use the following regex:

```regex
\blet\s+[a-zA-Z_][a-zA-Z_0-9]*\s*(=|;)
```

:::message
- `\b` represents a word boundary.
- `[a-zA-Z_]` represents alphabets and `_`.
- `[a-zA-Z_0-9]` represents alphabets and numbers.
- `=` represents assignment, `;` represents the end of a line.
:::

![Image-7](/images/doc8/doc8-7.png)

This structure ensures that sentences like the following are not captured:

```text
Let you go there
```

# 3. Detect Multi-line Comments

Sometimes you forget to delete commented-out code. Before committing, it's good to search for and check such code blocks using the following regex:

```
(\/\/.*\n){2,}
```

![Image-3](/images/doc8/doc8-3.png)

For multi-line comments using `/* ... */`:

```js
/*
 comment 1
 comment 2
*/
```

Use the following regex:

```regex
/\*(.|\n)*\*/
```

![Image-4](/images/doc8/doc8-4.png)

# 4. Detect Variations of "ToDo"

When leaving ToDo comments in a project, variations in notation can cause them to be missed. The regex I use before committing is:

```regex
to(.|\s){0,1}do
```

This captures variations like spaces or `-` between "to" and "do".

![Image-5](/images/doc8/doc8-5.png)

:::message
- `{0,1}` means the preceding character appears 0 or 1 times.
:::

This ensures that sentences like the following are not captured:

```text
I want **to** eat a pancake tonight. Let's **do** the party!
```

# 5. Primitive Hex Color Codes

When using CSS frameworks like Tailwind CSS, it's preferable not to write primitive hex color codes directly in components. Instead, use Tailwind CSS classes.

To search for hex color codes in a project:

```ts
const props = {
  name: "prop",
  color: "#FFF",
  background: "#000080"
}
```

Use the following regex:

```regex
#[0-9A-Fa-f]{6}\b|#[0-9A-Fa-f]{3}\b
```

:::message
- `[0-9A-Fa-f]` represents hexadecimal characters.
- `{6}` or `{3}` means the preceding character appears 6 or 3 times.
- `\b` represents a word boundary.
:::

![Image-6](/images/doc8/doc8-6.png)
