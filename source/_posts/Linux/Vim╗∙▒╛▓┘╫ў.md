---
date: 2018-10-29 09:57
status: public
title: Vim基本操作
categories: Linux
tags: Linux
---

####1.方向移动（hjkl代替方向键移动）tytal
```
  k
h    l    
  j
```

####2.插入编辑
```
i  a  A  o 
```

####3.删除，替换，复制粘贴

操作| 命令                 |
----|----------------------|
删除| x  dw  de  d$ dd  2dd
替换| rx  R                
复制| v....v +y   or  yy   
粘贴| p                    

####4.移动到home和end

```
home: 0
end:  $
start： gg
ended： G
查看文本信息： CTRL+G
回到前一次编辑的地方： '0
```

####5.撤销和反撤销

```
撤销：   u
反撤销： CTRL-R
```
####6.查找和替换

```
设置显示行号： set nu
查找 ： /xxx
查找下(上)一个： n  N

显示该字的关键字： *
取消高亮显示： nohls
高亮显示： hls

替换：
%s/old/new/g
```
####7.自动补全

`CTRL-P`

####8.写入文件 or 文件另存为

`:w filename`


----
