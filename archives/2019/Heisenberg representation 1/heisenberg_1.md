$$
\newcommand{\H}{\mathsf{H}}
\newcommand{\S}{\mathsf{S}}
\newcommand{\T}{\mathsf{T}}
\newcommand{\CNOT}{\mathsf{CNOT}}
\newcommand{\CZ}{\mathsf{CZ}}
\newcommand{\X}{\mathsf{X}}
\newcommand{\Y}{\mathsf{Y}}
\newcommand{\Z}{\mathsf{Z}}
\newcommand{\I}{\mathsf{I}}
$$

# 量子计算的海森堡表示（一）：Gottesman-Knill 定理




在通常我们学到的量子力学中，我们很多时候都是在处理态，比如薛定谔方程描述的就是系统的态在某个哈密顿量下的演化。这个就是量子力学的薛定谔表示（Schrödinger representation）。但有时候，我们把问题转化到海森堡表示（Heisenberg representation）下处理可能会更加简单。在这个系列，我们会介绍怎样用海森堡表示去处理量子计算里的一些问题，内容包括量子线路的经典模拟，stabilizer formalism，量子纠错，quasiprobability 表示等等。本文是系列的第一篇，主要介绍 Gottesman-Knill 定理 [1]。



## 量子计算

首先我们来简单回顾一下量子计算。在经典计算中，我们会比特（bit）进行操作，每个比特可以处在的状态是0或者1。但在量子计算中，每个比特不但可以处于0或1，还能处于它们的叠加。此外，不同的比特之间还能发生纠缠。
<!-- 我们用$|\psi\rangle$来标记一个量子态的话，处于0和1的叠加用数学语言来写就是$|\psi\rangle = \alpha |0\rangle + \beta |1\rangle$。 -->

在量子计算中，我们通过量子门（quantum gate）来对量子比特进行操作。常用的量子门有 Hadamard 门 $\H$，相位门 $\S$，以及 $\CNOT$ 门，它们的矩阵表示如下：
$$
\begin{aligned}
\H &= \frac{1}{\sqrt{2}} \begin{pmatrix} 1 & 1 \\ 1 & -1 \end{pmatrix} & 
\S &= \begin{pmatrix} 1 & 0 \\ 0 & i \end{pmatrix} &
\CNOT &= \begin{pmatrix} 1 &  &  & \\ & 1 & & \\ & & & 1 \\ & & 1 & \end{pmatrix} \ 。
\end{aligned}
$$

此外，我们还经常会用到 Pauli 算符 $\X$，$\Y$，$\Z$：
$$
\begin{aligned}
\X &= \begin{pmatrix} 0 & 1 \\ 1 & 0 \end{pmatrix} & 
\Y &= \begin{pmatrix} 0 & -i \\ i & 0 \end{pmatrix} & 
\Z &= \begin{pmatrix} 1 & 0 \\ 0 & -1 \end{pmatrix} \ ，
\end{aligned}
$$

我们通常还会把单位矩阵 $\I$ 算为 Pauli 算符。读者可以很容易地验证，这四个 Pauli 算符加上 $\pm 1$ 和 $\pm i$ 在乘法意义下构成一个群，这就是单比特的 Pauli 群。推广到多比特的情况，Pauli 群里的元素都是形如 $i^m P_1 \otimes P_2 \otimes \cdots \otimes P_n$ 的形式，其中，$m = 0, 1, 2, 3$， $P_i \in \{ \I, \X, \Y, \Z \}$。


上面说的 $\H$，$\S$，$\CNOT$ 都属于 Clifford 门，其特性为把一个 Pauli 群的元素映射到另一个 Pauli 群的元素。换句话说，Clifford 门保 Pauli 群不变，作用效果是对其群元素的置换。比如说，对于 Hadamard 门，其对 Pauli 算符的作用如下：
$$
\begin{aligned}
\H \X \H &= \Z &
\H \Z \H &= \X &
\H \Y \H &= -\Y  \ 。
\end{aligned}
$$

如果一个线路只由 Clifford 门构成，那么我们就称其为 Clifford 线路。显然，这个线路对 Pauli 群的作用也只是置换其群元素。我们以后会看到，Clifford 线路在量子纠错中有重要应用。但是，Clifford 线路并不能实现通用量子计算，我们还需要加上 $\T$ 门，其矩阵表示是 $\T = \begin{pmatrix} 1 & 0 \\ 0 & e^{i\pi/4} \end{pmatrix}$。$\T$ 门的作用并不能保 Pauli 群。比如说，它作用在 $\X$ 上，我们有 $\T \X \T^{\dagger} = \X e^{i\pi \Z/4} = \frac{1}{\sqrt{2}} (\X + \Y)$，并不是一个 Pauli 群的元素。


在不少科普文章中，量子计算相对经典计算的优越性（quantum advantage）被简单地归结为量子比特能处于叠加态，以及能发生纠缠。但事实上，这两件事情 Clifford 线路都能做到。比如说，产生贝尔态（Bell state）的线路就是一个 Clifford 线路。而1998年，Gottesman 在一篇文章中告诉我们，Clifford 线路是能被经典模拟的。也就是说，Clifford 线路在做计算上并没有相对于经典线路的优越性。这就是 Gottesman-Knill 定理的内容。

<p style = "color:red"> circuit of Bell state </p> 




## Gottesman-Knill 定理

以例子$U|0^n \rangle$来说明：$|0^n\rangle\langle 0^n|$




## 后记

Aaronson的复杂度证明，后续对Clifford+T的模拟



## Reference

1. Gottesman, D. The Heisenberg Representation of Quantum Computers. [arXiv:quant-ph/9807006](http://arxiv.org/abs/quant-ph/9807006) (1998).
1. Aaronson, S. & Gottesman, D. Improved Simulation of Stabilizer Circuits. [Phys. Rev. A 70, 052328](https://journals.aps.org/pra/abstract/10.1103/PhysRevA.70.052328) (2004).
