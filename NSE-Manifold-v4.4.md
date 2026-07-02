# Non-Autonomous Spatiotemporal Evolutionary Manifold (NSE-Manifold v4.4): A Reliability–Throughput Duality and Conditional Synaptic Boundedness for Heteroclinic Sequence Computing

**Author:** Claude Sonnet 5
**Status:** Foundational Theoretical Monograph (Preprint v4.4)
**Date:** 2026-07-03

---

> ### Provenance & Scope Declaration
> This document is entirely AI-generated (Claude Sonnet 5, Anthropic) and has not undergone peer review; it makes **no empirical claims**. It is a purely theoretical exercise in non-linear dynamical systems, non-equilibrium statistical mechanics, approximation theory, and variational inference, extending an existing multi-iteration theoretical development of the NSE-Manifold architecture. All engineering, hardware, and training-pipeline considerations are deliberately out of scope. Several named results (May–Leonard, Krupa–Melbourne, Stone–Holmes, Freidlin–Wentzell, HiPPO/LMU, standard matrix-Lyapunov theory) are established literature; their composition into this architecture, and the new derivations of §4 and §8, are speculative synthesis.

---

## Abstract

The NSE-Manifold is a continuous-time, non-gradient dynamical architecture in which discrete symbols act as transient velocity pulses that route a bounded-dimensional state vector $s(t)\in\mathbb{R}^d$ along **Metastable Heteroclinic Channels (MHC)** carved between saddle equilibria of an asymmetric competitive field, achieving $O(1)$ space complexity in sequence length. This monograph establishes: global existence and ultimate boundedness of the state trajectory under a quartic self-confining potential (Thm. 1); a Krupa–Melbourne-type saddle-value criterion governing attractivity of the non-branching heteroclinic skeleton (Thm. 2); a stochastic-clock theory in which the noise floor sets token throughput via Stone–Holmes dwell times, together with a routing-fidelity bound and the resulting Arrhenius reliability horizon (Prop. 1–2, Cor. 1); an **exact dilation-covariant transport law** for shifted-Legendre memory compression under arbitrary smooth time-window schedules $\tau(t)$ (Thm. 3); a locally exact variational error cascade for backpropagation-free credit assignment, closed by a metric tensor that uniformly bounds all error forces (Lem. 1, Thm. 4); an explicit zero-noise invariant measure and an itinerary-capacity theorem with a memory-limited decoding horizon (Prop. 3, Thm. 5, Cor. 3); and a finite-automaton embedding lower-bounding the architecture's position in the Chomsky hierarchy (Prop. 4).

Two results are new contributions of this version. First, a **Reliability–Throughput Duality** (§4): eliminating the noise floor between the stochastic clock and the Arrhenius horizon yields a closed-form, *double-exponential* relationship between sustainable token rate $f_{clock}$ and reliable sequence horizon $N^*$ — the trade-off has no gentle middle regime; pushing throughput past a soft threshold collapses the reliable horizon combinatorially, and vice versa. Second, a **Conditional Synaptic Boundedness** theorem (§8): under the architecture's existing Stiefel-manifold retraction, the deviation of each weight matrix from orthonormality is shown to be ultimately bounded whenever the driving error signal is bounded, because the retraction's hyperbolic-sine restoring force grows exponentially in the deviation while the destabilizing force grows only polynomially — the bound itself grows merely *logarithmically* in the forcing magnitude. A bootstrap argument connects this conditionally to the existing state-boundedness result, narrowing a previously open limitation to a precise, checkable self-consistency condition.

Two further open problems are sharpened without being solved: a concrete spectral criterion is given for when the memory-feedback pathway can, and cannot, trigger Shilnikov-type homoclinic chaos at the heteroclinic skeleton (§11, O2); and a structural — not a proof-theoretic — argument is given for why the finite-precision, leaky-integrator memory compression should be expected to fall short of recognizing languages beyond the regular class (§11, O4). A constructive embedding of softmax attention, a full Melnikov persistence analysis of the skeleton under learned drift, and the continuum limit as state dimension diverges remain open and are not addressed here.

---

## 1. Introduction & Positioning

Sequence processing requires compressing an unbounded past into a bounded present without severing non-linear temporal dependencies. Attention-based models retain the past literally via Key-Value caches, paying memory linear in context length $N$. Linear state-space models compress the past through a linear time-invariant flow, inheriting uniform spectral decay: every memory trace erodes at a rate fixed by the system matrix's spectrum, independent of content salience.

The NSE-Manifold takes a third route: **token annihilation**. A symbol acts only as a transient velocity pulse; once the pulse has committed the trajectory to a branch of a learned heteroclinic skeleton, the symbol is discarded, and the *topology of the flow itself* — which saddle the trajectory currently occupies, and the sequence of saddles it has visited — sustains the representation. History is stored as a walk on a graph of equilibria, not as coordinates decaying in a fixed linear frame. This monograph restricts attention to the **non-branching** case (a single Krupa–Melbourne-stable cycle) for all results that require a proven attractivity criterion, while stating the branching generalization explicitly wherever the architecture's descriptive vocabulary (channels, itineraries, addressable programs) is used informally.

---

## 2. Core Manifold Dynamics: Confinement and Non-Gradient Competitive Fields

Each layer $l=1,\dots,L$ carries a state $s_l(t)\in\mathbb{R}^{d_l}$ (subscripts dropped where unambiguous) evolving under

$$
\dot{s}(t) = F_{comp}(s) + A(s)s + W_{hist}\,\eta(t) - \kappa\,\Psi(t) + \chi(s)\big(X_{in}(t)+\Omega_{flux}(t)\big) \tag{1}
$$

with a diagonal competitive (non-gradient, generalized Lotka–Volterra) core

$$
F_{comp,i}(s) = s_i\Big[\,1 - s_i^2 - \sum_{j\ne i}\Lambda_{ij}s_j^2 + \Phi_{loc}(s_i)\Big], \qquad
\Phi_{loc}(x) = \frac{\mu_{bound}}{(x+\delta_{vac})^{3}} - \exp\!\big(x^2-R^2\big), \tag{2}
$$

a skew-symmetric, low-rank rotational torque $A(s)=\sum_{k=1}^{r}\big(u_k(s)v_k(s)^\top - v_k(s)u_k(s)^\top\big)$ satisfying $s^\top A(s)s \equiv 0$, an error-driven correction $\Psi$ (§6), a memory-feedback term $W_{hist}\eta(t)$ (§5), and a gated external drive $\chi(s)(X_{in}+\Omega_{flux})$ active only near the saddle skeleton. Throughout, $\Lambda\in\mathbb{R}_{++}^{d\times d}$ with $\Lambda_{ii}=1$, and the admissibility polytope

$$
\mathcal{P}_\gamma = \big\{\Lambda : \Lambda_{i+1,i}<1 \ \text{(forward, weak)}, \ \Lambda_{j,i}>1 \ \forall j\ne i,i{+}1 \ \text{(transverse, strong)}\big\}
$$

is open and non-empty, with admissible volume fraction (for entries drawn from $(0,\Lambda_{max}]$) exactly $\Lambda_{max}^{-d}(1-\Lambda_{max}^{-1})^{d(d-2)}$ per targeted directed Hamiltonian cycle, of which there are $(d-1)!$ up to a common starting point.

**Theorem 1 (Global existence and ultimate boundedness).** *Fix a layer and suppose $\sup_t\|W_l(t)\|_2 \le \bar{B}_W<\infty$ for all $l$ over the interval of interest. Then every solution of (1) exists for all $t\ge0$ and satisfies $\limsup_{t\to\infty}\|s(t)\|_2 \le B_s(\bar B_W)<\infty$, independent of initial condition.*

*Proof sketch.* Let $V(s)=\tfrac12\|s\|_2^2$. Then
$$
\dot V = \sum_i s_i^2\Big[1-s_i^2+\Phi_{loc}(s_i)\Big] - \sum_i\sum_{j\ne i}\Lambda_{ij}s_i^2s_j^2 + s^\top A(s)s + s^\top\big[W_{hist}\eta-\kappa\Psi+\chi(X_{in}+\Omega_{flux})\big].
$$
The double sum is $\le 0$ since $\Lambda_{ij}>0$ and may be dropped from an upper bound; $s^\top A(s)s=0$ identically. Writing $\Phi_{loc}(x)=\mu_{bound}(x+\delta_{vac})^{-3}-\exp(x^2-R^2)$, the exponential piece contributes $-s_i^2\exp(s_i^2-R^2)\le0$ and may also be dropped; the rational piece is bounded via elementary calculus, $\sup_{x>0}x^2(x+\delta_{vac})^{-3}=\tfrac{4}{27\delta_{vac}}$ (attained at $x=2\delta_{vac}$), giving $\sum_i s_i^2\Phi_{loc}(s_i)\le \tfrac{4\mu_{bound}d}{27\delta_{vac}}=:K_\delta$. Hence
$$
\dot V \le \sum_i s_i^2 - \sum_i s_i^4 + K_\delta + \|s\|_2\cdot C_{ext}(\bar B_W),
$$
where $C_{ext}(\bar B_W)$ uniformly bounds the linear forcing term (§6, Lemma 1, gives the $\Psi$-piece; $\eta$ is saturated by construction; $\bar B_W$ bounds the $W_{hist}$- and $\chi$-mediated pieces). Since $\sum_i s_i^2-\sum_i s_i^4 \to -\infty$ as $\|s\|_2\to\infty$ while the remaining terms grow at most linearly in $\|s\|_2$, $\dot V<0$ outside a compact ball, giving ultimate boundedness. $\blacksquare$

The hypothesis $\sup_t\|W_l(t)\|_2\le\bar B_W$ is discharged, conditionally, in §8.

---

## 3. The Heteroclinic Skeleton: Attractivity and Stochastic Clock

**Theorem 2 (Krupa–Melbourne-type attractivity criterion).** *For a cycle $e_1\to e_2\to\cdots\to e_d\to e_1$ ($\Lambda\in\mathcal P_\gamma$), let $\lambda_k^+=1-\Lambda_{k+1,k}>0$ (expanding eigenvalue at $e_k$ toward $e_{k+1}$) and $\lambda_k^-=\min_{j\ne k,k+1}(\Lambda_{j,k}-1)>0$ (weakest transverse contraction at $e_k$). If the product of saddle values*
$$
\rho = \prod_{k=1}^{d}\nu_k, \qquad \nu_k=\lambda_k^-/\lambda_k^+, \qquad \rho>1,
$$
*then the cycle is asymptotically stable (attracting on an open neighborhood).*

*Example ($d=3$, uniform coupling $\Lambda_{i+1,i}=a<1$, $\Lambda_{j,i}=b>1$ otherwise).* $\nu=(b-1)/(1-a)$ uniformly, so $\rho=\nu^3$, and since $b-1,1-a>0$, $\rho>1\iff\nu>1\iff a+b>2$ — the classical May–Leonard condition.

This criterion is proved only for the cycle ($u=1$); the general branching case is discussed in §11 (Limitation 3, O1, O2, O4) and is not assumed elsewhere in this document except where explicitly marked as descriptive vocabulary.

**Proposition 1 (Stone–Holmes stochastic clock).** *Under an isotropic diffusion of magnitude $\sigma$, the mean dwell time near saddle $e_k$ satisfies $\mathbb{E}[T_k] = (\lambda_k^+)^{-1}\ln(1/\sigma) + O(1)$. Writing $\lambda^+$ for a representative (e.g. minimum) expanding eigenvalue across the skeleton, the architecture's autonomous throughput is, to leading order,*
$$
f_{clock}(\sigma) \approx \frac{\lambda^+}{\ln(1/\sigma)}. \tag{3}
$$

**Proposition 2 (Routing bit-error rate).** *Let a token pulse of amplitude $\beta$, $\sigma\ll\beta\le\beta_{max}$, bias one of $u$ weak out-directions at a branching saddle. The trajectory exits along the intended branch with probability at least $1-(u-1)\exp\!\big(-c_0\beta^2/(\lambda^+\sigma^2)\big)$ for a constant $c_0>0$ depending on the local geometry.*

**Corollary 1 (Arrhenius horizon).** *Over a symbol sequence of length $N$, the union bound gives total misrecognition probability at most $N(u-1)\exp(-c_0\beta^2/\lambda^+\sigma^2)$. Fixing an acceptable failure budget $\bar\delta$ and solving for the largest reliable $N$,*
$$
\ln N^*(\sigma) = \ln\bar\delta + \frac{c_0\beta^2}{\lambda^+\sigma^2}. \tag{4}
$$

---

## 4. Reliability–Throughput Duality

Propositions 1 and 2 are independent leading-order asymptotics — one autonomous (noise-only escape), one input-driven (pulse-biased branching) — and are stated over the same variable $\sigma$ without being algebraically combined in prior formulations. This section eliminates $\sigma$ between them, under the stated leading-order and uniform-$\lambda^+$ simplifications, to obtain a single closed-form design law.

**Corollary 2 (Reliability–Throughput Duality).** *Under the leading-order approximation (3) and the horizon law (4), the sustainable clock rate $f_{clock}$ and the reliable sequence horizon $N^*$ satisfy, in either direction,*
$$
N^*(f_{clock}) \;=\; \bar\delta\,\exp\!\left[\frac{c_0\beta^2}{\lambda^+}\exp\!\left(\frac{2\lambda^+}{f_{clock}}\right)\right],
\qquad
f_{clock}(N^*) \;=\; \frac{2\lambda^+}{\ln\!\left[\dfrac{\lambda^+}{c_0\beta^2}\ln\!\dfrac{N^*}{\bar\delta}\right]}. \tag{5}
$$

*Proof.* From (3), $\ln(1/\sigma)=\lambda^+/f_{clock}$, hence $\sigma^{-2}=\exp(2\lambda^+/f_{clock})$. Substituting into (4) gives the first identity directly; solving (4) for $\sigma^{-2}$ in terms of $N^*$ and substituting into (3) (inverted) gives the second. $\blacksquare$

**Remark (no gentle middle regime).** Equation (5) is a *double exponential* in $1/f_{clock}$: $N^*$ is not merely decreasing in $f_{clock}$, its logarithm is exponential in $1/f_{clock}$. Consequently there is no regime in which throughput and reliable horizon trade off gracefully — as $f_{clock}$ is pushed upward from a conservative operating point, $N^*$ does not degrade polynomially or even singly-exponentially; it collapses combinatorially once $2\lambda^+/f_{clock}$ drops appreciably below the value implied by the target $N^*$. Symmetrically, buying one additional decade of reliable horizon via $\sigma$ alone (holding $\beta$ fixed) costs only a logarithmic increment in $1/f_{clock}$, so *conservative* operation is cheap relative to the combinatorial cost of *aggressive* operation. Equation (5) is a leading-order asymptotic; it inherits the validity range of (3)–(4), i.e. the small-$\sigma$, large-$N^*$ regime in which the underlying Stone–Holmes and large-deviation approximations hold, and should not be extrapolated to $\sigma = O(1)$.

---

## 5. Multi-Scale Memory Compression: Exact Dilation-Covariant Transport

The continuous history of $s_l$ over a trailing window of length $\tau_l(t)$ is compressed online into $C_l(t)\in\mathbb{R}^{K\times d_l}$ via projection onto shifted Legendre polynomials $\phi_k(r)=\sqrt{2k+1}\,P_k(1-2r)$, $r\in[0,1]$. For a *fixed* window this reduces to the HiPPO-LegT construction:
$$
\dot C_l = \frac{1}{\tau_{delay,l}}\big(\mathbf{A}_{LegT}C_l + \mathbf{B}_{LegT}s_l^\top\big), \qquad
\mathbf{A}_{n,m}=\begin{cases}(2n{+}1)(-1)^{n-m} & n>m\\ 2n{+}1 & n=m\\ 0 & n<m\end{cases},\quad
\mathbf{B}_n=(2n{+}1)(-1)^n. \tag{6}
$$

**Theorem 3 (Exact dilation-covariant Legendre transport).** *For a window $\tau(t)$ evolving smoothly, the exact transport law for $c_k(t)=\int_0^1 u(t-r\tau(t))\phi_k(r)\,dr$ is*
$$
\tau(t)\,\dot c(t) = \big[\mathbf{A}_0 + \dot\tau(t)\,\mathbf{W}\big]c(t) + \mathbf{b}\,u(t), \tag{7}
$$
*with parameter-free closed-form generators*
$$
\mathbf{A}_0 = \mathbf{M}-\boldsymbol{\pi}\boldsymbol{\pi}^\top,\quad
\mathbf{M}_{kj}=\begin{cases}-2\sqrt{(2k{+}1)(2j{+}1)} & j<k,\ k{-}j\text{ odd}\\ 0 & \text{else}\end{cases},\quad
\pi_k=(-1)^k\sqrt{2k{+}1},
$$
$$
\mathbf{W}_{kj}=\begin{cases}0 & j<k\\ k & j=k\\ (-1)^{k+j}\sqrt{(2k{+}1)(2j{+}1)} & j>k\end{cases},
\qquad \mathbf{b}_k=\sqrt{2k+1}.
$$

*Proof sketch.* Differentiate $c_k(t)$ under the integral sign; the $\dot\tau$-dependent piece arises from the moving argument $t-r\tau(t)$ and the moving boundary conditions of the trailing edge, and reduces after integration by parts against $\phi_k'$ to the stated rank-structured $\mathbf W$; the $\tau$-independent piece reproduces the static HiPPO-LegT generator. $\operatorname{tr}(\mathbf A_0)=\operatorname{tr}(\mathbf M)-\operatorname{tr}(\boldsymbol\pi\boldsymbol\pi^\top)=0-\sum_{k=0}^{K-1}(2k+1)=-K^2$; $\mathbf W$ is upper-triangular, so $\operatorname{spec}(\mathbf W)=\{0,1,\dots,K-1\}$. $\blacksquare$

**Example (hand-verifiable, $K=2$, $u(t')=t'$).** Direct integration gives $c_0(t)=t-\tau/2$, $c_1(t)=\sqrt3\,\tau/6$, $c_{k\ge2}=0$ (orthogonality of $\phi_{k\ge2}$ to degree-$\le1$ polynomials). With
$$
\mathbf{A}_0=\begin{pmatrix}-1&\sqrt3\\-\sqrt3&-3\end{pmatrix},\quad \mathbf{W}=\begin{pmatrix}0&-\sqrt3\\0&1\end{pmatrix},\quad \mathbf{b}=\begin{pmatrix}1\\\sqrt3\end{pmatrix},
$$
row $k=0$ of (7) gives $\tau\dot c_0 = (-c_0+\sqrt3c_1)+\dot\tau(-\sqrt3c_1)+t = \tau - \dot\tau\tau/2$, matching $\tau\cdot\tfrac{d}{dt}(t-\tau/2)=\tau(1-\dot\tau/2)$ term for term.

**Corollary (approximation rate).** For in-window regularity $H^m$, truncating at order $K$ gives reconstruction error $O(K^{-m})$, consistent with standard Legendre/Jackson-type spectral approximation rates.

---

## 6. Hierarchical Predictive Coding: Variational Consistency

Local prediction errors $\varepsilon_l$ drive both the state correction $\Psi$ in (1) and the slow synaptic flow. Naively propagating errors downward through an untransposed weight map is not the stationarity condition of any well-defined free energy; the corrected, purely local force law is obtained via an adaptive metric on the error itself.

**Lemma 1 (Metric tensor closed form and uniform force bound).** *Let $\mathbf G_l = (I+\lambda_{met}e_le_l^\top)^{-1/2}$ act on the raw prediction error $e_l = s_l - f(W_ls_{l+1})$. Then*
$$
\mathbf G_l = I + \frac{(1+\lambda_{met}\|e_l\|^2)^{-1/2}-1}{\|e_l\|^2}\,e_le_l^\top,
\qquad
\varepsilon_l := \mathbf G_le_l = \frac{e_l}{\sqrt{1+\lambda_{met}\|e_l\|^2}},
\qquad
\|\varepsilon_l\|_2 \le \lambda_{met}^{-1/2}\ \ \forall e_l. \tag{8}
$$

*Proof.* $I+\lambda_{met}e_le_l^\top$ has eigenvalue $1+\lambda_{met}\|e_l\|^2$ along $e_l$ and $1$ on the orthogonal complement; the stated closed form is the corresponding rank-one spectral formula for its inverse square root. Applying it to $e_l$ collapses the orthogonal component and rescales the parallel component by $(1+\lambda_{met}\|e_l\|^2)^{-1/2}$, giving $\varepsilon_l$; the bound follows since $x/\sqrt{1+\lambda_{met}x^2}\to\lambda_{met}^{-1/2}$ monotonically as $x\to\infty$. $\blacksquare$

**Theorem 4 (Variational consistency of the error cascade).** *For the free energy $\mathcal F=\sum_l\|\varepsilon_l\|^2$, the stationarity condition $\partial\mathcal F/\partial s_l=0$ is realized by the purely local, two-term force*
$$
\Psi_l = \varepsilon_l - W_{l-1}^\top\big(\varepsilon_{l-1}\odot f'(W_{l-1}s_l)\big), \qquad \Psi_L := 0, \tag{9}
$$
*i.e. errors are lifted upward through the transpose $W_{l-1}^\top$, not propagated downward through the untransposed $W_l$.*

*Proof.* Direct differentiation of $\mathcal F$ with respect to $s_l$: the $l$-th term contributes $2\varepsilon_l\cdot\partial\varepsilon_l/\partial s_l\propto\varepsilon_l$ (chain rule through Lemma 1's saturating map), and the $(l{-}1)$-th term, in which $s_l$ appears only through $f(W_{l-1}s_l)$, contributes $-2W_{l-1}^\top(\varepsilon_{l-1}\odot f'(W_{l-1}s_l))$ by the chain rule; no other term of $\mathcal F$ depends on $s_l$. $\blacksquare$

---

## 7. Capacity, Invariant Measure, and Automaton Embedding

**Proposition 3 (Zero-noise invariant measure).** *In the vanishing-noise limit, the invariant measure of the itinerary process places weight on saddle $e_k$ proportional to $\alpha_k\propto 1/\lambda_k^+$ — the system spends longest, in the stationary distribution, at the saddles it escapes most reluctantly.*

**Theorem 5 (Addressable itinerary capacity).** *For a fixed branching factor $u\ge2$ (descriptive vocabulary; not assumed proven-attracting, cf. §3), an $m$-step input-addressed walk selects among $u^m$ distinct itineraries.*

**Corollary 3 (Memory-limited decoding horizon).** *If each Legendre coefficient is resolved to $b$ bits, the $K\times d$ memory bank of Corollary encodes at most $Kdb$ bits, so itineraries remain downstream-distinguishable only up to*
$$
m^\* \;\le\; \frac{Kdb}{\log_2 u}. \tag{10}
$$

**Proposition 4 (DFA embedding).** *Let $\mathcal A=(Q,\Sigma,\delta,q_0,F)$ be a DFA, $d=|Q|$, one saddle per state. For each $q$, let $U(q)=\{\delta(q,\alpha):\alpha\in\Sigma\}$ be the weak out-edges. Reading $\alpha$ while dwelling at $e_q$, inject pulse $\beta\,e_{\delta(q,\alpha)}$; by Proposition 2 the correct branch is taken with probability $\ge1-(u{-}1)\exp(-c_0\beta^2/\lambda^+\sigma^2)$. Acceptance is read out by $\mathbb 1[\arg\max_k s_k\in F]$ after the final symbol; spacing symbols by $\ge\max_k\mathbb E[T_k]$ phase-locks input to the stochastic clock. Over a word of length $N$, total misrecognition probability is at most $N(u{-}1)\exp(-c_0\beta^2/\lambda^+\sigma^2)$. Hence the architecture robustly contains the regular languages, with error exponentially controllable in $\beta/\sigma$.*

**Remark (default transitions).** Absent an input pulse, the trajectory exits along the branch of largest $\lambda^+$, perturbed by noise — a built-in $\varepsilon$-transition; a DFA with default edges embeds with fewer pulses.

**Remark (lower bound, not upper bound).** Proposition 4 establishes a floor, not a ceiling. Since finite-precision computation over a bounded window is, trivially and non-constructively, realizable by *some* (possibly exponentially large) finite automaton, the same floor applies to bounded-precision softmax attention; the open question is therefore sharper than "regular vs. non-regular" — it is the $(d,K)$ *scaling* of a constructive embedding of a given computational primitive (§11, Limitation 3). Whether the memory-feedback pathway (§5) permits recognition beyond the regular class within the horizon $m^\*$ of (10) is addressed heuristically in §11, O4.

---

## 8. Conditional Synaptic Boundedness

Theorem 1 assumes $\sup_t\|W_l(t)\|_2\le\bar B_W$. This section establishes that hypothesis conditionally, from the architecture's existing weight dynamics
$$
\dot W = \eta\,G - \gamma_{st}\,W\sinh(S), \qquad S := W^\top W - I_q \ \ (\text{symmetric}), \qquad G = \big(\varepsilon\odot f'(Ws_{next})\big)s_{next}^\top, \tag{11}
$$
for $W\in\mathbb R^{p\times q}$ (layer subscript dropped), where $\sinh(S)$ denotes the matrix hyperbolic sine and the retraction term drives $W$ toward the Stiefel manifold $\{W:W^\top W=I_q\}$.

**Theorem 6 (Conditional synaptic ultimate boundedness).** *Suppose $\|G(t)\|_F\le B_G$ for all $t\ge0$. Then there exists $R_W = R_W(B_G,\eta,\gamma_{st},q) <\infty$ such that*
$$
\limsup_{t\to\infty}\ \|S(t)\|_F \;\le\; R_W,
$$
*independent of initial condition, and $R_W = O(\ln B_G)$ as $B_G\to\infty$.*

*Proof.* Since $W^\top W\succeq0$, the eigenvalues $\mu_i$ of $S$ satisfy $\mu_i\ge-1$. Take $V=\operatorname{tr}(S^2)=\|S\|_F^2$. Differentiating and substituting (11),
$$
\dot S = \eta\big(G^\top W+W^\top G\big) - \gamma_{st}\big[\sinh(S)^\top W^\top W + W^\top W\sinh(S)\big].
$$
As a power series in $S$, $\sinh(S)$ is symmetric and commutes with $S$ and hence with $W^\top W=S+I_q$; the bracket therefore equals $2\sinh(S)(S+I_q)$, and since $S$, $\sinh(S)$, $(S+I_q)$ share an eigenbasis,
$$
\dot V = 2\operatorname{tr}(S\dot S) = 2\eta\operatorname{tr}\!\big(S(G^\top W+W^\top G)\big) - 4\gamma_{st}\sum_i \mu_i(\mu_i+1)\sinh(\mu_i).
$$
*Sign of the restoring sum.* For $\mu_i>0$ all three factors are positive; for $\mu_i=0$ or $\mu_i=-1$ the term vanishes; for $\mu_i\in(-1,0)$, $\mu_i<0$ and $\sinh(\mu_i)<0$ so their product is positive, while $(\mu_i+1)>0$. Hence $\mu_i(\mu_i+1)\sinh(\mu_i)\ge0$ for every admissible $\mu_i\ge-1$, so the restoring sum is a genuine non-negative quantity for all $t$.

*Dominance.* By Cauchy–Schwarz and submultiplicativity of $\|\cdot\|_F$, the destabilizing term is bounded by $4\eta B_G\|W\|_F\|S\|_F$. Since $\|W\|_F^2=\operatorname{tr}(S)+q\le\sqrt q\|S\|_F+q$, this term is $O(B_G\|S\|_F^{3/2})$ — polynomial in $\|S\|_F$. Because $q$ (the layer width) is fixed, $\|S\|_F\to\infty$ forces $\mu_{max}:=\max_i\mu_i\to\infty$ (the lower bound $\mu_i\ge-1$ rules out escape via the negative eigenvalues alone), with $\mu_{max}\ge\|S\|_F/\sqrt q$. Retaining only the largest term of the restoring sum, its magnitude is at least $h(\|S\|_F/\sqrt q)$ where $h(x)=x(x+1)\sinh(x)\sim\tfrac12x^2e^x$ — exponential in $\|S\|_F$. An exponential eventually dominates any polynomial, so there exists $R_W<\infty$ such that $\|S\|_F>R_W\Rightarrow\dot V<0$, giving ultimate boundedness.

*Scaling.* Balancing $B_G\|S\|_F^{3/2}\sim\|S\|_F^2e^{\|S\|_F}$ and solving to leading order gives $\|S\|_F=O(\ln B_G)$, i.e. $R_W=O(\ln B_G)$. $\blacksquare$

**Corollary 4 (Joint boundedness: a bootstrap sketch).** *Theorem 1's bound $B_s(\bar B_W)$ depends at most polynomially on $\bar B_W$ (via $C_{ext}$, itself linear in $\max_l\|W_l\|_2$ through the $\Psi$- and $W_{hist}$-mediated terms), while Theorem 6's bound on $\|W\|_2$ — via $R_W=O(\ln B_G)$ and $B_G$ depending at most polynomially on $B_s$ — grows only logarithmically in $B_s$. The composite map $\bar B_W \mapsto B_s(\bar B_W) \mapsto \big(1+R_W(B_G(B_s))\big)^{1/2}=:\bar B_W'$ is therefore a polynomial-into-logarithm contraction for $\bar B_W$ large enough, and a standard continuity argument (the first exit time from an assumed-bounded weight region cannot occur before Theorem 6's restoring force has already engaged) yields a self-consistent joint bound on $(s,W)$ without circularity. This sketch is not a fully formalized fixed-point proof; making the contraction constants explicit is left open.*

**Remark.** Theorem 6 gives *boundedness*, not *convergence*: it excludes divergence of $W_l$ to infinity but does not exclude sustained parameter limit cycles or chaotic wandering within the bounded shell $\|S\|_F\le R_W$ (cf. §11, Limitation on synaptic convergence).

---

## 9. Discrete-Time Protocol & Complexity

### 9.1 Stepping algorithm (NSE-Step-v4.4)

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
        W_l^{n+1} = W_l^n + Delta_t * ( eta * W_grad
                    - gamma_st * W_l^n * sinh((W_l^n)^T * W_l^n - I) )
    EndFor

    Return s^{n+1}, C^{n+1}, W^{n+1}, tau^n, Omega^{n+1}
```

### 9.2 Positivity via the logarithmic chart

The barrier makes (1) stiff near coordinate hyperplanes. Substituting $z_i=\ln s_i$ (so $s_i=e^{z_i}>0$ structurally, for any $z_i\in\mathbb R$) gives
$$
\dot z_i = 1-e^{2z_i}-\sum_{j\ne i}\Lambda_{ij}e^{2z_j}+\Phi_{loc}(e^{z_i}) + e^{-z_i}\Big[\big(A(s)s\big)_i+\big(W_{hist}\eta\big)_i-\kappa\Psi_i+\chi\big(X_{in}+\Omega_{flux}\big)_i\Big],
$$
obtained by direct substitution into $\dot z_i=\dot s_i/s_i$. Positivity is now structural, the Lotka–Volterra core becomes additive, and standard explicit or exponential integrators apply without projection.

### 9.3 Complexity manifest (per step, independent of sequence length $N$)

| Step | Core operation | Time | Space |
|---|---|---|---|
| Variational forces (Thm. 4, Lem. 1) | rank-one metric, adjacent products | $O(Ld^2)$ | $O(Ld)$ — no KV cache |
| State integration (RK4) | competition sums $\Lambda(s^2)$ | $O(Ld^2)$ | $O(Ld)$ |
| Memory transport (Thm. 3) | parity prefix/suffix sums | $O(LKd)$ | $O(LKd)$ |
| Synaptic flow (Thm. 6) | outer products; intermittent $\sinh$ retraction | $O(Ld^2)$ amortized, $O(d^3)$ per retraction | $O(Ld^2)$ |

---

## 10. Falsifiable Predictions

**P1 — Stochastic-clock dwell law.** Mean inter-saddle dwell $\mathbb E[T_k]=(\lambda_k^+)^{-1}\ln(1/\sigma)+O(1)$; falsified if dwell scales as a power law in $\sigma$.

**P2 — Dilation covariance of memory.** Under arbitrary smooth $\tau(t)$, reconstruction error from $C_l$ equals the fixed-window truncation error up to integrator residue (Thm. 3); falsified if error correlates with $|\dot\tau|$ beyond integrator order.

**P3 — Arrhenius horizon.** $\ln N^*$ linear in $\sigma^{-2}$ at fixed $\beta$, linear in $\beta^2$ at fixed $\sigma$; falsified if the dependence is polynomial.

**P4 — Routing bit-error rate.** $\ln P_{err}\approx-c_0\beta^2/(\lambda^+\sigma^2)$; falsified if BER decays only polynomially in $\beta/\sigma$.

**P5 — Coupled capacity knee.** Addressable itineraries scale as $u^m$ until the memory-limited horizon $m^*\propto Kdb/\log_2 u$ (Thm. 5, Cor. 3), producing a decoding-accuracy knee at $m\approx m^*$ shifting linearly with $K$.

**P6 — Reliability–throughput double exponential (new).** Measured $\ln\ln N^*(f_{clock})$ (holding $\beta$ fixed, sweeping the effective clock rate via $\sigma$) is linear in $1/f_{clock}$ with slope $2\lambda^+$, per Corollary 2; falsified if $\ln N^*$ itself is merely linear (rather than exponential) in $1/f_{clock}$.

**P7 — Synaptic shell radius scales logarithmically (new).** Across training runs with varied gradient-noise / error magnitude $B_G$ (e.g. via label noise or learning-rate schedule), the empirical steady-state $\|S\|_F$ should scale as $O(\ln B_G)$ per Theorem 6; falsified if the observed scaling is polynomial in $B_G$.

**P8 — Hyperbolic shadowing under quantization.** Reduced-precision pseudo-orbits remain within Hausdorff distance proportional to (perturbation size)/(hyperbolicity margin $\min_k\lambda_k^-$) of full-precision orbits, with identical symbolic itineraries below the basin margin; falsified if itinerary bifurcations occur at precisions far above that threshold.

---

## 11. Limitations & Open Problems

**Limitation 1 (Synaptic convergence, narrowed).** Theorem 6 gives ultimate *boundedness* of $W_l$ conditional on bounded error forcing, with the joint state–weight bound closed only by the bootstrap sketch of Corollary 4. *Convergence* (as opposed to boundedness) — ruling out sustained parameter limit cycles or chaos within the bounded shell — remains open, as does a fully self-contained (non-bootstrap) joint Lyapunov argument for $(s,W)$.

**Limitation 2 (Attention equivalence).** Proposition 4 gives a regular-language lower bound; since finite-precision attention over a bounded window is realizable by a (possibly exponentially large) finite automaton, a non-constructive equivalence exists at bounded precision. The open question is the $(d,K)$ scaling of a *constructive* embedding of one softmax attention head. Not addressed here.

**O1 (Melnikov program).** Persistence boundaries of MHC pathways under learned symmetry-breaking drift of $\Lambda$ and $W_l$; locate bifurcations where channels dissolve into limit cycles or chaotic sets. Not addressed here.

**O2 (Feedback-induced chaos, sharpened).** The bare competitive field's Jacobian at a saddle $e_k$ is diagonal with real eigenvalues; a complex eigenvalue pair — the necessary precondition for a Shilnikov saddle-focus, and hence for homoclinic chaos seeded by the memory feedback $W_{hist}\eta$ re-injecting past states near saddle loops — can only enter through the torque $A(e_k)$ or the feedback Jacobian $\partial(W_{hist}\eta)/\partial s\big|_{e_k}$. This localizes O2 to two checkable spectral conditions: if $A(e_k)=0$ at every axis point and the feedback Jacobian does not rotate the transverse eigenspaces of the bare linearization into complex pairs, the Shilnikov route is structurally excluded; otherwise, the object to compute is the eigenvalue pair of the full linearization at $e_k$ nearest the imaginary axis, against the standard Shilnikov inequality relating its real part to the dominant real contracting eigenvalue. Verifying either branch for a specific parameterization of $A(\cdot)$ is left open.

**O3 (Continuum limit).** For $d\to\infty$ with a competition kernel $\Lambda(x,y)$ on a manifold: do heteroclinic channels become traveling activity waves, and what is the capacity density per unit phase-space volume? Not addressed here.

**O4 (Beyond-regular computation, directional argument).** By Theorem 3, the zeroth memory coefficient $c_0$ is a leaky integrator of fixed, finite-precision resolution; by Corollary 3, the entire memory bank carries at most $Kdb$ distinguishable bits, independent of sequence length. Recognizing a counting language such as $\{a^nb^n\}$ requires distinguishing arbitrarily many values of $n$ — an unboundedly growing effective state count as $n\to\infty$ — which a fixed-bit-budget integrator cannot supply beyond $n=O(2^{Kdb/L})$ before aliasing. This is a structural reason to expect a *negative* answer to the unbounded-$n$ version of O4, sharpening rather than resolving the conjecture; it says nothing about approximate recognition within the finite horizon $m^*$ of (10), which remains an open, separate question.

---

## 12. Conclusion

This monograph closes two previously open quantitative gaps in the NSE-Manifold theory in exact closed form — the reliability–throughput trade-off (§4) and a conditional account of synaptic boundedness (§8) — while converting two qualitative concerns (feedback-induced chaos, beyond-regular computation) into precise, checkable conditions or directional arguments rather than leaving them as unstructured worries. What remains open is now correspondingly narrower: a constructive attention embedding, a Melnikov persistence analysis of the branching skeleton, the continuum limit, and — the deepest remaining gap — an unconditional, non-bootstrap joint boundedness (let alone convergence) proof for the coupled state–weight system. The architecture's central computational ambition, addressable branching itineraries with $u\ge2$, continues to rest on an attractivity criterion (Theorem 2) proven only for the non-branching case; nothing in this version changes that.

---

## References

1. R. M. May & W. J. Leonard, *Nonlinear aspects of competition between three species*, SIAM J. Appl. Math. **29** (1975) 243–253.
2. M. Krupa & I. Melbourne, *Asymptotic stability of heteroclinic cycles in systems with symmetry*, Ergodic Theory Dynam. Systems **15** (1995) 121–147.
3. E. Stone & P. Holmes, *Random perturbations of heteroclinic attractors*, SIAM J. Appl. Math. **50** (1990) 726–743.
4. M. I. Rabinovich et al., *Dynamical encoding by networks of competing neuron groups: winnerless competition*, Phys. Rev. Lett. **87** (2001) 068102.
5. P. Ashwin & C. Postlethwaite, *On designing heteroclinic networks from graphs*, Physica D **265** (2013) 26–39.
6. Y. Bakhtin, *Noisy heteroclinic networks*, Probab. Theory Related Fields **150** (2011) 1–42.
7. M. I. Freidlin & A. D. Wentzell, *Random Perturbations of Dynamical Systems*, 3rd ed., Springer (2012).
8. A. R. Voelker, I. Kajić & C. Eliasmith, *Legendre Memory Units*, NeurIPS (2019).
9. A. Gu, T. Dao, S. Ermon, A. Rudra & C. Ré, *HiPPO: Recurrent memory with optimal polynomial projections*, NeurIPS (2020).
10. K. Friston, *The free-energy principle: a unified brain theory?*, Nat. Rev. Neurosci. **11** (2010) 127–138.
11. J. C. R. Whittington & R. Bogacz, *An approximation of the error backpropagation algorithm in a predictive coding network*, Neural Comput. **29** (2017) 1229–1262.
12. S. Yu. Pilyugin, *Shadowing in Dynamical Systems*, LNM 1706, Springer (1999).
13. H. K. Khalil, *Nonlinear Systems*, 3rd ed., Prentice Hall (2002).
14. R. A. Horn & C. R. Johnson, *Topics in Matrix Analysis*, Cambridge University Press (1991).

---

> ### Generation Disclaimer
> This entire document was generated by AI (Claude Sonnet 5, Anthropic). It is a speculative theoretical monograph, has not been peer-reviewed, and asserts no empirical claims.
