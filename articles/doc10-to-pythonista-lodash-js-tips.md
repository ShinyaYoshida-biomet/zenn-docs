---
title: "かつてPythonistaだったあなたに捧ぐ。JavaScript配列をPythonっぽく扱うためのlodashメソッド"
emoji: "📦"
type: "tech" # tech: 技術記事 / idea: アイデア
topics: ["JavaScript", "Lodash", "Numpy", "Python",  "Node"]
published: true
---


:::message
この記事は、Pythonには慣れている一方で、JavaScriptの処理に戸惑っている方に向けた記事です。
:::


# はじめに

私はかつてPythonをこよなく愛するPythonistaでした。
しかし、ひょんなことから人生の帰路に立たされ、JavaScriptを用いてWeb開発に携わることになりました。

そこに待ち受けていたのは、Pythonとは異なる言語構文でした。
中でもJavaScript配列の扱い方は、Pythonのリストとは異なるものでした。
※ 過去にも、[Pythonの`in`とJavaScriptの`in`の違いにつまづいたこと]((https://hogehoge.com))がありました。


ですが、徐々に気づいたのです 💡。
JSとPythonの配列は全く異なるものではないということに。
特に、`lodash`というライブラリを知ってからは、
Pythonの`numpy`や`pandas`の`Series`を使っている感覚で、JavaScriptの配列を扱える機会が増えました。

そこでこの記事では、Pythonistaの方が便利に感じるであろう`lodash`メソッドを紹介します。
Pythonから開発を始めた人がJavaScriptの配列に対するアレルギー反応を減らし、
開発体験が向上することを祈っております。🙏


# 下準備

## 環境

- Python: 3.10.4
- numpy: 1.2.5
- pandas: 2.0.2

- Node: v20.11.1
- lodash: lodash-es@4.17.21

numpyとlodashはそれぞれ、`pip`と`npm`(or `yarn`)でインストールしてください。

```sh
pip install numpy
npm install lodash
``` 


## テストデータ

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



# lodashメソッド紹介


## 上位(下位)n件数表示: take / takeRight

## 先頭の要素取得: take

Python`Series`(もしくは`DataFrame`)の先頭をチェックする際には、`head()`メソッドを使いますね。

```py
>>> print(test_series.head())
0    1
1    2
2    3
3    4
4    5
dtype: int64
```

ノートブックなどのRuntime環境で変数の中身をチェックする際に、嫌というほどおせわになりました。
デフォルトではそれぞれ上位5件が表示されますが、引数を指定することで表示件数を変更することもできます。

```py
>>> print(test_series.head(3))
0    1
1    2
2    3
```

さて、JavaScriptの組み込みメソッドには`head`やがなくて困っていたそこのあなた 👉。
`lodash`には、おなじみの`head`メソッドが存在します。

```js
> console.log(lodash.head(testArray))
1
```

ただし、lodashの`head`メソッドは、配列の**先頭の1件のみ**を返します。
`Pandas.Series``head`とは異なり、引数を指定して取得件数を変更することもできません。

もし、配列の上位n件を取得したい場合は、`take`メソッドを使いましょう。

```js
> console.log(lodash.take(testArray, 5))
[1, 2, 3, 4, 5]
```


## 末尾の要素取得: tail

同様に`Pandas.Series`の`tail`メソッドは、配列の末尾の要素を返しますね。

```py
>>> print(test_series.tail())
4    5
5    6
6    7
7    8
8    9
dtype: int64
```

しかし`lodash`では、`tail`メソッドが存在するものの、これは`Pandas`の`tail`メソッドとは異なります。


```js
> console.log(lodash.tail(testArray))
[
  2, 3, 4, 5,
  6, 7, 8, 9
]
```


`lodash`の`tail`メソッドの仕様は、Pythonの`tail`とは異なり、
配列の**末尾の要素を除いた配列**を返します。


ですので、`Pandas.Series`の`tail`メソッドと同じような挙動を実現するためには、
`takeRight`メソッドを使いましょう。
`takeRight`メソッドは、配列の末尾からn件を取得します。

```js
> console.log(lodash.takeRight(testArray, 5))
[5, 6, 7, 8, 9]
```

## 配列の結合: concat

`numpy`の`concatenate`メソッドは、複数の`np.ndarray`を結合する際に使います。

```py
>>> print(np.concatenate([test_array, test_array2]))
[ 1  2  3  4  5  6  7  8  9 10 11]
```
`pandas.Series`ならば`append`や`concat`メソッドを使うことで、同様のことが再現できますね。


さて、上記のような配列の結合をlodashで実現するためには、`concat`メソッドを使いましょう。

```js
> console.log(lodash.concat(testArray, testArray2))
[
   1, 2, 3, 4,  5,
   6, 7, 8, 9, 10,
  11
]
```

⚠️ `Numpy`の`concatenate`では引数がリストであるのに対し、
`lodash`の`concat`は可変長引数を取りことに注意しましょう。
例えば、下記のように指定することで複数の配列を結合することもできます。

```js
>  console.log(lodash.concat(testArray, testArray2, testArray2))
[
   1, 2, 3,  4,  5,  6,
   7, 8, 9, 10, 11, 10,
  11
]
```


## 1次元配列化: flatten

画像処理やNLPの分野では、よく多次元の配列を扱うことがあります。
`numpy`の`flatten`メソッドは、多次元配列を1次元配列に変換する際におせわになりました。

```py
>>> print(test_multi_array.flatten())
[1 2 3 4 5 6 7 8 9]
```

JavaScriptの`lodash`にも、`flatten`メソッドが存在します。

```js
> console.log(lodash.flatten(testMultiArray))
[
  1, 2, 3, 4, 5,
  6, 7, 8, 9
]
```

`flatten`メソッドを知らないと、余分なmap処理を書いてしまうことがあるかもしれません。
`flatten`メソッドを使うことで、コードの可読性を向上させましょう 👆。

## 重複の除去: uniq

`numpy`の`unique`メソッドは、配列内の重複を除去する際に重宝してくれました。

```py
>>> print(np.unique(test_non_unique_array))
[1 2 3]
```

同様に、`lodash`には`uniq`メソッドが存在します。

```js
> console.log(lodash.uniq(testNonUniqueArray))
[ 1, 2, 3 ]
```

`uniq`メソッドを知らないと、余分なfilter処理を書いてしまうことがあるかもしれません。
`uniq`メソッドを使うことで、コードの可読性を向上させましょう 👆。


## 配列間の差分の抽出: difference

1つ目の配列に含まれるが、2つ目の配列に含まれない要素を抽出する際には、`numpy`の`setdiff1d`メソッドが便利でしたね。

```py
>>> print(np.setdiff1d(test_array, test_non_unique_array))
[4 5 6 7 8 9]
```

同様に、`lodash`には`difference`メソッドが存在します。

```js
> console.log(lodash.difference(testArray, testNonUniqueArray))
[ 4, 5, 6, 7, 8, 9 ]
```

`difference`メソッドを知らないと、mapと条件分岐を使って実装することになって、
若干ややこしいコードになるかもしれません。
やはり、`lodash`のメソッドは偉大ですね 👆。

# まとめ

いかがでしたでしょうか。
`Numpy`や`Pandas`のメソッドを使っていたPythonistaの方にとっては、
`lodash`のメソッドは非常に使いやすいものだと感じられると思います。

今回ご紹介したメソッド以外にも、`lodash`には多くの便利なメソッドが存在します。
JavaScriptの配列処理でちょっとつまづいてしまった時、
Pythonにあったような1ライナーのコードが懐かしくなった時、
そんな時はぜひ`lodash`の[公式ドキュメント](https://lodash.com/docs/4.17.15)を参照してみてください。
きっと数分前より幸せな気持ちになれるはずです。

