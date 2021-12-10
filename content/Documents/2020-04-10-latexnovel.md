---
layout: post
title: ベスト文集制作記録⑧（本文の組み方⑤・作品タイトル篇）
date: 2020-04-10
tags: ["LaTeX", "DTP"]
---

# 本文の組み方⑤・作品タイトル篇
## デザイン
作品タイトルは以下のようなデザインとした。紙面を節約するために、タイトルを本文の右隣りに置くデザインとした。

![](/latex/assets/img/2020-04-09.jpg)

## LaTeXによる章の作り方（通常時）
普通、LaTeXで作品タイトルや章タイトルを出力するには、`\chapter`という命令を使う。`\chapter{作品タイトル}`のようにすればよい。

しかし今回は、`\chapter`に代替しうるマクロを独自に定義してこれを使った。なぜかというと、`\chapter`の定義を変更してデザインを変えるのが面倒であったからだ。LaTeXにもう少し詳しければ`\chapter`を再定義して用いたほうがよいであろう。

## LaTeXによる章の作り方（私家版）
上のような作品タイトルの出力をLaTeXで実現するために、次のようなマクロを組んだ。

```latex
%% title & author names
\newcommand{\booktitlename}{\relax}
\newcommand{\chaptertitle}[1]{\def\qbook@chaptertitle{#1}}
\newcommand{\chapterauthor}[1]{\def\qbook@chapterauthor{#1}}
\newcommand{\chapterauthorruby}[1]{\def\qbook@chapterauthorruby{#1}}
\newcommand{\chapterfirstappearance}[1]{\expandafter\qbook@fa#1//\@nil}
\def\qbook@fa#1/#2/#3/#4\@nil{%
    \def\qbook@fa@year{#1}%
    \def\qbook@fa@gou{#2}%
    \def\qbook@fa@booktitle{#3}%
}
\chaptertitle{\relax}
\chapterauthor{\relax}
\chapterauthorruby{\relax}
\chapterfirstappearance{\relax/\relax/\relax}

%% \makechapter
% \let\qbook@orig@tcy=\tcy
\def\qbook@5gyobox#1{\leavevmode\hbox to 5\baselineskip{\yoko#1\hfill}}
\def\qbook@makechapter{%
    \begingroup
    \ifnum\value{dangumi}=1
        \hspace*{-4.5zw}%
    \fi
    \qbook@5gyobox{\rule{70mm}{.4mm}}
    \qbook@5gyobox{\fontsize{12\bQ}{12H}\agencyfb The BEST SELECTION of\phantom{y}}\kern.05em
    \qbook@5gyobox{\fontsize{12\bQ}{12H}\agencyfb Kyushu University}\kern.05em
    \qbook@5gyobox{\fontsize{12\bQ}{12H}\agencyfb Literary Club}\kern.05em
    \qbook@5gyobox{\fontsize{12\bQ}{12H}\agencyfb Works}
    \qbook@5gyobox{\rule{70mm}{.4mm}}
    \begingroup
    \kanjiskip=-2H plus 0zw minus 0zw
    {\hskip2zw\fontsize{35\bQ}{35H}\selectfont\parbox{16zw}{\mgfamily\romanfamily{fbb-TLF}\selectfont\qbook@chaptertitle}}\par
    \endgroup
    \vspace{-.08\baselineskip}
    \hfill\leavevmode\hbox to 0pt{\yoko{\fontsize{8\bQ}{8H}\ttfamily Author:\phantom{y}}\hfill}\kern.2mm
    \leavevmode\hbox to 0pt{\yoko\rule{70mm}{.125mm}}\kern1mm
    \leavevmode\hbox to 0pt{\yoko\textgt{\fontsize{10\bQ}{10H}\selectfont\qbook@chapterauthor}\hfill}
    \leavevmode\hbox to 0pt{\yoko\textgt{%
        \fontsize{7\bQ}{7H}\selectfont
        \expandafter\ifx\qbook@chapterauthorruby\relax 　\else （\qbook@chapterauthorruby）\fi}\hfill
    }\kern2.5zw
    \leavevmode\hbox to 0pt{\yoko{\fontsize{8\bQ}{8H}\ttfamily Included in:\phantom{y}}\hfill}\kern.2mm
    \leavevmode\hbox to 0pt{\yoko\rule{70mm}{.125mm}}
    \leavevmode\hbox to 0pt{\vbox to 0pt{\vspace{-5\baselineskip}
            \begin{minipage}<y>[b]{5\baselineskip}
                \fontsize{7\bQ}{10H}\selectfont\par
                \textgt{\qbook@fa@year\kern.125zw 年度\qbook@fa@gou 号\\\qbook@fa@booktitle}
            \end{minipage}
        }%
    }%
    \hspace*{1zw}
    \vspace{1.67mm}
    \endgroup
}
\newcommand{\makechapter}{\qbook@makechapter}
```

本文での記述は以下のようにしてある。

```latex
\chaptertitle{走れメロス}
\chapterauthor{太宰治}
\chapterauthorruby{だざい・おさむ}
\chapterfirstappearance{????/？？/『？？？？？』}
\setcounter{dangumi}{2}\newhanmenII
\makechapter
\gyoaki[2]
\begin{honbun}
　メロスは激怒した。必ず、かの邪智暴虐《じゃち　ぼうぎゃく》の王を除かなければならぬと決意した。% 以下略
\end{honbun}
```

　

（[次回]({{< ref "2020-04-11-latexnovel.md" >}})に続く）
