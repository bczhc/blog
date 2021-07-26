##  通过计算得出的一些安卓中单位换算关系式：

为了更加简洁明了，每个变量都带单位，像物理中的公式一样。
$$
\begin{aligned}
设：\\
&P为\rm{px}，单位为\rm{px}\\
&D为\rm{dp}，单位为\rm{dp}\\
&m为\rm{inch}，单位为\rm{px\cdot in^{-1}}\\
&n为\rm{cm}，单位为\rm{cm}\\
常量：\\
&A=160\rm{dp\cdot in^{-1}}\\
&B=2.54\rm{cm\cdot in^{-1}}\\
&C=\frac {127}{8000}\rm{cm\cdot dp^{-1}}=\frac B A\\
则有：\\
&P=\frac {SD} A, D=\frac {AP} S\\
&m=\frac D A=\frac P S\\
&n=Bm=CD\\
&1\rm dp=\frac {127}{8000}\rm{cm}=\frac 1 {160}\rm{in}
\end{aligned}\\
$$


如果不支持LaTex，查看[图片](../images/formula_0.png)。