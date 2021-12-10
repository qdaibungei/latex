---
layout: post
title: 半角漢数字を出力するコマンド
date: 2021-04-13
tags: ["LaTeX"]
---

半角漢数字（正式名称ではないと思うが）は、少し昔の本の目次などで使われている。いまどきの本ではあまり見かけないが、かっこいいので私は好きである。

半角漢数字は、LaTeXだとotfパッケージの`\CID`を用いれば簡単に出力できる。しかしCIDをいちいち調べないといけないのが面倒だ。そこで`\hankakukansuji`というコマンドを作ってみた。

次を`hoge.tex`などのファイル名で保存し、upLaTeXで実行する。

```LaTeX
\documentclass[jafontsize=14Q, baselineskip=24.5H, line_length=45zw, number_of_lines=21, tate]{jlreq}
\usepackage[uplatex]{otf}% \CIDを用いるため必須
\usepackage[noalphabet]{pxchfon}
\setminchofont{HiraMinProN-W3.otf}% 半角漢数字が存在するフォントを選ぶ
%% 半角漢数字出力コマンド
\makeatletter
\newcounter{qbcl@c@hk}
\newcommand{\hankakukansuji}[1]{%
    \@tfor\@tempa:=#1\do{%
        \setcounter{qbcl@c@hk}{0}%
        \@whilenum\value{qbcl@c@hk}<10\do{%
            \ifnum\@tempa=\theqbcl@c@hk
                \addtocounter{qbcl@c@hk}{10185}\CID{\theqbcl@c@hk}\addtocounter{qbcl@c@hk}{-10185}%
            \fi
            \addtocounter{qbcl@c@hk}{1}%
        }%
    }%
}
\makeatother
\begin{document}
\hankakukansuji{764980}
\end{document}
```

すると次の結果を得る。

![](/latex/assets/img/2021-04-13.png)
