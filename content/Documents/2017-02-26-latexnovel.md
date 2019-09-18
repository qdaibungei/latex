---
layout: post
title: LaTeXによる小説組版法（初級）
aliases:
    - /documents/2017/02/26/latexnovel/
date: 2017-02-26
tags: ["LaTeX", "guide"]
---


この記事は、LaTeXで小説を組版するにはどうすればよいかを具体的に解説するものである。理論的な話は最小限に留め、主として実践的な話題を取り上げる。つまり、具体的にどう手を動かせばLaTeXで小説が組版できるのかを解説する。

コンピュータおよびLaTeXの初心者でも読めるように配慮したつもりだが、LaTeXの知識ゼロだと厳しいかもしれない。

なお、以下の方法はあくまでも初心者向けの簡易的方法であり、私が普段LaTeXで組版している実際の現場の方法とはかなり異なる。


# 電撃文庫の再現
## 雛形ファイルの用意
具体例として、LaTeXで電撃文庫のデザインで組版してみよう。といっても商用フォントが手元にない限り完全再現はできないので、大体同じようなデザインを作るだけであるが。

まず、以下を `novelstyle.sty` という名前で保存する。スタイルファイル内の記述の意味は、一応コメント文で書いておいた。

<script src="https://gist.github.com/qdaibungei/2bebd353eaeefa5db076402c75e095cd.js"></script>

　

次に、以下の文字列を `novel.tex` という名で保存する。これが雛形ファイルとなる。

```TeX
\documentclass[a6paper,papersize,autodetect-engine]{jsbook}
\usepackage{novelstyle}
\begin{document}
（ここに本文を書く）
\end{document}
```

最後に、[hanmen.sty](https://gist.github.com/qdaibungei/5f6986fa99fc9a7d86122a7a9417d64e)もダウンロードしておく。

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
\documentclass[a6paper,papersize,autodetect-engine]{jsbook}
\usepackage{novelstyle}
\begin{document}
（ここに本文を書く）
\end{document}
```

## 基本
`（ここに本文を書く）` の部分に本文を書けばよい。たとえば以下のように書く。

```TeX
\documentclass[a6paper,papersize,autodetect-engine]{jsbook}
\usepackage{novelstyle}
\begin{document}
　吾輩は猫である。名前はまだ無い。
　どこで生れたかとんと見当がつかぬ。何でも薄暗いじめじめした所でニャーニャー泣いていた事だけは記憶している。吾輩はここで始めて人間というものを見た。しかもあとで聞くとそれは書生という人間中で一番獰悪な種族であったそうだ。この書生というのは時々我々を捕えて煮て食うという話である。
\end{document}
```

通常のLaTeX記法であれば、段落毎に空行を挟む必要があるが、小説を書くときにそのようなLaTeX記法で書くと台詞が連続する場合などに入力しづらい。そこで、`novelstyle.sty` では、空行を挟まずとも段落分けできるようにしてある。**つまり、Wordなどで書くときとほぼ同じ感覚で本文を書けばよい。**

## ルビ
ルビを振りたいときは、ルビマクロが使える。

```TeX
\documentclass[a6paper,papersize,autodetect-engine]{jsbook}
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
\documentclass[a6paper,papersize,autodetect-engine]{jsbook}
\usepackage{novelstyle}
\begin{document}
　\ruby[h]{吾輩}{わが|はい}は猫である。名前はまだ無い。
　\tcy{12}時だ。そろそろお昼にしよう\tcy{!!}
\end{document}
```

## 疑問符・感嘆符の後の空白
最後に、ちょっと面倒だが、疑問符・感嘆符の後の全角空白は、`\zenkakuaki` に置換する。

```TeX
\documentclass[a6paper,papersize,autodetect-engine]{jsbook}
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
