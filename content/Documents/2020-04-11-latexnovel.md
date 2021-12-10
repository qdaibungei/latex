---
layout: post
title: ベスト文集制作記録⑨（著者コメント欄）
date: 2020-04-11
tags: ["LaTeX", "DTP"]
---

# 著者コメント欄
ベスト文集の終わりのほうに、著者コメント欄を設けた。

![](/latex/assets/img/2020-04-11.png)

版面設定は次のようにした。見開きに収めたかったので、文字サイズは10.5Qとかなり小さめ。

```latex
\newhanmen{Q=10.5,H=15.75,W=66.5,L=29,tate,ko=0mm,te=0mm,nodo=\relax,koguchi=14.625mm,chi=16.4375mm,footskip=3.25mm}
```

「著者コメント」とあるタイトル部分は、次のコードにより出力した。

```latex
\fboxsep=.25zw
\definecolor{creditgray}{gray}{0.1}
\colorbox{creditgray}{\emph{\color{white}~著者コメント}}{\color{creditgray}\rule[-.75zw]{59.75zw}{1.5zw}}
```

　

（[次回]({{< ref "2020-04-12-latexnovel.md" >}})に続く）
