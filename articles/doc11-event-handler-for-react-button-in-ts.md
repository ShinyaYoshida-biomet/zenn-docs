---
title: "Reactでイベントプロパティには"
emoji: "📑"
type: "tech" # tech: 技術記事 / idea: アイデア
topics: [JavaScript, React, TypeScript]
published: false
---

# 背景

Reactのイベントハンドラの引数では, 第一引数にイベントオブジェクトが渡されます。
しかし人間は愚かな生き物なため、この事実を忘れて誤ったデフォルトの引数を設定し、コードをバグらせることがあります。
初歩的な内容ではあるのですが、かくいう私も愚か無事に、「イベントハンドラの返り値がNaNになる」不具合を生じさせてしまいました。

しかもエラーを吐かないがために、デバックに小一時間かかりストレスフルだったので、
自分への教訓として書き残しておこうと思います。


# 問題のコードスニペット

```jsx
export default function App() {
  // a, b の順序で数値が渡ることを前提としている関数
  // しかし onClick にそのまま渡した場合、a はマウスイベントになる
  const handleClick = (a = 1, b = 5) => {
    console.log("------- args : ", a, b);
    console.log("a - b = ", a - b);
    return a - b;
  };

  return (
    <div
      style={{
        display: "flex",
        flexDirection: "column",
        justifyContent: "center",
        gap: "10px",
      }}
    >
      <h1 style={{ textAlign: "center" }}>
        イベントハンドラにDefault Argsを
        <br />
        設定する場合の注意点
      </h1>

      {/* 正しい順番（例：a=5, b=3）の場合 */}
      <button onClick={() => handleClick(5, 3)}>Case 1: (a=5, b=3) = 2</button>

      {/* 引数を指定しないが無名関数を用いる場合 */}
      <button onClick={() => handleClick()}>Case 2: (a=1, b=5) = -4</button>

      {/* イベントオブジェクトが a に入るがエラーにはならない（NaNが出る） */}
      <button onClick={handleClick}>Case 3: ??? = ???</button>
    </div>
  );
}

```



![alt text](/images/doc11/image.png)

一見すると、問題なさそうなコードですが、実際に実行してみると以下のような結果になります。

- Case1は正常に動作する(a -b = 2)が帰ってくる
- Case2も正常に動作する(a -b = -4)が帰ってくる
- Case3はNaNが帰ってくる


# 原因と予防策

Case3がNaNが帰ってくる理由は、イベントハンドラの第一引数にはイベントオブジェクトが渡されるためです。
そのため、bにはデフォルト引数の`5`が渡される一方で、aの値は数値ではなくObjectとなってしまいます。
このため、a - b の計算ができず、NaNが返ってくるのです。

タチが悪いのは、TypescriptではなくJavascriptを利用する場合、明示的にエラーが発生しない事です。
逆にTypescriptで上記のようなコードを書くと、下記のようなエラーがEditorで確認できるはずです。

```text
Type '(a?: number, b?: number) => number' is not assignable to type 'MouseEventHandler<HTMLButtonElement>'.
```

予防策としては、教訓として脳に刻み込むか、やはりTypeScriptを使用してコンパイル時にエラーを検知することが有効です。

# 結論
結論、私は愚かな生き物なので、脳に刻み込むだけでは不十分だと認識しています。
ですのでTypeScriptとともに生きていくことを決意した次第です。