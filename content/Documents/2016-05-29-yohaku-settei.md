---
layout: post
title: LaTeX文書における余白の設定方法
date: 2016-05-29
tags: ["LaTeX"]
---

この記事では、LaTeX文書における余白の設定方法を解説する。

- [横書きの場合](#横書きの場合)
  - [geometry.sty](#geometrysty)
  - [jlreq.cls](#jlreqcls)
  - [hanmen.sty](#hanmensty)
  - [手動で設定](#手動で設定)
- [縦書きの場合](#縦書きの場合)
  - [jlreq.cls](#jlreqcls-1)
  - [hanmen.sty](#hanmensty-1)
  - [手動で設定](#手動で設定-1)


# 横書きの場合
## geometry.sty
横書きの場合、geometry.styを使うのが便利だ。字数と行数を指定する場合は、プリアンブルに次のように書き込む。

```LaTeX
% 45字×41行の設定にする
\usepackage[textwidth=45zw,lines=41]{geometry}
```

字数・行数を指定するのではなく、余白を直接指定する場合、次のように書けばよい。

```LaTeX
% 上下に2cm、左右に1cmの余白を取る
\usepackage[top=2cm, bottom=2cm, left=1cm, right=1cm]{geometry}
```

ターミナルで`$ texdoc geometry`を実行すれば、geometry.styのマニュアルを読むことができる。

## jlreq.cls
jlreq.clsを用いている場合、クラスオプションを用いて余白設定を行なうことができる。プリアンブルに次のように書き込む。

```latex
\documentclass[jafontsize=14Q, baselineskip=24.5H, line_length=45zw, number_of_lines=41]{jlreq}
```

すると、

- 文字サイズ　14級
- 行送り　　　24.5歯
- 行長　　　　45字
- 行数　　　　41行

に設定することができる。字数・行数を指定するのではなく、余白の広さを直接指定する場合には、次のようにする。

```latex
% 上下に2cm、左右に1cmの余白を取る
% head_space: 上部余白
% foot_space: 下部余白
% gutter: ノド余白（左とじの場合は左側の余白、右とじの場合は右側の余白）
% fore-edge: 小口余白（ノドとは逆側の余白）
\documentclass[head_space=2cm, foot_space=2cm, gutter=1cm, fore-edge=1cm]{jlreq}
```

ターミナルで`$ texdoc jlreq`を実行すれば、jlreq.clsのマニュアルを読むことができる。

## hanmen.sty
拙作の[hanmen.sty]({{< ref "2017-02-18-hanmen.md" >}})を用いても、余白の設定を行なえる。プリアンブルに次のように書き込む。

```LaTeX
\usepackage[Q=10,H=15,W=38,L=40,ptj]{hanmen}
```

そうすると、

- 文字サイズ　10pt
- 行送り　　　15pt
- 行長　　　　38字
- 行数　　　　40行

に設定できる。hanmen.styは縦書きにも対応しているので便利である。

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
## jlreq.cls
geometry.styは縦書きに非対応である[^1]。しかしjlreq.clsを用いると、縦書きでも余白の設定を行なえる。

[^1]: もっとも、`lltjp-geometry.sty`というパッケージを使えば、無理矢理geometryパッケージを縦書きで使うことはできるらしいが、どの程度使える代物なのか、私は使ったことがないので分からない。詳しくは[geometry（TeX wiki内の一項目）](https://texwiki.texjp.org/?geometry)を参照のこと。

jlreq.clsで縦書きにする場合には、クラスオプションに`tate`を与えるだけでよい。余白の設定方法自体は、横書きのものと同様。


## hanmen.sty
[hanmen.sty]({{< ref "2017-02-18-hanmen.md" >}})を用いても、縦書きで余白の設定を行なえる。プリアンブルに次のように書き込む。

```LaTeX
\usepackage[Q=10,H=15,W=38,L=40,ptj,tate]{hanmen}
```

そうすると、

- 文字サイズ　10pt
- 行送り　　　15pt
- 行長　　　　38字
- 行数　　　　40行

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
