---
layout: page
title: Chapter 3 - The role of depth
categories: representation
date: 2022-01-16
---

For *shallow networks*, we now know upper bounds on the number of neurons required to "represent" data, where representation is measured either in the sense of exact interpolation (or memorization), or in the sense of universal approximation. We will continue to revisit these bounds when other important questions such as optimization/learning and out-of-sample generalization arise. In some cases, these bounds are even *tight*, meaning that we could not hope to do any better.

Which leads us to the natural question:

~~~
Does depth buy us anything at all?
~~~

If we have already gotten (some) tight results using shallow models, should there be any *theoretical* benefits in pursuing analysis of deep networks? Two reasons why answering this question is important:

One, after all, this is a course on "deep" learning theory, so we cannot avoid this question.

But two, for the last decade or so, a lot of folks have been trying very hard to replicate the success of deep networks with *highly* tuned shallow models (such as kernel machines), but so far have come up short. Understanding precisely why and where shallow models fall short (while deep models succeed) is therefore of importance.

A full answer to the question that we posed above will remain elusive. See, for example, the last paragraph of Belkin's monograph[^belkin] for some speculation. To quote a recent paper by Shankar et al.[^shankar]:

> "...the question remains open whether the performance gap between kernels and neural networks indicates a fundamental limitation of kernel methods or merely an engineering hurdle that can be overcome."  

Nonetheless: in this note, we will derive several interesting results that highlight the importance of depth in the representation power of neural network architectures. Let us focus on "reasonable-width" networks of depth $L > 2$ (because we already know from universal approximation that exponential-width two-layer networks can represent pretty much anything we like.) There are two angles of inquiry:

* Approach 1: prove that there exist datasets of some large enough size that can *only* be memorized by networks of depth $\Omega(L)$, but not by networks of depth $o(L)$.

* Approach 2: prove that there exist classes of functions that can be $\varepsilon$-approximated *only* by networks of depth $\Omega(L)$, but not by networks of depth $o(L)$.

Results of either type are *depth separation* results. Let us start with the latter.

## Depth separation in function approximation
{:.label}

We will first prove a depth-separation result for dense feed-forward networks with ReLU activations and univariate (scalar) inputs. This result will generally hold for other families of activations.

We will explicitly construct a (univariate) function $g$ that can be exactly represented by a deep neural network (with error $\varepsilon = 0$) but is provably inapproximable by a much shallower network. This result is by Telgarsky[^telgarsky].

**Theorem**{:.label #DepthSeparation}
  There exists a function $g : [0,1] \rightarrow \R$ that is exactly realized by a ReLU network of constant width and depth $O(L^2)$, but for *any* neural network $f$ with depth $\leq L$ and sub-exponential number of units, $\leq 2^{L^\delta}$, $f$ is at least $\varepsilon$-far from $g$, i.e.:

  $$
  \int_0^1 |f(x) - g(x)| dx \geq \varepsilon
  $$

{:.theorem}
for constants $\varepsilon > \frac{1}{32}$ and $0 < \delta < 1$.

The proof is elegant and will inform us also while proving memorization-style depth barriers. But let us make several remarks on the implications of the results.

**Remark**{:.label #DepthSepRem1}
  The "hard" example function $g$ constructed in the above Theorem is for scalar inputs. What happens for the general case of $d$-variate inputs? Eldan and Shamir[^eldan] showed that there exist 3-layer ReLU networks (and $\text{poly}(d)$ width) that cannot be $\varepsilon$-approximated by any two-layer ReLU network unless they have $\Omega(2^d)$ hidden nodes. Therefore, there is already a separation between depth=2 and depth=3 in the high-dimensional case.
{:.remark}

**Remark**{:.label #DepthSepRem2}
  In the general $d$-variate case, can we get depth separation results for networks of depth=4 or higher? Somewhat surprisingly, the answer appears to be *no*. Vardi and Shamir[^vardi] showed that a depth separation theorem between ReLU networks of $k \geq 4$ and $k' > k$ would imply progress on long-standing open problems in *circuit lower bounds*[^razborov].  To be precise: this (negative) result only applies to vanilla dense feedforward networks. But it is disconcerting that even for the simplest of neural networks, proving clear benefits of depth remains outside the realm of current theoretical machinery.
{:.remark}

**Remark**{:.label #DepthSepRem3}
  The "hard" example function $g$ constructed in the above Theorem is highly oscillatory within $[0,1]$ (see proof below) and therefore has an unreasonably large (super-polynomial) Lipschitz constant. So, perhaps if we limited our attention to simple/natural Lipschitz functions, then it is easier to prove depth-separation results? Not so: even if we only focused on "benign" functions (easy-to-compute functions with polynomially large Lipschitz constant), proving depth lower bonds would similarly imply progress in long-standing problems in computational complexity. See the recent result by Vardi et al.[^vardi2].
{:.remark}

**Remark**{:.label #DepthSepRem4}
  See this paper[^bengio] for an earlier depth-separation result for sum-product networks (which are somewhat less standard architectures).
{:.remark}


**Proof sketch**{:.label #DepthSeparationProof}
  High level idea: (a) observe that any ReLU network $g$ simulates a piecewise linear function. (b) prove that the number of pieces in the range of $g$ grows polynomially with width but exponentially in depth.
  **_(COMPLETE)_.**
{:.proof}

Let us reflect a bit more on the above proof. The key ingredient was the fact that superpositions (adding units, essentially increasing the "width") only have a polynomial increase on the number of pieces in the range of $g$, but compositions (essentially increasing the "depth") have an *exponential* increase in the number of pieces.

But! This "hard" function $g$, which is the sawtooth over $[0,1]$, was *very carefully constructed*. To  achieve the exponential scaling law in the number of pieces, the breakpoints in $g$ *have* to be exactly equispaced, and therefore the weights in every layer in the network have to be identical. Even a tiny perturbation to $g$ dramatically reduces the number of linear pieces in the range of the network. See the following figure illustrated in Hanin and Rolnick (2019)[^hanin]:

![(left) The sawtooth function $g$, representable via a depth-$O(L^2)$, width-3 ReLU net. (right) Range of the same network as $g$ but with a tiny amount of noise added to its weights.](/fodl/assets/sawtooth.png)

The plot on the left is the sawtooth function $g$, which, as we proved earlier, is representable via a depth-$O(L^2)$, width-3 ReLU net. The plot on the right is the function implemented by the same network as $g$ but with a tiny amount of noise added to its weights. So even when we did get a depth-separation result, it's not at all "robust".

All this to say: depth separation results can be rather elusive; they seem to only exist for very special cases; and progress in this direction would result in several fundamental breakthroughs in complexity theory.


## Depth-width tradeoffs in memorization
{:.label}

**(Under construction)**.



---

[^belkin]:
    Mikhail Belkin, [Fit without Fear](https://arxiv.org/pdf/2105.14368.pdf), 2021.

[^shankar]:
    V. Shankar, A. Fang, W. Guo, S. Fridovich-Keil, L. Schmidt, J. Ragan-Kelley, B. Recht, [Neural Kernels without Tangents](https://arxiv.org/abs/2003.02237), 2020.

[^telgarsky]:
    M. Telgarsky, [Benefits of depth in neural networks](http://proceedings.mlr.press/v49/telgarsky16.pdf), 2016.

[^eldan]:
    R. Eldan and O. Shamir, [The Power of Depth for Feedforward Neural Networks](http://proceedings.mlr.press/v49/eldan16.pdf), 2016.

[^vardi]:
    G. Vardi and O. Shamir, [Neural Networks with Small Weights and Depth-Separation Barriers](https://arxiv.org/pdf/2006.00625.pdf), 2020.

[^razborov]:
    A. Razborov and S. Rudich. [Natural proofs](https://www.sciencedirect.com/science/article/pii/S002200009791494X), 1997.

[^bengio]:
    O. Delalleau and Y. Bengio, [Shallow vs. Deep Sum-Product Networks](https://papers.nips.cc/paper/2011/file/8e6b42f1644ecb1327dc03ab345e618b-Paper.pdf), 2011.

[^vardi2]:
    G. Vardi, D. Reichmann, T. Pitassi, and O. Shamir, [Size and Depth Separation in Approximating Benign Functions with Neural Networks](http://proceedings.mlr.press/v134/vardi21a/vardi21a.pdf), 2021.

[^hanin]:
    B. Hanin and D. Rolnick, [Complexity of Linear Regions in Deep Networks](https://arxiv.org/pdf/1901.09021.pdf), 2019.
