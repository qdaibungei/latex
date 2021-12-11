---
layout: post
title: LaTeXによる小説組版法（初級）
aliases:
    - /2017/02/26/latexnovel.html
date: 2017-02-26
tags: ["LaTeX", "DTP"]
---


この記事は、LaTeXで小説を組版するにはどうすればよいかを具体的に解説するものである。理論的な話は最小限に留め、主として実践的な話題を取り上げる。つまり、具体的にどう手を動かせばLaTeXで小説が組版できるのかを解説する。

コンピュータおよびLaTeXの初心者でも読めるように配慮したつもりだが、LaTeXの知識ゼロだと厳しいかもしれない。

なお、以下の方法はあくまでも初心者向けの簡易的方法であり、私が普段LaTeXで組版している実際の現場の方法とはかなり異なる。


# 電撃文庫の再現
## 雛形ファイルの用意
具体例として、LaTeXで電撃文庫のデザインで組版してみよう。といっても商用フォントが手元にない限り完全再現はできないので、大体同じようなデザインを作るだけであるが。

まず、以下を `novelstyle.sty` という名前で保存する。スタイルファイル内の記述の意味は、一応コメント文で書いておいた。

```tex
\NeedsTeXFormat{pLaTeX2e}
\ProvidesPackage{novelstyle}

\RequirePackage[Q=11.5,H=19,W=42,L=17,te=-2mm,headsep=5mm,tate]{hanmen}
\RequirePackage[deluxe,uplatex]{otf}
\RequirePackage{plext,pxrubrica}% plextは縦組み時に有用なパッケージ、pxrubricaはルビ振りに必要なパッケージ。

%
%% header & footer
%
% ページ番号を出力するときは\thepageと書く。
\fancyhf{}% ヘッダー・フッターの初期化
\def\nvlsty@nombre{\textit{\thepage}}
\def\nvlsty@booktitle{ここにタイトルを書く}
\fancyhead[RE]{\vspace*{0pt}\scriptsize\nvlsty@nombre}
\fancyhead[LO]{\vspace*{0pt}\footnotesize\nvlsty@nombre\hskip1zw\scriptsize\nvlsty@booktitle}

%
%% \tcy
%
% LaTeXの解説書では、縦中横に\rensujiを使うこととされているが、
% \rensujiだと無駄なアキが入ってしまいベタ組みできない。
% そこで、ここでは新たに\tcyを定義する。
\chardef\nvlsty@zenkakuSpace=\jis"2121\relax
\def\tcy#1{%
    \nvlsty@zenkakuSpace\kern-1zw\relax
    \leavevmode\hbox to 1zw{%
        \centering\rensuji*{#1}%
    }%
    \kern-1zw\nvlsty@zenkakuSpace
}

%
%% \xobeylines
%
% これは、空行をあけなくても段落分けできるようにする工夫。
% 本文中、\xobeylinesと書けば、空行をあけなくても段落が変わる。
% \disobeylinesと書けば、LaTeX記法通り、空行をあけないと段落が変わらないようになる。
% ただし、併用するパッケージによってはいろいろ弊害が出る場合があるので、素直に空行によって段落分けしたほうが無難かもしれない（実際私も、普段は空行で段落分けしている）。
{\catcode`\^^M=\active
    \gdef\xobeylines{\catcode`\^^M\active \def^^M{\par\leavevmode}}%
    \global\def^^M{\par\leavevmode}%
}
\def\disobeylines{\catcode`\^^M=5 }
\let\disxobeylines=\disobeylines
\AtBeginDocument{\xobeylines}%% 本文開始直後から\xobeylinesが適用されるようにする。

%
%% 全角アキ
%
% 疑問符などの後に全角空白文字「　」を使ってアキを作ると、疑問符が行末に来たとき空白が行頭に出てしまって都合が悪い。
% そこで、全角アキは以下のマクロで作る必要がある。
\newcommand{\zenkakuaki}{\hskip1zw plus .125zw minus 0.03125zw}
% 二分アキ・四分アキなども、以下のマクロを使って出力する。
\newcommand{\nibusibuaki}{\hskip.75zw plus .125zw minus 0.03125zw}
\newcommand{\nibuaki}{\hskip.5zw plus .125zw minus 0.03125zw}
\newcommand{\sibuaki}{\hskip.25zw plus .125zw minus 0.03125zw}


%
%% 値の調整
%
\tbaselineshift=.3zw%% 欧文出力位置の調整
% 以下は、余計な空白が入らないようにするための設定。
\parindent=0pt
\parskip=0pt
\parsep=0pt
\partopsep=0pt
% 以下は、禁則処理を抑制するためのもの。
% ウィドウ処理などを厳密にやりすぎるとかえって見栄えが悪いので、
% ここでは禁則処理を抑制しておく。
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
\prebreakpenalty\jis"2139=0     %々（←これに関しては禁則処理を抑制しないほうが読みやすいかも）

% \endinputはスタイルファイルの末尾に記すおまじない。
\endinput
```

　

次に、以下の文字列を `novel.tex` という名で保存する。これが雛形ファイルとなる。

```TeX
\documentclass[uplatex,dvipdfmx,a6paper,papersize]{jsbook}
\usepackage{novelstyle}
\begin{document}
（ここに本文を書く）
\end{document}
```

最後に、[hanmen.sty]({{< ref "2017-02-18-hanmen.md" >}})もダウンロードしておく。

以上のファイルはすべて同じフォルダに入れておく。

## PDFファイルの生成
さて、あとは先程の雛形ファイルをPDF化できれば、ひとまずは成功である。

コンピュータに詳しい人なら、PDF化は容易であろう。しかし、コンピュータに不慣れな人にはこれが難しく感じられるはずである。

そこで、簡単にPDF化する方法として、以下のものを勧めたい（なお、以下の方法はWindows限定）。

次の文字列を `tex2pdf.bat` という名で保存する[^1]。

[^1]: ファイル名は末尾に `.bat` があれば何でもよいが、ここでは "tex to pdf" という意味で `tex2pdf.bat` という名にした。

```shell
cd %~dp0
uplatex novel
dvipdfmx novel
```

この `tex2pdf.bat` を、`novel.tex` と同じフォルダに入れておく。あとは、`tex2pdf.bat` をダブルクリックすると、見事PDFファイルが生成されるはずである。


# 本文の書き方
上で、`novel.tex` というファイルを作った。それをここに再掲する。

```TeX
\documentclass[uplatex,dvipdfmx,a6paper,papersize]{jsbook}
\usepackage{novelstyle}
\begin{document}
（ここに本文を書く）
\end{document}
```

## 基本
`（ここに本文を書く）` の部分に本文を書けばよい。たとえば以下のように書く。

```TeX
\documentclass[uplatex,dvipdfmx,a6paper,papersize]{jsbook}
\usepackage{novelstyle}
\begin{document}
　吾輩は猫である。名前はまだ無い。
　どこで生れたかとんと見当がつかぬ。何でも薄暗いじめじめした所でニャーニャー泣いていた事だけは記憶している。吾輩はここで始めて人間というものを見た。しかもあとで聞くとそれは書生という人間中で一番獰悪な種族であったそうだ。この書生というのは時々我々を捕えて煮て食うという話である。
\end{document}
```

通常のLaTeX記法であれば、段落毎に空行を挟む必要があるが、小説を書くときにそのようなLaTeX記法で書くと台詞が連続する場合などに入力しづらい。そこで、`novelstyle.sty` では、空行を挟まずとも段落分けできるようにしてある。つまり、Wordなどで書くときとほぼ同じ感覚で本文を書けばよい。

## ルビ
ルビを振りたいときは、ルビマクロが使える。

```TeX
\documentclass[uplatex,dvipdfmx,a6paper,papersize]{jsbook}
\usepackage{novelstyle}
\begin{document}
　\ruby[h]{吾輩}{わが|はい}は猫である。名前はまだ無い。
　どこで生れたかとんと見当がつかぬ。何でも薄暗いじめじめした所でニャーニャー泣いていた事だけは記憶している。吾輩はここで始めて人間というものを見た。しかもあとで聞くとそれは書生という人間中で一番\ruby[h]{獰悪}{どう|あく}な種族であったそうだ。この書生というのは時々我々を捕えて煮て食うという話である。
\end{document}
```

ルビマクロの使い方は、[LaTeX 文書で“美しい日本の”ルビを使う ～pxrubrica パッケージ～](http://qiita.com/zr_tex8r/items/42466cbcbeb670a3a2dc)を参照せよ。


## 縦中横
縦中横が使いたいときは、`\tcy` マクロが使える[^tcy]。

[^tcy]: 縦中横を実現するマクロとしては、plextパッケージですでに `\rensuji` が提供されている。しかし、`\rensuji` を直接使うとどうやら無駄に \xkanjiskip が挿入されてしまうようなので、ここでは独自に `\tcy` というマクロを使うこととした。

```TeX
\documentclass[uplatex,dvipdfmx,a6paper,papersize]{jsbook}
\usepackage{novelstyle}
\begin{document}
　\ruby[h]{吾輩}{わが|はい}は猫である。名前はまだ無い。
　\tcy{12}時だ。そろそろお昼にしよう\tcy{!!}
\end{document}
```

## 疑問符・感嘆符の後の空白
最後に、ちょっと面倒だが、疑問符・感嘆符の後の全角空白は、`\zenkakuaki` に置換する。

```TeX
\documentclass[uplatex,dvipdfmx,a6paper,papersize]{jsbook}
\usepackage{novelstyle}
\begin{document}
　\ruby[h]{吾輩}{わが|はい}は猫である。名前はまだ無い。
　\tcy{12}時だ！\zenkakuaki{}そろそろお昼にしよう\tcy{!!}\zenkakuaki{}ラーメンがいいな！
\end{document}
```

`！` と `？` だけは、直後に全角空白を入れないで記述しておくと、PDF化させたさいに自動的に全角アキを挿入してくれるから、最初からそのように書いてもよい。しかし、`\tcy{!!}` の直後には `\zenkakuaki` が必ず要る。


# まとめ
上で見たように、LaTeXで小説を組版するのはそんなに難しいことではない。誰でもできることである。

ただし、レイアウトを変更したり、扉ページを作ったりは少し難しいので、ここでは解説しなかった。後日、中級編を執筆する予定なので、そこで解説できればと思っている。

**追記**：[中級編]({{< ref "2019-10-12-latexnovel.md" >}})を執筆しました。
