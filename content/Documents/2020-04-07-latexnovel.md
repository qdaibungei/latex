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
LaTeXの標準機能でベタ組を実現するのは案外難しい。そこで、[hanmen.sty]({{< ref "2017-02-18-hanmen.md">}})を使用する。これを用いると、ほとんどの場合問題なくベタ組にできる。

## ベタ組のための基本設定
次の設定をしておくと字間や行間が広がりにくい。

```latex
%% 字間・行間の設定
\kanjiskip=0zw plus .25zw minus .03125zw
\xkanjiskip=.25zw plus .25zw minus .0625zw
\parindent=0zw
\parskip=0zw
\parsep=0zw
\partopsep=0zw
```

## 縦中横
LaTeXで縦書きをする際、縦中横には`\rensuji`を使うよう言われる。しかし`\rensuji`だと無駄に\xkanjiskipが挿入されてしまい、ベタ組にならない。そこで`\tcy`命令を新たに作り、これを使うのがよい。

```latex
\usepackage{pxghost}

%% \tcy
\newcommand{\tcy}[1]{%
    \jghostguarded{%
        \leavevmode\hbox to 1zw{%
            \centering\rensuji*{#1}%
        }%
    }%
}
```

数字やアルファベットを自動で縦中横にするために、『九大文学』製作時にはPythonで次の関数を作って置換した。

```python
nums = [
    ['０','0'],['１','1'],['２','2'],['３','3'],['４','4'],
    ['５','5'],['６','6'],['７','7'],['８','8'],['９','9']
]
alphabets = [
    ['Ａ','A'],['Ｂ','B'],['Ｃ','C'],['Ｄ','D'],['Ｅ','E'],
    ['Ｆ','F'],['Ｇ','G'],['Ｈ','H'],['Ｉ','I'],['Ｊ','J'],
    ['Ｋ','K'],['Ｌ','L'],['Ｍ','M'],['Ｎ','N'],['Ｏ','O'],
    ['Ｐ','P'],['Ｑ','Q'],['Ｒ','R'],['Ｓ','S'],['Ｔ','T'],
    ['Ｕ','U'],['Ｖ','V'],['Ｗ','W'],['Ｘ','X'],['Ｙ','Y'],
    ['Ｚ','Z'],
    ['ａ','a'],['ｂ','b'],['ｃ','c'],['ｄ','d'],['ｅ','e'],
    ['ｆ','f'],['ｇ','g'],['ｈ','h'],['ｉ','i'],['ｊ','j'],
    ['ｋ','k'],['ｌ','l'],['ｍ','m'],['ｎ','n'],['ｏ','o'],
    ['ｐ','p'],['ｑ','q'],['ｒ','r'],['ｓ','s'],['ｔ','t'],
    ['ｕ','u'],['ｖ','v'],['ｗ','w'],['ｘ','x'],['ｙ','y'],
    ['ｚ','z']
]
def alphabet_replace(s):
    for x in alphabets:
        s = s.replace(x[0],'\\tcy{'+x[1]+'}')
    return s
def num_replace(s):
    for x in nums:
        s = s.replace(x[0],'\\tcy{'+x[1]+'}')
    return s
```

## 禁則処理の抑制
禁則処理が多いとベタ組になりにくい。そこで禁則処理を程々に抑制する。

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

縦書きの場合、禁則処理をこのように抑制したほうが商業誌の組み方に近づけられる。

## 約物の処理
LaTeXの標準設定では、約物（句読点、括弧、中黒、疑問符・感嘆符など）に関してはベタ組にならない。そこで、さらにベタ組にするためには以下のような工夫が必要である。

### LaTeX側のマクロ定義
まずLaTeXで次のように約物用のマクロ定義を行なう。

```latex
%% punctuations
%%%% 句読点
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
%%%% 疑問符・感嘆符
\newcommand{\QueEx}{\nobreak ⁈\<}
\newcommand{\QueQue}{\nobreak ⁇\<}
\newcommand{\ExQue}{\nobreak ⁉\<}
\newcommand{\ExEx}{\nobreak ‼\<}
\newcommand{\Que}{\nobreak ？\<}
\newcommand{\Ex}{\nobreak ！\<}
\newcommand{\ExExExEx}{\nobreak{\tcy{!!!!}}}
\newcommand{\ExExEx}{\nobreak{\tcy{!!!}}}
\newcommand{\zenkakuaki}{\hskip1zw plus .125zw minus 0.03125zw}
\newcommand{\nibusibuaki}{\hskip.75zw plus .125zw minus 0.03125zw}
\newcommand{\nibuaki}{\hskip.5zw plus .125zw minus 0.03125zw}
\newcommand{\minusnibuaki}{\hskip-.5zw plus .125zw minus 0.03125zw}
\newcommand{\sibuaki}{\hskip.25zw plus .125zw minus 0.03125zw}
%%%% その他約物
\def\・{\jghostguarded{\leavevmode\hbox to 1zw{・}}}
\def\……{\jghostguarded{\hbox to 2zw{……}}}
\def\．{\jghostguarded{\leavevmode\hbox to 1zw{\raise.6zw\hbox{.}}}}
\def\‐{\jghostguarded{\leavevmode\hbox to 1zw{\hss\rule[-.25H]{.4zw}{.55H}\hss}}}% 必要に応じて使用
\def\：{\ifydir ：\else\CID{12101}\fi}
%%%% インデント
\newcommand{\normalIndent}{\hskip1zw}
\newcommand{\bracketIndentAmount}[1]{\def\bracketIndent{\hskip#1zw\<}}
\bracketIndentAmount{0.5}
```

### Pythonによる約物の調整
```python
brackets = [
    ['「','」'],['『','』'],['（','）'],['〈','〉'],
    ['【','】'],['〔','〕'],['《','》'],['［','］'],
    ['｛','｝'],['〝','〟'],['“','”'],['‘','’']
]
exque = [
    ['‼','ExEx'],
    ['⁉','ExQue'],
    ['⁈','QueEx'],
    ['⁇','QueQue'],
    ['！','Ex'],
    ['？','Que'],
    ['!!!!','ExExExEx'],
    ['!!!','ExExEx'],
    ['!!','ExEx'],
    ['!?','ExQue'],
    ['?!','QueEx'],
    ['??','QueQue']
]
kutotens = ['、','。']
def txt_replace(x):
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
    # その他約物
    x = re.sub('([^…])(……)([^…])',r'\1\\……\3',x)
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
