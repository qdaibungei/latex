---
layout: post
title: LaTeXによる小説組版法・中級編――ベスト文集制作記録③
date: 2019-10-14
tags: ["LaTeX", "guide"]
draft: false
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

## LaTeXによる目次の作り方（通常時）
通常、LaTeXで目次を生成するには、`\tableofcontents`という命令を使うことになっている。

しかし、`\tableofcontents`で上のような目次を作るのは結構難しい。なぜならば、第一に、デフォルトの`\tableofcontents`では、著者名と作品名をどちらも出力するということができないからだ（そもそもLaTeXのデフォルトの設定では、複数の著者名を扱うような仕組みがない）。

第二に、`\tableofcontents`で出力される目次のレイアウトを変更するのが難しい。`\tableofcontents`を再定義してやればいいのだが、`\tableofcontents`の元の定義はかなり複雑で、変更が面倒である。

## LaTeXによる目次の作り方（私家版）
さてそこで私は、`\tableofcontents`に代替しうるマクロを別途定義して使っている。そのマクロとは、以下に示すような`\Mokuji`マクロである。

```LaTeX
%% \Mokuji
\newcommand{\Mokuji}[3]{%
    #1\hskip1zw% 作品名
    \textgt{#2}\hskip1zw% 著者名
    \rensuji{\pageref{#3}}% ページ数
}
```

このようなマクロを作っておいたうえで、本文内に次のように記述する。

```LaTeX
\Mokuji{走れメロス}{太宰治}{dazai}% 目次生成。引数は、\Mokuji{<作品名>}{<著者名>}{<ページ参照用ラベル>}とする。

%

\label{dazai}% 小説開始時点にラベルを張っておく
% 以下、小説本文
```

すると、次のように出力される。

![](/latex/assets/img/2019-10-14e.png)

基本的には、このように`\Mokuji`マクロを何個も使って目次を作ればよい。目次デザインを変えるためには、`\Mokuji`マクロの定義を適宜変更すればよい。

ベスト文集では、`\mokuji`を次のように定義して使った。

```LaTeX
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

目次デザインがシンプルな割には定義が複雑である（これは、不要な記述も多々入っているためである。本当はもう少し簡潔に記述できる気がする）。

（次回に続く）
