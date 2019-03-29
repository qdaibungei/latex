---
layout: post
title: LaTeXによる小説誌制作のヒント
date: 2017-10-12
tags: LaTeX
---

# 緒言
以下では、小説誌制作をLaTeXでおこなう場合のヒントを述べる。

# フォントの指定
LaTeXのデフォルトフォントはあまり小説向きではない。そこで以下のように記述しフォントを変更する。

まず日本語フォントであるが、これは游明朝がよい。游明朝を使うには、以下のように書く（uplatexを使うことを前提として。いまどきわざわざplatexを使う理由はあるまい）。

```LaTeX
% uplatexでA5サイズの本を作る場合のdocumentclassの書きかた
\documentclass[a5paper,papersize,uplatex]{jsbook}

% 日本語フォントを游明朝にする
\usepackage[deluxe,uplatex]{otf}
\AtBeginDvi{\special{pdf:mapfile otf-up-yu-win10.map}}
```

欧文フォントも変更しておこう。

```LaTeX
% 欧文フォントの変更
\usepackage[scale=0.95]{fbb}
\usepackage[scale=0.95,defaultsans]{opensans}
```

# jdashパッケージ
游明朝だと、ダッシュ`――`がつながらない。そこでjdashパッケージを導入し、これを使う。jdashパッケージについては[ここ]({{ site.baseurl }}{% post_url 2017/02/2017-02-17-jdash %})を参照のこと。

# hanmenパッケージの使用
まず以下の二つのファイルをダウンロードし、TeXファイルと同じフォルダへ入れる。

* [hanmen.sty](https://gist.github.com/qdaibungei/5f6986fa99fc9a7d86122a7a9417d64e)
* [nombre_for_fancyhdr.sty](http://p-act.sakura.ne.jp/PARALLEL_ACT/LaTeX-Dojin/src/nombre_for_fancyhdr.sty)

そしてプリアンブルに以下を記述する。

```LaTeX
% 上のフォント設定をした後に書く
\usepackage[Q=13,H=22,W=49,L=21,ko=3.875truemm,te=6.5truemm,footskip=2.47truemm,tate]{hanmen}

% ついでに、欧文のベースラインなどを適当に変更しておこう
\tbaselineshift=.3zw
\kanjiskip=0pt plus .25zw minus 0zw
\xkanjiskip=.25zw plus .083333zw minus 0zw
```

これで厳密な版面の設定が可能となる。厳密というのは、たとえば表と裏とで行が揃っているということである（厳密な設定をしないと、行がズレる。紙を透かして見ると行がズレているかどうか分かる）。

# `\tcy`マクロ
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
\clubpenalty=0
\widowpenalty=0
\jcharwidowpenalty=0
\displaywidowpenalty=0
\prebreakpenalty\jis"2147=10000  % 5000 ’
\postbreakpenalty\jis"2148=10000 % 5000 “
\prebreakpenalty\jis"2149=10000  % 5000 ”
\inhibitxspcode`〒=2
\prebreakpenalty\jis"2133=10000
\prebreakpenalty\jis"2134=10000
\prebreakpenalty\jis"2135=10000
\prebreakpenalty\jis"2136=10000
\prebreakpenalty`ー=0
\prebreakpenalty`ぁ=0
\prebreakpenalty`ぃ=0
\prebreakpenalty`ぅ=0
\prebreakpenalty`ぇ=0
\prebreakpenalty`ぉ=0
\prebreakpenalty`っ=0
\prebreakpenalty`ゃ=0
\prebreakpenalty`ゅ=0
\prebreakpenalty`ょ=0
\prebreakpenalty\jis"246E=0     %ゎ
\prebreakpenalty`ァ=0
\prebreakpenalty`ィ=0
\prebreakpenalty`ゥ=0
\prebreakpenalty`ェ=0
\prebreakpenalty`ォ=0
\prebreakpenalty`ッ=0
\prebreakpenalty`ャ=0
\prebreakpenalty`ュ=0
\prebreakpenalty`ョ=0
\prebreakpenalty\jis"256E=0     %ヮ
\prebreakpenalty\jis"2575=0     %ヵ
\prebreakpenalty\jis"2576=0     %ヶ
\prebreakpenalty\jis"2139=0     %々
```

# フッター
『どこにもりんごがない』を組版したときのフッターの設定はだいたいこんな感じである。

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
