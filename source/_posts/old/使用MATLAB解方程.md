---
title: 使用MATLAB解方程
date: 2018-12-03 00:19:31
tags:
categories: math
---

# 前言

上一个博客（ https://blog.changwei.me/2018/12/03/%E4%BD%BF%E7%94%A8MATLAB%E6%B1%82%E6%9E%81%E9%99%90%E5%80%BC/ ）我们提到了如何使用`MATLAB`求极限值。

这篇文章我们将讲解如何使用`MATLAB`解方程。

我们平时做物理题，使用回路电流法求电路的电压电流时，有多少个回路就要列写多少个方程。

碰到回路多的电路，普通工程计算器甚至都无法完成，这个时候就需要强大的`MATLAB`的`solve`函数来帮我们解方程组了。

# solve

输入`help solve`获取函数的函数原型和参数意义

```matlab
>> help solve
solve - Equations and systems solver

    This MATLAB function solves the equation eqn for the variable var.

    S = solve(eqn,var)
    S = solve(eqn,var,Name,Value)
    Y = solve(eqns,vars)
    Y = solve(eqns,vars,Name,Value)
    [y1,...,yN] = solve(eqns,vars)
    [y1,...,yN] = solve(eqns,vars,Name,Value)
    [y1,...,yN,parameters,conditions] = solve(eqns,vars,'ReturnConditions',true)

    另请参阅 dsolve, isolate, linsolve, root, subs, symvar, vpasolve

    solve 的参考页
```

这里要着重注意，`solve`如果求解的是多元方程组，返回的则是一个struct结构体类型（类似于python中的list），也就是包含多个值得数据类型，所以有几个未知数，我们就要使用syms声明多少个未知数，并且需要让`solve`赋值给由这些未知数组成的struct（类似于python中的list）

然后eqns就是方程组了，也要使用方括号包裹起来，类似于python中的list，vars是未知数，也要写成struct类型并且使用方括号包裹起来。实际测试发现不用包裹也可以，当然推荐还是写成struct类型，比较规范。

**还有一个要注意的地方，就是在`MATLAB`中，单等于号`=`是赋值作用，而双等于号`==`才是相等，和C系列编程语言一样。**

# 实例

求解一个七元一次方程

首先使用clear命令清除环境中已经存在的变量，防止被干扰，然后输入如下命令即可

```matlab
>> clear
>> syms i1 i2 i3 i4 i5 i6 vx
>>  [i1,i2,i3,i4,i5,i6,vx] = solve([12+(2*i1-i4)*1000==0,12+(2*i2-i3-i5)*1000==0,2*vx+(2*i4-i1+i5)*1000==0,i3==6/1000,(2*i5-i2+i4)*1000==vx+(i6+i3)*1000,i5+i6==2*(i4+i5),i6*1000==vx],[i1,i2,i3,i4,i5,i6,vx])
 
i1 =
 
-81/12500
 
 
i2 =
 
-39/12500
 
 
i3 =
 
3/500
 
 
i4 =
 
-3/3125
 
 
i5 =
 
-3/12500
 
 
i6 =
 
-27/12500
 
 
vx =
 
-54/25
 
>> 
```

可以看到七个元分别被正确求出