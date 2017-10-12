---
layout: post
title: LaTeXによる小説誌制作のヒント
date: 2017-10-12
tags: LaTeX
---

### 緒言
以下では、小説誌制作をLaTeXでおこなう場合のヒントを述べる。

### フォントの指定
LaTeXのデフォルトフォントはあまり小説向きではない。そこで以下のように記述しフォントを変更する。

まず日本語フォントであるが、これは游明朝がよい。游明朝を使うには、以下のように書く（uplatexを使うことを前提として。いまどきわざわざplatexを使う理由はあるまい）。

```LaTeX
% 日本語フォントを游明朝にする
\usepackage[deluxe,uplatex]{otf}
\AtBeginDvi{\special{pdf:mapfile otf-up-yu-win10.map}}
```

欧文フォントも変更しておこう。

```LaTeX
% 欧文フォントの変更
\usepackage[scale=0.95]{lato,opensans}
\tbaselineshift=.3zw % ついでに、ベースラインを適当に変更しておこう
```

### jdashパッケージ
游明朝だと、ダッシュ`――`がつながらない。そこでjdashパッケージを導入し、これを使う。jdashパッケージについては[ここ]({{ site.baseurl }}{% post_url 2017/02/2017-02-17-jdash %})を参照のこと。

### hanmenパッケージの使用
まず以下の二つのファイルをダウンロードし、TeXファイルと同じフォルダへ入れる。

* [hanmen.sty](https://gist.github.com/qdaibungei/5f6986fa99fc9a7d86122a7a9417d64e)
* [nombre_for_fancyhdr.sty](http://p-act.sakura.ne.jp/PARALLEL_ACT/LaTeX-Dojin/src/nombre_for_fancyhdr.sty)

そしてプリアンブルに以下を記述する。

```LaTeX
\usepackage[Q=13,H=22,W=49,L=21,ko=3.875truemm,te=26\bQ,footskip=2.47truemm,tate]{hanmen}
```

これで厳密な版面の設定が可能となる。厳密というのは、たとえば表と裏とで行が揃っているということである（厳密な設定をしないと、紙を透かして見たときに行がズレてしまう）。

### `\tcy`マクロ
LaTeXで縦書きをするさい、縦中横には`\rensuji`を使うよう言われる。しかし`\rensuji`だと前後のアキがおかしくなる（たとえば、行頭で`\rensuji`を使うとインデント量が狂う）。そこで`\tcy`命令を新たに作り、これを使うのがよい。

```LaTeX
\makeatletter
\chardef\nvlsty@zenkakuSpace=\jis"2121\relax
\newcommand{\tcy}[1]{\relax
    \nvlsty@zenkakuSpace\kern-1zw\relax
    \leavevmode\hbox to 1zw{\relax
        \centering\rensuji*{#1}\relax
    }\relax
    \kern-1zw\nvlsty@zenkakuSpace
}
\makeatother
```

以上をプリアンブルなどに書いておく。使いかたは至ってかんたんで、`\tcy{12}時`、のようにすればよい。

### 結言
以上、「これだけやっておけばまず本文レイアウトにかんする文句は出るまい」と思われる点について、ヒントを列挙した。参考にしてください。
