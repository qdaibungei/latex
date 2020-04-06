---
layout: post
title: LaTeXによる小説組版法・中級編――ベスト文集制作記録②
date: 2019-10-13
tags: ["LaTeX", "guide"]
---

前回に引き続き、小説組版の方法を解説していく。今回は、**扉ページ**の作り方である。

# 扉
## 扉とは
「**扉**」とは、表紙をめくって最初にあるページのことである。書籍の題名などを書くページである。今回のベスト文集では、次のような扉を作った。

![](/latex/assets/img/2019-10-13-tobira.png)

我ながらシンプルでかっこいい扉である。「九大文学」という文字のフォントは、背表紙に使ったのと同じフォント（[廻想体](https://moji-waku.com/kaiso/)）を使っている。これにより統一感が出て、本全体のデザインのまとまりがよくなったはずである。

## ロゴ
扉の左下には、九大文藝部のロゴを配置してある。

このロゴは、かなり昔にデザインされ、部誌で使用されていたものである。一時期このロゴはまったく使用されなくなっていたが、私が再び使い始めた。

今回のベスト文集におけるロゴは、以前のロゴとは少しだけデザインが異なっている。欧文フォントは、以前のロゴとまったく同じもの（[Classico](https://fonts.adobe.com/fonts/classico-urw)）を使用している。だが和文フォントは、本全体の雰囲気に合わせて「廻想体」を選択した（以前のロゴではヒラギノ明朝が使われていた）。

文藝部の遺伝子は、少しずつアップデートされながらも、着実に継承されていく。

![](/latex/assets/img/2019-10-13-logo-compare.png)

## LaTeXによる扉の作り方
LaTeXで扉を作るには通常、`\title{九大文学}`のように本のタイトルを指定し、`\maketitle`という命令を書き込むことによって扉を生成する。しかしこの方法では、扉ページのレイアウトを変更するのが非常に難しい。具体的には、クラスファイルを編集して`\maketitle`という命令を定義し直せばよいと思われるが、この方法は余程TeXに習熟している人でない限り実行困難であろう。今回作ったベスト文集のように、扉だけ横書きで、本文は縦書き、といったような複雑なレイアウトにする場合は、特に面倒だと思われる。

そこで、扉を作るにあたっては、実現したいレイアウトを直接LaTeXの文法に従って書くほうが無難である。具体的には、私は以下のように記述して扉を作った（ただし、以下の記述のなかには独自に定義したマクロも多く含まれているので、これをそのままコピペしてもエラーが出るだけである。注意されたい）。

```LaTeX
\newhanmenII
\newhanmen{tate,te=-2mm}
\hanmenchangedir
\thispagestyle{empty}
\begingroup

\vspace*{70mm}
{\fontsize{30\bQ}{30H}\gtfamily\ebseries\romanfamily{agencyfb}\romanseries{bx}\selectfont\booktitlename}

\leftskip-1zw
\vspace{\fill}
\parbox{9zw}{%
    \begin{center}
        \fontsize{15\bQ}{16H}\classico
        \hspace*{0.18em}Kyushu \hspace{0.05em}Univ.\\
        \vspace{-.2em}%
        \mbox{}\hspace{-1.3zw}{\gtfamily\ebseries\fontsizeX 九州大学文藝部}\\
        \vspace{-.1em}%
        Literary Club
    \end{center}
}

\endgroup
```

何をやっているかというと、基本的には、「文字の大きさやフォントを切り替えつつ、`\vspace`や`\hspace`で文字の位置を調整して配置している」ということになる。すなわちWordなどと同じ要領でデザインすればよい。

文字の大きさは、

```LaTeX
\fontsize{20Q}{25H}\selectfont
```

のように書けば指定できる。また、フォントの指定をするには

```LaTeX
\kanjifamily{hmc}\romanfamily{qag}\selectfont
```

のように書けばよい。フォント指定のマクロには`\fontfamily`という命令もあるが、ここでは敢えて`\fontfamily`ではなく`\kanjifamily`と`\romanfamily`を使っている。こちらの命令を使えば、和文と欧文でまったく違うフォントを指定することが可能となるので、便利である。

　

（[次回]({{< ref "2019-10-14-latexnovel.md" >}})に続く）
