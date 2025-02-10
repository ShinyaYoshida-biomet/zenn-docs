---
title: "Speed Comparison Between Lodash and Native JS Methods"
emoji: "🏎️"
type: "tech" # tech: technical article / idea: idea article
topics: ['Javascript', 'lodash', 'JS']
published: true
---

# About This Article
Lodash is a JavaScript library that provides consistency and customization. It offers many utility functions to easily manipulate arrays, numbers, objects, and strings.

One of the benefits of using Lodash is its speed, in addition to the ease of use and readability of its methods ([reference](https://javascript.plainenglish.io/5-reasons-why-you-should-be-using-lodash-in-your-javascript-projects-fd08bdfbfe27#:~:text=Reason%20%233%3A%20Performance%20optimization)).

Lodash has many methods that perform similar functions to existing JS methods. If the benefits of using Lodash are true, it would be better to use Lodash methods instead of native JS methods. However, I don't feel that Lodash is as widely adopted in projects as I would expect.

In this article, I will focus on the speed of Lodash methods compared to native JS methods and examine the benefits of using Lodash.

# Speed Comparison

## Methods to Compare
- map
- filter
- reduce
- forEach
- find
- every
- includes
- some

## Environment
```sh
node -v
npm show lodash version
```

```
>>> v18.16.1
>>> 4.17.21
```

## Preparation

```sh
npm install lodash
```

or

```sh
yarn add lodash
```

```js
const {
  map,
  filter,
  reduce,
  forEach,
  find,
  every,
  includes,
  some,
  sum,
} = require('lodash');
const largeArray = [...Array(10000)].map((_, index) => index);

const timeFunction = async (func) => {
    let times = [];
    for (let i = 0; i < 100; i++) {
        const start = process.hrtime.bigint();
        func();
        const end = process.hrtime.bigint();
        times.push(Number(end - start) / 1e6);
    }
    return times;
};

async function compareExecutionTimes(jsFunction, lodashFunction) {
    const jsTimes = await timeFunction(jsFunction);
    const jsAverageTime = sum(jsTimes) / jsTimes.length;
    const lodashTimes = await timeFunction(lodashFunction);
    const lodashAverageTime = sum(lodashTimes) / lodashTimes.length;

    console.log('JavaScript times (ms):', jsAverageTime);
    console.log('Lodash times (ms):', lodashAverageTime);
}
```

`timeFunction` is a function that takes another function as an argument, executes it 100 times, and returns the average execution time.
`compareExecutionTimes` takes a JavaScript function and a Lodash function as arguments and prints their execution times to the console.
By running this in the node console, we can compare the execution times of various methods.

## map

```js
const jsMap = () => largeArray.map(x => x + 1);
const lodashMap = () => map(largeArray, x => x + 1);

compareExecutionTimes(jsMap, lodashMap);
```

```
JavaScript times (ms): 0.18186954000000002
Lodash times (ms): 0.030223310000000003
```

Using Lodash is about **6.1 times** faster!

## filter

```js
const jsFilter = () => largeArray.filter(x => x % 2 === 0);
const lodashFilter = () => filter(largeArray, x => x % 2 === 0);

compareExecutionTimes(jsFilter, lodashFilter);
```

```
JavaScript times (ms): 0.22823043999999992
Lodash times (ms): 0.15747039000000002
```

Using Lodash is about **1.5 times** faster!

## reduce

```js
const jsReduce = () => largeArray.reduce((acc, x) => acc + x, 0);
const lodashReduce = () => reduce(largeArray, (acc, x) => acc + x, 0);

compareExecutionTimes(jsReduce, lodashReduce);
```

```
JavaScript times (ms): 0.15384286
Lodash times (ms): 0.04705250999999994
```

Using Lodash is about **3.3 times** faster!

## forEach

```js
const jsForEach = () => {
  largeArray.forEach(x => {});
};
const lodashForEach = () => {
  forEach(largeArray, x => {});
};

compareExecutionTimes(jsForEach, lodashForEach);
```

```
JavaScript times (ms): 0.12699208000000006
Lodash times (ms): 0.09438289000000002
```

Using Lodash is about **1.3 times** faster!

## find

```js
const jsFind = () => {
  largeArray.find(x => x === 5000);
};
const lodashFind = () => {
  find(largeArray, x => x === 5000);
};

compareExecutionTimes(jsFind, lodashFind);
```

```
JavaScript times (ms): 0.09228125999999999
Lodash times (ms): 0.05709375
```

Using Lodash is about **1.6 times** faster!

## every

```js
const jsEvery = () => {
  largeArray.every(x => x < 10000);
};
const lodashEvery = () => {
  every(largeArray, x => x < 10000);
};

compareExecutionTimes(jsEvery, lodashEvery);
```

```
JavaScript times (ms): 0.13662708
Lodash times (ms): 0.036789979999999924
```

Using Lodash is about **3.7 times** faster!

## includes

```js
const jsIncludes = () => {
  largeArray.includes(5000);
};
const lodashIncludes = () => {
  includes(largeArray, 5000);
};

compareExecutionTimes(jsIncludes, lodashIncludes);
```

```
JavaScript times (ms): 0.00979915999999999
Lodash times (ms): 0.045401299999999985
```

For includes, using the native JavaScript method is about **5 times** faster.

## some

```js
const jsSome = () => {
  largeArray.some(x => x === 5000);
};
const lodashSome = () => {
  some(largeArray, x => x === 5000);
};

compareExecutionTimes(jsSome, lodashSome);
```

```
JavaScript times (ms): 0.062227170000000005
Lodash times (ms): 0.03312500000000001
```

Using Lodash is about **1.9 times** faster!

# Summary

The results of this experiment are summarized as follows:

| Method   | JavaScript (ms) | Lodash (ms) | Speed Ratio |
|----------|------------------|-------------|-------------|
| map      | 0.182            | 0.030       | **6.1x**    |
| filter   | 0.228            | 0.157       | **1.5x**    |
| reduce   | 0.154            | 0.047       | **3.3x**    |
| forEach  | 0.127            | 0.094       | **1.3x**    |
| find     | 0.092            | 0.057       | **1.6x**    |
| every    | 0.137            | 0.037       | **3.7x**    |
| includes | 0.010            | 0.045       | 0.2x        |
| some     | 0.062            | 0.033       | **1.9x**    |

Except for the includes method, it is clear that Lodash methods are generally faster. Especially for operations like map, reduce, and every, Lodash shows significantly better performance.

As mentioned at the beginning, Lodash not only offers performance benefits but also improves readability and ease of use. Therefore, it can be concluded that introducing Lodash into a project can improve development efficiency.

However, these results are just one example, and the results may vary depending on the size and content of the array and how each method is used. Additionally, there are many factors to consider when introducing Lodash into a project, so it is important to consider the project requirements and necessary operations.

I hope this article is helpful to web developers.

※ This was my first attempt at measuring performance related to JS. If there are any mistakes or improvements, please feel free to comment.
