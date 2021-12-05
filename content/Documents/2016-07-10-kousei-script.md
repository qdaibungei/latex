---
layout: post
title: 校正割り振りスクリプト
date: 2016-07-10
tags: ["Python"]
---

# はじめに
校正割り振りスクリプトをつくった。校正割り振りっていうより、「山分け問題」の解決をするスクリプトって言ったほうがいいか。というかこういう問題ってふつうどう呼ばれているのだろう。検索してもいまいちこれといったものが見つからなかった。「山分け問題」もしくは「分配問題」と呼んでいる本がgoogle booksで見つかったが、これが一般的呼称なのは分からない。


# 校正割り振りスクリプト
## 背景
* 文藝サークルでは、文集や部誌を発行するさい、校正係を置いている。
* ここで校正係とは、部員の原稿を事前に読み、誤字脱字などをチェックする係である。
* 各校正係は、すべての原稿に目を通す必要はなく、自分の担当原稿のみに目を通せばよい（各人がすべての原稿に目を通すのは非効率的であるため）。


## 問題
* 校正係へ原稿を割り振るとき、各人が読まねばならぬ原稿の文字数をできるかぎり揃えたい。
* しかし、これを手作業で揃えようとすると、面倒である。


## 解法
以下に示すpythonスクリプトを書いた。すべての可能な組み合わせを考えて、標準偏差を出し、標準偏差の最も低い組み合わせを出力するようにしてある。

```python
# -*- coding: utf-8 -*-
import math
import itertools


list1 = [1011,8595,5064,4978,6751,108,30218,732,'|','|','|']

junretu = [i for i in itertools.permutations(list1)] # すべての順列をひたすら出力
goodp = [] # good permitation
listhozon = [] # list保存

# 標準偏差 standard deviation
sd = -1

for p in junretu:
    list2 = []
    p = list(p)
    i = [i for (i, x) in enumerate(p) if x == '|'] # 区切り位置の取得

    # 取得した区切り位置を元に数字の組み合わせのリストを作る
    c = 0
    for x in i:
        list2.append(p[c:x])
        c = x+1
    else:
        p.append('dummy')
        list2.append(p[c:-1])

    # 空のリストを持っているかどうか判定
    kara = 0
    for x in list2:
        if x == []:
            kara = 1
            break
    if kara == 0:
        #print list2
        list3 = []
        total = 0
        for x in list2:
            for y in x:
                total += y
            list3.append(total)
            total= 0

        #print list3

        ave = sum(list3)/len(list3)
        total = 0
        for i in range(len(list3)):
            total += (list3[i] - ave)**2
        if sd == -1:
            sd = math.sqrt(total/len(list3))
            goodp = p
            listhozon = list3
        elif sd > math.sqrt(total/len(list3)):
            sd = math.sqrt(total/len(list3))
            goodp = p
            listhozon = list3
del goodp[-1]
print sd
print goodp
print listhozon
```


# 追記
流石に適当すぎて、リストが多すぎるとすぐフリーズしてしまうことが判明した。やっぱりそうか。悲しい。メモリを食い過ぎてしまうのだ。改善したいがなかなか資料が見つからない。[ここ](http://pythonlife.seesaa.net/article/243207369.html)などが参考になるかもしれない。


# さらに追記
pythonスクリプトに渡すリストの要素数が10以下ならば、フリーズしないことが経験的にわかった。用法を守って使えばまずまず使えるソフトだ。
