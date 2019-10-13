---
layout: post
title: LaTeXによる小説組版法・中級編――ベスト文集制作記録③
date: 2019-10-14
tags: ["LaTeX", "guide"]
draft: true
---

前回に引き続き、小説組版の方法を解説していく。今回は、「**目次**」の作り方である。

# 目次
## 目次のデザイン
今回のベスト文集では、目次は下図のようなデザインにした。このデザインは、[某小説誌](https://www.gentosha.co.jp/s/gento/)を参考に作った。

ノンブル[^1]のフォントには、[源暎ノンブル](https://okoneya.jp/font/download.html#dl-genb)を使った。たったこれだけの工夫でかなりデザインが引き締まる気がする。

[^1]: ノンブルとは印刷用語で、ページ番号のこと。

<!-- ![](/latex/assets/img/2019-10-14a.png) -->

![](/latex/assets/img/2019-10-14b.png)

<!-- ![](/latex/assets/img/2019-10-14c.png) -->

<!-- ![](/latex/assets/img/2019-10-14d.png) -->

## 目次のデザイン（見開き版）
見開きで見ると、目次はこんな感じ。目次ページの始まりを示す中扉には、「九大文学＊目次」と書いた。この「九大文学」のフォントは、扉ページと同じく[廻想体](https://moji-waku.com/kaiso/)にしてある。逐一フォントを揃えていくことで本全体に統一感を出すためである。

![](/latex/assets/img/2019-10-14-2in1a.png)

![](/latex/assets/img/2019-10-14-2in1b.png)

![](/latex/assets/img/2019-10-14-2in1c.png)

「九大文学」という文字だけのページ（最後の画像の左側）は、本文の始まりを明示するための中扉。

## LaTeXによる目次の作り方
通常、LaTeXで目次を生成するには、`\tableofcontents`という命令を使うことになっている。

しかし、`\tableofcontents`で上のような目次を作るのは結構難しい。なぜならば、第一に、デフォルトの`\tableofcontents`では、著者名と作品名をどちらも出力するということができないからだ（そもそもLaTeXのデフォルトの設定では、複数の著者名を扱うような仕組みがない）。

第二に、`\tableofcontents`で出力される目次のレイアウトを変更するのが難しい。`\tableofcontents`を再定義してやればいいのだが、`\tableofcontents`の元の定義はかなり複雑で、変更が面倒である。

さてそこで私は、`\tableofcontents`に代替しうるマクロを別途定義し、それを使うことにした。

メモ：`\mokuji`マクロの必要最低限な部分を抽出して、定義の意図と使い方とを解説。

```LaTeX
%% title & author names
\newcommand{\chaptertitle}[1]{\def\qbook@chaptertitle{#1}}
\newcommand{\chapterauthor}[1]{\def\qbook@chapterauthor{#1}}
\chaptertitle{\relax}
\chapterauthor{\relax}

%% \mokuji
\newcommand{\mokuji}[4]{%
    \expandafter\ifx#4\relax
        \leavevmode\hbox to 14zw{}%
    \else
        \leavevmode\hbox to 14zw{\rule[-1zw]{.4pt}{2zw}\kern.5zw\fontsize{12\bQ}{\baselineskip}\selectfont #4\hss}%
    \fi
    \expandafter\ifx#2\relax
    \else
        \leavevmode\hbox to 6zw{%
            \hss\textgt{\fontsize{12\bQ}{\baselineskip}\selectfont #2}%
        }%
    \fi
    \kern.5zw\rule[-1zw]{.4pt}{2zw}\kern.5zw #1
    \def\qbook@mkj@page{\pageref{#3}}%
    \expandafter\ifx\csname r@#3\endcsname\relax
        \def\qbook@mkj@page{??}%
    \fi
    \StrLeft{\qbook@mkj@page}{1}[\qbook@mkj@firstchar]%
    \if ?\qbook@mkj@firstchar
        \hfill\rule[-1zw]{.4pt}{2zw}\kern.5zw\hbox to 1.5zw{\tcy{?}}%
    \else
        \if i\qbook@mkj@firstchar
            \tcy{\small\qbook@mkj@page}\hspace*{-.17zw}%
        \else
            \hfill\rule[-1zw]{.4pt}{2zw}\kern.5zw\hbox to 1.5zw{%
                \fontsize{12\bQ}{12H}\selectfont
                \tcy{\phantom{\geneinombre 8}\geneinombre\qbook@mkj@page\phantom{\geneinombre 9}}%
            }%
        \fi
    \fi
}
```
