方程分类

* 未知量为数字：代数、分式、丢番图、差分
* 未知量为函数：曲线/面、微分、积分



常微分方程

解方程基本步骤



**基本积分式**
$$
\large{
    \int \frac{dx}{\sqrt{a^2-x^2}} = \arcsin(\frac{x}{a}) + C = -\arccos(\frac{x}{a}) + C
    \\
    \int \frac{dx}{a^2+x^2} = \frac1a\arctan(\frac{x}{a}) + C = -\frac1a arccot(\frac{x}{a}) + C
    \\
    \int a^xdx = \frac{1}{\ln a}a^x+C
}
$$
**常用积分式**
$$
\large{
    \\
    \int\frac{1}{x^2 - a^2}dx = \frac{\ln(\frac{x-a}{x+a})}{2a} +C
    \\
    \int\frac{x}{x^2 \pm 1}dx = \frac12\ln(x^2 \pm 1) +C
    \\
    \rule[0pt]{18cm}{0.05em}
    \\
    \int \frac{1}{\sqrt{1-x}}dx = -2\sqrt{1-x} + C
    \\
    \rule[0pt]{18cm}{0.05em}
    \\
    \int \frac{1}{x\sqrt{a^2-x^2}}dx = -\frac1a \ln(\frac{a+\sqrt{a^2-x^2}}{x}) + C
    \\
    \int \sqrt{a^2-x^2} = \frac12x\sqrt{a^2-x^2} + \frac{a^2}2\arcsin{\frac{x}{a}}+C
    \\
    \int \frac{x}{\sqrt{a^2-x^2}}dx = -\sqrt{a^2-x^2} + C
    \\
    \rule[0pt]{18cm}{0.05em}
    \\
    \left.\begin{array}{ll}
        \int\frac{dx}{\sqrt{a^2+x^2}} = \ln(x+\sqrt{a^2+x^2}) + C
        \\
        \int \frac{dx}{x^2\sqrt{a^2+x^2}} = - \frac{\sqrt{a^2+x^2}}{a^2x} + C
    \end{array}\right\}
    \Rightarrow
    \small{\int \frac{\sqrt{a^2+x^2}}{x^2}dx = \ln(x+\sqrt{a^2+x^2}) - \frac{\sqrt{a^2+x^2}}{x} + C}
    \\
    \rule[0pt]{18cm}{0.05em}
    \\
    \int \frac{dx}{\sqrt{x^2-a^2}} = \ln(x + \sqrt{x^2-a^2}) + C
    \\
    \rule[0pt]{18cm}{0.05em}
    \\
    \int e^{ax}\sin(bx)dx = \frac{e^{ax}}{a^2+b^2}(a\sin(bx)-b\cos(bx)) + C
    \\
    \int e^{ax}\cos(bx)dx = \frac{e^{ax}}{a^2+b^2}(a\cos(bx)+b\sin(bx)) + C
}
$$



线性微分方程注意==**重根**==啊

为什么像齐次齐次线性微分方程的共轭复根情况、非齐次的n重根的情况下，最后结果中都不含虚数单位？<u>根据定理2.8，复线性微分方程的解的实部和虚部分别是对应的实LDE和需LDE的解，因此若原方程不含虚数单位，解自然就不含</u>



**线性叠加原理**

