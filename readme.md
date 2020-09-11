
# Markdown语法

[TOC]

## 标题

N个‘#’开头就是N级标题，标题需要上下由空格包围，'\#' 与标题之间需要有空格

## 字体

*斜体用一对 \*或一对 _；*
**加粗用一对 \*\* 或一对 __；**  

***斜体加粗用一对 \*\*\* 或一对 \_\_\_；***  

~~删除线用一对 \~\~；~~

<u>下划线用 \<u\>content\</u\></u>

## 引用

> 第一级引用
> > 第二级引用
> >
> > > N个\>就是N级引用

## 分割线

---

通过 **---**，**\*\*\***（三个及以上）可以作为分割线，显示效果相同，但是会强制性从标题开始，且分割线不能用作结尾

---

## 图片

![earth](D:\Wallpaper\main\EARTH.png)  
通过 **\!\[title\]\(url/addr\)** 进行图片引用

## 超链接

[Baidu](https://www.baidu.com/)  
通过 **\[title\]\(url\)** 进行超链接引用

## 列表

+ 无序列表
  + 通过 **\- , \+,  \*** （统一使用同一个，否则算是不同列表）与空格，后面紧随内容，列表需要上下由空格包围
    + 数字
    + 中文
    + 英文
+ 有序列表
  + 通过数字加点，形如 **'num.content'** 的形式建立有序列表，点与内容间需要有空格
    	1. one
     	2. two
     	3. three
+ 列表嵌套
  + 上下级列表之间需要2个空格缩进
    + 类型
      - 数字
        1. 1
        2. 2
        3. 3
      - 中文
        - 一
        - 二
        - 三
      - 英文
        1. one
        2. two
        3. three

## 表格

通过如下格式实现：

|表头|表头|表头|  
| --- :|: --- :|: --- :| 
|内容|内容|内容|  
|内容|内容|内容|  

其中的 **---** 只是占位，实际上有一个就够了，在 **---** 的左边添加 **:** 说明文字左对齐，反之右对齐，若两边都有则居中，默认左对齐，例子如下

Test left|Test middle|Test right
--|:--:|--:
Left alignment|Middle alignment|Right alignment

## 代码

单行代码使用一对 **\`** 将代码括起来，多行则使用一对 **\`\`\`** ，且需独立成行，例子如下：  

`import this`

```python
import random  
import numpy as np
```

## 公式

单行使用一对 **'\$'**，多行使用一对 **'\$\$'**，例子如下：

$e^{i\pi}+1=0$



$$
sgn(x)=\begin{cases}
1&,&x>0\\
0&,&x=0\\
-1&,&x<0\\
\end{cases}\\

\vec{\nabla}\times\vec{F}=\left|\begin{array}\\
\vec{i} & \vec{j} & \vec{k}\\
\frac{\partial}{\partial x} & \frac{\partial}{\partial y} & \frac{\partial}{\partial z}\\
P & Q & R\\
\end{array}\right|\\

\int_{a}^{b}{f(x)}dx=F(b)-F(a)\\

\lim_{n\rightarrow+\infty}{\frac{F(t+\Delta t)-F(t)}{\Delta t}}=f'(t)\\

\sum_{n=1}^{\infty}{\frac{1}{n^2}}=\frac{\pi ^2}{6}\\

\prod_{i=1}^{n}{i}=n!\\

\vec{F}=q\vec{v}·\vec{B}
$$

