## 数值计算基础

### 泰勒级数

$$
\sum_{n=0}^{\infty }{\frac {f^{(n)}(a)}{n!}}(x-a)^{n}
$$

### 牛顿法

$$
x_{n+1}=x_{n}-{\frac{f(x_{n})}{f'(x_{n})}}
$$

**答疑一**：使用牛顿法求$$a$$的平方根。

$$\because x=\sqrt{a}$$

$$\therefore x^2-a=0$$

设$$f(x)=x^2-a$$，则$$f'(x)=2x$$

$$
\begin{aligned}
& \therefore x_{n+1}=x_{n}-{\frac{f(x_{n})}{f'(x_{n})}}={\frac{x_n}{2}}+{\frac{a}{2x_n}}
\end{aligned}
$$

其中，$$x_0=a$$

### 拉格朗日插值法

对某个多项式函数，已知有给定的$$k+1$$个取值点：$$(x_{0},y_{0}),\ldots ,(x_{k},y_{k})$$，其中$$x_{j}$$对应着自变量的位置，而$$y_{j}$$对应着函数在这个位置的取值。

假设任意两个不同的$$x_j$$都互不相同，那么应用拉格朗日插值公式所得到的拉格朗日插值多项式为：

$$
L(x)=\sum_{j=0}^{k} y_{j}\ell_{j}(x)
$$

其中每个$$\ell _{j}(x)$$为拉格朗日基本多项式（或称插值基函数），其表达式为：

$$
\ell_{j}(x)=\prod_{i=0,\,i \neq j}^{k}{\frac{x-x_{i}}{x_{j}-x_{i}}}={\frac{(x-x_{0})}{(x_{j}-x_{0})}}\cdots {\frac{(x-x_{j-1})}{(x_{j}-x_{j-1})}}{\frac{(x-x_{j+1})}{(x_{j}-x_{j+1})}} \cdots {\frac{(x-x_{k})}{(x_{j}-x_{k})}}
$$

拉格朗日基本多项式$$\ell_{j}(x)$$的特点是在$$x_{j}$$上取值为1，在其它的点 $$x_{i},\,i \neq j$$上取值为0。

**答疑一**：使用拉格朗日插值法求插值函数。

假设有某个二次多项式函数$$f$$，已知它在三个点上的取值为：$$f(4)=10$$，$$f(5)=5.25$$，$$f(6)=1$$，求$$f(18)$$的值。

首先写出每个拉格朗日基本多项式：

$$
\begin{aligned}
    & \ell_{0}(x)={\frac{(x-5)(x-6)}{(4-5)(4-6)}} \\
    & \ell_{1}(x)={\frac{(x-4)(x-6)}{(5-4)(5-6)}} \\
    & \ell_{2}(x)={\frac{(x-4)(x-5)}{(6-4)(6-5)}}
\end{aligned}
$$

然后应用拉格朗日插值法，就可以得到$$p$$的表达式（$$p$$为函数$$f$$的插值函数）：

$$
\begin{aligned}
    p(x)&=f(4)\ell _{0}(x)+f(5)\ell_{1}(x)+f(6)\ell_{2}(x) \\
    &=10 \cdot {\frac{(x-5)(x-6)}{(4-5)(4-6)}}+5.25 \cdot {\frac{(x-4)(x-6)}{(5-4)(5-6)}}+1 \cdot {\frac{(x-4)(x-5)}{(6-4)(6-5)}} \\
    &={\frac{1}{4}}(x^{2}-28x+136)
\end{aligned}
$$

此时代入数值$$18$$就可以求出所需之值： $$f(18)=p(18)=-11$$。
