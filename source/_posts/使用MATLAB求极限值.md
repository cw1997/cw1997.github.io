---
title: 使用MATLAB求极限值
date: 2018-12-03 00:05:45
tags:
categories: math
---

# 前言

我们在做高等数学题目时，需要验证自己答案的正确性，我们可以使用`MATLAB`来自动化求解极限值

# syms

首先要使用 `syms` 定义一个符号变量，你也可以理解为数学中的一些未知数，例如x。

# limit

这是求极限的函数，我们可以输入 `help limit` 来查看它的函数原型和参数意义

```matlab
>> help limit
--- sym/limit 的帮助 ---

 limit    Limit of an expression.
    limit(F,x,a) takes the limit of the symbolic expression F as x -> a.
    limit(F,a) uses symvar(F) as the independent variable.
    limit(F) uses a = 0 as the limit point.
    limit(F,x,a,'right') or limit(F,x,a,'left') specify the direction
    of a one-sided limit.
 
    Examples:
      syms x a t h;
 
      limit(sin(x)/x)                 returns   1
      limit((x-2)/(x^2-4),2)          returns   1/4
      limit((1+2*t/x)^(3*x),x,inf)    returns   exp(6*t)
      limit(1/x,x,0,'right')          returns   inf
      limit(1/x,x,0,'left')           returns   -inf
      limit((sin(x+h)-sin(x))/h,h,0)  returns   cos(x)
      v = [(1 + a/x)^x, exp(-x)];
      limit(v,x,inf,'left')           returns   [exp(a),  0]

    sym/limit 的参考页
```

可以看到

`limit(F,x,a) takes the limit of the symbolic expression F as x -> a.`

`F`表示要求极限的函数，`x`表示自变量，`a`表示自变量`x`趋向于的值，可以为任意数，或者无穷，但是无穷要使用`inf`来代替

`limit(F,x,a,'right') or limit(F,x,a,'left') specify the direction of a one-sided limit.`

而第四个参数如果输入`'left'`或者`'right'`，则表示求**左极限**或者**右极限**。**这里要注意参数类型是string，要用单引号包裹起来。**

# 实例

例如我求 $\lim\limits_{n \to \infty }{\frac{x-sin\ x}{x}} $ 的极限，可以这样操作

```matlab
>> syms x
>> limit((x-sin(x))/x,x,inf)
 
ans =
 
1
```

首先声明`符号变量x`，然后执行`limit`即可得到极限值为`1`

