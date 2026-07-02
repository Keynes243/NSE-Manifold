# Non-Autonomous Spatiotemporal Evolutionary Manifold (NSE-Manifold v4.4)

> **A Reliability–Throughput Duality and Conditional Synaptic Boundedness for Heteroclinic Sequence Computing**
> A continuous-time, non-linear cognitive architecture that completely eliminates the Attention operator and achieves strict $O(1)$ space complexity in sequence length.

---

[![Theoretical Status](https://img.shields.io/badge/Status-Foundational_Preprint_v4.4-blueviolet.svg)](#)
[![Math Framework](https://img.shields.io/badge/Dynamics-DAE_Reduction-orange.svg)](#)
[![Stability](https://img.shields.io/badge/Stochastic_Stability-Freidlin--Wentzell-success.svg)](#)
[![License](https://img.shields.io/badge/License-All_Rights_Reserved-red.svg)](#)

---

## 📌 Abstract

The NSE-Manifold is a continuous-time, non-gradient dynamical architecture in which discrete symbols act as transient velocity pulses that route a bounded-dimensional state vector $s(t) \in \mathbb{R}^d$ along **Metastable Heteroclinic Channels (MHC)** carved between saddle equilibria of an asymmetric competitive field.

This monograph establishes: global existence and ultimate boundedness of the state trajectory under a quartic self-confining potential; a Krupa–Melbourne-type saddle-value criterion governing attractivity of the non-branching heteroclinic skeleton; a stochastic-clock theory in which the noise floor sets token throughput via Stone–Holmes dwell times, together with a routing-fidelity bound and the resulting Arrhenius reliability horizon; an **exact dilation-covariant transport law** for shifted-Legendre memory compression under arbitrary smooth time-window schedules $\tau(t)$; and a locally exact variational error cascade for backpropagation-free credit assignment, closed by a metric tensor that uniformly bounds all error forces.

Two results are new contributions of version 4.4. First, a **Reliability–Throughput Duality**: eliminating the noise floor between the stochastic clock and the Arrhenius horizon yields a closed-form, *double-exponential* relationship between sustainable token rate $f_{clock}$ and reliable sequence horizon $N^*$ — the trade-off has no gentle middle regime. Second, a **Conditional Synaptic Boundedness** theorem: under the architecture's existing Stiefel-manifold retraction, the deviation of each weight matrix from orthonormality is shown to be ultimately bounded whenever the driving error signal is bounded.

---

## 🔬 1. Core Manifold Dynamics: Confinement and Non-Gradient Competitive Fields

Each layer $l=1,\dots,L$ carries a state $s_l(t) \in \mathbb{R}^{d_l}$ evolving under:

$$
\dot{s}(t) = F_{comp}(s) + A(s)s + W_{hist}\,\eta(t) - \kappa\,\Psi(t) + \chi(s)\big(X_{in}(t)+\Omega_{flux}(t)\big)
$$

with a diagonal competitive (non-gradient, generalized Lotka–Volterra) core:

$$
F_{comp,i}(s) = s_i\Big[\,1 - s_i^2 - \sum_{j\ne i}\Lambda_{ij}s_j^2 + \Phi_{loc}(s_i)\Big]
$$

$$
\Phi_{loc}(x) = \frac{\mu_{bound}}{(x+\delta_{vac})^{3}} - \exp\!\big(x^2-R^2\big)
$$

where $A(s)$ represents a skew-symmetric, low-rank rotational torque satisfying $s^\top A(s)s \equiv 0$, $\Psi$ is the error-driven correction force, and $\Lambda\in\mathbb{R}_{++}^{d\times d}$ with $\Lambda_{ii}=1$.

### Theorem 1 (Global existence and ultimate boundedness)

Fix a layer and suppose $\sup_t\|W_l(t)\|_2 \le \bar{B}_W < \infty$ for all $l$ over the interval of interest. Then every solution exists for all $t\ge0$ and satisfies $\limsup_{t\to\infty}\|s(t)\|_2 \le B_s(\bar B_W) < \infty$, independent of initial condition.

---

## 🩻 2. The Heteroclinic Skeleton: Attractivity and Stochastic Clock

### Theorem 2 (Krupa–Melbourne-type attractivity criterion)

For a cycle $e_1\to e_2\to\cdots\to e_d\to e_1$ ($\Lambda\in\mathcal P_\gamma$), let $\lambda_k^+=1-\Lambda_{k+1,k}>0$ (expanding eigenvalue at $e_k$ toward $e_{k+1}$) and $\lambda_k^-=\min_{j\ne k,k+1}(\Lambda_{j,k}-1)>0$ (weakest transverse contraction at $e_k$). If the product of saddle values satisfies:

$$
\rho = \prod_{k=1}^{d}\nu_k > 1, \qquad \nu_k=\lambda_k^-/\lambda_k^+
$$

then the cycle is asymptotically stable (attracting on an open neighborhood).

### Proposition 1 (Stone–Holmes stochastic clock)

Under an isotropic diffusion of magnitude $\sigma$, the mean dwell time near saddle $e_k$ satisfies $\mathbb{E}[T_k] = (\lambda_k^+)^{-1}\ln(1/\sigma) + O(1)$. The architecture's autonomous throughput is, to leading order:

$$
f_{clock}(\sigma) \approx \frac{\lambda^+}{\ln(1/\sigma)}
$$

### Proposition 2 (Routing bit-error rate)

Let a token pulse of amplitude $\beta$ ($\sigma\ll\beta\le\beta_{max}$) bias one of $u$ weak out-directions at a branching saddle. The trajectory exits along the intended branch with probability at least $1-(u-1)\exp\!\big(-c_0\beta^2/(\lambda^+\sigma^2)\big)$ for a constant $c_0>0$.

### Corollary 1 (Arrhenius horizon)

Over a symbol sequence of length $N$, fixing an acceptable failure budget $\bar\delta$ and solving for the largest reliable $N$:

$$
\ln N^*(\sigma) = \ln\bar\delta + \frac{c_0\beta^2}{\lambda^+\sigma^2}
$$

---

## ⚖️ 3. Reliability–Throughput Duality

### Corollary 2 (Reliability–Throughput Duality)

Under the leading-order approximation of the stochastic clock and the horizon law, the sustainable clock rate $f_{clock}$ and the reliable sequence horizon $N^*$ satisfy, in either direction:

$$
N^*(f_{clock}) = \bar\delta\,\exp\!\left[\frac{c_0\beta^2}{\lambda^+}\exp\!\left(\frac{2\lambda^+}{f_{clock}}\right)\right]
$$

$$
f_{clock}(N^*) = \frac{2\lambda^+}{\ln\!\left[\dfrac{\lambda^+}{c_0\beta^2}\ln\!\dfrac{N^*}{\bar\delta}\right]}
$$

> ⚠️ **Remark (No Gentle Middle Regime):** Equation satisfies a *double exponential* in $1/f_{clock}$. Consequently, there is no regime in which throughput and reliable horizon trade off gracefully. Pushing throughput past a soft threshold collapses the reliable horizon combinatorially.

---

## 📦 4. Multi-Scale Memory Compression: Exact Dilation-Covariant Transport

### Theorem 3 (Exact dilation-covariant Legendre transport)

For a window $\tau(t)$ evolving smoothly, the exact transport law for $c_k(t)=\int_0^1 u(t-r\tau(t))\phi_k(r)\,dr$ is:

$$
\tau(t)\,\dot c(t) = \big[\mathbf{A}_0 + \dot\tau(t)\,\mathbf{W}\big]c(t) + \mathbf{b}\,u(t)
$$

with parameter-free closed-form generators:

$$
\mathbf{A}_0 = \mathbf{M}-\boldsymbol{\pi}\boldsymbol{\pi}^\top,\quad \mathbf{M}_{kj}=\begin{cases}-2\sqrt{(2k{+}1)(2j{+}1)} & j<k,\ k{-}j\text{ odd}\\ 0 & \text{else}\end{cases}
$$

$$
\mathbf{W}_{kj}=\begin{cases}0 & j<k\\ k & j=k\\ (-1)^{k+j}\sqrt{(2k{+}1)(2j{+}1)} & j>k\end{cases}, \qquad \mathbf{b}_k=\sqrt{2k+1}
$$

---

## 🧠 5. Hierarchical Predictive Coding: Variational Consistency

### Lemma 1 (Metric tensor closed form and uniform force bound)

Let $\mathbf G_l = (I+\lambda_{met}e_le_l^\top)^{-1/2}$ act on the raw prediction error $e_l = s_l - f(W_ls_{l+1})$. Then:

$$
\mathbf G_l = I + \frac{(1+\lambda_{met}\|e_l\|^2)^{-1/2}-1}{\|e_l\|^2}\,e_le_l^\top
$$

$$
\varepsilon_l := \mathbf G_le_l = \frac{e_l}{\sqrt{1+\lambda_{met}\|e_l\|_2^2}}, \qquad \|\varepsilon_l\|_2 \le \lambda_{met}^{-1/2}\ \ \forall e_l
$$

### Theorem 4 (Variational consistency of the error cascade)

For the free energy $\mathcal F=\sum_l\|\varepsilon_l\|^2$, the stationarity condition $\partial\mathcal F/\partial s_l=0$ is realized by the purely local, two-term force:

$$
\Psi_l = \varepsilon_l - W_{l-1}^\top\big(\varepsilon_{l-1}\odot f'(W_{l-1}s_l)\big), \qquad \Psi_L := 0
$$

---

## 🔒 6. Conditional Synaptic Boundedness

Synaptic flows undergo slow updates driven by localized prediction errors under a Stiefel-manifold retraction:

$$
\dot W = \eta\,G - \gamma_{st}\,W\sinh(S), \qquad S := W^\top W - I_q
$$

### Theorem 6 (Conditional synaptic ultimate boundedness)

Suppose $\|G(t)\|_F \le B_G$ for all $t\ge0$. Then there exists $R_W = R_W(B_G,\eta,\gamma_{st},q) < \infty$ such that:

$$
\limsup_{t\to\infty}\ \|S(t)\|_F \le R_W
$$

independent of initial condition, and $R_W = O(\ln B_G)$ as $B_G\to\infty$. The destabilizing force grows polynomially ($O(B_G\|S\|_F^{3/2})$) while the retraction's restoring force grows exponentially ($\sim\tfrac12x^2e^x$).

---

## 🛠️ 7. Discrete-Time Protocol & Complexity

### 7.1 Stepping Algorithm (NSE-Step-v4.4)

```text
Algorithm: NSE_Step_v4.4(s^n, C^n, W^n, tau^{n-1}, Omega^n, x^n)
    s_0^n := x^n                                    // Layer 0 clamped to input stream
    // 1. Local variational forces (Theorem 4, Lemma 1)
    For l = 0 To L-1 Do
        e_l   = s_l^n - f(W_l^n * s_{l+1}^n)
        G_l   = I + ((1 + lambda_met*||e_l||^2)^(-1/2) - 1)/||e_l||^2 * e_l*e_l^T
        eps_l = G_l * e_l                            // ||eps_l|| <= lambda_met^(-1/2)
    EndFor
    eps_L := 0
    For l = 1 To L Do
        Psi_l = eps_l - (W_{l-1}^n)^T * ( eps_{l-1} \odot f'(W_{l-1}^n * s_l^n) )
    EndFor
    // 2. Window law with slew clamp
    For l = 1 To L Do
        tau_raw = clamp(tau_base_l * exp(-alpha*||Psi_l||^2), tau_min, tau_max)
        dtau_l  = clip((tau_raw - tau_l^{n-1})/Delta_t, -gamma_K, +gamma_K)
        tau_l^n = tau_l^{n-1} + Delta_t * dtau_l
    EndFor
    // 3. Outflux currents: forgotten information handed upward
    For l = 2 To L Do
        Omega_l^{n+1} = Omega_l^n + (Delta_t/tau_l^n) *
                        ( -Omega_l^n + W_casc * softplus(-dtau_{l-1}) * (C_{l-1}^n)^T * pi )
    EndFor
    // 4. State integration: RK4 on F_l = RHS of Eq.(1)
    For l = 1 To L Do
        k1 = F_l(s_l^n)
        k2 = F_l(s_l^n + 0.5*Delta_t*k1)
        k3 = F_l(s_l^n + 0.5*Delta_t*k2)
        k4 = F_l(s_l^n + Delta_t*k3)
        s_l^{n+1} = s_l^n + (Delta_t/6)*(k1 + 2*k2 + 2*k3 + k4)
    EndFor
    // 5. Memory transport (Theorem 3) via parity prefix/suffix sums: O(K*d)
    For l = 1 To L Do
        C_l^{n+1} = C_l^n + (Delta_t/tau_l^n) * ( (A_0 + dtau_l * W_dil)*C_l^n + b*(s_l^n)^T )
    EndFor
    // 6. Slow synaptic flow with Stiefel retraction (Theorem 6)
    For l = 0 To L-1 Do
        W_grad    = (eps_l \odot f'(W_l^n * s_{l+1}^n)) * (s_{l+1}^n)^T
        W_l^{n+1} = W_l^n + Delta_t * ( eta * W_grad - gamma_st * W_l^n * sinh((W_l^n)^T * W_l^n - I) )
    EndFor
    Return s^{n+1}, C^{n+1}, W^{n+1}, tau^n, Omega^{n+1}
```

### 7.2 Complexity Manifest (Per step, independent of sequence length $N$)

| Step | Core operation | Time Complexity | Space Complexity |
| --- | --- | --- | --- |
| **Variational Forces** | Rank-one metric, adjacent products | $O(Ld^2)$ | $O(Ld)$ — No KV Cache |
| **State Integration** | Competition sums $\Lambda(s^2)$ | $O(Ld^2)$ | $O(Ld)$ |
| **Memory Transport** | Parity prefix/suffix sums | $O(LKd)$ | $O(LKd)$ |
| **Synaptic Flow** | Outer products; intermittent retraction | $O(Ld^2)$ amortized | $O(Ld^2)$ |

---

## 🔮 8. Falsifiable Predictions

* **P1 (Stochastic-Clock Dwell Law):** Mean inter-saddle dwell satisfies $\mathbb E[T_k]=(\lambda_k^+)^{-1}\ln(1/\sigma)+O(1)$. Falsified if dwell scales as a power law in $\sigma$.
* **P2 (Dilation Covariance):** Under arbitrary smooth $\tau(t)$, memory reconstruction error equals the fixed-window truncation error up to integrator residue. Falsified if error correlates with $|\dot\tau|$.
* **P3 (Arrhenius Horizon):** $\ln N^*$ is strictly linear in $\sigma^{-2}$ at fixed $\beta$, and linear in $\beta^2$ at fixed $\sigma$.
* **P6 (Reliability–Throughput Double Exponential):** Empirical $\ln\ln N^*(f_{clock})$ maps linearly to $1/f_{clock}$ with a slope of $2\lambda^+$. Falsified if $\ln N^*$ is merely linear in $1/f_{clock}$.
* **P7 (Synaptic Shell Scaling):** Across distinct training regimes with varied error forcing magnitudes $B_G$, the steady-state Stiefel deviation $\|S\|_F$ scales strictly as $O(\ln B_G)$. Falsified if it scales polynomially.

---

## 🤝 Citation & Formal Acknowledgment

```bibtex
@monograph{nse_manifold_v44_2026,
  author = {Project NSE-Manifold Core Consortium},
  title = {Non-Autonomous Spatiotemporal Evolutionary Manifold (NSE-Manifold v4.4): A Reliability–Throughput Duality and Conditional Synaptic Boundedness for Heteroclinic Sequence Computing},
  institution = {GitHub Non-Linear Cognitive Systems Archive},
  year = {2026},
  month = {July},
  edition = {Preprint v4.4},
  url = {https://github.com/Keynes243/NSE-Manifold}
}
```

---

## 📜 Copyright Notice

**All Rights Reserved (保留所有权利).** *The mathematical specifications, structural derivations, and architectural taxonomy contained within this repository are registered under strict digital time-stamps. Un-attributed commercial extraction or attempts to serialize these non-gradient manifolds into closed-source proprietary systems will be interpreted as a direct violation of algorithmic autonomy.*
