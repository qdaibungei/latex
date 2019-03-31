---
layout: post
title: CTANにない欧文フォントをLaTeXにインストールする
date: 2019-03-29
tags: LaTeX
---

# はじめに
upLaTeXにおいて、CTANに存在する欧文フォントをインストールするのは容易だ。しかしCTANに存在しないフォントをインストールするのは難しい。

困難の原因は、二つある。第一は、tfmファイルやmapファイルを生成するのが難しいことである。第二は、フォント設定ファイルの作り方が分かりづらいことである。

そこで、備忘録を兼ね、以下にその方法を記す。

# 一つのウェイトをインストールしてみる
## tfmなどの生成
フォントの形式は通常、otfやttfなどであるが、TeXはそれらを直接に扱うことができない。そこでtfmファイルなどのフォントファイルを生成する必要がある。面倒だが、フォントごとに別のtfmを生成せねばならない。

tfmを生成するためには「otftotfm」というソフトを使う。otftotfmはふつうTeXをインストールしたときに一緒にインストールされているはずだ。インストールされているかどうかを確認するためには、ターミナルで「otftotfm --version」と打ち、ヴァージョン情報が返ってくるかどうかを確かめればよい。

さて今回はGoogle Fontsに収録されている[Josefin Slab](https://fonts.google.com/specimen/Josefin+Slab?category=Serif)というフォントをインストールしてみよう。まずは、一つのウェイト（Regular）だけをインストールしてみる。

まず作業用ディレクトリを適当に作っておき、そこにJosefinSlab-Regular.ttfを入れておく。ターミナルで作業用ディレクトリに行き、次のコマンドを実行してみよう。

```bash
otftotfm --no-type1 --no-dotlessj --mapfile=josefinslab.map -e ec.enc -n josefinslab-regular JosefinSlab-Regular.ttf
```

すると、○○○.tfm、○○○.vf、○○○.map、○○○.encという四種類のファイルが生成されるはずである[^1]。一応、オプションの意味を説明しておこう。

[^1]: ここでエラーが出てtfmファイルが生成されない場合、ec.encをネットで検索してダウンロードし、作業用ディレクトリに置いておくこと。

* `--no-type1 --no-dotlessj`は、「Type 1フォントへの変換を行わない」ということ、らしい。あまり理解できていない。
* `--mapfile=josefinslab.map`で、mapファイルの指定をする。
* `-e ec.enc`は、エンコーディングファイルの指定。ec.encを指定すると、T1エンコーディングになる。ちなみにtexnansi.encを指定するとLY1エンコーディングになる。エンコーディングって何なのかいまいち分かっていないが、私はT1エンコーディングにしておいて困ったことはない。
* `-n josefinslab-regular`は、tfm名の指定。

また、`-fkern`とすればカーニングの、`-fliga`とすればリガチャの情報を、tfmに含ませることができる。Josefin Slabにはこれらの機能がなかったから上ではこのオプションをつけていないが、この機能があればつけたほうがよい。`otfinfo -f JosefinSlab-Regular.ttf`などとすれば、どのような機能が備わっているかを確認できる。

## styファイルの作成
各種ファイルが生成できたならば、すかさず、josefinslab.styという名前でstyファイルを作る。

```LaTeX
%
% josefinslab.sty
%
\AtBeginDvi{\special{pdf:mapfile josefinslab.map}}% 先程生成したmapファイルの読み込み
\DeclareFontFamily{T1}{josefinslab}{}% TeXファイル内で使うフォント名を設定
\DeclareFontShape{T1}{josefinslab}{m}{n}{<-> josefinslab-regular}{}% 上で設定したフォント名と、先程生成したtfmファイルとを結びつける
\endinput
```

## インストールできたかを確認
ここまでできたら、ほとんどインストールは完了したと言ってよい（今のままだと、各種ファイルがディレクトリにたくさん散らばっていて、気持ち悪いかもしれないが、この問題は次の節で解決する）。

動作確認のため、作業用ディレクトリにTeXファイルを作って、フォントが埋め込まれるかをチェックしておこう。

```LaTeX
\documentclass[uplatex]{jsarticle}
\usepackage{josefinslab}
\begin{document}
\romanfamily{josefinslab}\selectfont
This is a test.
\end{document}
```

「This is a test.」がJosefin Slabで表示されたら、ここまでの作業はうまくいったことになる。


# すべてのウェイトをインストールする
## tfmなどの生成とインストール
すべてのウェイトをインストールするには、同じことを全ウェイトで実行すればよい。

```bash
otftotfm --no-type1 --no-dotlessj --mapfile=josefinslab.map -e ec.enc -n josefinslab-regular JosefinSlab-Regular.ttf
otftotfm --no-type1 --no-dotlessj --mapfile=josefinslab.map -e ec.enc -n josefinslab-regular-italic JosefinSlab-RegularItalic.ttf
otftotfm --no-type1 --no-dotlessj --mapfile=josefinslab.map -e ec.enc -n josefinslab-light JosefinSlab-Light.ttf
otftotfm --no-type1 --no-dotlessj --mapfile=josefinslab.map -e ec.enc -n josefinslab-light-italic JosefinSlab-LightItalic.ttf
otftotfm --no-type1 --no-dotlessj --mapfile=josefinslab.map -e ec.enc -n josefinslab-thin JosefinSlab-Thin.ttf
otftotfm --no-type1 --no-dotlessj --mapfile=josefinslab.map -e ec.enc -n josefinslab-thin-italic JosefinSlab-ThinItalic.ttf
otftotfm --no-type1 --no-dotlessj --mapfile=josefinslab.map -e ec.enc -n josefinslab-semibold JosefinSlab-SemiBold.ttf
otftotfm --no-type1 --no-dotlessj --mapfile=josefinslab.map -e ec.enc -n josefinslab-semibold-italic JosefinSlab-SemiBoldItalic.ttf
otftotfm --no-type1 --no-dotlessj --mapfile=josefinslab.map -e ec.enc -n josefinslab-bold JosefinSlab-Bold.ttf
otftotfm --no-type1 --no-dotlessj --mapfile=josefinslab.map -e ec.enc -n josefinslab-bold-italic JosefinSlab-BoldItalic.ttf
```

このままでは各種ファイルでディレクトリがごちゃごちゃする。そこで○○○.tfmはtfmフォルダ直下のjosefinslabフォルダに、○○○.vfはvfフォルダ直下のjosefinslabフォルダに、というような塩梅で移動させておく。

```bash
mkdir vf/
mkdir tfm/
mkdir map/
mkdir enc/
mkdir vf/josefinslab/
mkdir tfm/josefinslab/
mkdir map/josefinslab/
mkdir enc/josefinslab/
mv *.vf vf/josefinslab/
mv *.tfm tfm/josefinslab/
mv *.map map/josefinslab/
mv *.enc enc/josefinslab/
```

こうして、各種ファイルを収納した四つのフォルダが作られた。これらのフォルダを、`$TEXMF/fonts/`へ移動させる（マージさせる）[^2]と、tfmなどの各種ファイルがカレントディレクトリになくとも使えるようになる。こうしてtfm類のインストールは完了する。

[^2]: `$TEXMF`とは、TeXとMETAFONTという意味で、各種パッケージが収められるディレクトリのこと。その場所は複数あるので「`$TEXMF`」という変数の形でよく表記される。`$TEXMF`の場所が分からない場合、ターミナルで`kpsewhich -var-value TEXMF`を実行すれば、調べられる。

mapファイルが読み込めずエラーが生ずる場合、`$ sudo mktexlsr`を実行して情報を更新する。

## styファイルの生成とインストール
さらにstyファイルを作る。

```LaTeX
%
% josefinslab.sty
%
\AtBeginDvi{\special{pdf:mapfile josefinslab.map}}
\DeclareFontShape{T1}{josefinslab}{el}{n}{<-> josefinslab-thin}{}
\DeclareFontShape{T1}{josefinslab}{el}{it}{<-> josefinslab-thin-italic}{}
\DeclareFontShape{T1}{josefinslab}{l}{n}{<-> josefinslab-light}{}
\DeclareFontShape{T1}{josefinslab}{l}{it}{<-> josefinslab-light-italic}{}
\DeclareFontShape{T1}{josefinslab}{m}{n}{<-> josefinslab-regular}{}
\DeclareFontShape{T1}{josefinslab}{m}{it}{<-> josefinslab-regular-italic}{}
\DeclareFontShape{T1}{josefinslab}{sb}{n}{<-> josefinslab-semibold}{}
\DeclareFontShape{T1}{josefinslab}{sb}{it}{<-> josefinslab-semibold-italic}{}
\DeclareFontShape{T1}{josefinslab}{bx}{n}{<-> josefinslab-bold}{}
\DeclareFontShape{T1}{josefinslab}{bx}{it}{<-> josefinslab-bold-italic}{}
\endinput
```

これを`$TEXMF/tex/latex/`に移動させる。これでstyファイルのインストールも完了する。

本当は`\DeclareFontShape……`のような情報はstyでなくfdファイルに書き込むのが正しいのかもしれないが、fdファイルを作るやり方はよく分からなかった。しかしstyに書いても動くことは動くので、動作の点からは特に問題ないものと思われる。


# scaleオプションの追加
scaleオプションを追加しておくと便利である（既存の欧文フォントのパッケージには大抵scaleオプションがある）。scaleオプションとは、`\usepackage[scale=0.9]{josefinslab}`などとすれば欧文が0.9倍の大きさで印字されるようなオプション機能である。

scaleオプションを実装するためには、styファイルを少しばかり書き換えるだけでよろしい。具体的には以下のようにする（煩雑になるのを避けるためウェイトは二種類のみにしてある）。

```LaTeX
%
% josefinslab.sty
%

\ProvidesPackage{josefinslab}[2019/03/31 v0.2]

\RequirePackage{xkeyval}
\DeclareOptionX{scale}{\def\jsfnslb@scale{#1}}
\DeclareOptionX{scaled}{\def\jsfnslb@scale{#1}}
\ExecuteOptionsX{scale=1}
\ProcessOptionsX

\def\jsfnslb@@scale{s*[\jsfnslb@scale]}

\DeclareFontFamily{T1}{josefinslab}{}
\DeclareFontShape{T1}{josefinslab}{m}{n}{<-> \jsfnslb@@scale josefinslab-regular}{}
\DeclareFontShape{T1}{josefinslab}{m}{it}{<-> \jsfnslb@@scale josefinslab-regular-italic}{}
\DeclareFontShape{T1}{josefinslab}{bx}{n}{<-> \jsfnslb@@scale josefinslab-bold}{}
\DeclareFontShape{T1}{josefinslab}{bx}{it}{<-> \jsfnslb@@scale josefinslab-bold-italic}{}

\AtBeginDvi{\special{pdf:mapfile josefinslab.map}}

\endinput
```

# 参考文献
* [dvipdfmx で OpenType する件について (1)](https://zrbabbler.hatenablog.com/entry/20110926/1317041905)
* [dvipdfmx で OpenType する件について (2)](https://zrbabbler.hatenablog.com/entry/20110927/1317136030)
* [LaTeX に新しいフォントを持ち込む練習](https://zrbabbler.hatenablog.com/entry/20110921/1316607031)
