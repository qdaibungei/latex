---
layout: post
title: LaTeXによる小説組版法・中級編――ベスト文集制作記録⑥
date: 2020-04-08
tags: ["LaTeX", "guide"]
---

# 本文の組み方④・テキストからTeXへ変換する篇
## ダッシュ
倍角ダッシュや波ダッシュは、[jdash.sty]({{< ref "2017-02-17-jdash.md" >}})を用いてマクロ定義してある。このマクロを使うため、次のように置換処理を行なう。

```python
jdashes = ['—','―','─']
wdashes = ['〜','～']
def jdash_replace(x):
    for jdash in jdashes:
        if x.find(jdash) > -1:
            for y in range(30,2,-1):
                x = x.replace(jdash*y,'{\\jdash['+str(y)+']'+'}')
            x = x.replace(jdash*2,'\\――')
    return x
def wdash_replace(x):
    for wdash in wdashes:
        if x.find(wdash) > -1:
            for y in range(20,1,-1):
                x = re.sub(wdash*y,('\\'+wdash)*y,x)
    return x
```

## 濁点マクロ
例えば、平仮名の「な」に濁点を打ちたくなることがある。このような場合には濁点マクロを作っておくと便利だ。

### LaTeXでのマクロ定義
```latex
%% dakutens
\newcommand{\dakuten}[1]{%
    \jghostguarded{%
        \leavevmode\hbox to 1zw{%
            \rensuji{\hbox to 1zw{#1\hspace*{-0.25zw}゛}}%
        }%
    }%
}
\newcommand{\handakuten}[1]{%
    \jghostguarded{%
        \leavevmode\hbox to 1zw{%
            \rensuji{\hbox to 1zw{#1\hspace*{-0.25zw}゜}}
        }%
    }%
}
```

### Pythonでの置換処理
```python
def dakuten_replace(x):
    x = re.sub(r'([ぁ-ヴ])゛', r'\\dakuten{\1}', x)
    x = re.sub(r'([ぁ-ヴ])゜', r'\\handakuten{\1}', x)
    return x
```

## 圏点
「カクヨム」などの小説投稿サイトでは、`《《圏点》》`という記法で圏点を入力することができる。この記法を自動でLaTeXマクロへと置換できるようにしておくと都合がよい。

```python
def kakuyomu_replace(x):
    x = re.sub(r'《《(.+?)》》', r'\\kenten{\1}', x)
    return x
```

## 段落を区切る
LaTeX記法では、段落を区切るためには段落間に空行を挿入せねばならない。そこで次のような置換を行なう。

```python
honbun = 0
def txt_replace(x):
    # 段落を区切る
    x = x.replace('\r\n','\n')
    x = x.replace('\n','\n\n')
    # 空行の挿入
    global honbun
    if re.search('^\n',x) == None:
        honbun += 1
    if honbun > 3:
        x = re.sub('^\n',r'\\gyoaki\n',x)
    return x
```

## Pythonスクリプトの本体
```python
# -*- coding: utf-8 -*-
from __future__ import unicode_literals
import os,sys,codecs,re
import nkf
from tqdm import tqdm
cwd = os.getcwd()
argv = sys.argv
pre_document = '''\\documentclass[a5paper,papersize,autodetect-engine]{jsbook}
\\input{settings}
\\begin{document}
'''
post_document = '''\\end{document}
'''
def open_close(f_in_name,f_out_name):
    txt = open(f_in_name).read()
    f_in = codecs.open(f_in_name,'r',nkf.guess(txt))
    f_out = codecs.open(f_out_name,'w','utf-8')
    lines = f_in.readlines()
    lines2 = []
    global honbun
    # for x in lines:
    for x in tqdm(lines):
        x = kakuyomu_replace(x)
        if x.find('《' or '》') > -1:
            x = ruby_replace(x)
        x = aozora_replace(x)
        x = txt_replace(x)
        lines2.append(x)
    honbun = 0
    lines2.insert(0,pre_document)
    lines2.append(post_document)
    f_out.write(''.join(lines2))
    f_in.close()
    f_out.close()

def main():
    if len(argv) == 1:
        print 'input any file name'
    elif len(argv) > 3:
        print 'too many file names'
    else:
        if len(argv) == 2:
            if os.path.isfile(argv[1]):
                f_in_name = cwd+'/'+argv[1]
                f_out_name = re.sub('\.txt$','.tex',cwd+'/'+argv[1])
                open_close(f_in_name,f_out_name)
            else:
                for file in os.listdir(cwd+'/'+argv[1]):
                    f_in_name = cwd+'/'+argv[1]+'/'+file
                    f_out_name = re.sub('\.txt$','.tex',cwd+'/'+file)
                    open_close(f_in_name,f_out_name)
        elif len(argv) == 3:
            if os.path.isfile(argv[1]):
                print argv[1]
                f_in_name = cwd+'/'+argv[1]
                f_out_name = cwd+'/'+argv[2]
                open_close(f_in_name,f_out_name)
            else:
                for file in os.listdir(cwd+'/'+argv[1]):
                    print file
                    f_in_name = cwd+'/'+argv[1]+'/'+file
                    f_out_name = re.sub('\.txt$','.tex',cwd+'/'+argv[2]+'/'+file)
                    open_close(f_in_name,f_out_name)

if __name__ == '__main__':
    main()
```

解説は面倒になってきたのでまたしても割愛。

　

（次回に続く）
