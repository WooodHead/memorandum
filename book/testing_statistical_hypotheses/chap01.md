---
title: Chapter1. The General Decision Problem
book_title: Testing Statistical Hypotheses
book_chapter: 1
---

## 1. The General Decision Problem

## 1.1 Statistical Inference and Statistical Decisions

* $$(\Omega, \mathcal{F}, P)$$,
    * 確率空間
    * 本では$\Omega$はパラメータの空間に使われている
* $(\mathcal{X}, \mathcal{A})$
    * 可測空間
* $\Theta$
    * parameter space
    * パラメータの空間
    * 本では$\Omega$が使われている
* $X$
    * 真の確率変数
* $P_{X}$
    * $X$の分布
* $$P_{\theta}$$,
    * $X$の分布の探索空間
    * $\theta \in \Theta$
    * $$P_{X} = P_{\theta}$$となるようにするかもしくは仮定する
* $D$
    * 行動の全体
* $\delta$
    * 行動指針
* $L(\theta, \delta)$


### Remark
以下はWaldによる統計的決定問題(statistical decision problem)の定義。

* $$(\Omega, \mathcal{F}, P)$$,
    * 確率空間
* $$(\mathcal{X}, \mathcal{A})$$,
    * 可測空間
    * sample space, 標本空間
* $$X: \Omega \rightarrow \mathcal{X}$$,
    * 確率変数
* $$\mathcal{P} := \{P_{\theta}\}_{\theta \in \Theta}$$,
    * $(\mathcal{X}, \mathcal{A})$上の確率分布の族
* $$(D, \mathcal{B})$$,
    * 可測空間
    * decision space, action space, 決定空間、行動空間
* $$L: \Theta \times D \rightarrow \mathbb{R}_{\ge 0}$$,
    * $\forall \theta \in \Theta$について、$L(\theta, \cdot)$が$\mathcal{D}$可測
    * loss function, 損失関数
* $\delta: \mathcal{X} \rightarrow D$
    * 可測写像
    * nonrandomized decision function, 非確率的決定関数
* $\delta: D \times \mathcal{X} \rightarrow [0, 1]$
    * $\forall x \in \mathcal{X}$について$\delta(\cdot \mid x)$は$$(D, \mathcal{B})$$上の確率測度
    * $\forall B \in \mathcal{B}$について$\delta(B \mid \cdot)$は$\mathcal{A}$可測
    * randomized decision function
    * nonrandomized decision functionより一般的
* $$\Delta$$,
    * nonrandomizedかrandomized　decision functionの族

$$(\mathcal{X}, \mathcal{A}, \mathcal{P}, \Theta, D, \mathcal{B}, L, \Delta)$$を統計的決定問題という。

以下の例を考えよう。

* $$X_{1}, \ldots, X_{n}$$,
    * ある確率分布に従う確率変数
    * 同分布かも不明
* $$x_{1}, \ldots, x_{n}$$,
    * $X_{i}$の観測値
* $\mu$
    * $X_{1}$の平均、存在は仮定

上記の元$\mu$を推定する問題を考える。
平均の推定としてい例えば以下のような方法が考えられる。

1. $$t_{1} := x_{1}$$,
    * $X_{1}$の観測値を平均だと思う
2. $$t_{2} := \sum_{i=1}^{n} x_{j} / n$$,
    * $X_{i}$の観測値の算術平均を平均だと思う
3. $$t_{3} := x_{n}$$,
    * $X_{n}$の観測値を平均だと思う

これらは、観測値に基づく推定方法である。
観測値に基づく推定方法ではたまたま$\mu$に近くなることはありうるので、意味がない。
推定方法の良し悪しについては、推定量に対して比較をする。
つまり関数の一点で議論するのではなく、関数全体に対してその方法の良し悪しを議論する。
上記の観測値に基づく方法は、自然に以下の推定量を導くことができる。

1. $$T_{1} := X_{1}$$,
2. $$T_{2} := \sum_{i=1}^{n} X_{j} / n$$,
3. $$T_{3} := X_{n}$$,

i.i.d.であれば2が分散最良の推定量となることが分かるが、一般の場合には必ずしもそうではない。

これを例に統計的決定問題を考える。
まず、sample spaceを$$(\mathcal{X}, \mathcal{A}) := (\mathbb{R}^{n}, \mathcal{B}(\mathbb{R}^{n}))$$とする。
確率分布族として

$$
    \mathcal{P}_{1}
    :=
    \left\{
        P_{*}^{n}
        \mid
        P_{*}^{n} \text{: prob. measure over } \mathbb{R}^{n},
        \
        \int x^{2}\ P(dx) < \infty
    \right\}
$$

を考える。
$P \in \mathcal{P}_{1}$は$$X_{1}, \ldots, X_{n}$$の同時分布を与える。
また$$\mathcal{P}_{1}$$の添字$\Theta$を考えて$$\mathcal{P}_{1} = \{P_{\theta} \mid \theta \in \Theta\}$$とかけるとする。
$\Theta$がparameter spaceとなる。
decision spaceは平均値の全体なので、$$(D, \mathcal{B}) := (\mathbb{R}, \mathcal{B}(\mathbb{R}))$$とする。
$$\Delta := \{T_{1}, T_{2}, T_{3} \}$$は$D$に値を取る非確率的決定関数の族である。
損失関数を

$$
\begin{eqnarray}
    \theta \in \Theta,
    \
    a \in D,
    \
    L(\theta, a)
    & := &
        (a - \mu_{1}(P_{\theta}))^{2}
    \nonumber
    \\
    P_{\theta}^{1}(A)
    & := &
        \int_{A \times \mathbb{R}^{n-1}} \ P(dy_{1}, \ldots, dy_{n})
    \nonumber
    \\
    \mu_{1}(P_{\theta})
    & := &
        \int y\ P_{\theta}^{1}(dy)
    \nonumber
\end{eqnarray}
$$

で定義する。
$\mu_{1}(P_{\theta})$は$P_{\theta}$の下での$X_{1}$の周辺分布の平均値である。
risk functionは

$$
    \forall \theta \in \Theta,
    \
    \delta \in \Delta,
    \
    R(\theta, \delta)
    :=
    \int L(\theta, \delta(y_{1}, \ldots, y_{n})\ P_{\theta}(dy_{1}, \ldots, dy_{n})
$$

となる。
loss functionの定義からrisk functionは$P_{\theta}$の下での$\delta$の分散となる。
つまり、

$$
\begin{eqnarray}
    \forall \theta \in \Theta,
    \
    \delta \in \Delta,
    \
    R(\theta, \delta)
    & = &
        \int
            (\delta - \mu_{1}(P_{\theta}))^{2}
        \ P_{\theta}(dy_{1}, \ldots, dy_{n})
\end{eqnarray}
$$

である。
上記の統計的決定問題を考えれば、risk funcitonを最小にする推定量は分散最小の推定量ということになる。

$$
\begin{eqnarray}
    \forall \theta \in \Theta,
    \
    R(\theta, T_{1})
    & = &
        \int
            (y_{1}- \mu_{1}(P_{\theta}))^{2}
        \ P_{\theta}(dy_{1}, \ldots, dy_{n})
    \nonumber
    \\
    & = &
        \mathrm{Var}_{P_{\theta}}
        \left[
            X_{1}
        \right]
    \nonumber
    \\
    R(\theta, T_{3})
    & = &
        \int
            (y_{n}- \mu_{1}(P_{\theta}))^{2}
        \ P_{\theta}(dy_{1}, \ldots, dy_{n})
    \nonumber
    \\
    & = &
        \int
            (y_{n}- \mu_{1}(P_{\theta}))^{2}
        \ P_{\theta}^{n}(dy_{n})
    \nonumber
    \\
    R(\theta, T_{2})
    & = &
        \int
            \left(
                \sum_{i=1}^{n}
                    \frac{y_{i}}{n}
                -
                \mu_{1}(P_{\theta})
            \right)^{2}
        \ P_{\theta}(dy_{1}, \ldots, dy_{n})
\end{eqnarray}
$$

となる。
一方、$$\mathcal{P}_{2}$$を

$$
    \mathcal{P}_{2}
    :=
    \left\{
        (P_{*})^{n}
        \mid
        P_{*} \text{: prob. measure over } \mathbb{R},
        \
        \int_{\\mathbb{R}} x^{2}\ P(dx) < \infty
    \right\}
$$

と定義する。
$$X_{1}, \ldots, X_{n}$$の同時分布を1次元の分布の$n$個の直積として定義する。
つまり、同分布とすると、risk functionは以下のようになる。

$$
\begin{eqnarray}
    \forall \theta \in \Theta_{2},
    \
    R(\theta, T_{1})
    & = &
        \int
            (y_{1}- \mu_{1}(P_{\theta}))^{2}
        \ P_{\theta}(dy_{1}, \ldots, dy_{n})
    \nonumber
    \\
    & = &
        \mathrm{Var}_{P_{\theta}}
        \left[
            X_{1}
        \right]
    \nonumber
    \\
    R(\theta, T_{3})
    & = &
        \int
            (y_{n}- \mu_{1}(P_{\theta}))^{2}
        \ P_{\theta}(dy_{1}, \ldots, dy_{n})
    \nonumber
    \\
    & = &
        \mathrm{Var}_{P_{\theta}}
        \left[
            X_{1}
        \right]
    \nonumber
    \\
    R(\theta, T_{2})
    & = &
        \int
            \left(
                \sum_{i=1}^{n}
                    \frac{y_{i}}{n}
                -
                \mu_{1}(P_{\theta})
            \right)^{2}
        \ P_{\theta}(dy_{1}, \ldots, dy_{n})
    \nonumber
    \\
    & = &
        \frac{1}{n^{2}}
        \left(
            \sum_{i=1}^{n}
                \mathrm{Var}_{P_{\theta}}
                \left[
                    X_{i}
                \right]
            +
            2
            \sum_{i=1}^{n}
                \sum_{j=i+1}^{n}
                    \mathrm{Cov}
                    \left[
                        X_{i}, X_{j}
                    \right]
        \right)
    \nonumber
    \\
    & = &
        \frac{1}{n}
        \mathrm{Var}_{P_{\theta}}
        \left[
            X_{1}
        \right]
        +
        \frac{2}{n^{2}}
        \sum_{i=1}^{n}
            \sum_{j=i+1}^{n}
                \mathrm{Cov}
                \left[
                    X_{i}, X_{j}
                \right]
\end{eqnarray}
$$

となる。
更に、独立同分布であれば、risk functionが最小のものが分散最小になることも分かる。
<div class="QED" style="text-align: right">■</div>

大まかにいえば、Statistical Infereceとは、$X$の分布についての情報を観測されるものから得る方法のことを指す。
もしくは、$X$の分布を決定する$\theta$を観測されたものから得る方法である。

$X$の分布が未知の時に、統計的な分析が必要となる。
$X$の分布が未知の場合には、$X$に対する最良の行動を決定することはできない。
この行動の決定問題は次のように数学的に考えることができる。

我々は、$X$に影響を受ける幾つかの行動の選択肢を持っているとする。
$X$の観測は$X$の分布に対する情報を与えると共に、$X$に対する行動を決める指針にもなり得るだろう。
行動の決定問題は、$X$の観測値から取るべき行動を具体化することに他ならない。
観測値から行動への関数を$\delta$として定義する。
$X$の一つの観測値が$x$とするならば、行動$d$は$d := \delta(x)$として表現される。
$\delta$は観測値から行動を決定する為に、一つのruleを定める。

$\delta$がどのように選ばれるべきかを見るためには、異なる行動$d$をとった場合に得られる結果と比較する必要がある。
これを考えるにあたって、$X$の分布が$P_{\theta}$とした時に、ある行動$d$を取ったときのLossを$L(\theta, d)$としてかく。
$L(\theta, d)$は、非負値の関数である。
つまり、$X$の分布を$\theta$と思って、$d$で行動した時に発生する損失を表現する。
$L$は確率的な損失を表すから、平均的な損失については$L$の期待値によって、$R(\theta, \delta) := \mathrm{E}[L(\theta, \delta(X))]$と定義する。
$R(\theta, \delta)$は、risk functionと呼ばれる。
$d$の決定は、$L(\theta, d)$によって決まり、$\delta$の決定は$R(\theta, \delta)$で決まる。

## 1.2 Specification of a Decision Problem
統計的な問題を考えるにあたって以下の3つを決める必要がある。

* $$\mathcal{P} := \{P_{\theta} \mid \theta \in \Theta\}$$,
    * $X$の分布の候補のクラス
    * ある$\theta$について、$$P_{X} = P_{\theta}$$を仮定する
* $D$
    * 行動の全体
    * $d \in D$が行動
* $L$
    * loss function

まず$\mathcal{P}$を考える。
仮定としては以下が良く考慮される。

* 確率や確率分布についての数値的な仮定は置かない。
* 確率が等しいことや観測している確率変数が独立であるということが良くに仮定される。
* 極限をとったときの確率についての挙動
    * 極限をとれば0になるなど

$X$が二項分布に従う場合を例に考える。
つまり、真のパラメータ$$(n_{0}, p_{0})$$が存在して、

$$
\begin{equation}
    P(X = x)
    =
    \left(
        \begin{array}{c}
            n_{0} \\
            x
        \end{array}
    \right)
    p_{0}^{x}
    (1 - p_{0})^{n_{0} - x},
    \label{chap01_1_1_true_binomial_pdf}
\end{equation}
$$

とかけるとする。
$b(n, p)$を二項分布の分布とする。
つまり、

$$
    \forall n \in \mathbb{N},
    \
    0 \le p \le 1,
    \
    \forall x = 0, \ldots, n,
    \
    b(n, p)(x)
    :=
    P(X \le x)
$$

である。
このとき、$\Theta$や$\mathcal{P}$は以下のように考えることができる。

* $\Theta := \mathbb{N} \times [0, 1]$
* $$\mathcal{P} := \{b(n, p) \mid  (n, p) \in \Theta \}$$,

また、同様に$n$については先験的に分かっているとすれば

* $\Theta := [0, 1]$
* $$\mathcal{P} := \{b(n_{0}, p) \mid  p \in \Theta \}$$,

となる。
上の例から分かるように、何をパラメータとするかどうかは恣意的に決まることに注意する。

また、別の例としてポアソン分布の場合を考える。
つまり、$X$の真のパラメータ$\tau_{0}$とすれば

$$
\begin{equation}
    P(X = x)
    =
    \frac{
        \tau_{0}^{x}
    }{
        x!
    }
    e^{-\tau_{0}},
    \label{chap01_1_2_true_poisson_pdf}
\end{equation}
$$

とする。
$P(\tau)$をポアソン分布の分布とすれば、

$$
    \forall \tau > 0,
    \
    \forall x = 0, 1, \ldots, 
    \
    P(\tau)(x)
    :=
    \frac{
        \tau^{x}
    }{
        x!
    }
    e^{-\tau},
$$

である。
同様に、$\Theta$や$\mathcal{P}$は以下のように考えることができる。

* $\Theta := (0, \infty)$
* $$\mathcal{P} := \{P(\tau) \mid  \tau \in \Theta \}$$,

ポアソン分布は、

* ある区間の間に起こる事象の発生回数を表現するのに良く使われる。


また、別の例として正規分布の場合を考える。
つまり、$X$の真のパラメータ$\mu_{0}, \sigma_{0}$とすれば

$$
\begin{eqnarray}
    p(x)
    & := &
        \frac{
            1
        }{
            \sqrt{2\pi}
            \sigma_{0}
        }
        \exp
        \left(
            -
            \frac{
                1
            }{
                2\sigma_{0}^{2}
            }
            (x - \mu_{0})^{2}
        \right)
    \label{chap01_1_3_true_normal_pdf}
    \\
    F(x)
    & := &
        \int_{-\infty}^{x} 
            \frac{
                1
            }{
                \sqrt{2\pi}
                \sigma_{0}
            }
            \exp
            \left(
                -
                \frac{
                    1
                }{
                    2\sigma_{0}^{2}
                }
                (y - \mu_{0})^{2}
            \right)
        \ dy
    \nonumber
\end{eqnarray}
$$

を確率密度関数とする
$\mathrm{N}(\mu, \sigma)$を正規分布の分布とすれば、

$$
    \forall \mu \in (-\infty, \infty)
    \
    \forall \sigma \in (0, \infty)
    \
    \forall x \in (-\infty, \infty), 
    \
    \mathrm{N}(\mu, \sigma)(x)
    :=
    \int_{-\infty}^{x} 
        \frac{
            1
        }{
            \sqrt{2\pi}
            \sigma
        }
        \exp
        \left(
            -
            \frac{
                1
            }{
                2 \sigma^{2}
            }
            (y - \mu)^{2}
        \right)
    \ d y
$$

である。
同様に、$\Theta$や$\mathcal{P}$は以下のように考えることができる。

* $\Theta := \mathbb{R} \times (0, \infty)$
* $$\mathcal{P} := \{\mathrm{N}(\mu, \sigma) \mid  (\mu, \sigma) \in \Theta \}$$,

次にDecisionの空間$D$について考える。

### Example 1.2.1
* $$X_{1}, \ldots, X_{n}$$,
    * $$\eqref{chap01_1_1_true_binomial_pdf}$$-$$\eqref{chap01_1_3_true_normal_pdf}$$のいずれかの分布のi.i.d.
    * 例として$$\eqref{chap01_1_3_true_normal_pdf}$$の場合を考える。
* $\theta$
    * $$\eqref{chap01_1_1_true_binomial_pdf}$$の時は$p$
    * $$\eqref{chap01_1_2_true_poisson_pdf}$$の時は$\tau$
    * $$\eqref{chap01_1_3_true_normal_pdf}$$の時は$(\mu, \sigma)$
* $\gamma(\theta)$
    * $\mathbb{R}$値の関数
    * 本でこのような表記をしているのは、分布を上記3つのどれかとしているので、パラメータは離散値だったり、連続値だったりする細かい問題に対処する為。

#### (i) Two-decision problem or Testing a hypothesis
$\gamma(\theta)$の値がある値$\gamma_{0}$を超えているかいないかでdecisionを決定する。

* $$\mathcal{X}_{1}, \mathcal{X}_{2}$$,
    * $$\mathcal{X}$$の分割
* $$D := \{d_{0}, d_{1}\} = \{0, 1\}$$,
* $$\Theta \subseteq \mathbb{R}^{p}$$,
* $$A_{0}, A_{1}$$,
    * $$\Theta$$の分割

$$
    x \in \mathcal{X},
    \
    \delta(x_{1}, \ldots, x_{n})
    :=
    d_{0}
    1_{\mathcal{X}_{1}}(x)
    +
    d_{1}
    1_{\mathcal{X}_{2}}(x)
$$

二択の決定問題。
$d_{0}$なら採択、$$d_{1}$$なら非採択など。
検定もこのクラスの問題に属する。

$$
    L(\theta, d)
    :=
    \begin{cases}	
        1_{\{d_{0}\}}(d) & (\theta \in A_{0})\\
        1_{\{d_{1}\}}(d) & (\theta \in A_{1})
    \end{cases}
$$

#### (ii) Testing Point Estimate
点推定。
パラメータの推定の問題。

* $$D \subseteq \mathbb{R}^{d}$$,
    * 凸Borel集合くらいの条件をかす
* $\delta: \mathcal{X} \rightarrow D$
    * 何かしらの推定量
    * $$x_{1}, \ldots, x_{n}$$を観測値とすれば、$$\delta(x_{1}, \ldots, x_{n}) = \sum_{i=1}^{n}x_{i}/n$$などにあたる
* $L$
    * 凸関数くらいの条件を課す

例えば、$\theta$が正規分布の平均値であるとすれば$L$は以下のように定義できる。

$$
    L(\theta, d)
    :=
    |d - \theta|^{2}
$$


#### (iii) Multiple Decision Procedure
(i)を一般化した問題。
二択の決定問題から、n択の決定問題。
$\theta$の値がある部分集合に属しているかいないかでdecisionを決定する。

* $$\mathcal{X}_{1}, \mathcal{X}_{2}, \ldots, \mathcal{X}_{n}$$,
    * $\mathcal{X}$の分割
* $$D := \{d_{0}, d_{1}, \ldots, d_{n}\} = \{0, 1, \ldots, n\}$$,
* $\Theta \subseteq \mathbb{R}^{p}$
* $A_{0}, \ldots, A_{n}$
    * $\Theta$の分割

$$
    \forall x \in \mathcal{X},
    \
    \delta(x)
    :=
    \sum_{i=0}^{n}
        d_{i} 
        1_{\mathcal{X}_{i}}(x)
$$

損失関数は例えば以下のようになる。

$$
    L(\theta, d)
    :=
    \begin{cases}	
        1_{\{d_{0}\}}(d)
            & (\theta \in A_{0})
        \\
        1_{\{d_{1}\}}(d)
            & (\theta \in A_{1})
        \\
        \vdots
            & 
        \\
        1_{\{d_{n}\}}(d)
            & (\theta \in A_{n})
    \end{cases}
$$

<div class="QED" style="text-align: right">■</div>

### Example 1.2.2
* $s$
    * sampleの数
* $\mathrm{N}(\mu_{i}, \sigma^{2})$
    * $i = 1, \ldots, s$
* $$X_{i,j}$$, $\forall i = 1, \ldots, s$, $\forall j = 1, \ldots, n_{i}$
    * $i$番目のsampleの$j$個目のi.i.d.

#### (i)
* $s = 2$
* 2つの平均の差

$$
    \delta(\theta)
    :=
    d_{0}
    1_{\{|\mu_{2} - \mu_{1}| \le \Delta\}}
    +
    d_{1}
    1_{\{\mu_{2} - \mu_{1} > \Delta\}}
    +
    d_{2}
    1_{\{\mu_{1} - \mu_{2} > \Delta\}}
$$

$$
    \delta(\theta)
    :=
    d_{0}
    1_{\{\max_{i,j = 1, \ldots, s}|\mu_{i} - \mu_{j}| \le \Delta\}}
    +
    d_{1}
    1_{\{\max_{i,j = 1, \ldots, s}|\mu_{i} - \mu_{j}| > \Delta\}}
$$

#### (ii)
平均$\mu_{i}$の大きい順番に並べる問題

#### (iii)
$\mu_{0}$が与えられたとする。
$\mu_{0}$を超える平均が存在するかを判定する問題。

<div class="QED" style="text-align: right">■</div>

### Example 1.2.3
* $$P(\tau_{1}), P(\tau_{2})$$,
    * ポアソン分布
    * $$\tau_{1} < \tau_{2}$$であることは既知
* $$Z_{1}, \ldots, Z_{n}$$,
    * $P(\tau_{1})$か$P(\tau_{2})$のどちらか一方のi.i.d.列

loss functionは$$P(\tau_{1}), P(\tau_{2})$$のどちらかかを正しく分類できなかった回数。

<div class="QED" style="text-align: right">■</div>

今までの例は全てaction problemsと言われる問題である。
$\theta$を与えると一意なdecisiton $d$が存在し、$L(\theta, d) = 0$となることが仮定されている。
しかし、全ての統計的な問題にそのような仮定ができるわけではない。


### Example 1.2.4
* $$X_{1}, \ldots, X_{n}$$,
    * $$\mathrm{N}(\mu, \sigma^{2})$$,のi.i.d.
* $k \in \mathbb{R}$
    * 事前に決定する値
* $\underline{L}, L \in \mathbb{R}$
    * $\underline{L} < L$
    * $\mu \in [\underline{L}, L]$

<div class="QED" style="text-align: right">■</div>

最後にLoss functionについて述べる。
Example 1.2.1(i)を例に考える。

$$
    L(\theta, d_{0}) 
    =
    \begin{cases}	
        a & \gamma(\theta) \le \gamma_{0} \\
        b & \gamma(\theta) > \gamma_{0}
    \end{cases}
$$

さらに

$$
\begin{equation}
    R(\theta, \delta)
    :=
    \begin{cases}	
        a P_{\theta}(\delta(X) = d_{0}) & \gamma(\theta) \le \gamma_{0} \\
        b P_{\theta}(\delta(X) = d_{1})  & \gamma(\theta > \gamma_{0} 
    \end{cases}
    \label{chap01_1_4_risk_function}
\end{equation}
$$


## 1.3 Randomizations: Choice of Experiment

## 1.4 Optimum Procedures

## 1.5 Invariance and Unbiasedness

## 1.6 Bayes and Minimax Procedures

## 1.7 Maximum Likelihood

## 1.8 Complete Classes

## 1.9 Sufficient Statistics

## 1.10 Problems

## 1.11 Notes

