$$
\newcommand{\H}{\mathsf{H}}
\newcommand{\S}{\mathsf{S}}
\newcommand{\CNOT}{\mathsf{CNOT}}
\newcommand{\CZ}{\mathsf{CZ}}
\newcommand{\X}{\mathsf{X}}
\newcommand{\Y}{\mathsf{Y}}
\newcommand{\Z}{\mathsf{Z}}
\newcommand{\I}{\mathsf{I}}
$$

# 量子计算的海森堡表示（一）：Gottesman-Knill theorem




在通常我们学到的量子力学中，我们很多时候都是在处理态，比如薛定谔方程描述的就是系统的态在某个哈密顿量下的演化。这个就是量子力学的薛定谔表示（Schrödinger representation）。但有时候，我们把问题转化到海森堡表示（Heisenberg representation）下处理可能会更加简单。在这个系列，我们会介绍怎样用海森堡表示去处理量子计算里的一些问题，内容包括量子线路的经典模拟，stabilizer formalism，量子纠错，quasiprobability表示等等。本文是系列的第一篇，主要介绍 Gottesman-Knill 定理 [1]。



## 量子计算

首先我们来简单回顾一下量子计算。在经典计算中，我们会比特（bit）进行操作，每个比特可以处在的状态是0或者1。但在量子计算中，每个比特不但可以处于0或1，还能处于它们的叠加。此外，不同的比特之间还能发生纠缠。
<!-- 我们用$|\psi\rangle$来标记一个量子态的话，处于0和1的叠加用数学语言来写就是$|\psi\rangle = \alpha |0\rangle + \beta |1\rangle$。 -->

在量子计算中，我们通过量子门（quantum gate）来对量子比特进行操作。常用的量子门有Hadamard门 $\H$，相位门 $\S$，$\CZ$ 门以及$\CNOT$ 门，它们的矩阵表示如下：
$$
\begin{aligned}
\H &= \frac{1}{\sqrt{2}} \begin{pmatrix} 1 & 1 \\ 1 & -1 \end{pmatrix} & 
\S &= \begin{pmatrix} 1 & 0 \\ 0 & i \end{pmatrix} &
\CZ &= \begin{pmatrix} 1 &  &  & \\ & 1 & & \\ & & 1 &  \\ & & & -1 \end{pmatrix} &
\CNOT &= \begin{pmatrix} 1 &  &  & \\ & 1 & & \\ & & & 1 \\ & & 1 & \end{pmatrix} \ 。
\end{aligned}
$$
此外，我们还经常会用到Pauli算符$\X$，$\Y$，$\Z$：
$$
\begin{aligned}
\X &= \begin{pmatrix} 0 & 1 \\ 1 & 0 \end{pmatrix} & 
\Y &= \begin{pmatrix} 0 & -i \\ i & 0 \end{pmatrix} & 
\Z &= \begin{pmatrix} 1 & 0 \\ 0 & -1 \end{pmatrix} \ ，
\end{aligned}
$$




## Gottesman-Knill定理


## 后记



## Reference

1. Gottesman, D. The Heisenberg Representation of Quantum Computers. [arXiv:quant-ph/9807006](http://arxiv.org/abs/quant-ph/9807006) (1998).
1. Aaronson, S. & Gottesman, D. Improved Simulation of Stabilizer Circuits. [Phys. Rev. A 70, 052328](https://journals.aps.org/pra/abstract/10.1103/PhysRevA.70.052328) (2004).
