# [DeepSeaSpray's Blog](index.html)

> [Blog List](list.html)

## [李超线段树](article/article5.html)

> 2023年09月21日 / 信息学 / 算法

### 问题引入

在一个平面直角坐标系中，需要在线维护两种操作。

- 加入一条线段
- 给出对于一条直线$x=a$求所有与之相交的线段中交点最高（或低）的一条

![image1](https://z1.ax1x.com/2023/09/20/pPIeqG8.png)

如图，红线$y=x+2 (1 \leq x \leq 4)$,蓝线$y=2x+1 (-1 \leq x \leq 5)$和紫线$y=9 (0 \leq x \leq 2)$为加入的三条线段，现在查询为绿线$x=3$，在与绿线有交点的线段中蓝线交点最高，所以答案为蓝线。

[Article Link](article/article5.html)

## [2023多校赛第一场](article/article4.html)

> 2023年09月08日 / 信息学 / 比赛

### 赛时

注:题目在HDUOJ上都可以找到

时间:12:00~17:00,故没睡午觉

队友:DengDuck,彬彬

中午吃饭还迟到了.

先乱翻了一下题,DengDuck切了1009,彬彬切了1002,我做了1005.

然后就不会了,DengDuck主攻1001,彬彬主攻1010,我乱看,搞机会主义.

尽管DengDuck,彬彬都想出了正解,但都没打完,我看了1012,推出了一半,不会换根DP祭天.

[Article Link](article/article4.html)

<br>

## [HDU-2065 "红色病毒"问题](article/article3.html)

> 2023年09月08日 / 信息学 / 题解 

题目链接:[http://acm.hdu.edu.cn/showproblem.php?pid=2065](http://acm.hdu.edu.cn/showproblem.php?pid=2065)

### 题目大意
有`A`,`B`,`C`,`D`四种字符,其中字符`A`,`C`只能出现偶数次,其他字符没有要求.给出一个$N$要求按照上述规则构造一个字符串,求方案数$\pmod{100}$,其中$1 \leq N \leq 2^{64}$.

[Article Link](article/article3.html)

<br>

## [快速傅里叶变换 FFT](article/article2.html)

> 2023年09月08日 / 信息学 / 算法

### 前置知识
向量,复数的运算.

### 多项式

#### 次数界
如果一个多项式$f(x)=a_{0}+a_{1}x+a_{2}x^2+\dots+a_{n}x^{n}$,那么$f$的次数界就是$n+1$.换句话说,次数界为$n$的多项式最高次项是$x^{n-1}$

#### 多项式的表示方法

##### 系数表示法
一个次数界为$n$的多项式最多有$n$个系数,这些系数一旦确定,多项式也随之确定.

##### 点值表示法
一个次数界为$n$的多项式,如果知道上面的$n$个点(即$n$对$x$与$f(x)$),那么多项式也随之确定.

[Article Link](article/article2.html)

<br>

## [DeepSeaSpray的GDKOI游记](article/article1.html)

> 2023年09月08日 / 信息学 / 比赛

### Day -5 to -2

连续打了很多天的模拟赛.

### Day -1 to 0

复习了动态规划,数据结构,字符串算法和数论.

### Day 0

下午从学校出发去广州,见识到了很多不一样的东西.先去了酒店办入住,然后去广州六中报到.晚上去了广州塔和珠江(买了一个纪念币,后来送人了),最后回酒店又复习了一下,就睡觉了.

[Article Link](article/article1.html)
