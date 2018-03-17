## 数值计算

### 泰勒级数

$$
\sum _{n=0}^{\infty }{\frac {f^{(n)}(a)}{n!}}(x-a)^{n}
$$

### 牛顿法

$$
x_{{n+1}}=x_{n}-{\frac {f(x_{n})}{f'(x_{n})}}
$$

**答疑一**：使用牛顿法求$$a$$的平方根。

$$\because x=\sqrt{a}$$

$$\therefore x^2-a=0$$

设$$f(x)=x^2-a$$，则$$f'(x)=2x$$

$$
\therefore x_{{n+1}}=x_{n}-{\frac{f(x_{n})}{f'(x_{n})}}={\frac{x_n}{2}}+{\frac{a}{2x_n}}
$$

其中，$$x_0=a$$