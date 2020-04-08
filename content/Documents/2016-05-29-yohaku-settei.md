---
layout: post
title: LaTeX文書における余白の設定方法
date: 2016-05-29
tags: ["LaTeX"]
---

この記事では、LaTeX文書における余白の設定方法を解説する[^2]。

[^2]: 最近この記事を大幅に書き直した。昔載せていた記事は、[LaTeXにおける版面設計]({{< ref "2016-05-28-hanmen-sekkei.md" >}})に移動させた。必要に応じて参照されたい。

# 横書きの場合
## geometryパッケージ
横書きの場合、geometryパッケージを使うのが便利だ。字数と行数を指定する場合は、プリアンブルに次のように書き込む。

```LaTeX
% 45字×41行の設定にする
\usepackage[textwidth=45zw,lines=41]{geometry}
```

字数・行数を指定するのではなく、余白を直接指定する場合、次のように書けばよい。

```LaTeX
% 上下に2cm、左右に1cmの余白を取る
\usepackage[top=2cm, bottom=2cm, left=1cm, right=1cm]{geometry}
```

## hanmenパッケージ
拙作の[hanmenパッケージ]({{< ref "2017-02-18-hanmen.md" >}})を用いても、余白の設定を行なえる。プリアンブルに次のように書き込む。

```LaTeX
\usepackage[Q=10,H=15,W=38,L=40,ptj]{hanmen}
```

そうすると、

* 文字サイズ　10pt
* 行送り　　　15pt
* 行長　　　　38字
* 行数　　　　40行

に設定できる。hanmenパッケージは縦書きにも対応しているので便利である。

## 手動で設定
パッケージに頼らず、手動で余白を設定することもできる。例えば、字数・行数を指定する場合は次をプリアンブルに書く。

```LaTeX
%
% 45字×41行の設定にする
%

% 字数・行数マクロ定義
\def\mojiparline{45}
\def\linesparpage{41}

% 字数と行数
\textwidth = \mojiparline zw
\textheight = \linesparpage\baselineskip
\advance\textheight by -1\baselineskip
\advance\textheight by 1zw

% 版面を中央に（上下）
\topmargin=\paperheight
\advance\topmargin by -\textheight
\divide\topmargin by 2
\advance\topmargin by -1truein
\advance\topmargin by -\headheight
\advance\topmargin by -\headsep

% 版面を中央に（左右）
\oddsidemargin=\paperwidth
\advance\oddsidemargin by -\textwidth
\divide\oddsidemargin by 2
\advance\oddsidemargin by -1truein
\evensidemargin=\paperwidth
\advance\evensidemargin by -\textwidth
\divide\evensidemargin by 2
\advance\evensidemargin by -1truein

% \topskip調整
\topskip = 0.88zw
```

# 縦書きの場合
## hanmenパッケージ
geometryパッケージは縦書きに非対応である[^1]。しかし、例えば[hanmenパッケージ]({{< ref "2017-02-18-hanmen.md" >}})を用いると、縦書きでも余白の設定を行なえる。プリアンブルに次のように書き込む。

[^1]: もっとも、`lltjp-geometry.sty`というパッケージを使えば、無理矢理geometryパッケージを縦書きで使うことはできるらしいが、どの程度使える代物なのか、私は使ったことがないので分からない。詳しくは[geometry（TeX wiki内の一項目）](https://texwiki.texjp.org/?geometry)を参照のこと。

```LaTeX
\usepackage[Q=10,H=15,W=38,L=40,ptj,tate]{hanmen}
```

そうすると、

* 文字サイズ　10pt
* 行送り　　　15pt
* 行長　　　　38字
* 行数　　　　40行

に設定できる。詳しい使い方は[hanmenパッケージ]({{< ref "2017-02-18-hanmen.md" >}})の記事を参照せよ。

## 手動で設定
手動で設定する場合、次をプリアンブルに書く。

```LaTeX
%
% 46字×19行の設定にする
%

% 字数・行数マクロ定義
\def\mojiparline{46}
\def\linesparpage{19}

% 字数と行数
\textwidth = \mojiparline zw
\textheight = \linesparpage\baselineskip
\advance\textheight by -1\baselineskip
\advance\textheight by 1zw

% 版面を中央に（上下）
\topmargin=\paperheight
\advance\topmargin by -\textwidth
\divide\topmargin by 2
\advance\topmargin by -1truein
\advance\topmargin by -\headheight
\advance\topmargin by -\headsep

% 版面を中央に（左右）
\oddsidemargin=\paperwidth
\advance\oddsidemargin by -\textheight
\divide\oddsidemargin by 2
\advance\oddsidemargin by -1truein
\evensidemargin=\paperwidth
\advance\evensidemargin by -\textheight
\divide\evensidemargin by 2
\advance\evensidemargin by -1truein

% \topskip調整
\topskip = 0.5zw
```


手動で設定する方法は、一見煩雑であるが、やっていることはそんなに難しいことではない。詳しい解説は、[LaTeXにおける版面設計]({{< ref "2016-05-28-hanmen-sekkei.md" >}})を参照されたい。
