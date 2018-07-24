---
title: python数据转换操作
tags:
  - python
url: 726.html
id: 726
categories:
  - python
  - 未分类
date: 2018-01-19 18:30:17
---

#数据并不麻烦，它们不过就是数据。如果多了，就过滤。如果不是我们要的，就映射。聚焦在数据上，摒弃费力的劳作。

#重点是捕捉 “是什么以及为什么”，而不是 “如何做”。重点是函数的定义而不是状态机的实现。

  

#List排重

i = \[1, 2, 3, 4, 5, 6, 7, 1, 2, 3, 5, 6, 9, 0\]

\# o = \[0, 1, 2, 3, 4, 5, 6, 7, 9\]

o = list( set(i) )

  

#合并List

i = \[\[1\], \[2, 3\], \[4, 5, 6\], \[1, 2, 3\]\]

\# o = \[1, 2, 3, 4, 5, 6, 1, 2, 3\]

o = sum( i, \[\] )

  

#序号化序列

i = \['a', 'b', 'c'\]

\# o = \[(0, 'a'), (1, 'b'), (2, 'c')\]

o = list( enumerate(i) )

  

#反向化序列j

i = \['a', 'b', 'c'\]

\# o = \['c', 'b', 'a'\]

o = list( reversed(i) )

  
  
  
  

#把所有非List元素转为List

i = \[\[1\], \[2, 3\], 4\]

\# o = \[\[1\], \[2, 3\], \[4\]\]

o = \[ x if type(x)==type(\[\]) else \[x,\] for x in i \]

  

#不唯一的元素

i = \[1, 2, 3, 4, 5, 6, 7, 8, 1, 3, 5, 7, 8\]

\# o = \[1, 3, 5, 7, 8, 1, 3, 5, 7, 8\]

o = \[ x for x in i if i.count(x)!= 1 \]

  

#找出List中所有长度为3的单词的所在位置

i = \['How', 'are', 'you', '?', 'Fine', '.', 'Thank', 'you', '.'\]

\# o = \[0, 1, 2, 7\]

o = \[ idx for idx, x in enumerate(i) if len(x) == 3 \]

  

#切分List

i = \[1, 'hello', 5, 6, 'world', 7\]

\# o = \[\[1, 5, 6, 7\], \['hello', 'world'\]\]

o = \[ \[ y for y in i if type(y) == x \] for x in set(\[type(x) for x in i\])\]

#反向索引

i = \[1, 1, 4, 5, 9, 6, 4\]

\# o = \[(1, \[0, 1\]), (4, \[2, 6\]), (5, \[3\]), (6, \[5\]), (9, \[4\])\]

o = \[ ( x, \[ idx for idx, y in enumerate(i) if x == y \] ) for x in set(i) \]

  
  
  
  

#取区间

i = \[2, 3, 5, 7, 11, 13, 17, 23\]

\# o = \[(2, 3), (3, 5), (5, 7), (7, 11), (11, 13), (13, 17), (17, 23)\]

o = zip( i\[:-1\], i\[1:\] )

  

#取区间2

i = \[2, 3, 5, 7, 11, 13, 17, 23\]

\# o = \[(2, 3), (5, 7), (11, 13), (17, 23)\]

o = zip( i\[::2\], i\[1::2\] )

  

#取区间3

i = \[2, 3, 5, 7, 11, 13, 17, 23\]

\# o = \[(2, 3, 5), (5, 7, 11), (11, 13, 17)\]

o = zip( i\[::2\], i\[1::2\], i\[2::2\] )

  
  
  
  
  
  

#求置换矩阵

i = \[\['a', 'b', 'c'\], \['d', 'e', 'f'\], \['g', 'h', 'i'\]\]

\# o = \[\['a', 'd', 'g'\], \['b', 'e', 'h'\], \['c', 'f', 'i'\]\]

o = zip(*i)

  

#矩阵乘法

i = \[\[1, 2, 3\], \[4, 5, 6\]\]

\# o = \[(1, 4), (1, 5), (1, 6), (2, 4), (2, 5), (2, 6), (3, 4), (3, 5), (3, 6)\]

o = \[ ( a, b ) for a in i\[0\] for b in i\[1\] \]

  

#矩阵乘法2

i = \[\[1, 2, 3\], \[4, 5, 6\], \[7, 8, 9\]\]

\# o = \[\[1, 4, 7\], \[1, 4, 8\], \[1, 4, 9\], 

\#      \[1, 5, 7\], \[1, 5, 8\], \[1, 5, 9\], 

\#      \[1, 6, 7\], \[1, 6, 8\], \[1, 6, 9\],

\#      \[2, 4, 7\], \[2, 4, 8\], \[2, 4, 9\],

\#      ...

\#      \[3, 6, 7\], \[3, 6, 8\], \[3, 6, 9\]\]

o = reduce( lambda x, y : \[ a+\[b,\] for a in x for b in y \], i, \[\[\]\] )

  
  
  
  
  

#分段落1

i = '''\

this a phase

this a phase

this

  a

  phase

this a phase

'''.splitlines()

\# o = \['this a phase', 'this a phase', 'this\\n  a\\n  phase', 'this a phase'\]

o = \[ idx for idx, l in enumerate(i) if not l.startswith('  ') \]

o = zip( o, o\[1:\]+\[None,\] )

o = \[ '\\n'.join(i\[s:e\]) for s, e in o \]

  

#分段落2

i = '''\

\[sectionA\]

itemA

  

itemB

\[sectionB\]

itemC

itemD

  

\[sectionC\]

\[sectionD\]

itemE

'''.splitlines()

\# o = {'sectionA': \['itemA', 'itemB'\], 

\#      'sectionB': \['itemC', 'itemD'\],

\#      'sectionC': \[\],

\#      'sectionD': \['itemE'\]}

o = \[ idx for idx, l in enumerate(i) if l.startswith('\[') and l.endswith('\]') \]

o = zip( o, o\[1:\]+\[None,\] )

o = dict(\[ ( i\[s\]\[1:-1\], \[ l for l in i\[s+1:e\] if l != '' \] ) for s, e in o \])

  
  
  
  

#生成替换Dict

i = (('a', 'b', 'c'), ('e', 'f', 'g'), ('h'))

\# o = {'a': 'a', 'b': 'a', 'c': 'a', 'e': 'e', 'f': 'e', 'g': 'e', 'h': 'h'}

o = dict( sum( \[ zip( x, \[x\[0\],\]*len(x) ) for x in i \], \[\] ) )

  

#合并Dict

i = \[{'a': 1, 'b': 2}, {'a': 3, 'b': 4, 'c': 6}, {'c': 7, 'd': 8}\]

\# o = {'a': \[1, 3\], 'b': \[2, 4\], 'c': \[6, 7\], 'd': \[8\]}

o = set(sum(\[ x.keys() for x in i \],\[\]))

o = dict(\[ (key,\[ x\[key\] for x in i if key in x \]) for key in o \])

  

#以'idx'为关键字合并Dict ('切分List'与'合并Dict'结合)

i = \[{'a': 1, 'b': 2, 'idx': 'hello'},

{'a': 3, 'b': 4, 'c': 6, 'idx': 'hello'},

{'c': 7, 'd': 8, 'idx': 'world'}\]

\# o = {'hello': {'a': \[1, 3\], 'b': \[2, 4\], 'c': \[6\]}, 

\#      'world': {'c': \[7\], 'd': \[8\]}}

o = set(\[ d\['idx'\] for d in i \])

ods = \[ \[ d for d in i if d\['idx'\] == k \] for k in o \]

oks = \[ set(sum(\[ d.keys() for d in od \],\[\])) for od in ods \]

ods = \[ dict(\[ ( k, \[ d\[k\] for d in od if k in d \] ) for k in ok if k!='idx'\]) 

   for od, ok in zip(ods, oks) \]

o = dict( zip( o, ods ) )