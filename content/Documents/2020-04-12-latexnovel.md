---
layout: post
title: ベスト文集制作記録⑩（奥付）
date: 2020-04-12
tags: ["LaTeX", "DTP"]
---

# 奥付
奥付は次のようなデザインにした[^colophon]。奥付でもやはり書籍タイトルに[廻想体](https://moji-waku.com/kaiso/)を使い、統一感を出した。

[^colophon]: この奥付のデザインは、2015年度学祭号『伊都計劃』を制作したときに作ったもので、それ以来気に入ってずっと使い続けている。アイディアの元となったのは橋本淳一郎『[神の仕掛けた玩具](https://www.hanmoto.com/bd/isbn/9784061542914)』（講談社サイエンティフィク、2006年）の奥付である。

![](/latex/assets/img/2020-04-12.png)

LaTeXでは次のように実現してある。

```latex
%% macro for colophon
\makeatletter
\def\@thisdate#1/#2/#3/#4\@nil{%
    \newcommand{\thisyear}{#1}%
    \newcommand{\thismonth}{#2}%
    \newcommand{\thisday}{#3}%
}
\def\thisdate#1{\expandafter\@thisdate#1//\@nil}
\newcommand{\bookmaker}[2]{%
    ■\leavevmode\hbox to 2.7zw{%
        {\gtfamily\bfseries\fontsizeX #1}%
    }%
    ―\kern-.7zw\――#2%
}
\newcommand{\fontcredit}[2]{%
    \leavevmode\hbox to 6zw{%
        \hfill\emph{#1}%
    }%
    {\kern1.5zw\okfamily #2}%
}
\makeatother

%% colophon
\newhanmen{Q=11.5,H=19,W=58,L=21,ko=2mm,koguchi=\relax,nodo=\relax,chi=16.4375mm,tate}
% \pagegridon{g=56}
\thispagestyle{empty}
\columnsep=4zw
\begin{multicols*}{2}
\parbox<y>[c]{\textheight}{%
    \fontcredit{組版処理}{\LaTeXe}\\

    \fontcredit{使用書体}{}{\raise-.125zw\hbox{\gtfamily\ebseries\fontsize{14\bQ}{19H}\selectfont \kern-.1zw 廻\kern-.25zw 想\kern-.25zw 体}＋{\agencyfb Agency FB}\fontsizeVIII 〈表紙〉}\\
    \fontcredit{}{\raise-.125zw\hbox{\gtfamily\ebseries\fontsize{14\bQ}{19H}\selectfont \kern-.1zw 廻\kern-.25zw 想\kern-.25zw 体}＋{\classico Classico}\fontsizeVIII 〈ロゴ〉}\\
    \fontcredit{}{{\mcfamily 游明朝体R＋Cochineal}\fontsizeVIII 〈本文・ルビ・柱〉}\\
    \fontcredit{}{游明朝体五号かなR＋fbb\fontsizeVIII 〈本文〉}\\
    \fontcredit{}{{\mgfamily 筑紫Aオールド明朝R}＋fbb\fontsizeVIII 〈各作品タイトル〉}\\
    \fontcredit{}{{\gtfamily 游ゴシック体M}＋\textgt{Open Sans Regular}\fontsizeVIII 〈見出し・作品情報〉}\\
    \fontcredit{}{\emph{筑紫ゴシックD}＋\emph{Open Sans Semibold}\fontsizeVIII 〈強調・小見出し〉}\\
    \fontcredit{}{\texttt{Nimbus15 Mono Narrow}\fontsizeVIII 〈作品情報〉}\\
    \fontcredit{}{源暎ノンブル\fontsizeVIII 〈ノンブル〉}
}
\columnbreak

\thisdate{2019/10/04}
\leftskip=-6zw
\okfamily
\vspace*{.5zw}
\vspace*{-1mm}
\rule{\linewidth + 6zw}{1mm}
\obeylines
\gyodori{2}{{\gtfamily\ebseries\fontsize{25\bQ}{25H}\selectfont\booktitlename}\kern.25zw{\fontsizeIX ［きゅうだいぶんがく］}}
\gyodori{2}{■\kansuji\thisyear 年\kansuji\thismonth 月\kansuji\thisday 日　第一版第一刷発行}
\bookmaker{編集者}{＊＊＊＊＋＊＊＊＊}
\bookmaker{発行者}{＊＊＊＊＊}
\bookmaker{印刷者}{＊＊＊＊}
\bookmaker{印刷所}{＊＊＊＊＊＊＊＊＊}
\bookmaker{発行所}{九州大学文藝部}
\gyodori{4}{\includegraphics[angle=90,width=5zw]{hanko.eps}}
\vspace*{-4\baselineskip}
\hspace*{6zw}福岡市西区元岡七四四番地
\hspace*{6zw}郵便番号八一九―〇三九五
\hspace*{6zw}{\fontsize{10.5\bQ}{19trueH}\selectfont Website: \url{https://kyudai-bungeiclub.wixsite.com/kyudai-bungei}}
\hspace*{6zw}{\fontsize{10.5\bQ}{19trueH}\selectfont Twitter: @kyudaibungei}
\vspace{-.25\baselineskip}
\rule{\linewidth + 6zw}{.125truemm}
\vspace{-.25\baselineskip}
造本には十分注意しておりますが、乱丁・落丁の場合は、お取り替えいたしますので九州大学文藝部までご連絡ください。
本書の一部あるいは全部を無断で転写・複写することは法律で認められた場合を除き、著作権の侵害にあたります。
\textcopyright{} Kyushu University Literary Club and Authors \thisyear \hfill Printed in Japan
\disobeylines
\rule{\linewidth + 6zw}{1mm}
\end{multicols*}
```

## 印鑑画像
奥付に使われている印鑑の画像は、修正を施して画質を向上させてから用いた[^inkan]。

[^inkan]: なお、「修正前」に比べて「修正後」は印鑑の線を細くしすぎなのではないかとの印象を持った方がいるかもしれない。しかし、その印象は誤っている。実は、「修正後」の画像は「修正前」の画像から直接生成したのではない。「修正前」とある画像よりもさらに古い印鑑画像が文藝部のOneDriveに残っており、そちらはもっと線が細かったのである。つまり、元々は「修正後」の画像のように線は細めであった。それが、いつの間にか太くされ、ついでに線がジャギジャギにされており、いわば改悪されていたのであった。それゆえ、修正の方向性は決して誤ってはいない。上で提示した比較画像は、修正後の綺麗さをより際立たせるために、改悪されたバージョンとの比較をしてみたまでである。

![](/latex/assets/img/2020-04-12a.jpg)

　

（[次回]({{< ref "2020-04-13-latexnovel.md" >}})に続く）
