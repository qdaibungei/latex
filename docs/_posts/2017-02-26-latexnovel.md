---
layout: post
title: LaTeXによる小説組版法（初級）
date: 2017-02-26
---

（※この記事は書きかけです。）

この記事は、LaTeXで小説を組版するにはどうすればよいかを具体的に解説するものである。理論的な話は最小限に留め、主として実践的な話題を取り上げる。つまり、具体的にどう手を動かせばLaTeXで小説が組版できるのかを解説する。

LaTeX（およびパソコン）初心者でも読めるように配慮したつもりだが、LaTeXについての最低限の知識は持っていることを前提とする。


### 雛形ファイルの用意
まず、LaTeXで電撃文庫のデザインで組版してみよう。

第一に、拙作のhanmenパッケージを用意する。

第二に、以下の文字列を `novel.tex` という名で保存する。

```LaTeX
\documentclass[a6paper,papersize,autodetect-engine]{jsbook}
\usepackage[Q=11.5,H=19,W=42,L=17,tate]{hanmen}
\usepackage{plext,pxrubrica}
\begin{document}
（ここに本文を書く）
\end{document}
```

これが雛形ファイルとなる。 `novel.tex` はhanmenパッケージと同じフォルダに入れておく。

### PDFファイルの生成
さて、あとは先程の雛形ファイルをPDF化できれば、まずは成功である。

PDF化の仕方は簡単である。ターミナルでカレントディレクトリに移動した後、次のように打ち込めばよい。

```shell script
uplatex novel
dvipdfmx novel
```

これでPDFファイルが生成されればひとまず第一関門はクリアである。

しかし、ターミナル（Windowsの人はコマンドプロンプト）を使ったことがない人は、上でどういうことをしたらいいのか分からなかっただろう。

そこで、ここでは意味は分からずともとりあえずPDFを生成する方法として次の方法を勧めたい。以下の文字列を `tex2pdf.bat` という名で保存する（ファイル名は末尾に `.bat` があれば何でもよいが、ここでは "tex to pdf" という意味で `tex2pdf.bat` という名にした）。

```bat
cd %~dp0
uplatex novel
dvipdfmx novel
```

この `tex2pdf.bat` を、`novel.tex` と同じフォルダに入れておく。あとは、`tex2pdf.bat` をダブルクリックすると、見事PDFファイルが生成されるはずである。
