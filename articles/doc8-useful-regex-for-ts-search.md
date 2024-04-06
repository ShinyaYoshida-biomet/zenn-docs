---
title: "Frontend開発で役立つ正規表現検索5選"
emoji: "🔍"
type: "tech" # tech: 技術記事 / idea: アイデア
topics: ["regex", "React", "TypeScript", "JavaScript", "VSCode"]
published: false
---


Frontend開発を続けていると、プロジェクト内のファイルが増えがちです。
プロジェクト内やファイル内で「〇〇な記述があるファイルを見つけたい〜」といった要望がよくあるかと思います。

こんな時正規表現を使って検索すると楽になることがあります。
今回は特にJavaScript, TypeScriptを用いて開発を行っている人向けに、
ファイル内検索/プロジェクト内検索を楽にするtipsを紹介します。

✅ Vscodeの正規表現を用いて検索すると便利です。

![Image from Gyazo](/images/doc8/doc8-1.png)

# 1. 特定のモジュールのimportを見つけたい

```regex
from\s+.*モジュール名
```

例えばReactのUIライブラリであるMUI(旧Material-UI)を使っているファイルを、
網羅的に検索したいとします。

```ts
import { YYY } from '@mui/material/XXX';
```

上記のようなimport文を検索するためには、下記のようなregexを用います。

```regex
from\s+.*mui
```

![Image-2](/images/doc8/doc8-2.png)


:::message
- `\s`は空白文字を表す。
- `+`は直前の文字が1回以上繰り返されることを表す。
- `.*`は任意の文字列を表す。
:::

# 2. letを探す

JavaScript, TypeScriptの変数宣言において、
再代入防止のために`let`ではなく`const`による宣言が推奨される場合が多いです。

プロジェクト内で`let`を使っている箇所を検索したいとします。

```ts
array.map((item) => {
  let name = item.name;
  console.log(name);
})
```

この場合のregexは下記です。

```regex
\blet\s+[a-zA-Z_][a-zA-Z_0-9]*\s*(=|;)
```

:::message
- `\b`は単語の境界を表す。
- `[a-zA-Z_]`はアルファベットと`_`を表す。
- `[a-zA-Z_0-9]`はアルファベット、数字を表す。
- `=`は代入、`;`は行末を表す。
:::

![Image-7](/images/doc8/doc8-7.png)

このような構造にすることで、下記のような文はキャッチしないようになっているのがミソです。

```text
Let you go there
```



# 3. 複数行のコメントアウトを検出する

コメントアウトして後で消そうと思っていたのに、消し忘れていたことがあります。
git commitする前にこのようなコードブロックを確実に消すために、
以下の正規表現で検索し、チェックしておくといいかもしれないです。


```
(\/\/.*\n){2,}
```


![Image-3](/images/doc8/doc8-3.png)

js, tsのコメントアウトの記述方法はもう一種類あります。
`/* ... */` の記述を用いた複数行のコメントアウトを見つけたいとします。

```js
/*
 comment 1
 comment 2
*/
```

この場合のregexは下記です。
```regex
/\*(.|\n)*\*/
```


![Image-4](/images/doc8/doc8-4.png)

# 4. 「ToDo」の表記揺れを検出して検索する

プロジェクト内でToDoを残すことがあるが、表記揺れがあるとToDoを見逃すことがあります。
私が個人的にcommit前に使用している正規表現が下記です。


```regex
to(.|\s){0,1}do
```

toの間にスペースや`-`がある場合もキャッチするため、
`(.|\s)`を使っています。

```text
![Image-5](/images/doc8/doc8-5.png)
```

:::message
- `{0,1}`は直前の文字が0回、もしくは1回繰り返されることを表す。
:::


そのため、たとえtoとdoが別々に出現していたとしても、
次のような文はキャッチしないようになっています。

```text
I want **to** eat a pancake tonight. Let's **do** the party!
```



# 5. 原始的なhex color code

プロジェクトにtailwind css をはじめとするCSSフレームワークを使っている場合、
できれば原始的なhex color codeはコンポーネント内に記述したくないです。
代わりに、tailwind cssのクラスを使うことが推奨されているためです。

リファクタリングのためにプロジェクト内に潜む、下記のようなhex color codeを検索したいとします。

```ts
const props = {
  name: "prop",
  color: "#FFF",
  background: "#000080"
}
```

この時有効なのは下記のregexです。

```regex
#[0-9A-Fa-f]{6}\b|#[0-9A-Fa-f]{3}\b
```

:::message
- `[0-9A-Fa-f]` は16進数を表す。
- `{6}`もしくは`{3}`は直前の文字が6回、もしくは3回繰り返されることを表す。
- `\b`は単語の境界を表す。
:::

![Image-6](/images/doc8/doc8-6.png)

