---
layout: post
title: LaTeXによる小説誌制作のヒント
aliases:
    - /2017/10/12/book-making-hints.html
date: 2017-10-12
tags: ["LaTeX", "guide", "tips"]
---

# 緒言
以下では、小説誌制作をLaTeXでおこなう場合のヒントを述べる。

# フォントの指定
LaTeXのデフォルトフォントはあまり小説向きではない。そこで以下のように記述しフォントを変更する。

まず日本語フォントであるが、特にこだわりがないならば、とりあえず游明朝にしておくとよい。游明朝を使うには、以下のように書く（以下はuplatexを使うことを前提とした書き方である。いまどきわざわざplatexを使う理由はあるまい）。

```LaTeX
% uplatexでA5サイズの本を作る場合のdocumentclassの書きかた
\documentclass[a5paper,papersize,uplatex]{jsbook}

% 日本語フォントを游明朝にする
\usepackage[deluxe,uplatex]{otf}
\AtBeginDvi{\special{pdf:mapfile otf-up-yu-win10.map}}% Windows 10以外の場合は適宜変更せよ
```

欧文フォントも変更しておこう。

```LaTeX
% 欧文フォントの変更
\usepackage[scale=1.05]{cochineal}% この欧文フォントはわりと游明朝に合う
\usepackage[scale=0.95,defaultsans]{opensans}
```

# jdashパッケージ
游明朝だと、ダッシュ`――`がつながらない。そこでjdashパッケージを導入し、これを使う。jdashパッケージについては[ここ]({{< ref "2017-02-17-jdash.md" >}})を参照のこと。

# hanmenパッケージの使用
まず[hanmenパッケージ](https://gist.github.com/qdaibungei/5f6986fa99fc9a7d86122a7a9417d64e)をダウンロードし、TeXファイルと同じフォルダへ入れる。

そしてプリアンブルに以下を記述する。

```LaTeX
% 上のフォント設定をした後に書く
\usepackage[Q=13,H=22,W=49,L=21,ko=3.875truemm,te=6.5truemm,footskip=2.47truemm,tate]{hanmen}

% ついでに、欧文のベースラインなどを適当に変更しておこう
\tbaselineshift=.3zw
\kanjiskip=0zw plus .25zw minus .03125zw
\xkanjiskip=.25zw plus .25zw minus .0625zw
```

これで厳密な版面の設定が可能となる。厳密というのは、たとえば表と裏とで行が揃っているということである（厳密な設定をしないと、行がズレる。紙を透かして見ると行がズレているかどうか分かる）。

hanmenパッケージの使い方の詳細は[この記事]({{< ref "2017-02-18-hanmen.md" >}})を参照のこと。

# \tcyマクロ
LaTeXで縦書きをするさい、縦中横には`\rensuji`を使うよう言われる。しかし`\rensuji`だと前後のアキがおかしくなる（たとえば、行頭で`\rensuji`を使うとインデント量が狂う）。そこで`\tcy`命令を新たに作り、これを使うのがよい。

```LaTeX
\usepackage{plext}
\makeatletter
\chardef\nvlsty@zenkakuSpace=\jis"2121\relax
\newcommand{\tcy}[1]{%
    \nvlsty@zenkakuSpace\kern-1zw\relax
    \leavevmode\hbox to 1zw{%
        \centering\rensuji*{#1}\relax
    }\relax
    \kern-1zw\nvlsty@zenkakuSpace
}
\makeatother
```

以上をプリアンブルなどに書いておく。使いかたは至ってかんたんで、`\tcy{12}時`、のようにすればよい。

# 禁則処理の設定
縦書きの場合、禁則処理はほどほどに抑制しておくと商業出版物の組版に似せることができる。プリアンブルなどに以下を記述する。

```LaTeX
\makeatletter
\def\hoge@set@prebreakpenalty#1#2{%
    \@tfor\@tempa:=#1\do{\expandafter\prebreakpenalty\expandafter`\@tempa =#2}}
\clubpenalty=0
\widowpenalty=0
\jcharwidowpenalty=0
\displaywidowpenalty=0
\prebreakpenalty\jis"2147=10000 % 5000 ’
\postbreakpenalty\jis"2148=10000 % 5000 “
\prebreakpenalty\jis"2149=10000 % 5000 ”
\inhibitxspcode`〒=2
\hoge@set@prebreakpenalty{ヽヾゝゞ々〻}{10000}
\hoge@set@prebreakpenalty{ーぁぃぅぇぉっゃゅょゎァィゥェォッャュョヮヵヶゕゖㇰㇱㇲㇳㇴㇵㇶㇷㇸㇹㇺㇻㇼㇽㇾㇿ…}{0}
\makeatother
```

# フッター
以前出した本のフッターの設定はだいたいこんな感じである。

```LaTeX
\usepackage{fancyhdr}
\usepackage[scale=0.9]{tgadventor}
\makeatletter
\def\qbook@chaptertitle{タイトル}
\def\booktitlename{ブックタイトル}
\def\qbook@nombre{%
    {\fontsize{10\bQ}{11trueH}\romanfamily{qag}\bfseries\thepage}%
}
\def\qbook@lhasira{%
    {\fontsize{10\bQ}{11trueH}\gtfamily\sffamily
    \kern1em\rule[-.25truemm]{0.15truemm}{1zw}\kern1em
    \qbook@chaptertitle}%
}
\def\qbook@rhasira{%
    {\fontsize{10\bQ}{11trueH}\gtfamily\sffamily
    \booktitlename
    \kern1em\rule[-.25truemm]{0.15truemm}{1zw}\kern1em}%
}
\fancyfoot{}
\fancyfoot[LO]{\vspace*{65\bQ}\qbook@nombre\qbook@lhasira}
\fancyfoot[RE]{\vspace*{65\bQ}\qbook@rhasira\qbook@nombre}
\makeatother
```

# 結言
以上、ヒントを列挙した。参考にしてください。
