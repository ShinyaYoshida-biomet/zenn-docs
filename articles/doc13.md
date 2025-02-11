---
title: ""
emoji: "👏"
type: "tech" # tech: 技術記事 / idea: アイデア
topics: [react-i18next, i18next, Next.js, React.js, TypeScript]
published: true
---





# 背景


react-i18nextで国際化対応する時の問題。
テキストの一部分をハイライトしたい場合、通常はstyleを当てた `span` などのタグで囲む。
しかしこのタグの部分が国際化対応したい文の一部である場合、どうすればいいかわからず途方に暮れてしまった。

ライブラリの説明やsetupの実施方法については、すでに詳しく取り上げてくれている先人がいますので割愛します。

- [react-i18nextを使って、React環境に1から多言語翻訳の仕組みを構築してみる](https://zenn.dev/aldagram_tech/articles/2551d2a969829a)
- [React-i18nextによるReactアプリの国際化ガイド](https://qiita.com/Exerea/items/cd980562b2a2919a322d)



# 結論

Transコンポーネントを使うことで解決できる。

```tsx
        <Trans
          i18nKey="greeting"
          values={{ name: "Test User" }}
          components={{
            highlight: <span style={{ backgroundColor: "yellow" }} />,
          }}
        />
```


コード全貌

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


英語の表示

![alt text](/images/doc13/en.png)


日本語の表示

![alt text](/images/doc13/ja.png)