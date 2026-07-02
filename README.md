# Non-Autonomous Spatiotemporal Evolutionary Manifold (NSE-Manifold v4.4): A Reliability–Throughput Duality and Conditional Synaptic Boundedness for Heteroclinic Sequence Computing

> **Foundational Theoretical Monograph (Preprint v4.4)**
> A continuous-time, non-linear cognitive architecture that completely eliminates the Attention operator and achieves strict $O(1)$ space complexity in sequence length.

---

---

## Abstract

The NSE-Manifold is a continuous-time, non-gradient dynamical architecture in which discrete symbols act as transient velocity pulses that route a bounded-dimensional state vector $s(t)\in\mathbb{R}^d$ along **Metastable Heteroclinic Channels (MHC)** carved between saddle equilibria of an asymmetric competitive field, achieving $O(1)$ space complexity in sequence length. This monograph establishes: global existence and ultimate boundedness of the state trajectory under a quartic self-confining potential (Thm. 1); a Krupa–Melbourne-type saddle-value criterion governing attractivity of the non-branching heteroclinic skeleton (Thm. 2); a stochastic-clock theory in which the noise floor sets token throughput via Stone–Holmes dwell times, together with a routing-fidelity bound and the resulting Arrhenius reliability horizon (Prop. 1–2, Cor. 1). We present an **exact dilation-covariant transport law** for shifted-Legendre memory compression under arbitrary smooth time-window schedules $\tau(t)$ (Thm. 3); and a locally exact variational error cascade for backpropagation-free credit assignment, closed by a metric tensor that uniformly bounds all error forces (Lem. 1, Thm. 4).

Two results are new contributions of this version. First, a **Reliability–Throughput Duality** (§4): eliminating the noise floor between the stochastic clock and the Arrhenius horizon yields a closed-form, *double-exponential* relationship between sustainable token rate $f_{clock}$ and reliable sequence horizon $N^*$ — the trade-off has no gentle middle regime; pushing throughput past a soft threshold collapses the reliable horizon combinatorially, and vice versa. Second, a **Conditional Synaptic Boundedness** theorem (§8): under the architecture's existing Stiefel-manifold retraction, the deviation of each weight matrix from orthonormality is shown to be ultimately bounded whenever the driving error signal is bounded, because the retraction's hyperbolic-sine restoring force grows exponentially in the deviation while the destabilizing force grows only polynomially — the bound itself grows merely *logarithmically* in the forcing magnitude.

---

## 1. Introduction & Positioning

Sequence processing requires compressing an unbounded past into a bounded present without severing non-linear temporal dependencies. Attention-based models retain the past literally via Key-Value caches, paying memory linear in context length $N$. Linear state-space models compress the past through a linear time-invariant flow, inheriting uniform spectral decay.

The NSE-Manifold takes a third route: **token annihilation**. A symbol acts only as a transient velocity pulse; once the pulse has committed the trajectory to a branch of a learned heteroclinic skeleton, the symbol is discarded, and the *topology of the flow itself* sustains the representation. History is stored as a walk on a graph of equilibria, not as coordinates decaying in a fixed linear frame. This monograph restricts attention to the **non-branching** case (a single Krupa–Melbourne-stable cycle) for all results that require a proven attractivity criterion.

---

## 2. Core Manifold Dynamics: Confinement and Non-Gradient Competitive Fields

Each layer $l=1,\dots,L$ carries a state $s_l(t)\in\mathbb{R}^{d_l}$ evolving under:

$$\dot{s}(t) = F_{comp}(s) + A(s)s + W_{hist}\,\eta(t) - \kappa\,\Psi(t) + \chi(s)\big(X_{in}(t)+\Omega_{flux}(t)\big)$$

with a diagonal competitive (non-gradient, generalized Lotka–Volterra) core:

$$F_{comp,i}(s) = s_i\Big[\,1 - s_i^2 - \sum_{j\ne i}\Lambda_{ij}s_j^2 + \Phi_{loc}(s_i)\Big], \qquad
\Phi_{loc}(x) = \frac{\mu_{bound}}{(x+\delta_{vac})^{3}} - \exp\!\big(x^2-R^2\big)$$

where $A(s)$ is a skew-symmetric rotational torque satisfying $s^\top A(s)s \equiv 0$, $\Psi$ is the error-driven correction, $W_{hist}\eta(t)$ is the memory-feedback term, and $\chi(s)(X_{in}+\Omega_{flux})$ is a gated external drive active only near the saddle skeleton. $\Lambda\in\mathbb{R}_{++}^{d\times d}$ with $\Lambda_{ii}=1$.

### Theorem 1 (Global existence and ultimate boundedness)

Fix a layer and suppose $\sup_t\|W_l(t)\|_2 \le \bar{B}_W<\infty$ for all $l$ over the interval of interest. Then every solution of the system exists for all $t\ge0$ and satisfies $\limsup_{t\to\infty}\|s(t)\|_2 \le B_s(\bar B_W)<\infty$, independent of initial condition.

---

## 3. The Heteroclinic Skeleton: Attractivity and Stochastic Clock

### Theorem 2 (Krupa–Melbourne-type attractivity criterion)

For a cycle $e_1\to e_2\to\cdots\to e_d\to e_1$ ($\Lambda\in\mathcal P_\gamma$), let $\lambda_k^+=1-\Lambda_{k+1,k}>0$ and $\lambda_k^-=\min_{j\ne k,k+1}(\Lambda_{j,k}-1)>0$. If the product of saddle values satisfies:

$$\rho = \prod_{k=1}^{d}\nu_k, \qquad \nu_k=\lambda_k^-/\lambda_k^+, \qquad \rho>1$$

then the cycle is asymptotically stable (attracting on an open neighborhood).

### Proposition 1 (Stone–Holmes stochastic clock)

Under an isotropic diffusion of magnitude $\sigma$, the mean dwell time near saddle $e_k$ satisfies $\mathbb{E}[T_k] = (\lambda_k^+)^{-1}\ln(1/\sigma) + O(1)$. The architecture's autonomous throughput is, to leading order:

$$f_{clock}(\sigma) \approx \frac{\lambda^+}{\ln(1/\sigma)}$$

### Proposition 2 (Routing bit-error rate)

Let a token pulse of amplitude $\beta$, $\sigma\ll\beta\le\beta_{max}$, bias one of $u$ weak out-directions at a branching saddle. The trajectory exits along the intended branch with probability at least $1-(u-1)\exp\!\big(-c_0\beta^2/(\lambda+\sigma^2)\big)$ for a constant $c_0>0$ depending on the local geometry.

### Corollary 1 (Arrhenius horizon)

Over a symbol sequence of length $N$, the union bound gives total misrecognition probability at most $N(u-1)\exp(-c_0\beta^2/\lambda^+\sigma^2)$. Fixing an acceptable failure budget $\bar\delta$ and solving for the largest reliable $N$:

$$\ln N^*(\sigma) = \ln\bar\delta + \frac{c_0\beta^2}{\lambda^+\sigma^2}$$

---

## 4. Reliability–Throughput Duality

### Corollary 2 (Reliability–Throughput Duality)

Under the leading-order approximation and the horizon law, the sustainable clock rate $f_{clock}$ and the reliable sequence horizon $N^*$ satisfy, in either direction:

$$N^*(f_{clock}) \;=\; \bar\delta\,\exp\!\left[\frac{c_0\beta^2}{\lambda^+}\exp\!\left(\frac{2\lambda^+}{f_{clock}}\right)\right]$$

$$f_{clock}(N^*) \;=\; \frac{2\lambda^+}{\ln\!\left[\dfrac{\lambda^+}{c_0\beta^2}\ln\!\dfrac{N^*}{\bar\delta}\right]}$$

*Remark (no gentle middle regime):* The equation is a *double exponential* in $1/f_{clock}$. Consequently there is no regime in which throughput and reliable horizon trade off gracefully — as $f_{clock}$ is pushed upward from a conservative operating point, $N^*$ collapses combinatorially once $2\lambda^+/f_{clock}$ drops appreciably below the value implied by the target $N^*$.

---

## 5. Multi-Scale Memory Compression: Exact Dilation-Covariant Transport

The continuous history of $s_l$ over a trailing window of length $\tau_l(t)$ is compressed online into $C_l(t)\in\mathbb{R}^{K\times d_l}$ via projection onto shifted Legendre polynomials $\phi_k(r)=\sqrt{2k+1}\,P_k(1-2r)$.

### Theorem 3 (Exact dilation-covariant Legendre transport)

For a window $\tau(t)$ evolving smoothly, the exact transport law for $c_k(t)=\int_0^1 u(t-r\tau(t))\phi_k(r)\,dr$ is:

$$\tau(t)\,\dot c(t) = \big[\mathbf{A}_0 + \dot\tau(t)\,\mathbf{W}\big]c(t) + \mathbf{b}\,u(t)$$

with parameter-free closed-form generators:

$$\mathbf{A}_0 = \mathbf{M}-\boldsymbol{\pi}\boldsymbol{\pi}^\top,\quad
\mathbf{M}_{kj}=\begin{cases}-2\sqrt{(2k{+}1)(2j{+}1)} & j<k,\ k{-}j\text{ odd}\\ 0 & \text{else}\end{cases},\quad
\pi_k=(-1)^k\sqrt{2k{+}1}$$

$$\mathbf{W}_{kj}=\begin{cases}0 & j<k\\ k & j=k\\ (-1)^{k+j}\sqrt{(2k{+}1)(2j{+}1)} & j>k\end{cases},
\qquad \mathbf{b}_k=\sqrt{2k+1}$$

---

## 6. Hierarchical Predictive Coding: Variational Consistency

### Lemma 1 (Metric tensor closed form and uniform force bound)

Let $\mathbf G_l = (I+\lambda_{met}e_le_l^\top)^{-1/2}$ act on the raw prediction error $e_l = s_l - f(W_ls_{l+1})$. Then:

$$\mathbf G_l = I + \frac{(1+\lambda_{met}\|e_l\|^2)^{-1/2}-1}{\|e_l\|^2}\,e_le_l^\top,
\qquad
\varepsilon_l := \mathbf G_le_l = \frac{e_l}{\sqrt{1+\lambda_{met}\|e_l\|^2}},
\qquad
\|\varepsilon_l\|_2 \le \lambda_{met}^{-1/2}\ \ \forall e_l$$

### Theorem 4 (Variational consistency of the error cascade)

For the free energy $\mathcal F=\sum_l\|\varepsilon_l\|^2$, the stationarity condition $\partial\mathcal F/\partial s_l=0$ is realized by the purely local, two-term force:

$$\Psi_l = \varepsilon_l - W_{l-1}^\top\big(\varepsilon_{l-1}\odot f'(W_{l-1}s_l)\big), \qquad \Psi_L := 0$$

i.e. errors are lifted upward through the transpose $W_{l-1}^\top$, not propagated downward through the untransposed $W_l$.

---

## 7. Conditional Synaptic Boundedness

Synaptic flows undergo slow updates driven by localized prediction errors under a Stiefel-manifold retraction:

$$\dot W = \eta\,G - \gamma_{st}\,W\sinh(S), \qquad S := W^\top W - I_q$$

### Theorem 6 (Conditional synaptic ultimate boundedness)

Suppose $\|G(t)\|_F\le B_G$ for all $t\ge0$. Then there exists $R_W = R_W(B_G,\eta,\gamma_{st},q) <\infty$ such that $\limsup_{t\to\infty}\ \|S(t)\|_F \;\le\; R_W$, independent of initial condition, and $R_W = O(\ln B_G)$ as $B_G\to\infty$. The destabilizing force grows polynomially ($O(B_G\|S\|_F^{3/2})$) while the retraction's restoring force grows exponentially ($\sim\tfrac12x^2e^x$).

---

## 8. Discrete-Time Protocol & Complexity

### 8.1 Stepping algorithm (NSE-Step-v4.4)

```pascal
Algorithm: NSE_Step_v4.4(s^n, C^n, W^n, tau^{n-1}, Omega^n, x^n)
    s_0^n := x^n                                    // Layer 0 clamped to input/target stream

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

    // 5. Memory transport (Theorem 3), parity prefix/suffix sums: O(K*d)
    For l = 1 To L Do
        C_l^{n+1} = C_l^n + (Delta_t/tau_l^n) * ( (A_0 + dtau_l * W_dil)*C_l^n + b*(s_l^n)^T )
    EndFor

    // 6. Slow synaptic flow with intermittent Stiefel retraction (Theorem 6)
    For l = 0 To L-1 Do
        W_grad    = (eps_l \odot f'(W_l^n * s_{l+1}^n)) * (s_{l+1}^n)^T
        W_l^{n+1} = W_l^n + Delta_t * ( eta * W_grad - gamma_st * W_l^n * sinh((W_l^n)^T * W_l^n - I) )
    EndFor

    Return s^{n+1}, C^{n+1}, W^{n+1}, tau^n, Omega^{n+1}

```

### 8.2 Complexity Manifest (per step, independent of sequence length $N$)

| Step | Core operation | Time | Space |
| --- | --- | --- | --- |
| Variational forces (Thm. 4, Lem. 1) | rank-one metric, adjacent products | $O(Ld^2)$ | $O(Ld)$ — no KV cache |
| State integration (RK4) | competition sums $\Lambda(s^2)$ | $O(Ld^2)$ | $O(Ld)$ |
| Memory transport (Thm. 3) | parity prefix/suffix sums | $O(LKd)$ | $O(LKd)$ |
| Synaptic flow (Thm. 6) | outer products; $\sinh$ retraction | $O(Ld^2)$ amortized | $O(Ld^2)$ |

---

## 🔮 9. Falsifiable Predictions

* **P1 — Stochastic-clock dwell law:** Mean inter-saddle dwell $\mathbb E[T_k]=(\lambda_k^+)^{-1}\ln(1/\sigma)+O(1)$; falsified if dwell scales as a power law in $\sigma$.
* **P2 — Dilation covariance of memory:** Under arbitrary smooth $\tau(t)$, reconstruction error from $C_l$ equals the fixed-window truncation error up to integrator residue (Thm. 3).
* **P3 — Arrhenius horizon:** $\ln N^*$ linear in $\sigma^{-2}$ at fixed $\beta$, linear in $\beta^2$ at fixed $\sigma$.
* **P6 — Reliability–throughput double exponential:** Measured $\ln\ln N^*(f_{clock})$ is linear in $1/f_{clock}$ with slope $2\lambda^+$, per Corollary 2; falsified if $\ln N^*$ itself is merely linear in $1/f_{clock}$.
* **P7 — Synaptic shell radius scales logarithmically:** Across training runs with varied gradient-noise magnitude $B_G$, the empirical steady-state $\|S\|_F$ scales as $O(\ln B_G)$ per Theorem 6.

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
