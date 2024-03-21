---
title: "lodashと既存のJSメソッドの速度比較"
emoji: "🏎️"
type: "tech" # tech: 技術記事 / idea: アイデア
topics: ['Javascript', 'lodash', 'JS']
published: true
---


# 本記事について
Lodashは、一貫性とカスタマイズ性を提供するJavaScriptライブラリです。
配列、数値、オブジェクト、文字列などの操作を簡単に行うための便利なユーティリティ関数を多数提供しています。

lodashの導入メリットとしては、メソッドの使いやすさや可読性の向上の他にも、処理が高速であることが挙げられます([参考](https://javascript.plainenglish.io/5-reasons-why-you-should-be-using-lodash-in-your-javascript-projects-fd08bdfbfe27#:~:text=Reason%20%233%3A%20Performance%20optimization))。

lodashの中には既存のJSメソッドと同じような機能を持つものが多数存在しておりますが、
上記導入メリットが真実ならば既存のJSのメソッドを使うよりもlodashのメソッドを用いた方が良いと言えるでしょう。
ですが、(私の)肌感としては期待している以上にlodashが世間のプロジェクトに導入されているという印象は受けません。

本記事では、lodashの導入メリットの中でも大きいウェイトを占めるであろう処理速度に焦点を当て、
lodashのメソッドと既存のJSメソッドの速度を比較し、
lodashの導入メリットについて検討していきたいと思います。


# 速度比較

## 比較するメソッド
- map
- filter
- reduce
- forEach
- find
- every
- includes
- some

## 環境
```sh
node -v
npm show lodash version
```


```
>>> v18.16.1
>>> 4.17.21
```

## 準備

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

`timeFunction` は、引数に渡された関数を100回実行し、その平均実行時間を返す関数です。
`compareExecutionTimes` は、JavaScriptの関数とlodashの関数を引数に取り、それぞれの実行時間を標準出力します。
これをnodeのconsoleで実行することで、各種メソッドの実行時間を比較していきます。

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

lodashを使った方が**約6.1倍**速い！


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

lodashを使った方が**約1.5倍**速い！


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

lodashを使った方が**約3.3倍**速い！


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



```js
JavaScript times (ms): 0.12699208000000006
Lodash times (ms): 0.09438289000000002
```

lodashを使った方が**約1.3倍**速い！


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

loashを使った方が**約1.6倍**速い！


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

lodashを使った方が**約3.7倍**速い！

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

includesに関しては、JavaScriptのメソッドを用いた方が約5倍速い



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

lodashを使った方が**約1.9倍**速い！


# まとめ

今回の実験結果をまとめると下記のようになります。

| メソッド  | JavaScript (ms) | Lodash (ms) | 倍速比 |
|-----------|-----------------|-------------|--------|
| map       | 0.182           | 0.030       | **6.1x**   |
| filter    | 0.228           | 0.157       | **1.5x**   |
| reduce    | 0.154           | 0.047       | **3.3x**   |
| forEach   | 0.127           | 0.094       | **1.3x**   |
| find      | 0.092           | 0.057       | **1.6x**   |
| every     | 0.137           | 0.037       | **3.7x**   |
| includes  | 0.010           | 0.045       | 0.2x   |
| some      | 0.062           | 0.033       | **1.9x**   |


includesメソッドを除いて、**基本的にはlodashのメソッドの方が高速**であることがわかりました。
特にmap、reduce、everyのような操作でLodashが顕著に優れたパフォーマンスを発揮することがわかります

また、冒頭申し上げた通り、lodashはパフォーマンスだけでなく、可読性や使いやすさも高いため、
プロジェクトに導入することで、開発効率を向上させることができると結論付けられそうです。

ただし、本結果はあくまで一例であり、
配列の大きさや内容、各種メソッドの使い方によっては結果が異なる可能性があります。
また、lodashの導入にはパフォーマンス以外にも様々な要素が関わってくるため、
やはりプロジェクトの要件や必要な処理に合わせて検討することが重要かと思います。

本記事がWeb開発者である皆様のお役に立てれば幸いです。

※JS関連でパフォーマンス計測を行うのは、個人的に初めての試みでした。
もし間違いや改善点があれば、お気軽にコメントいただけると幸いです。