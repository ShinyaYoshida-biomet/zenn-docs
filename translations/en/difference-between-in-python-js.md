---
title: "The Difference Between 'in' in Python and JS" # 
emoji: "🐬" # Emoji used as a thumbnail (only one character)
type: "tech" # tech: technical article / idea: idea article
topics: ["javascript", "python", "list", "in"] # Tags. Specify like ["markdown", "rust", "aws"]
published: true # Publication setting (false for draft)
---

# Background

I have been using Python for several years in my work, but recently I started working mainly with JavaScript due to a job change.

The mistake I encountered this time is probably very basic for those who have studied JavaScript thoroughly, but for those who chose Python as their first programming language, it might be something they stumble upon. Depending on personal ability, it took me several tens of minutes to figure out the cause, so I decided to share it in an article.

# Phenomenon

First, in Python, to check if a specific value is in an array, you use the syntax `variable in array`. For example, the following code:

```py
a = [1, 3, 5]
b = 5
print(b in a)

>>> True
```
As expected, this output returns True.

Now, knowing that there is a reserved word `in` in JavaScript, I wrote the code to check if a specific value is in an array in the same way without any doubt.

```js
const a = [1, 3, 5]
const b = 5
console.log(b in a)

>>> False
```

What happened!!
Despite the value `5` being in the array, the result of `b in a` returns False.

# Difference in the meaning of 'in'

When `in` is used with an array in Python, it checks if the "value" exists in the array.

On the other hand, when `in` is used with an array in JavaScript, it checks if the "index" exists in the array.

In the previous JavaScript example, it checks if the 5th index exists in the array `a` since `b = 5`.

The length of the array `a` is 3, so the maximum index is 2, and since the 5th index does not exist, it returns False.

In the first place, the `in` clause in JavaScript is applied to Objects to check if a specific key exists in the Object.
Since the keys of an array can be considered as indexes, applying it to an array results in this behavior.

By the way, in Python, applying `in` to a dict checks if a key exists in the dict.

# Summary
Finally, let's summarize the difference in the behavior of `in` between Python and JavaScript.

| ObjectType | Python | JavaScript |
| --- | --- | --- |
| Dict / Object | Checks if key exists | Checks if key exists |
| List / Array | Checks if value exists | Checks if index (key) exists |

※ As you know, `in` in Python can also be applied to str and tuple.
