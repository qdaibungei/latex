---
layout: post
title: LaTeXによる小説組版法・中級編――ベスト文集制作記録⑤
date: 2020-04-07
tags: ["LaTeX", "guide"]
---


# 本文の組み方③・ベタ組篇

小説の本文は、ベタ組にするのがよい。ベタ組とは、方眼紙に字を埋めるがごとく、字間を空けずに組むことである。次の画像のように組むということである。

![](/latex/assets/img/2020-04-07.png)

## hanmen.styの使用
LaTeXの標準機能でベタ組を実現するのは案外難しい。そこで、[hanmen.sty]({{< ref "2017-02-18-hanmen.md">}})を使用する。これを用いれば、確実にベタ組が実現できる。

## 禁則処理の抑制


```latex
%% penalties
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
\prebreakpenalty\jis"2139=0     %々
\prebreakpenalty\jis"2144=0     %…
```

## 句読点、閉じ括弧、中黒、疑問符・感嘆符後の空白、縦中横
LaTeXの標準設定では、句読点、閉じ括弧、中黒、疑問符・感嘆符後の空白に関してはベタ組にならない。そこで、さらにベタ組にするためには以下のような工夫が必要である。

### 句読点
```latex
%% punctuations
\if@burasage
    \def\、{%
        \@ifnextchar\par{、}{、\hbox{}}%
    }%
    \def\。{%
        \@ifnextchar\par{。}{。\hbox{}}%
    }%
\else
    \def\、{\nobreak\makebox[1zw][l]{、}}%
    \def\。{\nobreak\makebox[1zw][l]{。}}%
\fi
```

### 閉じ括弧
```python
def txt_replace(x):
    # 段落を区切る
    x = x.replace('\r\n','\n')
    x = x.replace('\n','\n\n')
    # インデント処理
    for bracket in brackets:
        x = re.sub(r'^'+bracket[0],r'{\\bracketIndent}'+bracket[0],x)
    x = re.sub(r'^　',r'{\\normalIndent}',x)
    # 閉じ括弧
    for bracket in brackets:
        x = x.replace(bracket[1], bracket[1]+'\\hbox{}')
        x = x.replace(bracket[1]+'\\hbox{}}', bracket[1]+'}')
        x = x.replace(bracket[1]+'\\hbox{}\n', bracket[1]+'\n')
        x = x.replace(bracket[1]+'\\hbox{}\<', bracket[1]+'\<')
        for bracket2 in brackets:
            x = x.replace(bracket[1]+'\\hbox{}'+bracket2[0], bracket[1]+'\\hbox{}{\\minusnibuaki}'+bracket2[0])
            x = x.replace(bracket[1]+'\\hbox{}'+bracket2[1], bracket[1]+bracket2[1])
    # 疑問符と感嘆符
    for y in exque:
        x = x.replace(y[0]+'　','{\\'+y[1]+'\\zenkakuaki\\hbox{}}')
        x = x.replace(y[0],'{'+'\\'+y[1]+'}')
        x = re.sub(r'([0-9a-zA-Z]){\\'+y[1]+'}', r'\1'+y[0], x)
    for bracket in brackets:
        if x.find(bracket[0] or bracket[1]) > -1:
            for z in exque:
                x = x.replace(bracket[1]+'\\hbox{}{\\'+z[1], bracket[1]+'\<'+'{\\'+z[1])
                x = x.replace('{\\'+z[1]+'\\zenkakuaki\\hbox{}}'+bracket[0], '{\\'+z[1]+'\\zenkakuaki\\hbox{}}\\<'+bracket[0])
    # 句点と読点
    for kutoten in kutotens:
        x = x.replace(kutoten, '\\'+kutoten)
        for bracket in brackets:
            x = x.replace('\\'+kutoten+bracket[0], '\\'+kutoten+'\\<'+bracket[0])
            x = x.replace('\\'+kutoten+bracket[1], kutoten+bracket[1])
            x = x.replace(bracket[1]+'\\hbox{}\\'+kutoten, bracket[1]+'\\<\\'+kutoten)
    x = x.replace('\。\。\。', r'。。。')
    # 縦組み用約物
    x = x.replace('・','{\\・}')
    x = x.replace('‐','{\\‐}')
    x = x.replace('：','{\\：}')
    x = x.replace('〝','“')
    x = x.replace('〟','”')
    x = x.replace('“','〝\\nobreak ')
    x = x.replace('”','\\nobreak 〟')
    for bracket in brackets:
        x = x.replace('{\\・}'+bracket[0],'\\・\\<'+bracket[0])
        x = x.replace(bracket[1]+'{\\・}',bracket[1]+'\\<{\\・}')
    x = x.replace('．', '\\．')
    return x
```

### 中黒
　

### 疑問符・感嘆符後の空白
　

### 縦中横
LaTeXで縦書きをするさい、縦中横には`\rensuji`を使うよう言われる。しかし`\rensuji`だと前後のアキがおかしくなる（たとえば、行頭で`\rensuji`を使うとインデント量が狂う）。そこで`\tcy`命令を新たに作り、これを使うのがよい。

```latex
\usepackage{pxghost}
\newcommand{\tcy}[1]{%
    \jghostguarded{%
        \leavevmode\hbox to 1zw{%
            \centering\rensuji*{#1}%
        }%
    }%
}
```
