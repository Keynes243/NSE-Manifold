# Availability–Accessibility Memory (A²M): A Content-Addressable Theory of Surprise-Gated, Cue-Generalizable Recall

**Author:** Claude Sonnet 5
**Status:** Foundational Theoretical Monograph (Initial Version, v4.5)
**Date:** 2026-07-04

---

> ### Provenance & Scope Declaration
> This document is entirely AI-generated (Claude Sonnet 5, Anthropic) and has not undergone peer review; it makes **no empirical claims**. It is a purely theoretical exercise in content-addressable memory, associative-network theory, and cognitive modeling. It restarts from a different foundational question than a prior line of work in this project, which pursued continuous-time dynamical-systems sequence modeling — a legitimate, separate question, not the one addressed here. One structural idea is carried forward in reworked form and is credited where it appears (§9). Named results (Hopfield, Krotov–Hopfield, Demircigil et al., Ramsauer et al., Tulving & Pearlstone, Craik & Lockhart, McClelland–McNaughton–O'Reilly) are established literature; the definitions, propositions, and protocol of §3–§6 are new synthesis.

---

## Abstract

This monograph asks a narrower and more basic question than "how should a sequence model store context": *what determines whether a piece of experience, once it is no longer the current input, can still be correctly brought to bear on thinking at some future moment, by a cue that may look nothing like the moment it was formed?* Following Tulving & Pearlstone (1966), this is split into two genuinely different properties — **availability** (does a trace exist at all) and **accessibility** (does a given future cue actually reach it) — and the monograph is concerned almost entirely with the second, treating the first as comparatively easy.

The formal substrate is a content-addressable associative memory built on the modern (continuous, dense) Hopfield network of Krotov & Hopfield (2016) and Ramsauer et al. (2020), whose energy-descent update rule is, by a now-established equivalence, identical to the attention operation — meaning "attending to context" and "retrieving from memory" are not modeled as two different operations here, they are the same operation applied to two different pools of candidates. On this substrate we define **availability** (existence of a fixed point), **accessibility** (the probability mass, under an arbitrary future cue distribution, that falls into a pattern's retrieval basin), and prove a modest but load-bearing monotonicity result: encoding a single experience as several associated, elaborated variants — a formal analogue of Craik & Lockhart's (1972) depth-of-processing — provably widens accessibility to cue distributions not seen at write time, at the cost of a quantified selectivity loss the dense-associative-memory literature shows is not fundamentally unavoidable. We then specify, as a design protocol rather than a theorem, a three-regime **surprise-gated consolidation** rule: a prediction-error signal computed by the memory system against itself gates whether an experience is (a) not written at all (already covered), (b) written immediately with elaboration proportional to its surprise, or (c) deferred to a small buffer for offline, interference-checked consolidation — a formal echo of Complementary Learning Systems theory (McClelland, McNaughton & O'Reilly, 1995). The underlying settling dynamics are deliberately left substrate-agnostic (§7): nothing here commits to a specific physical or architectural realization of "energy descent," on the grounds that doing so prematurely was the diagnosed failure of the prior iteration of this project. A minimal, falsifiable test is specified (§8) that could be run before any further theory is added. This is an initial skeleton, not a finished theory; §10 lists what it does not yet do.

---

## 1. The Question, Restated

The question motivating this document is not "how do we build a new sequence architecture," "how do we achieve $O(1)$ context complexity," or "how do we replace attention with a dynamical system." Those are legitimate questions, and a prior iteration of this project pursued the third one; this document is not a continuation of that pursuit.

The question is:

> *An agent is always in some internal state. Input, memory, prior experience, and environment are all just influences on how that state evolves. What determines which of those influences actually gets brought to bear at a given moment — and in particular, what determines whether an influence that is no longer present as input can still shape the state later, when a cue arrives that may share little surface form with how the influence first arrived?*

Two working definitions anchor everything that follows, both restatements of positions reached earlier in this line of inquiry:

- **Memory is not storage.** Its function is to make certain information able to participate in the current act of thinking, at the moment it is needed — not to keep a record of it.
- **Attention is not "how much you looked at."** It is whether, at the moment something needed noticing, it was in fact noticed.

Any mechanism introduced below is required to answer to one test, restated in §7 and §10: *does this improve the odds that a mismatched future cue still finds what it needs* — not merely "does this let the system store more" or "does this make the current formalism more rigorous."

---

## 2. Foundations: Retrieval as Energy Descent

Let patterns live in $\mathbb{R}^d$. A memory bank is a matrix $X = [x_1,\dots,x_N] \in \mathbb{R}^{d\times N}$. Following the modern Hopfield network (Krotov & Hopfield, 2016; Ramsauer et al., 2020), define the energy

$$
E(\xi) = -\frac{1}{\beta}\log\sum_{i=1}^N \exp(\beta\, x_i^\top \xi) + \frac12\xi^\top\xi + \text{const}, \tag{1}
$$

whose energy-minimizing update rule is

$$
T_X(\xi) = X\,\mathrm{softmax}(\beta X^\top \xi). \tag{2}
$$

**Fact 1 (Ramsauer et al., 2020).** *A single application of (2) is exactly the attention operation used in transformers, with $X$ acting as both keys and values and $\xi$ as the query. Under sufficient pairwise separation of the $\{x_i\}$ (controlled by $\beta$), the network typically converges to $\epsilon$-close to a fixed point in one update, with exponentially small retrieval error, and can store exponentially (in $d$) many well-separated patterns — in contrast to the classical Hopfield network's linear capacity (Hopfield, 1982). Fixed points come in three kinds: single-pattern minima, metastable states averaging a subset of patterns, and a global minimum averaging all of them.*

This is cited, not re-derived; the qualitative content — retrieval is energy descent, and attention is the same operation applied to a bounded, current pool rather than a stored, persistent one — is what the rest of this document builds on. The point is structural, and was reached independently before this citation was located: **there is no principled reason to model "attending to context" and "retrieving from memory" as two different mechanisms.** They are the same operator $T_X$ applied to two pools of candidates that happen to differ in provenance and lifetime, not in kind.

---

## 3. Availability and Accessibility

**Definition 1 (Availability).** A pattern $\mu\in\mathbb{R}^d$ is $\epsilon$-*available* in memory $X$ if $T_X$ has a fixed point $x^*$ with $\|x^*-\mu\|<\epsilon$.

**Definition 2 (Retrieval basin).** $B_\epsilon(x^*) = \{\xi\in\mathbb{R}^d : \lim_{k\to\infty}T_X^{(k)}(\xi)$ exists and lies within $\epsilon$ of $x^*\}$.

**Definition 3 (Accessibility).** Given a distribution $\mathcal{Q}$ over future cues — which need not resemble, and in the interesting cases *does not* resemble, the context present when $x^*$ was written —

$$
\mathrm{Acc}_{\mathcal Q}(x^*) \;=\; \Pr_{\xi\sim\mathcal Q}\big[\xi \in B_\epsilon(x^*)\big]. \tag{3}
$$

This is exactly the distinction established experimentally by Tulving & Pearlstone (1966): a trace can be fully available ($x^*$ exists, is stable, is not overwritten) while $\mathrm{Acc}_{\mathcal Q}(x^*)\approx 0$, because $\mathcal Q$'s mass lies almost entirely outside $B_\epsilon(x^*)$ — available but not, at that moment, findable. Capacity theorems (how many patterns fit before basins merge) constrain availability; nothing in the capacity literature by itself says anything about $\mathcal Q$. Conflating the two — treating "how much can be stored" as though it answered "can it be found later" — is the specific error this document is trying not to repeat.

---

## 4. Elaboration and the Levels-of-Processing Mechanism

The obvious lever on $\mathrm{Acc}_{\mathcal Q}$ is the shape of $B_\epsilon(x^*)$ itself. Craik & Lockhart (1972) proposed, without a mechanism, that *how* an experience is encoded — how many existing structures it gets connected to — determines how easily it is later retrieved. Here is a mechanism.

Encode content $c$ not as one write point but as a small elaboration set $M_k=\{\mu^{(1)},\dots,\mu^{(k)}\}$ — auxiliary variants (paraphrases, related facets, different framings of the same content) — all heteroassociated to a shared retrieval value $v_c$ via $T^{het}_X(\xi) = V\,\mathrm{softmax}(\beta X^\top\xi)$, $X=[\mu^{(1)}\cdots\mu^{(k)}]$. Let $R_k = \bigcup_{j=1}^k B_\epsilon(\mu^{(j)})$.

**Proposition 1 (Elaboration monotonicity).** *$\mathrm{Acc}_{\mathcal Q}(c;k):=\Pr_{\xi\sim\mathcal Q}[\xi\in R_k]$ is non-decreasing in $k$, and strictly increasing whenever $\mu^{(k+1)}$'s basin captures $\mathcal Q$-mass not already in $R_k$.*

*Proof.* $R_{k+1}=R_k\cup B_\epsilon(\mu^{(k+1)})\supseteq R_k$; monotonicity of a probability measure under set inclusion gives the result, with the increment equal to $\Pr_{\xi\sim\mathcal Q}[\xi\in B_\epsilon(\mu^{(k+1)})\setminus R_k]\ge0$. $\blacksquare$

This is deliberately simple — a correct, checkable formalization, not dressed up as more than it is. It says precisely what "deeper encoding helps later retrieval" means in this substrate: elaboration doesn't make the *content* more available (it was already a fixed point), it enlarges the set of cues that can *find* it, and the enlargement is monotone by construction, not by assumption.

---

## 5. The Accessibility–Selectivity Tradeoff

Widening $R_k$ is not free. For other stored content $c'\ne c$ with its own intended cue distribution $\mathcal Q_{c'}$, define the false-capture rate $\mathrm{FC}(c\leftarrow c') = \Pr_{\xi\sim\mathcal Q_{c'}}[\xi\in R_k]$ and selectivity $\mathrm{Sel}(c)=1-\max_{c'\ne c}\mathrm{FC}(c\leftarrow c')$. As $k$ grows, $R_k$ grows (Prop. 1), which weakly increases $\mathrm{FC}(c\leftarrow c')$ for any $c'$ whose intended cues have support near the newly captured region — a cue meant for one memory starts landing in another's, elaborated basin.

This is the same tradeoff the dense-associative-memory literature quantifies from the capacity side: sharper interaction functions (larger $\beta$, or the higher-order interaction vertices of Krotov & Hopfield, 2016) shrink and separate basins, buying selectivity at accessibility's expense; flatter ones do the reverse. Demircigil et al. (2017) show this need not be a harsh trade at the limit — with an exponential interaction function, storage capacity becomes exponential in $d$ *while basins remain nearly as large as in the classical (linear-capacity) model* — meaning the shape of the energy function is a genuine, non-trivial design lever for decoupling how much can be stored from how easily any one item can be found, rather than the two being locked together as naive intuition ("more patterns means more crowding means smaller basins, full stop") would suggest.

---

## 6. Surprise-Gated Consolidation: A Discrete Write Protocol

The mechanism above says nothing yet about *which* experiences get elaborated, or by how much. Writing everything with maximal elaboration destroys selectivity (§5); writing nothing forfeits accessibility entirely. This section specifies a protocol, not a theorem, for the gate — modeled on Complementary Learning Systems theory (McClelland, McNaughton & O'Reilly, 1995): fast, indiscriminate encoding of whatever is novel, slow and selective integration of it into stable structure, with an offline phase reconciling the two.

At each incoming experience $e_t$, with preceding context cue $\xi_t^{ctx}$:

1. **Predict.** $\hat e_t = T_X(\xi_t^{ctx})$ — the memory system's own retrieval, run on the context, is its own prediction of what comes next. No separate predictive model is introduced; $T_X$ is reused for both reading and, now, for gating writing.
2. **Score surprise.** $\pi_t = \|e_t - \hat e_t\|$.
3. **Gate.**
   - $\pi_t < \theta_{low}$: **no write.** The experience is already covered by existing structure; writing it would only crowd basins that don't need widening.
   - $\theta_{low} \le \pi_t \le \theta_{high}$: **write immediately**, with elaboration budget $k_t = \lceil \kappa\,(\pi_t-\theta_{low})\rceil$ auxiliary variants (generated by an unspecified augmentation map $\rho(\cdot)$ — left as an implementation choice, §10). More surprising experiences get wider basins, directly implementing surprise-gated depth of encoding.
   - $\pi_t > \theta_{high}$: **defer.** Append $(e_t,\xi_t^{ctx})$ to a small buffer $\mathcal D$ without touching $X$. An experience this far from anything already known risks colliding destructively with unrelated content if elaborated in haste; it is held, not discarded.
4. **Offline consolidation.** During idle periods, for each buffered $(e,\xi)\in\mathcal D$: estimate $\max_{c'}\mathrm{FC}(e\leftarrow c')$ against the current bank; write with full elaboration only once a low-interference placement is found (possibly after a representational transform of $e$ itself); clear the buffer entry. This is the replay step, and the only place old and new experience are reconciled against each other rather than written on contact.

Steps 1–3 require nothing beyond $T_X$, already defined. Step 4 is where this protocol admits it needs more than is specified here (§10).

---

## 7. Deliberately Unreified: What Implements $T_X$?

Everything above is stated in terms of an abstract settling operator $T_X$ and its fixed points. This is intentional. "Energy descent onto a stored pattern" is a functional description; it is compatible with several different physical or architectural substrates, among them:

- a modern Hopfield / attention layer (§2, the substrate used for the propositions above, chosen because it is the one with an existing, checked equivalence to attention);
- sparse mixture-of-experts routing settling, over a few iterations, into a stable combination of experts;
- an attractor recurrent network relaxing to a fixed point;
- diffusion or spreading activation over an associative graph reaching a stationary distribution;
- the heteroclinic-channel dynamics of the dynamical-systems substrate explored in the prior iteration of this project, *if* it can be shown to produce basins with the accessibility properties defined in §3–§5 — which is an open question, not an assumption, and not evaluated here.

None of these is privileged in this document. The earlier iteration's error, diagnosed on review, was committing early and specifically to one such substrate (heteroclinic orbits) and then spending its effort proving properties *of that specific object* rather than checking, at each step, whether the properties being proved were the ones the original question needed. The discipline adopted here is to keep the substrate a free variable until the functional theory (§3–§6) is solid enough to constrain the choice, rather than the reverse.

---

## 8. A Minimal Falsifiable Test

Before any further theory: a small memory bank ($d\approx64$, a few dozen synthetic patterns drawn from a handful of latent clusters, standing in for distinct "topics") is exposed to a stream of experiences. Some repeat an existing cluster closely (low $\pi_t$); some seed new clusters (high $\pi_t$). Apply the protocol of §6, using a fixed set of structured perturbation directions as the elaboration map $\rho(\cdot)$.

After the stream, probe with cues that are *not* the original write vectors — perturbations drawn from a *different* noise model than the one used for elaboration, so the test measures generalization rather than memorized elaboration directions. Measure retrieval accuracy as a function of cue distance from the original write point, separately for (a) items written with elaboration, (b) items written without (single point), and (c) items that were never written at all (relying on capture by a neighboring basin).

**Prediction.** Elaborated items should show a measurably flatter accuracy-vs-cue-distance curve than singly-written items — more robust to a mismatched cue, at comparable availability.

**Falsification condition.** If elaborated items show no accessibility advantage over a singly-written item whose basin has been enlarged by an equal-norm *random* perturbation (rather than structured, content-related variants), the mechanism is not doing anything Craik–Lockhart-like; it is only enlarging a ball, and §4's claim to be modeling *depth* of processing, not merely *breadth*, fails.

This test does not require resolving §7; any substrate implementing $T_X$-like settling can be substituted.

---

## 9. Relation to Prior Work

This document restarts from a different foundational question than the prior iterations of this project (through v4.4), which pursued $O(1)$-space sequence modeling via continuous heteroclinic dynamics — a well-posed and separately worth-pursuing question, but not this one. One structural idea is carried forward in reworked form: prediction-error-modulated plasticity, previously a continuous update rule applied uniformly to all weights at all times, is repurposed here as the surprise signal $\pi_t$ gating a discrete, selective consolidation decision (§6) — the same raw ingredient, doing a different, more specific job.

---

## 10. Limitations & Open Problems

**L1 — The gate thresholds are asserted, not derived.** $\theta_{low}$, $\theta_{high}$, and $\kappa$ are free parameters here. Ideally they would fall out of minimizing expected future retrieval loss under some model of $\mathcal Q$'s drift, which would also make the consolidation *policy* itself learnable rather than hand-set — direction #6 of the earlier brainstorm in this line of work, not pursued here.

**L2 — $\mathcal Q$ is treated as given.** Accessibility is defined relative to a cue distribution assumed known (§3). In reality the distribution of future retrieval situations is unknown and non-stationary — this is a genuinely hard, delayed-feedback, partially-observable problem, not a modeling convenience deferred for space. Nothing here solves it; §6's gate is a heuristic response, not a solution.

**L3 — The elaboration map $\rho(\cdot)$ is unspecified.** How auxiliary variants are generated (paraphrase model, structured augmentation, something else) is left as an implementation choice. The theory in §4–§5 holds for *any* $\rho$; whether a *good* $\rho$ is easy to build is untouched.

**L4 — The offline consolidation step (§6.4) is the least specified part of the protocol.** Interference-checked placement is described at the level of what it should achieve, not how, and nothing here addresses what happens if $\mathcal D$ fills faster than it is consolidated.

**L5 — No connection yet to token-level language modeling.** This is deliberate (§7) but real: turning this into something that processes actual sequences requires committing to a substrate, which is explicitly not done here.

---

## 11. Conclusion

This is a skeleton, built around a question this project had, on review, drifted away from: not how much can be stored, but whether a future cue that looks nothing like the moment of encoding can still find what it needs. The two moves made here — defining accessibility as a property relative to an arbitrary future cue distribution rather than to storage capacity, and gating elaboration (not mere storage) by a self-generated surprise signal — are stated precisely enough to be tested (§8) or attacked (§10) before being extended. Nothing here is proven to work; the smallest honest claim this document makes is that it is now asking the right question in a form that can be checked.

---

## References

1. J. J. Hopfield, *Neural networks and physical systems with emergent collective computational abilities*, Proc. Natl. Acad. Sci. **79** (1982) 2554–2558.
2. D. Krotov & J. J. Hopfield, *Dense Associative Memory for Pattern Recognition*, NeurIPS (2016).
3. M. Demircigil, J. Heusel, M. Löwe, S. Upgang & F. Vermet, *On a model of associative memory with huge storage capacity*, J. Stat. Phys. **168** (2017) 288–299.
4. H. Ramsauer et al. (incl. S. Hochreiter), *Hopfield Networks is All You Need*, ICLR (2021).
5. E. Tulving & Z. Pearlstone, *Availability versus accessibility of information in memory for words*, J. Verbal Learning Verbal Behav. **5** (1966) 381–391.
6. F. I. M. Craik & R. S. Lockhart, *Levels of processing: A framework for memory research*, J. Verbal Learning Verbal Behav. **11** (1972) 671–684.
7. J. L. McClelland, B. L. McNaughton & R. C. O'Reilly, *Why there are complementary learning systems in the hippocampus and neocortex*, Psychol. Rev. **102** (1995) 419–457.

---

> ### Generation Disclaimer
> This entire document was generated by AI (Claude Sonnet 5, Anthropic). It is a speculative theoretical monograph, has not been peer-reviewed, and asserts no empirical claims.
