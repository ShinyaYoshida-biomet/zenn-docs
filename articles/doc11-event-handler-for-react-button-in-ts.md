---
title: "ReactでイベントハンドラにDefault Argsを設定する場合の注意点"
emoji: "🔨"
type: "tech" # tech: 技術記事 / idea: アイデア
topics: [JavaScript, React, TypeScript]
published: true
---

# 背景

`React.js`のイベントハンドラでは、第一引数にイベントオブジェクト（`MouseEvent`など）が渡されます。
人間は愚かな生き物なため、この初歩的な事実を忘れて **誤ったデフォルトの引数を設定しコードをバグらせる** ことがあります。

私も実際過去に「イベントハンドラの返り値がNaNになる」という不具合を起こしてしまいました。
しかもエラーを明示的に吐かない不具合だったため原因の特定に時間がかかり、とてもストレスフルでした。

そこで、今回の件を自分への教訓として書き残しておこうと思います。


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

- Case 1: (a=5, b=3) → 2 が返ってくる（正常動作）
- Case 2: (a=1, b=5) → -4 が返ってくる（こちらも正常動作）
- Case 3: NaN が返ってくる



# 原因と予防策

Case3 が NaN を返す理由は、Reactのイベントハンドラにおいて、第一引数にはイベントオブジェクトが渡されるためです。
`onClick={handleClick}` のように無名関数でラップしない形で直接渡すと、
`a` にイベントオブジェクトが入る一方で `b` にはデフォルト引数 5 が割り当てられます。
その結果、`a - b` では数値の計算ができずNaNが帰ってきます。

ここで厄介なのは、JavaScriptでは動的型付けのため明示的にエラーになりにくいことです。
一方、TypeScriptなら以下のようにエラーとして検知できます。

```text
Type '(a?: number, b?: number) => number' is not assignable to type 'MouseEventHandler<HTMLButtonElement>'.
```

## 予防策
予防策としてまとめると以下のようになるかと思います。

1. **TypeScriptで型を厳格にする**
  イベントオブジェクトを受け取る型と、数値を計算する関数の型が混在しないように設計すること。
1. **イベントオブジェクトを受け取る部分と、計算処理を分ける**
  イベントハンドラは MouseEvent を受け取り、その中でビジネスロジック用の関数を呼び出す形にするなど、役割を分離した設計にすること。
1. **無名関数で明示的に引数を指定する**
  `onClick={() => handleClick(5, 3)}` のように、引数を正確に渡す場合は必ず無名関数でラップする。

# 結論
結論として、愚かな私は **「TypeScriptとともに生きていく」** ことを選びました。
JavaScriptは自由度が高い反面、このようなバグが潜り込みやすく、さらに実行時エラーになりにくいという欠点があります。
今回の事例を教訓に、型チェックを利用してバグを予防していきたいと誓った次第です。