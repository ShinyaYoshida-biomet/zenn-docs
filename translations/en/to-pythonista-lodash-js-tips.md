---
title: "For Former Pythonistas: Lodash Methods to Handle JavaScript Arrays Like Python"
emoji: "📦"
type: "tech" # tech: technical article / idea: idea article
topics: ["JavaScript", "Lodash", "Numpy", "Python", "Node"]
published: true
---

:::message
This article is for those who are familiar with Python but are struggling with JavaScript.
:::

# Introduction

I used to be a Pythonista who loved Python. However, due to a twist of fate, I found myself working on web development using JavaScript.

What awaited me was a different language syntax from Python. Among them, handling JavaScript arrays was different from Python lists. ※ In the past, I stumbled upon the [difference between `in` in Python and JavaScript](https://hogehoge.com).

However, I gradually realized 💡 that JS and Python arrays are not entirely different. Especially after discovering the `lodash` library, I found myself handling JavaScript arrays with a sense similar to using Python's `numpy` or `pandas` `Series`.

In this article, I will introduce `lodash` methods that Pythonistas might find useful. I hope this helps reduce the allergic reaction to JavaScript arrays and improves the development experience for those who started with Python. 🙏

# Preparation

## Environment

- Python: 3.10.4
- numpy: 1.2.5
- pandas: 2.0.2

- Node: v20.11.1
- lodash: lodash-es@4.17.21

Install numpy and lodash using `pip` and `npm` (or `yarn`), respectively.

```sh
pip install numpy
npm install lodash
```

## Test Data

```py
import numpy as np
import pandas as pd

test_array = np.array([1,2,3,4,5,6,7,8,9])
test_array2 = np.array([10,11])
test_series = pd.Series(test_array)
test_multi_array = np.array([[1,2,3],[4,5,6],[7,8,9]])
test_non_unique_array = np.array([1,1,2,2,3,3])
```

```js
const { _ : lodash } = require('lodash')

const testArray = [1,2,3,4,5,6,7,8,9]
const testArray2 = [10,11]
const testMultiArray = [[1,2,3],[4,5,6],[7,8,9]]
const testNonUniqueArray = [1,1,2,2,3,3]
```

# Introduction to Lodash Methods

## Display Top (Bottom) n Items: take / takeRight

### Get the First Element: take

In Python, you use the `head()` method to check the top of a `Series` (or `DataFrame`).

```py
>>> print(test_series.head())
0    1
1    2
2    3
3    4
4    5
dtype: int64
```

This method is very useful when checking the contents of a variable in a runtime environment like a notebook. By default, it displays the top 5 items, but you can change the number of items displayed by specifying an argument.

```py
>>> print(test_series.head(3))
0    1
1    2
2    3
```

Now, for those of you who are struggling because JavaScript doesn't have a built-in `head` method 👉, `lodash` has a familiar `head` method.

```js
> console.log(lodash.head(testArray))
1
```

However, the `head` method in lodash returns only the **first item** of the array. Unlike `Pandas.Series` `head`, it does not allow you to change the number of items retrieved by specifying an argument.

If you want to get the top n items of an array, use the `take` method.

```js
> console.log(lodash.take(testArray, 5))
[1, 2, 3, 4, 5]
```

## Get the Last Element: tail

Similarly, the `tail` method of `Pandas.Series` returns the last elements of the array.

```py
>>> print(test_series.tail())
4    5
5    6
6    7
7    8
8    9
dtype: int64
```

However, in `lodash`, although there is a `tail` method, it is different from the `tail` method of `Pandas`.

```js
> console.log(lodash.tail(testArray))
[
  2, 3, 4, 5,
  6, 7, 8, 9
]
```

The `tail` method in `lodash` returns the array **excluding the last element**.

Therefore, to achieve the same behavior as the `tail` method of `Pandas.Series`, use the `takeRight` method. The `takeRight` method retrieves n items from the end of the array.

```js
> console.log(lodash.takeRight(testArray, 5))
[5, 6, 7, 8, 9]
```

## Array Concatenation: concat

The `concatenate` method of `numpy` is used to concatenate multiple `np.ndarray`.

```py
>>> print(np.concatenate([test_array, test_array2]))
[ 1  2  3  4  5  6  7  8  9 10 11]
```
In `pandas.Series`, you can achieve the same thing using the `append` or `concat` methods.

To achieve the above array concatenation in `lodash`, use the `concat` method.

```js
> console.log(lodash.concat(testArray, testArray2))
[
   1, 2, 3, 4,  5,
   6, 7, 8, 9, 10,
  11
]
```

⚠️ Note that while `numpy`'s `concatenate` takes a list as an argument, `lodash`'s `concat` takes variable-length arguments. For example, you can concatenate multiple arrays by specifying them as follows.

```js
> console.log(lodash.concat(testArray, testArray2, testArray2))
[
   1, 2, 3,  4,  5,  6,
   7, 8, 9, 10, 11, 10,
  11
]
```

## Flattening Arrays: flatten

In fields like image processing and NLP, multidimensional arrays are often handled. The `flatten` method of `numpy` was useful for converting multidimensional arrays to one-dimensional arrays.

```py
>>> print(test_multi_array.flatten())
[1 2 3 4 5 6 7 8 9]
```

JavaScript's `lodash` also has a `flatten` method.

```js
> console.log(lodash.flatten(testMultiArray))
[
  1, 2, 3, 4, 5,
  6, 7, 8, 9
]
```

If you don't know about the `flatten` method, you might end up writing extra map processing. Using the `flatten` method can improve code readability 👆.

## Removing Duplicates: uniq

The `unique` method of `numpy` was useful for removing duplicates in arrays.

```py
>>> print(np.unique(test_non_unique_array))
[1 2 3]
```

Similarly, `lodash` has a `uniq` method.

```js
> console.log(lodash.uniq(testNonUniqueArray))
[ 1, 2, 3 ]
```

If you don't know about the `uniq` method, you might end up writing extra filter processing. Using the `uniq` method can improve code readability 👆.

## Extracting Differences Between Arrays: difference

The `setdiff1d` method of `numpy` was convenient for extracting elements that are in the first array but not in the second array.

```py
>>> print(np.setdiff1d(test_array, test_non_unique_array))
[4 5 6 7 8 9]
```

Similarly, `lodash` has a `difference` method.

```js
> console.log(lodash.difference(testArray, testNonUniqueArray))
[ 4, 5, 6, 7, 8, 9 ]
```

If you don't know about the `difference` method, you might end up implementing it using map and conditional branching, which could result in slightly complicated code. Indeed, the methods of `lodash` are great 👆.

# Conclusion

How was it? For those who used to use the methods of `Numpy` or `Pandas`, the methods of `lodash` should feel very user-friendly.

In addition to the methods introduced this time, `lodash` has many other useful methods. When you get stuck with JavaScript array processing, or when you miss the one-liner code you had in Python, please refer to the [official documentation of `lodash`](https://lodash.com/docs/4.17.15). It should make you feel happier than a few minutes ago.