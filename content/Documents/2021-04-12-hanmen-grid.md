---
layout: post
title: LaTeXで本文領域にマス目を表示させる
date: 2021-04-12
tags: ["LaTeX", "memo"]
---

jlreq.clsで版面設計を行なうときでも、グリッド線（マス目）が表示できると都合がよい。この場合、hanmen.styの`pass`オプションを用いるとよい。

```LaTeX
\documentclass[jafontsize=14Q, baselineskip=24.5H, line_length=45zw, number_of_lines=41]{jlreq}
\usepackage[Q=14,H=24.5,W=45,L=41,g=45,pass]{hanmen}
\usepackage{bxjalipsum}
\begin{document}
\jalipsum[4-6]{wagahai}    
\end{document}
```

こうすると、次の結果を得る。

![](/latex/assets/img/2021-04-12.png)

確かにグリッド線がうまく表示されている。ただし現状、縦書き時にはグリッド線が少しずれて表示されてしまう。後日修正したい。
