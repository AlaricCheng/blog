$$
\newcommand{\H}{\mathsf{H}}
\newcommand{\S}{\mathsf{S}}
\newcommand{\T}{\mathsf{T}}
\newcommand{\CNOT}{\mathsf{CNOT}}
\newcommand{\CZ}{\mathsf{CZ}}
\DeclareMathOperator{\Tr}{Tr}
$$

# 量子计算的海森堡表示（一）：Gottesman-Knill 定理
> 作者：麻坑窝散人




在通常我们学到的量子力学中，我们很多时候都是用某个量子态来表示一个系统，然后考虑其在某个哈密顿量下的演化。这个就是量子力学的薛定谔表示（Schrödinger representation）。但有时候，我们把问题转化到海森堡表示（Heisenberg representation）下处理可能会更加简单。海森堡表示主要考虑的是用算符来描述物理系统，考虑算符的演化。在这个系列，我们会介绍怎样用海森堡表示去处理量子计算里的一些问题，内容包括量子线路的经典模拟，stabilizer formalism，量子纠错，quasiprobability 表示等等。本文是系列的第一篇，主要介绍 Gottesman-Knill 定理 [1]。



## Clifford 线路

首先我们来简单回顾一下量子计算。在经典计算中，我们对比特（bit）进行操作，每个比特可以处在的状态是0或者1。但在量子计算中，每个比特不但可以处于0或1，还能处于它们的叠加。此外，不同的比特之间还能发生纠缠。



在量子计算中，我们通过量子门（quantum gate）来对量子比特进行操作。常用的量子门有 Hadamard 门 $\H$，相位门 $\S$，以及 $\CNOT$ 门，它们的矩阵表示如下：
$$
\begin{aligned}
\H &= \frac{1}{\sqrt{2}} \begin{pmatrix} 1 & 1 \\ 1 & -1 \end{pmatrix} & 
\S &= \begin{pmatrix} 1 & 0 \\ 0 & i \end{pmatrix} &
\CNOT &= \begin{pmatrix} 1 &  &  & \\ & 1 & & \\ & & & 1 \\ & & 1 & \end{pmatrix} \ 。
\end{aligned}
$$

此外，我们还经常会用到 Pauli 算符 $X$、$Y$、$Z$：
$$
\begin{aligned}
X &= \begin{pmatrix} 0 & 1 \\ 1 & 0 \end{pmatrix} & 
Y &= \begin{pmatrix} 0 & -i \\ i & 0 \end{pmatrix} & 
Z &= \begin{pmatrix} 1 & 0 \\ 0 & -1 \end{pmatrix} \ ，
\end{aligned}
$$

我们通常还会把单位矩阵 $I$ 算为 Pauli 算符。读者可以很容易地验证，这四个 Pauli 算符加上 $\pm 1$ 和 $\pm i$ 在乘法意义下构成一个群，这就是单比特的 Pauli 群。推广到多比特的情况，Pauli 群里的元素都可以表示 $i^m P_1 \otimes P_2 \otimes \cdots \otimes P_n$ 的形式，其中，$m \in \{ 0, 1, 2, 3\}$， $P_i \in \{ I, X, Y, Z \}$。


上面说的 $\H$、$\S$、$\CNOT$ 都属于 Clifford 门，其特性为把一个 Pauli 群的元素映射到另一个 Pauli 群的元素。换句话说，Clifford 门保 Pauli 群不变，作用效果是对其群元素的置换。量子门 $U$ 对算符 $P$ 的作用定义为 $U P U^{\dagger}$。比如说，对于 Hadamard 门，我们有 $\H^{\dagger} = \H$，其对 Pauli 算符的作用如下：
$$
\begin{aligned}
\H X \H &= Z &
\H Z \H &= X &
\H Y \H &= -Y  \ 。
\end{aligned}
$$

如果一个线路只由 Clifford 门构成，那么我们就称其为 Clifford 线路。任何 Clifford 线路都可以由 $\H$、$\S$、$\CNOT$ 这三个门构成。显然，这个线路对 Pauli 群的作用也只是置换其群元素。我们以后会看到，Clifford 线路在量子纠错中有重要应用。但是，Clifford 线路并不能实现通用量子计算，我们还需要加上 $\T$ 门，其矩阵表示是 $\T = \begin{pmatrix} 1 & 0 \\ 0 & e^{i\pi/4} \end{pmatrix}$。$\T$ 门的作用并不能保 Pauli 群。比如说，当它作用在 $X$ 上，我们有 $\T X \T^{\dagger} = X e^{i\pi Z/4} = \frac{1}{\sqrt{2}} (X + Y)$，并不是一个 Pauli 群的元素。


在有些科普文章中，量子计算相对经典计算的优越性（quantum advantage）被简单地归结为量子比特能处于叠加态，以及能发生纠缠。事实上，这两件事情 Clifford 线路都能做到。比如说，产生贝尔态（Bell state）的线路就是一个 Clifford 线路。而1998年，Daniel Gottesman 在一篇文章 [1] 中告诉我们，Clifford 线路是能被经典有效模拟的。换句话说，Clifford 线路并没有相对于经典线路的优越性。这就是 Gottesman-Knill 定理。

<!-- <p style = "color:red"> circuit of Bell state </p>  -->




## Gottesman-Knill 定理

在 Gottesman-Knill 定理中，「经典有效模拟」的「经典」指经典算法，「有效」指算法的复杂度是关于输入比特个数 $n$ 的多项式。那么「模拟」指什么呢？事实上，经典模拟有各种不同的定义，而且，每种定义对应的经典复杂度也不一样。因此，要想好好地讨论问题的话，我们应该先把「模拟」的意思定义清楚，不能张口就来。


给定初始态、量子线路以及测量基之后，每个测量结果 $x$ 都有一个对应的概率 $p(x)$。在这里，$x$ 既可以是所有 $n$ 个比特的测量结果，也可以是某几个比特的测量结果。Gottesman-Knill 中所用的模拟的定义为精确地计算这样的 $p(x)$ 的值。这种模拟我们称为「强模拟」（strong simulation）。


为了说明 Gottesman-Knill 背后的想法，我们来看一个例子。假设初始态是 $|0^n\rangle$，量子线路 $U$ 是个 Clifford 线路，并且只包含 Hadamard 门、相位门以及 $\CNOT$ 门。我们希望计算测量 $n$ 个比特得到 $x$ 的概率（$x = x_1\cdots x_n$）。我们现在来看一下怎样精确计算它。


首先，初始态可以写成下面的形式：
$$
\begin{aligned}
|0^n\rangle \langle 0^n | = \frac{1}{2^n} \prod_{i = 1}^n (I + Z_i) \ 。
\end{aligned}
$$

其中，$Z_i$ 表示作用到第 $i$ 个比特上的 Pauli $Z$ 算符。写成 $i^m P_1 \otimes P_2 \otimes \cdots \otimes P_n$ 形式的话，则是 $m=0$，$P_i = Z$，而其他的 $P_j = I$。顺便一提，$Z_i$ 被称为 $|0^n\rangle$ 的 stabilizer，因为 $Z_i |0^n\rangle = |0^n\rangle$；而 $|0^n\rangle$ 是由 $Z_i, \ldots, Z_n$ 决定的一个 stabilizer state。我们会在后续的文章详细说到。

读者可以很容易地验证，末态可以写成
$$
\begin{aligned}
U |0^n\rangle \langle 0^n | U^{\dagger} = \frac{1}{2^n} \prod_{i = 1}^n (I + Q_i) \ ，
\end{aligned}
$$

其中，$Q_i := U Z_i U^{\dagger}$ 是 $Z_i$ 经过 $U$ 演化后得到的算符。因为 $U$ 是一个 Clifford 线路，所以 $Q_i$ 也是 Pauli 群的元素。Gottesman-Knill 的核心在于每个 $Q_i$ 都是可以很容易地找到。事实上，我们只需要把 Hadamard 门、相位门以及 $\CNOT$ 门对 Pauli 算符的作用写成一张表存起来，然后对线路里面每一个门都查一次表，就能够知道 $Z_i$ 最终会演化成什么算符，也就是 $Q_i$ 了。


要想求测量得到 $|x\rangle$ 的概率的话，我们用如下式子：
$$
\begin{aligned}
p(x) = \Tr(|x\rangle \langle x | U |0^n\rangle \langle 0^n | U^{\dagger}) \ 。
\end{aligned}
$$

很容易可以看到，上面的式子写开之后就是
$$
\begin{aligned}
p(x) = \frac{1}{2^n} \prod_{i = 1}^n \langle x | (I + Q_i) | x \rangle \ 。
\end{aligned}
$$

这里，$\langle x | I | x \rangle = 1$。由 Hermitian 算符的定义我们可以知道，每个 $Q_i$ 都是 Hermitian 的，因为 $Z_i$ 是 Hermitian 的。因此，$Q_i$ 的相位部分 $i^m = \pm 1$，$\langle x | Q_i | x \rangle = \pm 1$ 或者 $0$。所以，我们可以看到，乘积中的每一项都可以有效精确地算出来——这也就意味着，$p(x)$ 也可以有效精确地算出来。如果我们只测量部分比特，$p(x)$ 依然可以用类似的方法计算。细节我就留给读者补全了。


总结一下，在 Gottesman-Knill 定理中，我们通过把量子态写成 Pauli 算符的展开，然后考虑 Pauli 算符在 Clifford 线路下的演化，从而得到最后想要求的输出概率。有兴趣的读者可以思考一下，如果线路里面包含 $\T$ 门的话，用上面的方法做模拟会出什么问题？





## 多说两句

量子线路模拟是量子计算的一个重要方向，而 Gottesman-Knill 是其中的一个非常漂亮的结果。在这个定理之后，Scott Aaronson 在2004年跟 Gottesman 合作，改进了定理中的算法 [2]。之后还有一系列的工作考虑不同情形下对 Clifford + T 线路的模拟。比如说，在16年，Sergey Bravyi 和 David Gosset 提出一个算法，能有效模拟线路中有少量 $\T$ 门的情况 [3]。在上周的一篇 PRL 中 [4]，卜凯峰和 Dax Koh 则是考虑 nonstabilizer 的输入态 + Clifford 线路的模拟（在后续的系列中我们会介绍 stabilizer 态和 nonstabilizer 态，敬请期待吧:laughing:）。但值得注意的是，不同的文章考虑的经典模拟的定义可能不太一样。



## Reference

1. Gottesman, D. The Heisenberg Representation of Quantum Computers. [arXiv:quant-ph/9807006](http://arxiv.org/abs/quant-ph/9807006) (1998).
1. Aaronson, S. & Gottesman, D. Improved Simulation of Stabilizer Circuits. [Phys. Rev. A 70, 052328](https://journals.aps.org/pra/abstract/10.1103/PhysRevA.70.052328) (2004).
1. Sergey Bravyi & David Gosset. Improved Classical Simulation of Quantum Circuits Dominated by Clifford Gates. [Phys. Rev. Lett. 116, 250501](https://journals.aps.org/prl/abstract/10.1103/PhysRevLett.116.250501) (2016).
1. Kaifeng Bu and Dax Enshan Koh. Efficient Classical Simulation of Clifford Circuits with Nonstabilizer Input States. [Phys. Rev. Lett. 123, 170502](https://journals.aps.org/prl/abstract/10.1103/PhysRevLett.123.170502)
