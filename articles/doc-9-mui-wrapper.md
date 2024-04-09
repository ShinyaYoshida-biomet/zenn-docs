---
title: "MUIのwrapper系コンポーネント(Box, Container, Paper, Stack)の違いを、そろそろ理解してみた"
emoji: "🎁"
type: "tech" # tech: 技術記事 / idea: アイデア
topics: ["React", "MUI", "TypeScript", "JavaScript", "Frontend"]
published: true
---


React.jsでフロントエンド開発をしていると、MUIの美しさと便利さに助けられることが多々あるかと思います。
MUIを使用していると、特によく登場するのが, `Box`, `Container`, `Paper`, `Stack`などのwrapper系コンポーネントです。

これらのコンポーネントは、それぞれ異なる目的のために設計されています。
ですが今まで、個人的にその違いを明確に理解して使っていなかったことに気づきました。

そろそろ `Box`, `Container`, `Paper`, `Stack`の違いを理解してみようと重い腰を上げた次第です。
この記事では備忘録も兼ねて、MUIのwrapper系コンポーネントの違いをそれぞれ整理していきたいと思います。

:::message
この記事は、React.js + MUI(旧Material-UI)で開発をしているフロントエンドエンジニアの方を対象としています。
:::


# Box

まず`Box`コンポーネント。[MUIの公式ページ](https://mui.com/material-ui/react-box/)でどのような説明がなされているか見てみましょう。


> The Box component differs from other containers available in Material UI in that its usage is intended to be multipurpose and open-ended, just like a `<div>`. Components like Container, Stack and Paper, by contrast, feature usage-specific props that make them ideal for certain use cases: Container for main layout orientation, Stack for one-dimensional layouts, and Paper for elevated surfaces.

日訳
> Boxコンポーネントは、Material UIで利用可能な他のコンテナとは異なり、その使い方は`<div>`のように多目的で自由であることが意図されています。対照的に、Container、Stack、Paperのようなコンポーネントは、特定のユースケースに理想的な、用途に特化したプロップを備えています： Containerはメインレイアウトに、Stackは一次元レイアウトに、Paperは昇降面に適しています。

つまり`Box`は`div`のように、多目的な用途が前提なコンポーネントです。
用途が豊富ということは柔軟なスタイリングが可能です。
対して、`Container`、`Stack`、`Paper`はそれぞれ特定の用途に特化したプロパティを持っています。

✅ この柔軟さに起因しているのが、propertyの豊富さです。
マージン、パディング、タイポグラフィ、カラー、レイアウト、ポジショニングなどのスタイリングプロパティを直接指定することができます。

```tsx
<Box backgroundColor="rgba(0, 0, 0, 0.1)" margin="60px 0">
  <div style={{ backgroundColor: "rgba(255, 255, 0, 0.3)", width: "40%", height: "50px" }}>Box Content</div>
  <div style={{ backgroundColor: "rgba(255, 255, 0, 0.3)", width: "70%", height: "50px" }}>Box Content</div>
  <div style={{ backgroundColor: "rgba(255, 255, 0, 0.3)", width: "100%", height: "50px" }}>Box Content</div>
</Box>
```      


![alt text](/images/doc9/image9-2.png)



上記のような実装は`sx` プロパティを用いてもスタイリングが可能です。
ただし、直接のプロパティを利用するメリットとして、`mb`(margin-bottom)などの略語を使うことができるため、コードが簡潔になることが挙げられます。



柔軟性をもう一つ挙げつとするならば、`component`プロパティです。
✅ `component`プロパティを指定することで異なるHTML要素をレンダリングすることも可能です。

```tsx
<Box component="span"/>
<Box component="form">
```


# Container

次に`Container`について。[公式の説明](https://mui.com/material-ui/react-container/)を見てましょう。

> The container centers your content horizontally. It's the most basic layout element.

日訳
> Containerは、コンテンツを水平方向に中央配置します。最も基本的なレイアウト要素です。

この水平方向中央配置というのは、Containerで囲まれたコンテンツが画面の中央に配置されるということです。

Boxの説明の際に、
>Container、Stack、Paperはそれぞれ特定の用途に特化したプロパティを持っています。

と記載しましたが、Containerの場合の「特定の用途」とは中身を水平方向に中央配置することです。

![alt text](/images/doc9/image9-1.png)

✅ イメージとしては、`Container`の中身に対して、`<div style="margin: 0 auto;">`が適用されていると考えればわかりやすいかもしれません。

※ あくまでイメージです。Containerで囲うのと上記のdiv要素を直接書くのとでは、挙動が異なることがあります。


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

![alt text](/images/doc9/image9-3.png)


角が丸くなって、影っぽい装飾がついているのがわかります。
✅ ですのでPaperは`div`タグに、`border-radius`と`box-shadow`を適用したようなものと考えるとわかりやすいかもしれません。

※ あくまでイメージです。Paperで囲うのと上記のdiv要素を直接書くのとでは、挙動が異なることがあります。

# Stack

最後に[Paperコンポーネント](https://mui.com/material-ui/react-stack/)。

> Stack is a container component for arranging elements vertically or horizontally.

日訳
> Stackは、要素を縦または横に並べるためのコンテナ・コンポーネントである。

`Box`が多目的で柔軟なスタイリング(グリッドレイアウトや複雑な二次元レイアウト)が可能であるのに対して、
`Stack`は水平 or 垂直方向の一次元レイアウトに特化したコンポーネントです。


✅ そのため、下記のような単純なレイアウトの場合、`Stack`を使うことでコードが簡潔になります。


```jsx
<h1>Boxを使った例</h1>
<Box display="flex" flexDirection="column">
  <Box mb={2} bgcolor="lightblue" p={2}>Item 1</Box>
  <Box mb={2} bgcolor="lightgreen" p={2}>Item 2</Box>
  <Box bgcolor="lightcoral" p={2}>Item 3</Box>
</Box>

<h1>Stackを使った例</h1>
<Stack spacing={2}>
  <Box bgcolor="lightblue" p={2}>Item 1</Box>
  <Box bgcolor="lightgreen" p={2}>Item 2</Box>
  <Box bgcolor="lightcoral" p={2}>Item 3</Box>
</Stack>
```

![alt text](/images/doc9/image9-4.png)

他にも、`direction`プロパティで並び方向を指定することができたり、
✅ `divider`プロパティで要素間に区切り線を表示することもできます(👈 これ便利)。



# まとめ

以上、MUIのwrapper系コンポーネント(Box, Container, Paper, Stack)の違いについて整理してみました。
✅ 最後に、それぞれの特徴を表形式でまとめておきます。

| コンポーネント名 | 特徴と概要 |
|----------------|----------------|
| Box            | 多目的で柔軟なスタイリングが可能なコンポーネント。`<div>`のように使用できる。 |
| Container      | ページの主要なレイアウトを定義するために使用。中央配置や最大幅の制御に適している。 |
| Paper          | シャドウや丸みを帯びた角など、浮き出た表面を表現するためのコンポーネント。 |
| Stack          | 一次元レイアウトを容易にするためのコンポーネント。縦か横の線形スタックを作成する。 |


この記事を理解することで、
今まで**Boxで書いていたところが、ContainerやPaper、Stackを用いるとシンプルに書ける**ようになったり、
逆に、**Boxの柔軟性を活かして複雑なレイアウトを実装する**ことができるようになるかもしれません。

私個人だけではなくどなたかの備忘録として、もしこの記事がお役に立ちそうであれば幸いです。
また後で見返せるようにブックマークの方もよろしくお願いいたします。