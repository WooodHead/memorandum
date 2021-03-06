---
title: Chapter3. Uniformly Most Powerful Tests
book_title: Testing Statistical Hypotheses
book_chapter: 3
---

## 3.1 Stating The Problem
3章は仮説検定の話。
ここでは、仮説を採択するか棄却するかの2-side decision prolblemを扱う。
このような決定問題を 仮説のtest（検定） という。

* $$(\Omega, \mathcal{F}, P)$$,
    * probability sp.
* $$(\mathcal{X}, \mathcal{A})$$,
    * measurable sp.
* $X: \Omega \rightarrow \mathcal{X}$
    * r.v.
* $$\mathcal{P} := \{P_{\theta} \mid \theta \in \Theta\}$$,
* $H, K \subset \mathcal{P}$
    * $\mathcal{P}$の分割
    * $$H \cup K = \mathcal{P}$$,
    * $K$はalternativesクラスとも言われる
* $$\Theta_{H}, \Theta_{K}$$,
    * $H, K$に対応するパラメータの分割
    * $$\Theta_{H} \cup \Theta_{K} = \Theta$$,
* $$D := \{d_{0}, d_{1}\}$$,
    * decision space
    * $$d_{0}$$は、仮説を採択するという決定
    * $$d_{1}$$は、仮説を棄却するという決定
* $$\delta_{i}: \mathcal{X} \rightarrow D$$,
* $$S_{i} := \{X(\omega) \in \mathcal{X} \mid \omega \in \Omega,\ \delta(X(\omega)) = d_{i} \} \ (i = 0, 1)$$,
    * $X$の値域の分割
    * $S_{0}$の時採択となるようにとる
    * $S_{0}$ is called acceptance region
    * $S_{1}$の時棄却となるようにとる
    * $S_{1}$ is called critical region

検定の2つのえらー。

1. 仮説が正しいのに、棄却する
    * Error of the first kind
    * True Negativeとも
2. 仮説が間違っているのに、採択する
    * Error of the second kind
    * False Positiveとも


病気の有無の検定を考える。
病気であるという仮説をたてる。

1. 病気であるのに、病気でないという 
    * 病気の場合は治療が必要なので、命に関わる病気であれば、このミスは減らす必要がある
2. 病気でないのに、病気であるという 
    * 病気の場合は治療が必要なので、このミスが増えると、無駄な治療費がかかる

どちらを減らすかは、問題によって考える必要がある。
一般にどちらのミスを減らすような両立はできないことが知られている。

そのため、間違って仮説$H$を棄却する確率$\alpha \in [0, 1]$を使って、以下のような条件をつける。

$$
\begin{equation}
    P_{\theta}(\delta(X) = d_{1})
    =
    P_{\theta}(X \in S_{1})
    \le
    \alpha
    \quad
    \theta \in \Theta_{H}
    \label{chap03_3_1_level_of_significance}
\end{equation}
$$

仮説が正しい（つまり、parameterが$\theta \in \Theta_{H}$)の時の、棄却するという決定$d_{1}$を取る確率が$\alpha$以下であるとする。
この条件のもと、仮説が間違っている（つまり、parameterが$\theta \in \Theta_{K}$)ときの、採択する確率を最小にするようにする。
つまり、以下の最適化問題を解く。

$$
\begin{align}
    \min_{\theta \in \Theta_{K}}
    & & &
        P_{\theta}(\delta(X) = d_{0})
    \nonumber
    \\
    \mathrm{subject\ to}
    & & &
        P_{\theta}(\delta(X) = d_{1})
        \le
        \alpha
        \quad
        \theta \in \Theta_{H}
    \nonumber
\end{align}
$$

これは、以下と同値である。

$$
\begin{align}
    \min_{\theta \in \Theta_{K}}
    & & &
        P_{\theta}(X = S_{0})
    \nonumber
    \\
    \mathrm{subject\ to}
    & & &
        P_{\theta}(X = S_{1})
        \le
        \alpha
        \quad
        \theta \in \Theta_{H}
    \nonumber
\end{align}
$$

* 目的関数は、Error of the second kindの最小化
* 制約は、Error of the first kindの確率が$\alpha$以下となる。

多くの場合、目的関数の最小化によって、制約は等号で達成されることが期待される。
つまり、

$$
\begin{equation}
    \sup_{\theta \in \Theta_{H}} P_{\theta}(X \in S_{1})
    =
    \alpha
    \label{chap03_3_3_size}
\end{equation}
$$

$$\eqref{chap03_3_3_size}$$の左辺は、size of the testと呼ばれるものである。
また、Error of the second kindに当たる、以下の式をpower of the testという。

$$
\begin{equation}
    \theta \in \Theta_{K},
    \
    P_{\theta}(\delta(X) = d_{1})
    =
    P_{\theta}(X \in S_{1})
    \label{chap03_3_2_power}
\end{equation}
$$

また、Error of the second kindを$\Theta_{K}$で定義された関数とみなした$\beta: \Theta_{K} \rightarrow [0, 1]$をpower function of the testという。

$$
    \theta \in \Theta_{K},
    \
    \beta(\theta)
    :=
    P_{\theta}(\delta(X) = d_{1})
$$

Error of the first kindの値については問題ごとに設定するしかない。
$\alpha$の値は0.05や0.01が使われることが多い。

level of siginifanceは小さい値をとることが多いが、本来であればpower of the testを考慮して決めるべきものである。

理想的には、significance levelとpowerの両方に対して適当な値をえらうためには、sample sizeは十分大きく取る必要があることが知られている。
Cohen(1962), Freiman et al. (1978)など。
powerと両立した$\alpha$の選び方については、Lehmann (1958), Arrow(1960), Sanathanan (1974)などがある。
また、Bayesianの観点から Savage(1962 pp 62-66)、Rosenthal and Rubin (1958)などがある。

また、significance levelを決める別の指針は、仮定に対する確信度である。
仮定が成り立つとかんがえられる時は、十分significance levelを小さくする。

次に、randomized testについて述べる。

* $0 \le \phi(x) \le 1$
    * critical function
* $X: \Omega \rightarrow \mathcal{X}$
    * r.v.
* $P_{\theta}$
    * prob. measure on $$(\mathcal{X}, \mathcal{A})$$,

randomized testのrejectionの確率は

$$
    \beta_{\phi}(\theta)
    :=
    E_{\theta}
    \left[
        \phi
    \right]
    =
    \int
        \phi(x)
    \ d P_{\theta}(x)
$$

で定義する。

$$
    \beta_{\phi}: \Theta_{K} \rightarrow [0, 1]
$$

is said to be power.
$\phi$は、Error of the first kindとError of the second kindを減らすように選ぶべきである。
よって、

$$
\begin{equation}
    \theta \in \Theta_{K},
    \
    \beta_{\theta}(\phi)
    \label{chap03_3_4}
\end{equation}
$$

の最大化を

$$
\begin{equation}
    \theta \in \Theta_{H},
    \
    \mathrm{E}_{\theta}(\phi)
    \le
    \alpha,
    \label{chap03_3_5}
\end{equation}
$$

の下に行いたい。
もし、$$\Theta_{K} := \{\theta_{K}\}$$が1点集合であれば、適当な$\phi$のクラス$A$から

$$
\begin{align}
    \max_{\phi \in A}
    & & &
        \beta_{\phi}(\theta_{K})
    \nonumber
    \\
    \mathrm{subject\ to}
    & & &
        \mathrm{E}_{\theta}(\phi)
        \le
        \alpha,
        \
        \forall \theta \in \Theta_{H}
    \nonumber
\end{align}
$$

を解けば良い。
一般に$\Theta_{K}$が1点でない場合は、Uniformly most powerful (UMP) testというクラスが重要となる。
これについては、Section 3.4, Section 3.7で議論する。

検定の場合は、次の2つのloss function$$L_{1},L_{2}$$を考えることができる。

$$
\begin{eqnarray}
    L_{1}(\theta, d_{1})
    & := &
        \begin{cases}	
            1 & \theta \in \Theta_{H} \\
            0 & \theta \in \Theta_{K} 
        \end{cases}
    \nonumber
    \\
    L_{1}(\theta, d_{0})
    & := &
        0
        \
        \forall \theta
    \nonumber
\end{eqnarray}
$$

$$
\begin{eqnarray}
    L_{2}(\theta, d_{0})
    & := &
        \begin{cases}	
            0 & \theta \in \Theta_{H} \\
            1 & \theta \in \Theta_{K} 
        \end{cases}
    \nonumber
    \\
    L_{2}(\theta, d_{1})
    & := &
        0
        \
        \forall \theta
    \nonumber
\end{eqnarray}
$$

### Remark
* $S_{0} \subseteq \mathcal{X}$,
    * given
    * acceptance region
* $S_{1} \subseteq \mathcal{X}$,
    * given
    * critical region

Then we can construct decision function.

$$
\begin{eqnarray}
    \delta(x)
    :=
    1_{S_{1}}(x)
    .
\end{eqnarray}
$$

$$
  \phi(x)
  :=
  \gamma
  1_{S_{1}}(x)
  +
  1_{S_{1}}
$$

$$
    x \in \mathcal{X},
    \
    \delta_{\phi}(dz \mid x)
    :=
    \phi(x)
    \epsilon_{1}(dz)
    +
    (1 - \phi(x))
    \epsilon_{0}(dz)
$$


<div class="end-of-statement" style="text-align: right">■</div>

### Defintion test
* $\phi: \mathcal{X} \rightarrow [0, 1]$
    * mesurable
* $\alpha \in [0, 1]$,

$\phi$ is said to be test at level $\alpha$ if

$$
\begin{equation}
    \sup_{\theta \in \Theta_{H}}
        \mathrm{E}_{\theta}
        \left[
            \phi
        \right]
    \le
    \alpha
    .
    \nonumber
\end{equation}
$$

We denote by $\Phi_{\alpha}$ a set of test at level $\alpha$, that is,

$$
    \Phi_{\alpha}
    :=
    \{
        \phi: \text{test}
        \mid
        \theta \in \Theta_{H},
        \
        \mathrm{E}_{\theta}
        \left[
            \phi
        \right]
        \le
        \alpha
    \}
    .
$$

<div class="end-of-statement" style="text-align: right">■</div>

### Defintion most poweful test
* $\alpha \in [0, 1]$,
* $\phi: \mathcal{X} \rightarrow [0, 1]$
    * test at level $\alpha$

We define power function $\beta_{\phi}: \Theta \rightarrow [0, 1]$ of test $\phi$ by

$$
    \beta_{\phi}(\theta)
    :=
    \mathrm{E}_{\theta}
    \left[
        \phi
    \right]
    .
$$

$\phi$ is said to be most poweful test at level $\alpha$ if

$$
\begin{equation}
    \forall \phi_{0} \in \Phi_{\alpha},
    \
    \forall \theta \in \Theta_{K},
    \
    \beta_{\phi}(\theta)
    \ge
    \beta_{\phi}(\theta^{\prime})
    \nonumber
\end{equation}
$$

<div class="end-of-statement" style="text-align: right">■</div>

### Remark
The following equation holds;

$$
    \arg\inf_{\theta \in \Theta_{K}}
        P(X \in S_{0})
    =
    \arg\sup_{\theta \in \Theta_{K}}
        P(X \in S_{1})
$$

Indeed,

$$
\begin{eqnarray}
    \inf_{\theta \in \Theta_{K}} P_{\theta}(X \in S_{0})
    & = &
        \inf_{\theta \in \Theta_{K}}
            (P_{\theta}(X \in (S_{0} \cup S_{1})) - P_{\theta}(X \in S_{1}))
    \nonumber
    \\
    & = &
        1
        +
        \inf_{\theta \in \Theta_{K}}-P_{\theta}(X \in S_{1})
    \nonumber
    \\
    & = &
        1
        -
        \sup_{\theta \in \Theta_{K}}P_{\theta}(X \in S_{1})
    \nonumber
\end{eqnarray}
$$

In particular, $$\Theta_{H} := \{\theta_{0}\}$$, then we have

$$
\begin{eqnarray}
    \inf_{\theta \in \Theta_{H}}
        P_{\theta}(X \in S_{0})
    & = &
        \inf_{\theta \in \Theta_{H}}
            (P_{\theta}(X \in (S_{0} \cup S_{1})) - P_{\theta}(X \in S_{1}))
    \nonumber
    \\
    & = &
        1
        -
        \sup_{\theta \in \Theta_{H}}
             P_{\theta}(X \in S_{1})
    \nonumber
    \\
    & \ge &
        1
        -
        \alpha
\end{eqnarray}
$$

<div style="text-align: right">■</div>
