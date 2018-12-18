---
date: 2018-10-29 17:30
status: public
title: Markdown笔记
categories: Linux
tags: Markdown
---

　　　　　　　　　　　　　　　　　　　　　Written by xblin

![logo](http://wx4.sinaimg.cn/large/007fPWmPly1fy41o22i1gj3074074q3p.jpg)
## 标题

```
# 一级标题
## 二级标题
### 三级标题
#### 四级标题
##### 五级标题
###### 六级标题 
```
>注：# 和「一级标题」之间建议保留一个字符的空格，这是最标准的 Markdown 写法

## 代码注释
#### 1.单行注释
`code `
#### 2.多行注释

``` C++
#include <iostream>
int main(void){
    count <<"hello world!"<<endl;
}
```

## 列表
#### 1.无序列表
```
- 文本1
- 文本2
- 文本3
```
output：
- 文本1
- 文本2
- 文本3

#### 2.有序列表
```
1. 文本1
2. 文本2
3. 文本3
```

## 表格
```
标题1 | 标题2 | 标题3 |
----- | :---- | ----: |
lab1  | lab2  | lab3  |

注：    :---- 为左对齐 ， ----:为右对齐
```
output:

标题1 | 标题2 | 标题3 |
----- | :---- | ----: 
lab1  | lab2  | lab3  


## 下划线
```
----
```
output:

----


## 空格和换行
####空格
1.按shift + space 切换为全角模式，输入空格就有效
2.
```
&bsp; aa
```
### 换行 
```
<br/>
下行文本
```
output:
<br>
aa

----
