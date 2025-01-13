---
title: "Reactでイベントプロパティには"
emoji: "📑"
type: "tech" # tech: 技術記事 / idea: アイデア
topics: []
published: false
---

# 背景

Reactのイベントハンドラの引数では, 第一引数にイベントオブジェクトが渡されます。
しかし人間は愚かな生き物なため、この事実を忘れて誤ったデフォルトの引数を設定し、コードをバグらせることがあります。
初歩的な内容ではあるのですが、かくいう私も、「イベントハンドラの返り値がNaNになる」不具合を生じさせてしまいました。

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
      <button onClick={() => handleClick(5, 3)}>Case 1: (5, 3) = 2</button>

      {/* イベントオブジェクトが a に入るがエラーにはならない（NaNが出る） */}
      <button onClick={handleClick}>Case 2: ??? = ???</button>
    </div>
  );
}

```




![alt text](/images/doc11/image.png)