---
layout: post
title: LaTeX文書における余白の設定方法
date: 2016-05-29
tags: ["LaTeX"]
---

# はじめに
この文書は、LaTeXで縦書き組版をするとき、余白をどう設定すればよいかを解説するものである。

LaTeXの初心者／初級者にとって、余白の設定はかなり難しいものである（難しいならば余白はデフォルトのままで我慢してもよさそうなものだが、都合の悪いことにLaTeXのデフォルトの余白は広すぎて我慢できるものではない）。そこで、余白設定問題について取り上げ、わたしの理解が及んだ範囲で解説をおこなう。

まず、余白設定のさいよく使われるパッケージであるgeometryパッケージを使うことの弊害を説明する。続いて、geometryパッケージに頼らず余白設定をするための方法のひとつを紹介する。

# geometryパッケージ
余白の設定と聞くと、一般的なLaTeXユーザはgeometryパッケージの利用を思いつくかもしれない。geometryパッケージは、たしかに便利だ。ふつう、LaTeXで余白の設定をしようとすると、なるべく簡潔に書こうとしても以下のように記述する必要があり、見た目が煩雑になりがちである（この例はgeometryパッケージのドキュメントからの引用）。

```LaTeX
\usepackage{calc}
\setlength\textwidth{7in}
\setlength\textheight{10in}
\setlength\oddsidemargin{(\paperwidth-\textwidth)/2 - 1in}
\setlength\topmargin{(\paperheight-\textheight-\headheight-\headsep-\footskip)/2 - 1in}
```

ところがgeometryパッケージを使えば一発、

```LaTeX
\usepackage[top=2cm, bottom=2cm, left=1cm, right=1cm]{geometry}
```

というふうに記述するだけで、上下左右の余白を指定できるのである（この場合、上下2cm左右1cmずつの余白を設定したことになる）。

しかし、geometryパッケージにも欠点がいくつか存在する。

## 字数と行数を指定するのが難しい
たしかに、geometryパッケージは、Wordを使うときのように、たんに余白を指定したいだけのときならば、非常に使い勝手のよいパッケージである。しかし、geometryパッケージは、字数と行数を指定しようとすると、とたんに面倒になる。これが第一の欠点である。

（もしかすれば、geometryパッケージで字数と行数をかんたんに設定する方法はあるのかもしれない。しかし、わたしの調べた範囲では、そのような方法は発見できなかった。）

（追記：`\usepackage[lines=41,textwidth=45zw]{geometry}`のようにオプションを与えれば、字数と行数を指定できるらしいが、まだきちんと試してはいない。）

字数と行数を指定したいときというのは、案外に多い。とくに、なんらかの書籍のレイアウトを真似したいときなどは、字数と行数をきっちり設定できると嬉しい。

## 縦書きに対応していない
じつは、geometryパッケージは縦書きに非対応である。今回の目標は縦書き組版であるため、geometryパッケージの使用は望ましくないということになる。

（もっとも、`lltjp-geometry.sty`というパッケージを使えば、むりやりgeometryパッケージを縦書きで使うことはできるらしいが、どのていど使える代物なのか、わたしは使ったことがないのでわからない。）

## ふつうは余白主体でなく版面主体
geometryパッケージは、余白を設定するためのパッケージである。しかし、ふつうDTPでは、組版作業の最初に上下左右の余白を決めるということはしない。ではなにを最初に決めるのかというと、版面である。版面設計をこそ、最初にすべきなのである。

版面設計とは、本文領域のレイアウトを設計することである。具体的には、一行あたり何文字にするか、1ページあたり何行にするか、そして字の大きさはどれくらいかを指定する作業だ。

LaTeXにかぎらず、一般にDTPソフトで文章主体の冊子のレイアウトをつくるとき、ふつう版面から設計する。余白を先に決めてしまうと、「一行あたり何文字にするか、1ページあたり何行にするか、そして字の大きさはどれくらいか」が、余白に制約されてしまうからである。

というわけで、LaTeXにおける余白設定方法を学ぶこととは、じつは、版面設計方法を学ぶことだったのである。

# 版面設計の方法
それでは、版面を設計する方法を解説しよう。

この方法は、一見複雑そうに見えるが、LaTeXの余白決定の仕組みを理解するという意味でもお勧めできる方法である。


## 字数と行数
まずは字数と行数を設定してみよう。プリアンブルにつぎのように書いてみる。

```LaTeX
% 字数と行数
\textwidth = 46zw
\textheight = 19\baselineskip
\advance\textheight by -1\baselineskip
\advance\textheight by 1zw
```

これで、本文を46字×19行で組むことができる。

`\textwidth = 46zw`は、字数の設定をしている部分である（縦の長さが46字ぶんになるよう設定している）。

（なお、`=`を用いるやり方は、TeXの記法である。LaTeXでは`\setlength{\textwidth}{46zw}`などとする。LaTeXの記法も悪くはないのだが、TeXの記法のほうが簡潔に書けるので今回はこちらを採用する。）

`\textheight = 19\baselineskip`は、行数の設定をしている部分である（横の長さが19ベースラインぶんになるよう設定している。1ページあたり19行となる）。なお`\baselineskip`とは、組版用語で「行送り」のことであり、「1字ぶんの長さ＋行間の長さ」のことである。

以上2つの設定で基本的にはOKだ。では、残り2行はなんなのか。残り2行は、ちょっとした調整である。これを文章で説明しようとすると面倒なので省略する。どうしてこれらが必要なのかは、この2行をコメントアウトさせてみれば簡単にわかるから、興味がある人は試みられたい。

なお、`\textwidth`と`\textheight`は逆ではないか、と思った人がいるかもしれない。`\textheight`と言いながら、そこで設定したのはテキスト領域の高さ（height）でなく横幅（width）だったからである。

本当は、読んで字のごとく、`\textwidth`とはテキスト領域の「横」幅を、`\textheight`とはテキスト領域の「縦」の長さを、それぞれ指すのである。しかし、ここで注意したいことは、縦書きLaTeXにおいて`\textwidth`と`\textheight`は役割が逆転してしまうということである。LaTeXはもともと、横書きのためのソフトだ。これを、いわばむりやり縦書きに利用しているので、縦横が逆転してしまうのである。したがって、たとえば`\textwidth`は、縦書きしているときにかぎり「テキスト領域の縦幅」を示すことになる。


## 版面を中央に持っていく
それでは、つぎいこう。

じつは、字数と行数を指定しただけだと、テキスト領域が中央に来ず、端っこに寄った状態となってしまう。これはLaTeXの仕様上どうしようもない。

そういうわけで、今度は版面を中央に持っていく設定だ。まずは上下を中央に持っていこう。

```LaTeX
% 版面を中央に（上下）
\topmargin=\paperheight
\advance\topmargin by -\textwidth
\divide\topmargin by 2
\advance\topmargin by -1truein
\advance\topmargin by -\headheight
\advance\topmargin by -\headsep
```

まず`\topmargin=\paperheight`によって、上部の余白の長さ（`\topmargin`）を、紙の縦の長さ（`\paperheight`。これは縦書きでも意味は逆にならない）に合わせる。

このままでは余白がありすぎる（というか紙面全部が余白となってしまう）のであるが、もちろんここから調整を重ねていって、徐々に望む結果に近づけてゆくのである。

続く2行が、その調整である。`\topmargin`から`\textwidth`（テキストの縦の長さ）を引く（`\advance\topmargin by -\textwidth`）。そののち、`\topmargin`÷2（`\divide\topmargin by 2`）をする。こうすれば、上と下の余白が同じになり、版面が上下中央に来る。

残りの、

```LaTeX
\advance\topmargin by -1truein
\advance\topmargin by -\headheight
\advance\topmargin by -\headsep
```

はなんなのかというと、まあこれもちょっとした調整だと思っていただきたい。たとえば`\advance\topmargin by -1truein`というのを説明すると、実はLaTeX文書にはもともと1インチの余白が設けられている。なぜデフォルトで1インチも余白が空いているのか、意味がわからないのだが、とにかく空いているので、これを引く必要があるのだ。ほか2つもそういった感じのものだと思ってください。詳しくはLaTeX入門書でも見てください。


左右も同じ感じで設定できる。上下とほとんど同じなので、説明はほぼ不要だろう。

```LaTeX
% 版面を中央に（左右）
\oddsidemargin=\paperwidth
\advance\oddsidemargin by -\textheight
\divide\oddsidemargin by 2
\advance\oddsidemargin by -1truein
\evensidemargin=\paperwidth
\advance\evensidemargin by -\textheight
\divide\evensidemargin by 2
\advance\evensidemargin by -1truein
```

なお、`\oddsidemargin`は、奇数ページの左余白を、`\evensidemargin`は偶数ページの左余白を、それぞれ示す。


```LaTeX
% 版面を中央に（左右）
\oddsidemargin=\paperwidth
\advance\oddsidemargin by -\textheight
\divide\oddsidemargin by 2
\advance\oddsidemargin by -1truein
\evensidemargin=\paperwidth
\advance\evensidemargin by -\textheight
\divide\evensidemargin by 2
\advance\evensidemargin by -1truein
```

## \topskip
縦書きなら`\topskip = 0.5zw`と、横書きなら`\topskip=0.88zw`と書いておくこと。

## 最終形はこれだ
前の例だと字数と行数の設定を変えるのがやりにくいので、これを抜き出して、こんなふうに定義しておくと便利だ。

```LaTeX
% 字数・行数マクロ定義
\def\mojiparline{46}
\def\linesparpage{19}
```

こうして、下の最終形が完成する。これをプリアンブルに記述しておけば、余白の設定は完璧である。

```LaTeX
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


# 参考文献
* [geometry（TeX wiki内の一項目）](https://texwiki.texjp.org/?geometry)
* 村上智一『[よくわかるLaTeX小説](http://p-act.sakura.ne.jp/PARALLEL_ACT/LaTeX-Dojin/)』
