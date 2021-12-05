---
layout: post
title: ベスト文集制作記録⑤（本文の組み方②・版面設計篇）
date: 2020-04-07
tags: ["LaTeX", "guide"]
---

# 本文の組み方②・版面設計篇
## 各種パラメータ
### 紙サイズ
今回はA5サイズを使用した。A5サイズは、文庫サイズなどに比べて安く作れるので、同人誌界隈ではよく使用される。文藝部にはそこまでお金がないので、今回もA5サイズ以外に選択の余地はなかった。

### 文字サイズ
『九大文学』では、文字サイズは13Qとした[^Q]。13Qくらいが、大きすぎず小さすぎず、ちょうどよい塩梅ではあるまいか。

**初心者は文字サイズを小さくして失敗してしまいがちなので、注意**すること。印刷所に入稿する前に、家庭用プリンターで本文を印刷してみて、どれくらいの文字サイズになっているかを確認するのが最も安全である。

[^Q]: 「Q」とは「級」という意味で、文字サイズの単位の一つ。13Qは約9.2ptである。

### 行送り
行送りは、19.5Hとした。これは、文字サイズの1.5倍である（それゆえ行間は文字サイズの0.5倍である）。これは、行間の設定としてはかなり狭めである。一般には、行送りは文字サイズの1.5〜2倍程度がよいとされている。

なお、フォントによって、行送りを広く取ったほうがよいもの、狭くても読みやすいものがある。したがって行送りはフォントを選んだ後に決めるべきであろう。今回は游明朝だから行送りを狭めにしたが、例えばヒラギノ明朝であればもう少し広めに取ったほうがよい。

### 行長・行数
行長は26字、行数は24行とした。二段組とし、段間は2.5字分とした。

### ノド
ノド側の余白を狭くしすぎると、文字が食われてしまって読みづらい。A5で縦書きの場合、ノドの広さは約20mmは欲しい[^nodo]。今回は、ノドは18mm取った。

[^nodo]: 横書きの場合は、縦書きよりもさらにノドを広く取ったほうが読みやすい。A5で横書きの場合、20mmでは狭い。

## LaTeXでの実現方法
まとめると、版面は次のような設定にしたことになる。

- 紙はA5サイズ
- 文字サイズ13Q
- 行送り19.5H
- 26字×24行×2段組
- 段間2.5字分
- ノドの広さは18mm

これは、「頁数節約のため一頁あたりの文字数を多めにしつつ、最大限、可読性も確保する」というコンセプトのもとで、幾度も調整を重ねて設計したものである。

具体的には次のように記述してLaTeXで実現している（細かい解説は面倒なので割愛）。

```latex
%% ドキュメントクラス読み込み時にA5サイズ指定
\documentclass[a5paper,autodetect-engine]{jsbook}
\usepackage[tate]{hanmen}

%% papersize
\AtBeginDvi{\special{papersize=\the\paperwidth,\the\paperheight}}% 通常
% 本文に上下左右3mmの塗り足しを作る細工
% \newdimen\stockwidth
% \newdimen\stockheight
% \setlength{\stockwidth}{\paperwidth}
% \setlength{\stockheight}{\paperheight}
% \advance\stockwidth by 6mm
% \advance\stockheight by 6mm
% \advance\voffset by 3mm
% \advance\hoffset by 3mm
% \AtBeginDvi{\special{papersize=\the\stockwidth,\the\stockheight}}

%% layout
\newcounter{dangumi}
\setcounter{dangumi}{0}
\newcommand{\newhanmenI}{%
    % H = ((19.5/4)*(24-1)/(22-1))*4
    % TBmargin: (210-((13/4)*50))/2=23.75
    % ko=(newhanmenIIに追随), te=23.75-16.4325=7.3175
    % (3.25*6)=19.5
    % footskip=(newhanmenIIに追随)
    \newhanmen{Q=13,H=21.357142857,W=50,L=22,ko=0mm,te=-7.3175mm,nodo=18mm,chi=\relax,footskip=3.25mm,tate}%
    % \pagegridon{g=50,column=1}
    \fancyfoot[RE]{\vspace*{0pt}\hspace*{1zw}\qbook@nombre}%
    \fancyfoot[LO]{\vspace*{0pt}\qbook@nombre\hspace*{1zw}\qbook@lhasira}%
}
\newcommand{\newhanmenII}{%
    % RLmargin: (148-(19.5/4)*(24-1)-(13/4))/2=16.3125, TBmargin: (210-((13/4)*54.5))/2=16.4375
    % ko=18−16.3125=1.6875, te=1
    \newhanmen{Q=13,H=19.5,W=54.5,L=24,ko=0mm,te=0mm,nodo=18mm,chi=\relax,footskip=3.25mm,tate}%
    % \pagegridon{g=26,column=2,columnskip=28.5}
    \fancyfoot[RE]{\vspace*{0pt}\hspace*{1zw}\qbook@nombre}%
    \fancyfoot[LO]{\vspace*{0pt}\qbook@nombre\hspace*{1zw}\qbook@lhasira}%
}
\fancyhf{}
\def\qbook@nombre{%
    {\fontsizeXI\geneinombre\thepage}%
}
\def\qbook@lhasira{%
    {\fontsizeIX\qbook@chaptertitle}%
}
\def\qbook@rhasira{%
    {\fontsizeX\gtfamily\selectfont
    \booktitlename}%
}
\newenvironment{honbun}{% 環境の名称はmymulticolsくらいでよいかも
    \ifnum\value{dangumi}=1
    \fi
    \ifnum\value{dangumi}>1
        \multicolsep=0pt
        \begin{multicols*}{\value{dangumi}}%
    \fi}{%
    \ifnum\value{dangumi}>1
        \end{multicols*}%
    \fi
}
```

以上の設定により、『九大文学』の本文組版はこのような仕上がりとなった。

![](/latex/assets/img/2020-04-06.jpg)

　

（[次回]({{< ref "2020-04-08-latexnovel.md" >}})に続く）
