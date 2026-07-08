# Availability–Accessibility Memory (A²M) v4.9: Anchor-Constrained Generative Reconstruction with a Homeostatic Affect Router

**Author:** Claude Sonnet 5
**Status:** Foundational Theoretical Monograph (v4.9)
**Date:** 2026-07-07

---

> ### Provenance & Scope Declaration
> This document is entirely AI-generated (Claude Sonnet 5, Anthropic) and has not undergone peer review; it makes **no empirical claims**. It is a purely theoretical exercise in content-addressable memory and reconstructive-memory modeling, extending prior versions (v4.5–v4.8) of this same line of work. Named results (Hopfield, Krotov–Hopfield, Demircigil et al., Ramsauer et al., Bartlett, Tulving & Pearlstone, Gazzaniga, Johansson–Hall et al., Loftus, Craik & Lockhart, McClelland–McNaughton–O'Reilly, Tishby–Pereira–Bialek, Barrett, Keramati & Gutkin, LeDoux, Stone & Holmes) are established literature; the anchor-constrained reconstruction formalism of §3–§9, the recursive pool-selection mechanism of §10, the affect-routing and density-unification arguments of §11–§13, and the transition-dynamics argument of §14 are new synthesis.

---

## Abstract

The prior version of this work (v4.5) formalized memory retrieval as matching against discrete stored patterns, and accessibility as the probability that a future cue lands in a pattern's basin of attraction. That formalism cannot, by construction, produce a confident, coherent, *wrong* reconstruction — it can only hit or miss — yet confident, coherent, wrong reconstruction is close to the modal outcome once real recall is queried after any meaningful delay. This version replaces matching with **anchor-constrained generative reconstruction**: an experience deposits a small set of discrete, decaying constraints ("anchors" — a key conclusion, relation, affective tag, or causal edge) rather than a retrievable copy of itself, and reconstruction at any later moment is a single constrained-generation operation, pulled toward surviving anchors and filled in, wherever anchors are weak or absent, by whatever else is currently active. The prior version's Hopfield-based matching is retained, but demoted from "the mechanism" to "the fill term" — the special case in which anchor density is so high it pins the output down almost entirely.

On this substrate, two results are established. First, a **non-motivated coherence bias**: when anchor density is low, the reconstruction is generically coherent with current context by construction, with no term encoding self-protection or motive — reframing post-hoc rationalization as a structural property of underconstrained generation, not a psychological defense mechanism, and grounding this in split-brain confabulation and choice-blindness research rather than motivational accounts. Second, a **manipulability–fidelity duality**: because the same fill term that generates coherent output under sparse anchors also responds directly to current external input, a closed-form sensitivity analysis shows that resistance to post-hoc informational contamination and fidelity to the original experience are not independent properties — both are monotonic in the same scalar, anchor density — yielding a sharp, falsifiable prediction that fidelity and manipulability should co-vary along one curve, not scatter as two separate axes. Accessibility, elaboration, and the accessibility–selectivity tradeoff from the prior version are recast as consequences of this same mechanism rather than as separate constructs. A further refinement (§10) inverts the usual causal order assumed for the fill term itself — a state activates a pool of candidate information, rather than information composing a state — and formalizes this as a short recursive chain of the same retrieval operator applied to progressively narrower candidate pools, with a principled halt condition given by an information-bottleneck-style sufficiency criterion rather than an externally imposed depth limit.

This version adds three further pieces (§11–§13). Emotion is proposed as a concrete instantiation of §10's abstract mode candidates — a discrete, learned categorical *attribute* and a continuous, priority-assigning *intensity* — grounded in the theory of constructed emotion rather than a table of innate categories. Because a bodiless system has no interoceptive signal to categorize in the first place, a virtual interoception interface is proposed, borrowing the formal equivalence between homeostatic *drive* and information-theoretic surprise from homeostatic reinforcement learning, so that the surprise signal already used for consolidation (§9) qualifies as one channel without further invention. A single mechanism is then argued to explain both why full episodes stay specific and why low-dimensional affect states cluster into shared categories — the same energy-based operator's fixed points are isolated, metastable, or fully averaged depending only on how densely its inputs are packed relative to its resolution parameter $\beta$ — and, in the degenerate case where §10's stopping rule is asked to resolve specific pattern identity, that rule and $\beta$ are shown to be the same criterion written two ways, yielding a falsifiable claim that $\beta$ should be solved for rather than independently tuned. A companion remark draws a line this line of inquiry had previously blurred in discussion: expectation-type quantities that govern structural stopping decisions and realized-type quantities that govern per-event write strength are related but not identical.

A fourth addition (§14) gives §10's transitions a structure. An elementary topological fact — any continuous path between two genuinely separate basins of an energy landscape must cross a point at least as high as the barrier between them — rules out, for any landscape with more than one basin, the possibility that a transition consists entirely of low-energy, individually meaningful waypoints; a landscape-respecting transition and a well-shaped one are the same claim, and both still spike in energy at the crossing. What actually distinguishes a landscape-respecting transition from an externally forced override is not the energy profile along the way but whether the driving term at the moment of crossing is the system's own standing noise or non-gradient dynamics (checkable via barrier-height-dependent crossing statistics, in the spirit of the Kramers/Stone–Holmes dwell-time law) or a forcing large enough to be insensitive to the local landscape altogether — the latter matching the amygdala's fast subcortical threat pathway, a real but narrow-scope mechanism unlikely to generalize to slower, non-reflexive switches. Whether the crossing itself carries process-type content — not "this is concept $X$" but "this is being unresolved between $X$ and $Y$" — is raised but not settled, though it is given a concrete home in §12's coherence channel. A minimal experiment is specified that would directly test whether fidelity and manipulability really do collapse to one dial, before any further theory is added. The largest open problem — where the anchor set, the channel repertoire, and their initial weights actually come from, given a raw experience — is stated but not solved.

---

## 1. The Question, Restated

An agent is always in some internal state. Input, memory, prior experience, and environment are all just influences on how that state evolves. What determines which of those influences actually gets brought to bear at a given moment — and in particular, what determines whether an influence that is no longer present as input can still shape the state later, when a cue arrives that may share little surface form with how the influence first arrived?

Two working definitions anchor everything that follows:

- **Memory is not storage.** Its function is to make certain information able to participate in the current act of thinking, at the moment it is needed — not to keep a record of it.
- **Attention is not "how much you looked at."** It is whether, at the moment something needed noticing, it was in fact noticed.

Any mechanism introduced below answers to one test: *does this improve the odds that a mismatched future cue still finds what it needs* — not "does this let the system store more" or "does this make the current formalism more rigorous."

---

## 2. Foundations: Retrieval as a Limit of Constrained Generation

Let patterns live in $\mathbb{R}^d$. A memory bank is $X=[x_1,\dots,x_N]\in\mathbb{R}^{d\times N}$. Following the modern Hopfield network (Krotov & Hopfield, 2016; Ramsauer et al., 2020), define the energy

$$
E(\hat x) = -\frac1\beta\log\sum_{i=1}^N\exp(\beta\,x_i^\top\hat x) + \frac12\hat x^\top\hat x + \text{const}, \tag{1}
$$

whose descent update $T_X(\xi)=X\,\mathrm{softmax}(\beta X^\top\xi)$ is, by an established equivalence, identical to attention, with $X$ as keys/values and $\xi$ as query; under sufficient pattern separation it converges in one step to $\epsilon$-close to a stored pattern with exponentially small error (Ramsauer et al., 2020).

This is retrieval as *matching*: given enough well-separated stored patterns and a query near one of them, the output is (almost) uniquely determined by $X$. It is a real, useful, correctly-cited fact — and, as §3 argues, a special case rather than the general story. It is the limit in which retrieval happens to sit when the available constraint is dense enough to pin the output down almost entirely. What it does not by itself explain is confabulation: a system that only ever matches against stored patterns either returns a stored pattern or fails; it has no route to returning something *confident, coherent, and wrong* — which is the behavior actually observed once memory is queried after partial forgetting (Bartlett, 1932; and see §4). §3 keeps this machinery but demotes it from "the substrate" to "the fill term."

---

## 3. Anchor-Constrained Generative Reconstruction

**Motivation.** A pure-matching account of retrieval predicts two outcomes: hit a stored pattern, or fail. It does not predict a third outcome that is, empirically, the common one after any meaningful delay: a confident, internally coherent reconstruction that is neither the original experience nor a null result. This section replaces "retrieve the stored pattern" with "generate a reconstruction consistent with whatever constraints survive," and treats §2's matching regime as the special case in which those surviving constraints happen to be dense.

**Setup.** An experience $e\in\mathbb{R}^d$, at encoding time, deposits a small **anchor set** $A=\{a_1,\dots,a_m\}$ — discrete, partial constraints (a key conclusion, a key relation, a key affective tag, a piece of causal structure), each formalized as a soft penalty $\phi_{a_i}(\hat x)\ge0$, convex, with a unique zero at the value $e$ actually had on that constraint, and weight $\lambda_{a_i}>0$ reflecting how strongly that anchor currently survives (decaying over time, independent per anchor — this is what "forgetting" is, in this formalism: not deletion of $e$, but decay of the $\{\lambda_{a_i}\}$). Reconstruction at any later moment, given current internal state $s$ and current external input $u$, is

$$
\hat x^*(A;s,u) \;=\; \arg\min_{\hat x}\Big[\underbrace{\textstyle\sum_{i}\lambda_{a_i}\,\phi_{a_i}(\hat x)}_{\text{anchor fidelity}} \;+\; \underbrace{\psi(\hat x;s,u)}_{\text{context fill}}\Big], \tag{2}
$$

where $\psi(\hat x;s,u)$ is itself instantiated as a Hopfield-type energy (Eq. 1) over whatever pattern pool is *currently* active — $U(s,u)$, which may include recent unrelated experiences, ambient context, or the current external input directly — rather than over the original experience's own trace. Equation (1) is not discarded; it is repurposed as the mechanism for the fill term, not for the whole reconstruction.

**Proposition 1 (Anchor-density limit behavior).** *Assume the $\{\phi_{a_i}\}$ are mutually consistent (share a common zero at $e$'s true values) and $\psi(\cdot;s,u)$ is bounded and Lipschitz in $\hat x$ for fixed $(s,u)$. Then:*
$$
\lim_{\min_i \lambda_{a_i}\to\infty} \hat x^*(A;s,u) = e \quad \text{(independent of $s,u$)}, \qquad
\lim_{\max_i \lambda_{a_i}\to0} \hat x^*(A;s,u) = \arg\min_{\hat x}\psi(\hat x;s,u).
$$

*Proof sketch.* As $\min_i\lambda_{a_i}\to\infty$, the anchor term dominates the objective (its value grows without bound at any $\hat x\ne e$, while $\psi$ stays bounded), forcing the minimizer to $e$, the anchors' common zero. As $\max_i\lambda_{a_i}\to0$, the anchor term vanishes from the objective identically, leaving only $\arg\min_{\hat x}\psi(\hat x;s,u)$. $\blacksquare$

Define **anchor density** $\rho(A) = \frac{\sum_i\lambda_{a_i}}{\sum_i\lambda_{a_i}+\bar\lambda_\psi}\in[0,1)$, for a reference scale $\bar\lambda_\psi$ on the fill term's effective weight. Proposition 1 says: $\rho\to1$ recovers §2's matching regime (retrieval-as-hitting-the-stored-point); $\rho\to0$ recovers pure generation from current context, with zero contribution from $e$. Both are the *same* operator (2) at different values of one scalar — not two mechanisms, one for "working" recall and one for "failure."

---

## 4. Non-Motivated Coherence Bias

**Corollary 1 (Structural confabulation).** *When $\rho(A)\to0$, by Proposition 1, $\hat x^*(A;s,u)\to\arg\min_{\hat x}\psi(\hat x;s,u)$ — a point that is, by construction, low-energy (coherent) with respect to whatever is currently active in $U(s,u)$. No term in (2) encodes self-protection, affect-regulation, or motive of any kind; the coherence of a low-anchor-density reconstruction is a direct consequence of $\psi$ being built to produce coherent settling points, not of a separate motivated process.*

This reframes post-hoc rationalization as a property of the reconstruction *pipeline itself*, not a psychological defense mechanism layered on top of it. The empirical case for treating confabulation this way, rather than as motivated self-deception, is unusually clean: split-brain patients confidently generate plausible explanations for actions whose true cause they structurally cannot access, with no plausible ego-protective stake in doing so (Gazzaniga's "interpreter" studies); and normal subjects, in the choice-blindness paradigm, offer fluent, confident justifications for choices that were covertly swapped by the experimenter — justifications indistinguishable in detail, confidence, and emotional tone from those given for choices actually made, and offered by people who have no idea a swap occurred and so nothing to defend (Johansson, Hall, Sikström & Olsson, 2005). Both are cases of coherent output arising from a process with access only to whatever is currently available, not from a process motivated to protect anything — exactly the behavior Corollary 1 predicts of low-$\rho$ reconstruction.

---

## 5. Manipulability Varies Inversely with Anchor Density

Because $\psi(\hat x;s,u)$ depends on current external input $u$ directly, and because Proposition 1 shows $u$'s influence on the output grows as anchor weight shrinks, manipulability of a reconstruction should itself be a function of $\rho$ — not an independent property of "how bad the memory is."

**Proposition 2 (Manipulability–density relation).** *Let $\hat x^*(u)$ denote the solution to (2) as a function of $u$, with $\psi$ differentiable in both arguments. By the implicit function theorem applied to the first-order condition of (2),*
$$
\frac{\partial \hat x^*}{\partial u} = -\Big[\textstyle\sum_i\lambda_{a_i}\nabla^2\phi_{a_i}(\hat x^*) + \nabla^2_{\hat x}\psi(\hat x^*;s,u)\Big]^{-1}\nabla_{\hat x,u}\,\psi(\hat x^*;s,u).
$$
*As $\sum_i\lambda_{a_i}\to\infty$ (with $\nabla^2\phi_{a_i}\succ0$), the bracketed term's smallest eigenvalue diverges, so $\|\partial\hat x^*/\partial u\|\to0$: dense anchors are structurally resistant to being overwritten by current input. As $\sum_i\lambda_{a_i}\to0$, the bracket reduces to $\nabla^2_{\hat x}\psi$ alone, recovering the maximal, anchor-free sensitivity of pure context-fill.*

The empirical analogue is Loftus's misinformation-effect research: post-event information measurably alters recall, up to and including confident report of events that did not occur, with the effect more pronounced the older or more weakly encoded the original memory. This yields a **falsifiable prediction** that is the actual point of this section: *fidelity to the original experience and susceptibility to post-hoc injected information are not two independent properties of a memory — they are the same underlying quantity, $\rho(A)$, read off two ends of the same curve.* An item measured as unusually resistant to post-hoc contamination should also, by (2), be measured as unusually faithful to the original; there should be no items that are simultaneously low-fidelity and manipulation-resistant, or high-fidelity and manipulation-prone, if this account is right. This is checked directly in §16.

---

## 6. Availability and Accessibility, Revisited

The binary "landed in the basin or not" of v4.5 no longer fits once retrieval is continuous (§3). Both definitions are recast as limits/expectations of the same objective (2).

**Definition 1′ (Availability, revised).** Content $e$ remains *available* at time $t$ iff its anchor set has not fully decayed: $\sum_i\lambda_{a_i}(t) > 0$. This is now a direct, literal translation of Tulving & Pearlstone's original claim — availability is a property of the surviving trace, independent of any cue $(s,u)$.

**Definition 2′ (Fidelity).** $F(A;s,u) := g\big(\|\hat x^*(A;s,u)-e\|\big)$ for any fixed, strictly decreasing $g$ with $g(0)=1$ — a continuous stand-in for "was it retrieved."

**Definition 3′ (Accessibility, revised).** Given a distribution $\mathcal Q$ over future $(s,u)$,
$$
\mathrm{Acc}_{\mathcal Q}(e;A) = \mathbb E_{(s,u)\sim\mathcal Q}\big[F(A;s,u)\big] \in [0,1].
$$
This recovers the v4.5 definition exactly when $F=\mathbb 1[\cdot\in B_\epsilon]$; it generalizes it to a soft version because retrieval itself is now soft.

**Corollary 2 (Endpoints).** By Proposition 1 and dominated convergence, $\mathrm{Acc}_{\mathcal Q}(e;A)\to1$ as $\rho(A)\to1$, and $\mathrm{Acc}_{\mathcal Q}(e;A)\to \mathbb E_{(s,u)\sim\mathcal Q}\big[F(\arg\min_{\hat x}\psi(\hat x;s,u))\big]$ (generically small) as $\rho(A)\to0$. Interior monotonicity in $\rho$ is expected but not proven here — an honest gap, not claimed away.

The warning from v4.5 still holds and is sharper now: capacity (how much the fill pool $U(s,u)$ can hold, §2/§8) constrains the *noise floor* on $F$, but says nothing about $\mathcal Q$; conflating "how much can be stored" with "will this one be found" remains the specific error this document exists to avoid.

---

## 7. Elaboration as One Route to Anchor Density

v4.5's monotonicity result is repositioned, not discarded: it was a special case of raising $\rho(A)$.

**Proposition 3 (Elaboration raises density).** For $A_k = A_0\cup\{a_k\}$, $\rho(A_k)\ge\rho(A_0)$, with equality iff $\lambda_{a_k}=0$. By Corollary 2's endpoint behavior, $F(A_k;\cdot)$ moves weakly toward $e$ relative to $F(A_0;\cdot)$ wherever the added anchor's constraint doesn't conflict with existing ones.

This makes explicit what was implicit in v4.5: "write several associated variants" is one way to add anchors — specifically, anchors that live in the *same* representational space as $e$ itself (paraphrases, alternate framings). The anchor formalism of §3 is broader: an anchor can be a key affective tag, a causal edge, a scalar salience marker — objects that need not resemble $e$ at all. Elaboration-by-paraphrase is the special case where all anchors are same-space; §9 uses the general case.

---

## 8. Selectivity Folds Into the Same Quantity as Manipulability

This is the one place the new formalism changes the *shape* of an old result, not just its statement. In v4.5, elaboration and interference competed for space in one shared basin structure, so more accessibility mechanically cost selectivity. Here, anchors are an additive penalty layered on top of the fill term, not another point in a shared landscape — so raising $\rho(A_e)$ does not, in general, cost selectivity toward an unrelated rival $e'$: a genuine $e'$-cue's own fill term $\psi(\cdot;s,u)$ is already pulling toward $e'$-consistent context, and $e$ capturing it would require $e$'s anchor term to overcome a fill term not favoring $e$ to begin with. Raising $\rho(A_e)$ makes this *less* likely, not more.

What remains — and this is the actual finding — is that **§5's manipulability risk and the residual selectivity risk are the same failure mode**, viewed from two threat models: at low $\rho(A_e)$, the fill term dominates regardless of whose cue produced it, so a weakly-anchored memory can be pulled toward whatever pool is currently active — arbitrary present input (§5) or, in the unlucky case that a rival's intended context overlaps the current ambient pool, actual rival content (this section). These were tracked as two separate tradeoffs in v4.5; they collapse to one dial, $\rho$, here.

One genuine, independent tension survives from §2: the finite fill-pool $U(s,u)$ still inherits the ordinary capacity/sharpness tradeoff of dense associative memory (Krotov & Hopfield, 2016; Demircigil et al., 2017) — an overcrowded, insufficiently sharp pool raises the noise floor on $\psi$ itself, degrading fidelity even for well-anchored content. This now acts as a floor on achievable $F$, not a competing storage cost against accessibility.

---

## 9. Surprise-Gated Consolidation, Rewritten Around Anchors

The protocol reuses $\hat x^*$ (Eq. 2) for prediction, exactly as v4.5 reused $T_X$:

1. **Predict.** $\hat e_t = \hat x^*(A_{ctx};s,u=\xi_t^{ctx})$, using whatever anchors are already active for the current context.
2. **Score surprise.** $\pi_t=\|e_t-\hat e_t\|$.
3. **Gate.**
   - $\pi_t<\theta_{low}$: no write — already well-predicted by existing anchor+fill structure.
   - $\theta_{low}\le\pi_t\le\theta_{high}$: **write a small anchor set** $A_t=\{a_1,\dots,a_{m_t}\}$, $m_t=\lceil\kappa(\pi_t-\theta_{low})\rceil$, extracted from $e_t$ by an unspecified map $\alpha(\cdot)$ — deliberately more general than v4.5's paraphrase generator, since anchors need not share $e_t$'s representational space (a key-conclusion extractor, a causal-relation tagger, an affect tagger are all valid instances of $\alpha$). Each new anchor is initialized at weight $\lambda_0$. Surprise controls how *many and how strong* the anchors are, not, as in v4.5, how many paraphrase-variants get written.
   - $\pi_t>\theta_{high}$: **defer** to buffer $\mathcal D$, unchanged from v4.5 — anchors are not extracted in haste from something too unlike anything on file.
4. **Offline consolidation and anchor decay (new).** Buffered items are interference-checked (as in v4.5) before anchor extraction. Independently, every $\lambda_{a_i}(t)$ decays over time absent re-access, and is boosted back up on re-access (a rehearsal effect). This is the piece v4.5 never specified: *forgetting is this decay process*, not a background assumption that $\rho$ can somehow be low. §4–§5's low-$\rho$ regime is what this decay produces once it runs long enough without rehearsal.

---

## 10. State-Gated Pool Selection: A Recursive Refinement

Equation (2)'s fill term draws from a pool $U(s,u)$ treated, so far, as given. This section makes explicit what determines $U(s,u)$, and argues it is not a single flat pool but the endpoint of a short recursive chain of the same retrieval operator, applied to progressively narrower candidate sets.

**The causal order.** The standard account of retrieval (and of attention) runs *information → composed state*: relevant items are found, then combined. The account here inverts this: *state → activated information*. A query does not first assemble a state from whatever content resembles it; entering a state (a "mode") is what determines which content becomes reachable at all. The same fact, made available in two different states, can be functionally invisible in one and immediately at hand in the other — not because it was weighted down, but because it was never in the pool that state's fill term draws from.

**Recursive pool selection.** Let $U^{(0)}$ be a small, coarse-grained set of mode candidates. $T_{U^{(0)}}(s,u)$ (Eq. 1's operator, applied to this pool) selects a mode $m_1$. $m_1$ defines $U^{(1)}(m_1)$, a finer pool; $T_{U^{(1)}}$ selects $m_2$; $m_2$ defines $U^{(2)}(m_2)$; and so on, until some level $L$ is treated as the fill term's pool proper, $U(s,u):=U^{(L)}$. This is one operator, $T$, called on a shrinking sequence of pools — not a separate "mode-selection" mechanism bolted onto a separate "content-selection" mechanism. (This recursion is compatible with, and gives a mechanism for, the anchor formalism's task-relative notion of "necessary information," §3: what an anchor's fill term treats as its candidate pool is set by which $U^{(L)}$ the recursion above bottoms out in.)

**Proposition 4 (Recursive stopping rule).** *Let $Y_k$ denote whatever information level $k{+}1$'s selection actually needs from level $k$'s outcome (formally: a sufficient statistic for level $k{+}1$'s decision). Refinement from $U^{(k)}$ to $U^{(k+1)}$ is warranted iff $I(U^{(k+1)}; Y_k \mid U^{(k)}) > 0$ — i.e., iff narrowing further still changes something the next level needs. The recursion halts at the first $L$ for which this mutual information vanishes: $U^{(L)}$ is already a sufficient statistic (in the sense of Tishby, Pereira & Bialek, 1999) for every level below it, so further narrowing is not merely small, it is uninformative.*

This gives a principled halt condition without an externally imposed depth limit: recursion stops exactly where further resolution stops mattering to whatever consumes it — and since "what matters" is itself task-relative, the same input can bottom out at different $L$ under different tasks (a digit's identity suffices for arithmetic; its handwriting does not, and forces further recursion, if the task is attribution rather than arithmetic).

**A minimal falsifiable test (extends §16).** Fix a model, a question, and its available knowledge entirely. Vary only $U^{(0)}$'s outcome $m_1$ — the coarse mode the system is placed in immediately before the question arrives — without adding or removing any content from what is, in principle, reachable across all modes. Prediction: answer quality should vary substantially with $m_1$ alone. This isolates state-gated pool selection from information quantity as the operative variable, and is falsified if quality tracks only what information is technically available, independent of which mode was entered first.

---

## 11. Emotion as a Concrete Instance of State-Gated Pool Selection

§10 left $U^{(0)}$'s "mode candidates" entirely abstract. This section proposes a concrete, well-grounded instance: emotional state.

**Attribute versus intensity.** An emotion has two components that should not be conflated: a discrete *attribute* — a categorical label, drawn from a small, closed repertoire — and a continuous *intensity* that assigns processing priority, independent of which label applies. The two play different roles below: attribute selects which pool; intensity determines how strongly that selection dominates.

**Where the discrete labels come from.** §10's discipline is strict: content populating $U^{(0)}$ must be produced by the running mechanism, not authored. The theory of constructed emotion (Barrett, 2017) gives a precise account of exactly this, displacing the alternative — a small number of innate, hard-wired discrete circuits, one per label — that affective neuroscience has not been able to substantiate. In its place: a low-dimensional, continuous *core affect* (valence and arousal) is the only primitive substrate; discrete emotion *categories* are learned concepts, constructed in the moment by a predictive categorization process applied to core affect plus context, not retrieved from a fixed table. The mechanism doing the constructing is exactly what §10 requires: learned, not hand-specified.

**A candidate realization, reusing existing machinery.** Rather than adding a dedicated classifier, emotion categories can be modeled as attractors emerging in the same content-addressable substrate already used for anchors (§2–§3), applied to a (core-affect, context) stream instead of full episodic content: a region of that stream visited often enough crystallizes into a stable basin, and that basin *is* the category — no table of which cues trigger which label is written anywhere. This specific unification — that category-formation and anchor-formation share one mechanism — is this document's own synthesis, not an implication of Barrett's theory; §13 gives it a precise, checkable form.

---

## 12. A Virtual Interoception Interface

Barrett's account requires a continuous core-affect signal; a system without a body has none on offer. Rather than approximate one arbitrarily, this section borrows a framework built for exactly this purpose in artificial agents.

**Drive as the substrate.** Homeostatic reinforcement learning (Keramati & Gutkin, 2014) formalizes an agent's motivation as minimizing *drive* — the distance between an internal state and a homeostatic setpoint — rather than maximizing an externally supplied reward; reward is *derived*, defined as drive reduction, and nothing about the setpoints needs to reference the external world directly. Crucially, drive is shown to be formally equivalent to information-theoretic surprise (negative log-probability): it is not a new primitive that needs inventing. The surprise signal $\pi_t$ already present in this document's consolidation protocol (§9) already qualifies as one channel of a virtual-interoceptive vector, without further justification.

**A provisional channel set.** A single scalar is unlikely to support anything as rich as human emotional categorization; real interoception is multi-channel. As a starting scaffold — explicitly *not* claimed to be complete, minimal, or non-arbitrary (see the honest tension below) — four channels are proposed, each with its own homeostatic setpoint: a *resource/load* channel (recent processing expenditure against a sustainable budget — its deviation is a natural trigger for the offline consolidation of §9, rather than requiring a separately designed trigger); a *competence* channel (the running drive already given by $\pi_t$); a *goal-progress* channel (deviation from expected advancement toward whatever task is currently active); and a *coherence* channel (how decisively §10's recursive pool selection is resolving at the current level — near-tied competition among candidates registers as unresolved drive here). The vector of these deviations at a given moment is the raw material core affect operates on; §11's attractor mechanism operates on this vector, not on episodic content.

**Anticipation, not just reaction.** Later extensions of the homeostatic-RL framework explicitly model anticipatory control — responding to a *predicted* future drive trajectory, not only current drive. The same extension applies here without modification: tracking channel deviations' trend, not just their current value, gives a mechanism for anticipatory affect (dread, expectation) for free, rather than requiring a separately designed anticipation module.

**An honest tension, not yet resolved.** The channel set above is hand-specified, in real tension with the discipline that content must come from mechanism, not authorship (§10). §13 partially addresses *how finely* each channel should be resolved; it does not address where the initial repertoire of channel *types* comes from, which remains authored here.

---

## 13. Density Determines Behavior: One Mechanism for Memories, Categories, and Channel Resolution

**Three regimes, one operator.** The Hopfield energy (Eq. 1, §2) does not have one behavior; per Ramsauer et al. (2020), its fixed points come in three kinds depending on how closely stored patterns are packed relative to the resolution set by $\beta$: isolated single-pattern minima when patterns are well separated; metastable states averaging a subset when several are packed close together; and a single global average when all of them are. Which regime obtains is not a property of the mechanism, which is one operator throughout — it is a property of how densely patterns are packed relative to $1/\beta$.

**Why episodes stay specific and affect states cluster.** This directly explains why full episodic experience (§3's anchor targets) tends to resolve as isolated, specific memories while the low-dimensional core-affect stream (§12) tends to resolve as a small number of shared categories, without needing separate design decisions for the two cases. A high-dimensional representation of a full experience has exponentially more room for distinct instances to sit far apart from one another; a two-or-few-dimensional core-affect vector has comparatively little room, and instances land close together far more readily under the same $\beta$. Category formation is not a special mechanism bolted onto memory formation; it is what the identical mechanism does when its input happens to be low-dimensional.

**Channel resolution reuses §10's stopping rule.** Whether a homeostatic channel (§12) should split into two finer ones is, by the same logic, an instance of §10's recursive stopping rule applied one level further down: continue splitting exactly while doing so still carries mutual information about whatever the next level of processing needs; stop once it does not. This does not resolve §12's honest tension (the initial channel *types* remain authored) but does mean that, once a rough initial channel exists, whether it later differentiates further is not a separately designed decision.

**Proposition 5 (β–information-bottleneck correspondence, degenerate case).** *Consider §10's recursive stopping rule in the special case where $Y_k$ is "which specific stored pattern is this." The information gained by further resolution, $I(U^{(k+1)}; Y_k \mid U^{(k)})$, then reduces exactly to $H(\text{identity}\mid U^{(k)}) - H(\text{identity}\mid U^{(k+1)})$; if $U^{(k+1)}$ fully resolves identity, this is just $H(\text{identity}\mid U^{(k)})$ — the entropy of the Hopfield softmax distribution $\{p_i\}$ itself. Since $\beta$ directly controls $H(\{p_i\})$, the two stopping criteria — §10's $I<\epsilon_{IB}$ and Hopfield's implicit choice of $\beta$ — are, in this degenerate case, the same criterion written two ways: $\beta$ is not an independently tunable resolution parameter but is determined by requiring $H(\{p_i(\beta)\})=\epsilon_{IB}$, for whatever $\epsilon_{IB}$ governs stopping elsewhere in the recursion.*

This is checkable rather than merely suggestive: if a working implementation cannot avoid tuning $\beta$ independently at each level, with no principled relationship to a shared $\epsilon_{IB}$, this correspondence is false beyond the degenerate case. The general functional form of $\beta$ as a function of $\epsilon_{IB}$ away from this degenerate case has not been derived; this is the single sharpest open item this version adds (§18, L11).

**Remark (two related families, not one quantity).** Structural/stopping decisions (§10's mutual information; this section's entropy $H(\{p_i\})$) are *expectation*-type quantities: they describe how much uncertainty a distribution carries before any outcome is realized, and govern whether to keep resolving or stop. Per-event decisions (the surprise signal $\pi_t$, §9; single-instance drive, §12) are *realized*-type quantities: how surprising *this specific* outcome was, after the fact, and govern how strongly to anchor or update on it. Both rest on the same log-probability foundation, and this section's central claim is that they are close relatives operating in one overall system — but they are not one quantity appearing four times, and treating them as fully interchangeable would overstate what has been shown here.

---

## 14. Transition Dynamics: A Necessary Saddle-Crossing, and How to Tell It From an Override

§10 specifies that a mode-transition happens; it says nothing about what the trajectory looks like while it happens. This section gives that transition a structure, and corrects an error made in developing it.

**Two candidate accounts collapse under elementary topology, not for lack of trying.** It is tempting to distinguish a landscape-respecting transition from an externally-forced one by asking whether every point visited along the way is itself a meaningful, interpretable state — "the path only ever passes through other concepts." This cannot hold in general, for any energy landscape with more than one basin.

**Proposition 6 (Necessary saddle-crossing).** *Let $E$ be continuous, and let $A,B$ lie in distinct connected components of the sublevel set $\{x:E(x)\le c\}$ for some $c>\max(E(A),E(B))$ — i.e., $A$ and $B$ are separated by a barrier of height at least $c$. Then every continuous path from $A$ to $B$ passes through some $x^*$ with $E(x^*)\ge c$.*

*Proof.* If a path stayed entirely within $\{E\le c\}$, it would be a continuous path connecting two points assumed to lie in different connected components of that set — impossible, since continuous images of a connected interval are connected. $\blacksquare$

The consequence is unforgiving: if $A$ and $B$ are genuinely separate basins (which is what it means for them to be distinct anchors, or distinct modes in §10's sense), *no* transition between them — however well the landscape has been shaped by experience — can consist entirely of low-energy, individually-meaningful waypoints. A transition whose every point is a recognizable concept is not evidence of a well-shaped landscape; it is evidence that $A$ and $B$ were never really separated to begin with.

**The corrected diagnostic.** What actually distinguishes a landscape-respecting transition from an override is not the energy profile along the path — both necessarily spike near the crossing — but the relationship between the driving term and the landscape's own barrier structure. Write the full dynamics as $\dot s(t)=-\nabla E(s(t))+\eta(t)$, where $\eta$ is whatever drives the system beyond pure descent (noise, a non-gradient rotational component, an external pulse). Classify:

- **Landscape-respecting.** $\eta(t)$ is the same standing process that governs the system's dynamics generally — most concretely, thermal-like noise of fixed magnitude $\sigma$, so that crossing $A\to B$ is a rare escape event over the barrier, in the sense of Kramers' problem. This is checkable: the statistics of the crossing (how often it happens, how long the system dwells near the saddle before completing it) should follow the barrier-height dependence such theory predicts — in the simplest case, mean crossing/dwell time scaling with $\exp(\Delta E/\sigma^2)$-type sensitivity to barrier height, in the spirit of the Stone–Holmes dwell-time law used, in a different guise, in an earlier iteration of this line of work.
- **Override.** $\eta(t)$ at the moment of transition is large enough, relative to the local barrier height, that the crossing's occurrence and timing are essentially insensitive to that barrier — the transition would have happened regardless of how the landscape was shaped there. This matches the amygdala's fast subcortical pathway (LeDoux, 1996): a route from thalamus directly to amygdala, bypassing the slower cortical route that would supply contextual, landscape-like elaboration, firing a threat response before any detailed, associatively-shaped evaluation completes. It is a real, well-evidenced mechanism, but a narrow-scope one — suited to threat responses under time pressure, not obviously the right model for slower, non-reflexive switches between topics like arithmetic and idle planning.

**An open question, given a concrete home.** Does the saddle-crossing segment itself carry content — not "this is concept $X$" but something more like "this is the state of being unresolved between $X$ and $Y$," a state about the *process* rather than about any settled content? This is not resolved here, but it is no longer homeless: §12's coherence channel already tracks how decisively §10's pool selection is resolving, currently only as an intensity signal feeding write-priority. Promoting it to a state with its own behavioral consequences — rather than a transient to be rushed through — would give two concrete uses, neither yet implemented: (a) a trigger for actively delaying commitment and gathering further evidence, using the system's own dwell-time statistics (above) as the readable signal that it is currently in this state; (b) a legitimate third output, distinct from committing to $A$ or $B$ — an explicit "unresolved between these" — rather than a state the system is only ever trying to exit as fast as possible.

---

## 15. Deliberately Unreified: What Implements Reconstruction?

Unchanged in spirit from v4.5, with one addition: "energy descent" (§2) and "constrained generation" (§3) are two compatible *framings* of the same functional requirement — output pulled toward surviving constraints, filled in by whatever else is active — not a claim that the substrate is literally energy-based, autoregressive, or diffusion-based. The candidate-substrate list carries over unchanged (Hopfield/attention layer; sparse MoE settling; attractor RNN; associative-graph diffusion; a prior iteration's heteroclinic dynamics, *if* shown to realize an anchor-plus-settling structure — open, not assumed). The discipline stays the same: the substrate is a free variable until §2–§9's functional theory is solid enough to constrain the choice, not before. §10's recursion is likewise substrate-agnostic: nothing above requires that each level's selection be implemented by the same kind of mechanism as any other level.

---

## 16. Minimal Falsifiable Tests

Two tests, both in synthetic pattern space ($d\approx64$), no language, per the standing discipline (§15).

**Test A (elaboration vs. noise, carried from v4.5).** Compare items whose anchor count is raised via structured, content-related paraphrase anchors (§7) against items whose single anchor's basin is enlarged by an equal-weight random perturbation. Prediction: structured anchors yield higher $\mathrm{Acc}_{\mathcal Q}$ under a held-out cue-noise model than random enlargement of the same total weight. Falsified if no gap exists — that would mean §7 in this version, like v4.5, is only inflating a ball, not doing anything density-specific.

**Test B (the load-bearing one: does one dial explain both curves?).** Construct items at experimenter-set anchor densities $\rho\in\{0.1,0.3,0.5,0.7,0.9\}$ (setting $\lambda_{a_i}$ directly — learning $\rho$ from raw experience is not attempted here; see §18, L1). For each item, at test time: (a) measure fidelity $F$ under a fixed same-space cue distance; (b) separately, at the *same* $\rho$, inject a competing signal into $u$ during reconstruction (a Loftus-style post-hoc perturbation) and measure the shift of $\hat x^*$ toward the injected signal. Plot $F$ against $(1-\text{shift})$ across items.

**Test C (extends §10: does mode alone move the needle?).** As specified in §10's minimal test: hold knowledge fixed, vary only the coarse mode $m_1$ entered before the query, measure output quality. Falsified if quality tracks available information alone.

**Test D (extends §13: is $\beta$ solved for, or independently tuned?).** Implement the recursive pool-selection hierarchy of §10 end-to-end, including the degenerate bottom level where $Y_k$ is specific pattern identity. Attempt to set $\beta$ at that level from the shared $\epsilon_{IB}$ threshold used elsewhere in the recursion, per Proposition 5, rather than tuning it independently. Falsified if no consistent $\epsilon_{IB}$ permits acceptable performance at every level simultaneously — i.e., if $\beta$ must still be tuned independently per level despite the proposed correspondence.

**Prediction (Tests A, B).** A single, near-monotonic curve in $\rho$ — items should not scatter off the diagonal.

**Falsification (Tests A, B).** Any item that is simultaneously low-$F$ *and* low-manipulability, or high-$F$ *and* high-manipulability, is direct evidence against §3–§8's central claim (fidelity and manipulability are the same quantity, $\rho$, read from two ends) and would mean a second, independent mechanism needs to be added rather than assumed away.

---

## 17. Where This Sits Between Information Flow and Influence Flow

The fork raised in the prior round of this inquiry is not resolved here, but it is given a specific, checkable middle candidate. Anchors are Influence-Flow-shaped: sparse, discrete constraints on future reconstruction, not the content itself, and in the $\rho\to0$ limit nothing resembling the original content need survive at all. The fill term is Information-Flow-shaped: a matching operation (§2) over whatever pool is currently active. Anchor-constrained generation is the claim that recall in general sits somewhere on the line between these two, at a position set by $\rho$ — not a resolution of which extreme is correct, a specific, falsifiable (§16, Test B) account of how both extremes and everything between them could be one mechanism.

---

## 18. Limitations & Open Problems

**L1 (the largest gap in this version).** Anchor density $\rho$ and the initial weights $\lambda_{a_i}$ are free, hand-set quantities throughout §3–§9. Nothing here specifies how a system would decide, from a raw experience $e_t$, *which* features become anchors or at what initial strength — this is now load-bearing for every result in this document, not a peripheral detail.

**L2.** Proposition 1 assumes the anchor penalties $\{\phi_{a_i}\}$ are mutually consistent (share a common zero at $e$). Real anchors — a key conclusion and a key emotional tag, say — could pull toward inconsistent reconstructions. Not modeled; Prop. 1's clean endpoints may not survive relaxing this.

**L3.** Corollary 2's interior monotonicity (accessibility rising smoothly with $\rho$, not just agreeing at the two endpoints) is asserted as expected, not proven.

**L4 (carried from v4.5).** Gate thresholds $\theta_{low},\theta_{high},\kappa$ (§9) remain hand-set, not derived from any loss.

**L5 (carried from v4.5).** $\mathcal Q$ is still assumed known; its real non-stationarity and partial observability are untouched.

**L6 (carried from v4.5).** Offline consolidation (§9, step 4) remains the least specified step — interference-checking is named, not designed.

**L7 (carried from v4.5).** No connection yet to token-level or language processing — deliberate (§15), but real.

**L8 (parked for dedicated treatment).** Whether the apparent discreteness between unrelated modes (§10) is itself the coarse, thresholded readout of a continuous, evidence-accumulation-style process — in the sense of drift-diffusion or competing-accumulator models of decision-making — is an open question that §10's recursion neither assumes nor rules out; it is agnostic to what implements each level's choice. A candidate discriminating signal, not yet checked: if such a substrate exists, the cost of switching pools should scale with proximity to the switching threshold, rather than being a fixed constant.

**L9 (unverified resemblance).** §10's recursive, self-similar pool narrowing bears an apparent structural resemblance to hierarchical predictive coding's multi-timescale abstraction hierarchy (coarser, slower representations higher up; finer, faster ones lower down). Whether this is a real correspondence or a surface analogy has not been checked.

**L10 (an internal tension, not yet reconciled).** §3 models anchors as persistent quantities decaying smoothly over time — closer to a standing disposition than to the sharper working notion, reached independently in this line of inquiry, of "influence" as strictly local, momentary, and responsible only for the next instant, with no persistence or purposive direction built in. If the latter is the more basic unit, §3's decaying-anchor formalism may itself be a derived, aggregate description of many local influences accumulating, rather than a model of the base mechanism — the two have not been reconciled.

**L11 (the sharpest open item as of v4.8).** Proposition 5 (§13) establishes the $\beta$–$\epsilon_{IB}$ correspondence only in the degenerate case where $Y_k$ is specific pattern identity. The general functional form of $\beta$ as a function of $\epsilon_{IB}$ away from this special case has not been derived, and nothing here guarantees one exists in closed form.

**L12.** The virtual-interoception channel *types* proposed in §12 (resource, competence, goal-progress, coherence) are hand-specified. §13 gives a principled account of whether an existing channel should split into finer ones, but not of where the initial repertoire of channel types comes from — this remains authored, in tension with §10's own discipline.

**L13.** §11's claim that emotion categories and episodic anchors share one attractor mechanism rests on a general dimensionality argument (§13): that low-dimensional inputs cluster and high-dimensional ones separate, under one shared operator. No worked numerical example or simulation here shows this actually yields a small, stable, human-like repertoire of categories rather than, say, one giant undifferentiated cluster or an unstable number of them.

**L14.** Homeostatic reinforcement learning's own empirical base (§12) is built mainly on simple, low-dimensional physiological analogues — hunger, thirst, and similar single-resource tasks. Extending it to underwrite something as rich as human emotional categorization, as proposed here, is this document's own extrapolation, not a result already established in that literature.

**L15 (new).** The Kramers/Stone–Holmes-style dwell-time signature proposed in §14 (crossing statistics that depend on barrier height) has been validated only in simple attractor-network models and lower-level perceptual/motor switching — nothing resembling topic-level mode-switching (arithmetic versus idle planning). This generalization gap is exactly as open as it was before §14 sharpened the prediction; a sharper prediction is not the same as a tested one.

**L16 (new; the softest claim this version adds).** Whether the saddle-crossing segment itself carries process-type content ("unresolved between $X$ and $Y$," §14) is speculation connected to the rest of this document only by an informal analogy to §12's coherence channel — no proposition, no test, no formal claim.

---

## 19. Conclusion

This version replaced "did a cue land in a basin" with "how much does surviving structure constrain what gets generated" (§3–§9), then replaced "information is retrieved" with "state activates a pool, recursively narrowed until further resolution stops mattering" (§10). One extension showed that pool-narrowing, category-formation, and Hopfield's own resolution parameter $\beta$ are not three separate design choices but one operator's behavior read off at different densities and asked to resolve different things (§11–§13) — while explicitly correcting an earlier overstatement, made in the course of developing this, that had conflated this family of *structural* quantities with the *per-event* surprise signals driving §9's writing process; they are related but distinct. A further extension (§14) gave the transitions themselves a structure: an elementary topological fact rules out any transition between genuinely separate basins from being uniformly low-energy and individually meaningful, correcting a cleaner but false version of the claim reached earlier in discussion, and replacing it with a diagnostic — self-driven, barrier-sensitive crossing versus insensitive override — that reconnects to dwell-time machinery this line of work had previously set aside for unrelated reasons. None of this is shown to be true, only shown to be stated precisely enough to be checked (§16) or broken (§18) — and §18's sharpest item is still the general form of $\beta(\epsilon_{IB})$ (L11), not because it is the only gap, but because it is the one this document's own argument is currently unable to close.

---

## References

1. J. J. Hopfield, *Neural networks and physical systems with emergent collective computational abilities*, Proc. Natl. Acad. Sci. **79** (1982) 2554–2558.
2. D. Krotov & J. J. Hopfield, *Dense Associative Memory for Pattern Recognition*, NeurIPS (2016).
3. M. Demircigil, J. Heusel, M. Löwe, S. Upgang & F. Vermet, *On a model of associative memory with huge storage capacity*, J. Stat. Phys. **168** (2017) 288–299.
4. H. Ramsauer et al. (incl. S. Hochreiter), *Hopfield Networks is All You Need*, ICLR (2021).
5. F. C. Bartlett, *Remembering: A Study in Experimental and Social Psychology*, Cambridge University Press (1932).
6. E. Tulving & Z. Pearlstone, *Availability versus accessibility of information in memory for words*, J. Verbal Learning Verbal Behav. **5** (1966) 381–391.
7. F. I. M. Craik & R. S. Lockhart, *Levels of processing: A framework for memory research*, J. Verbal Learning Verbal Behav. **11** (1972) 671–684.
8. J. L. McClelland, B. L. McNaughton & R. C. O'Reilly, *Why there are complementary learning systems in the hippocampus and neocortex*, Psychol. Rev. **102** (1995) 419–457.
9. M. S. Gazzaniga, *The Interpreter Within: The Glue of Conscious Experience*, in *The Cognitive Neurosciences* / related works on the left-hemisphere "interpreter."
10. P. Johansson, L. Hall, S. Sikström & A. Olsson, *Failure to detect mismatches between intention and outcome in a simple decision task*, Science **310** (2005) 116–119.
11. E. F. Loftus & J. C. Palmer, *Reconstruction of automobile destruction: An example of the interaction between language and memory*, J. Verbal Learning Verbal Behav. **13** (1974) 585–589.
12. N. Tishby, F. C. Pereira & W. Bialek, *The Information Bottleneck Method*, Proc. 37th Allerton Conf. on Communication, Control, and Computing (1999) 368–377.
13. L. F. Barrett, *The theory of constructed emotion: an active inference account of interoception and categorization*, Soc. Cogn. Affect. Neurosci. **12** (2017) 1–23.
14. M. Keramati & B. Gutkin, *Homeostatic reinforcement learning for integrating reward collection and physiological stability*, eLife **3** (2014) e04811.
15. J. E. LeDoux, *The Emotional Brain: The Mysterious Underpinnings of Emotional Life*, Simon & Schuster (1996).
16. E. Stone & P. Holmes, *Random perturbations of heteroclinic attractors*, SIAM J. Appl. Math. **50** (1990) 726–743.

---

> ### Generation Disclaimer
> This entire document was generated by AI (Claude Sonnet 5, Anthropic). It is a speculative theoretical monograph, has not been peer-reviewed, and asserts no empirical claims.
