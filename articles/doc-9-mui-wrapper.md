---
title: "MUIのwrapper系コンポーネント(Box, Container, Paper, Stack)の違いを、そろそろ理解してみた"
emoji: "🎁"
type: "tech" # tech: 技術記事 / idea: アイデア
topics: ["React.js", "Material-UI", "TypeScript", "JavaScript", "Frontend"]
published: false
---


React.jsでフロントエンド開発をしていると、MUIの美しさと便利さに助けられることが多々あるかと思います。
MUIを使用していると、よく登場するのが, `Box`, `Container`, `Paper`, `Stack`などのwrapper系コンポーネントです。
これらのコンポーネントは、それぞれ異なる目的のために設計されています。
ですが、個人的にその違いを明確に理解して使っていなかったことに気づきました。

今回は備忘録も兼ねて、MUIのwrapper系コンポーネントの違いを整理してみたいと思います。

:::message
この記事は、React.js + MUI(旧Material-UI)で開発をしているフロントエンドエンジニアの方を対象としています。
:::


# Box

まずBoxコンポーネント。[MUIの公式ページ](https://mui.com/material-ui/react-box/)でどのような説明がなされているか見てみましょう。


> The Box component differs from other containers available in Material UI in that its usage is intended to be multipurpose and open-ended, just like a `<div>`. Components like Container, Stack and Paper, by contrast, feature usage-specific props that make them ideal for certain use cases: Container for main layout orientation, Stack for one-dimensional layouts, and Paper for elevated surfaces.

日訳
> Boxコンポーネントは、Material UIで利用可能な他のコンテナとは異なり、その使い方は`<div>`のように多目的で自由であることが意図されています。対照的に、Container、Stack、Paperのようなコンポーネントは、特定のユースケースに理想的な、用途に特化したプロップを備えています： Containerはメインレイアウトに、Stackは一次元レイアウトに、Paperは昇降面に適しています。

つまりBoxは`div`のように、多目的な用途が前提で柔軟なスタイリングが可能なコンポーネントです。
対して、Container、Stack、Paperはそれぞれ特定の用途に特化したプロパティを持っています。

この柔軟という言葉で言わんとしているのは、propertyの豊富さです。
マージン、パディング、タイポグラフィ、カラー、レイアウト、ポジショニングなどのスタイリングプロパティを直接指定することができます。

```tsx
<Box backgroundColor="rgba(0, 0, 0, 0.1)" margin="60px 0">
  <div style={{ backgroundColor: "rgba(255, 255, 0, 0.3)", width: "40%", height: "50px" }}>Box Content</div>
  <div style={{ backgroundColor: "rgba(255, 255, 0, 0.3)", width: "70%", height: "50px" }}>Box Content</div>
  <div style={{ backgroundColor: "rgba(255, 255, 0, 0.3)", width: "100%", height: "50px" }}>Box Content</div>
</Box>
```      

`sx` プロパティを用いてもスタイリングが可能です。
ただ、直接のプロパティを利用するメリットとしては、`mb`(margin-bottom)などの略語を使うことができるため、
コードが簡潔になることが挙げられます。


![alt text](/images/image9-2.png)

また、さらに下記のように `component`プロパティを指定することで異なるHTML要素をレンダリングすることも可能です。

```tsx
<Box component="span"/>
<Box component="form">
```


# Container

次にContainerについて。[公式の説明](https://mui.com/material-ui/react-container/)を見てましょう。

> The container centers your content horizontally. It's the most basic layout element.

日訳
> コンテナは、コンテンツを水平方向に中央配置します。最も基本的なレイアウト要素です。

この水平方向中央配置というのは、Containerで囲まれたコンテンツが画面の中央に配置されるということです。

![alt text](/images/image9-1.png)

イメージとしては、Containerの中身に対して、
`<div style="margin: 0 auto;">`が適用されていると考えればわかりやすいかもしれません。

※ あくまでイメージです。Containerで囲うのと上記のdiv要素を直接書くのとでは、挙動が異なることがあります。

Boxの説明の際に、
>Container、Stack、Paperはそれぞれ特定の用途に特化したプロパティを持っています。

と表記しましたが、Containerの場合の「特定の用途」とは、中身を水平方向に中央配置することなのですね。

中央配置するコンテナの中身の幅は、`maxWidth`プロパティで制御できます。

```jsx
<Container maxWidth="sm"></Container>
```

# Paper

続いて[Paperコンポーネント](https://mui.com/material-ui/react-paper/)。

> The Paper component is a container for displaying content on an elevated surface.

日訳
> Paper Componentはコンテンツを浮き上がったような表面を表示するためのコンテナです。

Containerの特定の用途が「中央配置」であるのに対し、Paperの特定の用途は「表面を浮き上がらせること」です。

```jsx
<Paper sx={{ backgroundColor: "rgba(0, 0, 0, 0.1)", marginY: "60px" }}>
  <div style={{ backgroundColor: "rgba(255, 0, 0, 0.3)", width: "40%", height: "50px" }}>Paper Content</div>
  <div style={{ backgroundColor: "rgba(255, 0, 0, 0.3)", width: "70%", height: "50px" }}>Paper Content</div>
  <div style={{ backgroundColor: "rgba(255, 0, 0, 0.3)", width: "100%", height: "50px" }}>Paper Content</div>
</Paper>
```

![alt text](/images/image9-3.png)


角が丸くなって、box-shadowがついているのがわかります。
ですのでPaperは`div`タグに、`border-radius`と`box-shadow`を適用したようなものと考えるとわかりやすいかもしれません。

※ あくまでイメージです。Paperで囲うのと上記のdiv要素を直接書くのとでは、挙動が異なることがあります。

# Stack



# 普通の`div`タグ

# まとめ


| コンポーネント名 | 特徴と概要 | メリット | デメリット |
|----------------|----------------|-----------------------------------|--------------------------------------------|
| Box            | 多目的で柔軟なスタイリングが可能なコンポーネント。`<div>`のように使用できる。 | 直接スタイルプロパティを適用でき、細かいスタイリングが容易。 | 多くのプロパティを理解し適切に使う必要がある。 |
| Container      | ページの主要なレイアウトを定義するために使用。中央配置や最大幅の制御に適している。 | レスポンシブデザインのためのレイアウト整備が簡単。 | 細かいスタイリングの柔軟性には限界がある。 |
| Paper          | シャドウや丸みを帯びた角など、浮き出た表面を表現するためのコンポーネント。 | 視覚的な深みを与えるデザインが簡単に実現できる。 | 単体ではレイアウトの構成要素として機能しない。 |
| Stack          | 一次元レイアウトを容易にするためのコンポーネント。縦か横の線形スタックを作成する。 | アイテムを縦または横に整列させるのが簡単で、レイアウトが直感的。 | 複雑なレイアウトには適さない可能性がある。 |
| 通常のdivタグ  | 最も基本的なHTML要素。カスタムスタイリングやレイアウトのために広く使われる。 | 汎用性が高く、どんなスタイルも適用可能。 | レスポンシブやアクセシビリティの機能を自分で管理する必要がある。 |
