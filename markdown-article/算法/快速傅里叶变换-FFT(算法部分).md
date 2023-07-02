# 快速傅里叶变换-FFT(算法部分)

## 前置知识
向量,复数的运算.

## 多项式

### 次数界
如果一个多项式$f(x)=a_{0}+a_{1}x+a_{2}x^2+\dots+a_{n}x^{n}$,那么$f$的次数界就是$n+1$.换句话说,次数界为$n$的多项式最高次项是$x^{n-1}$

### 多项式的表示方法

#### 系数表示法
一个次数界为$n$的多项式最多有$n$个系数,这些系数一旦确定,多项式也随之确定.

#### 点值表示法
一个次数界为$n$的多项式,如果知道上面的$n$个点(即$n$对$x$与$f(x)$),那么多项式也随之确定.

### 多项式的运算

#### 加减法
如果是系数表示法,我们直接用系数相加减即可.如果是点值表示法,我们也可以直接用点值相加减.

对于上面两种方法时间复杂度都是$O(n)$.

#### 乘法
如果是系数表示法,我们需要在两个多项式中分别取值相乘再相加.如果是点值表示法,我们可以直接将点值相乘(当然,需要取$n+m-1$个点,其中$n$,$m$为两个多项式的次数界).

对于上面两种方法,时间复杂度分别是$O(n^{2})$和$O(n)$.

尽管点值表示法乘法的时间复杂度非常小,但是我们一般都是使用系数表示法,所以我们要想办法优化系数表示法下乘法的时间复杂度.直接优化没有想法,所以我们思考能否将系数表示法转换为点值表示法(这一过程叫取值),相乘,再转换回去(这一过程叫插值).

对于取值过程,因为我们一共要取$n$个值,每个值计算的时间复杂度又是$O(n)$,所以取值的时间复杂度是$O(n^{2})$的.

对于插值过程,因为我们一共有$n$个值,带入得到$n$个系数方程花费$O(n^{2})$的时间复杂度,再用高斯消元解出方程花费$O(n^{2})$的时间复杂度.或者直接使用拉格朗日插值法,但也需要$O(n^{2})$的时间复杂度.

那么总的时间复杂度并没有变少,怎么办呢?为了解决这个问题,快速傅里叶变换-FFT应运而生.

## 单位根
开始之前,请先了解一下单位根.

### 定义

将一个复平面上的单位圆分成$n$份($n$为$2$的正整数次幂),那么我们分出来的第一个点就是$\omega_{n}^{1}=\cos{\dfrac{2\pi}{n}}+i \sin{\dfrac{2\pi}{n}}$我们称之为$n$次单位根.根据复数的运算不难得出第$k$个点就是$\omega_{n}^{k}=\cos{k\dfrac{2\pi}{n}}+i \sin{k\dfrac{2\pi}{n}}$.

### 性质

$\omega_{n}^{k}=\omega_{2n}^{2k}$证明如下(当然也可以从几何层面理解):
$$
\begin{aligned}
    \omega_{2n}^{2k}&=\cos 2k\dfrac{2\pi}{2n}+i \sin 2k\dfrac{2\pi}{2n} \\
    &=\cos k\dfrac{2\pi}{n}+i \sin k\dfrac{2\pi}{n} \\
    &=\omega_{n}^{k}
\end{aligned}
$$

$\omega_{n}^{k+\frac{n}{2}}=-\omega_{n}^{k}$证明如下(当然也可以从几何层面理解):
$$
\begin{aligned}
    \omega_{n}^{\frac{n}{2}}&=\cos \dfrac{n}{2}\times\dfrac{2\pi}{n}+i\sin \dfrac{n}{2} \times \frac{2\pi}{n} \\
    &=\cos \pi + i \sin \pi \\
    &=-1 \\
\end{aligned}
$$
等式两边同时乘以$\omega_{n}^{k}$得$\omega_{n}^{k+\frac{n}{2}}=-\omega_{n}^{k}$
为了方便表达,我们姑且叫它`单位根的对称性`(自创叫法).

## 快速傅里叶变换-FFT
设:
$$
\begin{aligned}
    f_{1}(x)&=a_{0} + a_{2}x + \dots a_{n}x^{\frac{n}{2}} \\
    f_{2}(x)&=a_{1} + a_{3}x + \dots a_{n-1}x^{\frac{n}{2}-1} \\
    f(x)&=a_{0} + a_{1}x + \dots a_{n}x^{n} \\
    &=(a_{0} + a_{2}x^{2} + \dots a_{n}x^{n})+(a_{1}+a_{3}x^{3}+\dots + a_{n-1}x^{n-1}) \\
    &=f_{1}(x^{2})+xf_{2}(x^{2})
\end{aligned}
$$
将$\omega_{n}^{k}$代入:
$$
\begin{aligned}
    f(\omega_{n}^{k})=f_{1}(\omega_{n}^{2k})+\omega_{n}^{k}f_{2}(\omega_{n}^{2k})
\end{aligned}
$$
将$\omega_{n}^{k+\frac{n}{2}}$代入:
$$
\begin{aligned}
    f(\omega_{n}^{k+\frac{n}{2}})&=f_{1}(\omega_{n}^{2k+n})+\omega_{n}^{k+\frac{n}{2}}f_{2}(\omega_{n}^{2k+n}) \\
    &=f_{1}(\omega_{n}^{2k} \times \omega_{n}^{n})-\omega_{n}^{k}f_{2}(\omega_{n}^{2k} \times \omega_{n}^{n}) \\
    &=f_{1}(\omega_{n}^{2k})-\omega_{n}^{k}f_{2}(\omega_{n}^{2k})
\end{aligned}
$$ 

观察到两个式子只有一个常数上的差异,所以我们成功的把问题的规模缩小了一半,并且拆分成的问题仍然满足原先问题的所有性质.我们可以用分治来解决,时间复杂度成功缩小到$O(n\log_{2}n)$

## 快速傅里叶逆变换-IFFT
上面的做法解决了快速求值的问题,接下来我们要解决快速插值的问题.

求值运算可以用如下矩阵表示:

$$
\begin{pmatrix}
    & \omega_{0}^{0} & \omega_{0}^{1} & \cdots & \omega_{0}^{n} \\
    & \omega_{1}^{0} & \omega_{1}^{1} & \cdots & \omega_{1}^{n} \\
    & \vdots & \vdots & \ddots &
    \vdots \\
    & \omega_{n}^{1} & \omega_{n}^{2} & \cdots & \omega_{n}^{n}
\end{pmatrix}
\begin{pmatrix}
    a_{0} \\
    a_{1} \\
    \vdots \\
    a_{n}
\end{pmatrix}=
\begin{pmatrix}
    b_{0} \\
    b_{1} \\
    \vdots \\
    b_{n}
\end{pmatrix}
$$

其中$a$为乘积多项式的系数,$b$为FFT所求得的值.

所以,要求$a$我们只需要求出$\omega$的逆矩阵再乘上$b$就好了.那么:
$$
\begin{pmatrix}
    a_{0} \\
    a_{1} \\
    \vdots \\
    a_{n}
\end{pmatrix}=
\begin{pmatrix}
    & \dfrac{\omega_{0}^{-0}}{n} & \dfrac{\omega_{0}^{-1}}{n} & \cdots & \dfrac{\omega_{0}^{-n}}{n} \\
    & \dfrac{\omega_{1}^{-0}}{n} & \dfrac{\omega_{1}^{-1}}{n} & \cdots & \dfrac{\omega_{1}^{-n}}{n} \\
    & \vdots & \vdots & \ddots &
    \vdots \\
    & \dfrac{\omega_{n}^{-1}}{n} & \dfrac{\omega_{n}^{-2}}{n} & \cdots & \dfrac{\omega_{n}^{-n}}{n}
\end{pmatrix}
\begin{pmatrix}
    b_{0} \\
    b_{1} \\
    \vdots \\
    b_{n}
\end{pmatrix}
$$

其中除以$n$可以放到最后去除,并且
$$
\begin{aligned}
    \omega_{n}^{-1}&=\cos{-\dfrac{2\pi}{n}}+i \sin{-\dfrac{2\pi}{n}} \\
    &=\cos{\dfrac{2\pi}{n}}-i\sin{\dfrac{2\pi}{n}}
\end{aligned}
$$

## 代码实现

一般来说有两种实现方法,一种是用递归,另一种是用循环.显然,循环的做法更为优秀,所以这里只介绍循环做法.

我们先来观察一下我们分类完之后的序列:
```plain
1 2 3 4
2 4|1 3
4|2|3|1 (10进制)
100|010|011|001 (2进制)
001|010|110|100 (2进制左右翻转)
```
不难发现其中的规律就是按照二进制左右翻转后的大小排序.

### 代码示例
> 详解请看注释

```cpp
#include<bits/stdc++.h>
using namespace std;

#define cd complex<double>
const int maxn=10*1e6;

int n,m;
cd a[maxn+5],b[maxn+5];

int tp=1,ltp;
int r[maxn+5];

inline void fft(cd f[],int typ)
{
    //求出按翻转值排序后的序列(注意到i=r[r[i]])
	for(int i=0;i<tp;i++)
		if(i<r[i])
			swap(f[i],f[r[i]]);

	for(int i=1;i<tp;i<<=1)
	{
		cd wn(cos(M_PI/i),typ*sin(M_PI/i));//单位根
		int tmp=i<<1;//合并区间的长度
		for(int j=0;j<tp;j+=tmp)//合并区间的开头
		{
			cd w(1,0);//带入的值
			for(int k=0;k<i;k++)//枚举区间前一半
			{
				cd x=f[j+k],y=w*f[i+j+k];
				f[j+k]=x+y;
				f[i+j+k]=x-y;//单位根的对称性
				w*=wn;
			}
		}
	}
}

signed main()
{
	int ix;
	scanf("%d%d",&n,&m);
	for(int i=0;i<=n;i++)
		scanf("%d",&ix),a[i].real(ix);
	for(int i=0;i<=m;i++)
		scanf("%d",&ix),b[i].real(ix);
	
    //注意至少需要取n+m+1个值
	while(tp<=n+m)
		tp<<=1,ltp++;
    
    //利用动态规划快速求出翻转后的值
	for(int i=0;i<tp;i++)
		r[i]=(r[i>>1]>>1)|((i&1)<<(ltp-1));
	
    //取值
	fft(a,1),fft(b,1);
	for(int i=0;i<tp;i++)
		a[i]*=b[i];

    //插值
	fft(a,-1);
	
    //注意四舍五入
	for(int i=0;i<=n+m;i++)
		printf("%d ",(int)(a[i].real()/tp+0.5));
	
	return 0;
}
```

## 后记
古语有云:"教学相长",我也是在写文章的时候菜对`FFT`有了更深刻的理解.
接下来还会出关于`FFT`的一些题目选讲,以及上文中一笔带过的`范德蒙矩阵`的求逆过程.
