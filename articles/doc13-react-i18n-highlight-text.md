---
title: "React-i18nextで翻訳用テキストの一部をハイライトする方法"
emoji: "🔦"
type: "tech" # tech: 技術記事 / idea: アイデア
topics: [react-i18next, i18next, Next.js, React.js, TypeScript]
published: true
---




# 背景
`react-i18next` で国際化対応する時に出会った課題です。




## 課題

国際化対応では通常、辞書となるjsonファイルを用意し、その中に翻訳用のテキストを記述します。

**en.json**

```json
{
  "welcome": "Welcome!",
}
```

**ja.json**

```json
{
  "welcome": "ようこそ",
}
```

これらのテキストをカスタマイズ(eg. 動的変数の埋め込みやリンクの埋め込みなど)する場合、少々工夫が必要です。

私の場合、テキストの一部分をハイライトしたい場面に出会しました。

翻訳用のテキスト全てをハイライトしたい場合は、通常styleを当てた `span` などのタグで囲うかと思います。
しかしこのタグの部分が国際化対応したい文の一部である場合、パッとどうすればいいかわからず途方に暮れてしまいました。

例としては下記の辞書の`name`の部分を黄色くハイライトしたいようなケースです。

**en.json**

```json
{
  "greeting": "Hello, {{name}}!"
}
```

**ja.json**

```json
{
  "greeting": "こんにちは、{{name}}さん！"
}
```



## Setup, Version等

ライブラリの説明やsetupの実施方法については、すでに詳しく取り上げてくれている先人がいますので割愛します。

- [react-i18nextを使って、React環境に1から多言語翻訳の仕組みを構築してみる](https://zenn.dev/aldagram_tech/articles/2551d2a969829a)
- [React-i18nextによるReactアプリの国際化ガイド](https://qiita.com/Exerea/items/cd980562b2a2919a322d)



# 結論
`react-i18next`に用意されている`Trans`コンポーネントの`components` propertyを使うことで解決できます！

```tsx
        <Trans
          i18nKey="greeting"
          values={{ name: "Test User" }}
          components={{
            highlight: <span style={{ backgroundColor: "yellow" }} />,
          }}
        />
```


下記がコードの全貌です。


```tsx
import React from "react";
import { useTranslation, Trans } from "react-i18next";

function App() {
  const { t, i18n } = useTranslation();
  const changeLanguage = (lang: string) => {
    i18n.changeLanguage(lang);
  };

  return (
    <div
      style={{
        display: "flex",
        justifyContent: "center",
        flexDirection: "column",
        width: "800px",
        marginX: "auto",
      }}
    >

      <div style={{ marginBottom: "1rem" }}>
        <button
          onClick={() => changeLanguage("ja")}
          style={{
            fontSize: "16px",
            margin: "0 5px",
            cursor: "pointer",
            width: 100,
            height: 40,
          }}
        >
          日本語
        </button>
        <button
          onClick={() => changeLanguage("en")}
          style={{
            fontSize: "16px",
            margin: "0 5px",
            cursor: "pointer",
            width: 100,
            height: 40,
          }}
        >
          English
        </button>
      </div>

      <h2>{t("welcome")}</h2>
      <p>
        <Trans
          i18nKey="greeting"
          values={{ name: "Test User" }}
          components={{
            highlight: <span style={{ backgroundColor: "yellow" }} />,
          }}
        />
      </p>
    </div>
  );
}

export default App;
```


辞書ファイルの方も忘れずに設定するようにしましょう。
`highlight`というキーが `Trans` コンポーネントのcomponents propertyで指定したものに対応します。

**en.json**

```json
{
  "welcome": "Welcome!",
  "greeting": "Hello, <highlight>{{name}}</highlight>!"
}
```

**ja.json**

```json
{
  "welcome": "ようこそ",
  "greeting": "こんにちは、<highlight>{{name}}</highlight>さん！"
}
```


英語の表示

![alt text](/images/doc13/en.png)


日本語の表示

![alt text](/images/doc13/ja.png)