# 02 Diamond Research — a real run

One topic, six angles, thirty-one agents, eight claims put on trial. This is a transcript of [template 02](../prompts/02-diamond-research.md) executed on 2026-08-03 in Claude Code: 1 canary + 6 parallel search agents + 24 parallel verifiers, each with real web search, none able to see another's reasoning. Approximately 1.38M subagent tokens across ~439 web searches and fetches; 37 minutes wall-clock from dispatch to report. **Budget cap: Step 4 verified the top 8 high-importance claims at 3 verifiers each, not the template's 25** — the other 23 deduped claims were never adversarially tested and are labeled UNVERIFIED throughout. The topic is the repo's own premise: *do multi-agent LLM systems actually beat single-agent baselines on research and analysis, and by how much?* The point of running it here was to let the method attack the thing that produced it. It partly succeeded, and not in the direction anyone expected.

## Step 0 — the gate, and the assumptions it left behind

The user wrote "use your judgment on scope — do not ask me clarifying questions," which satisfies the template's explicit-waiver condition. Assumptions chosen, stated here as the template requires:

- **Multi-agent** = two or more LLM instances with separate context windows under a coordination protocol (debate, orchestrator-worker, mixture-of-agents, verifier pipelines). One agent making many tool calls in one context does not count.
- **Single-agent baseline** = one instance, one context, allowed chain-of-thought, extended thinking, tools, and long multi-turn loops. Every claim carries a compute-matched flag: yes / no / unknown.
- **Research and analysis tasks** = open-ended web research (GAIA, BrowseComp, BrowseComp-Plus, DeepResearch Bench, HLE), multi-document QA and synthesis, fact verification, market and financial analysis. Coding and SWE benchmarks appear only as contrast, tagged out-of-core.
- **Time frame** = January 2024 onward; anything earlier tagged era-bound.
- **Decision this feeds** = whether the repo's templates should keep claiming multi-agent superiority, and with what caveats.

## Step 1 — the angle split (orchestrator, no agents)

Six mutually exclusive angles, each answerable by a different source type:

| # | Angle | Source type it targets |
|---|---|---|
| 1 | Frontier-lab and vendor self-reports | Engineering blogs, system cards, lab technical reports |
| 2 | Academic evidence **for** multi-agent gains | arXiv / peer-reviewed papers with quantified deltas |
| 3 | Null and negative results | Replication failures, failure taxonomies, compute-matched refutations |
| 4 | Cost and token economics | Iso-cost comparisons, test-time-compute scaling papers |
| 5 | Independent third-party evaluation | GAIA / DeepResearch Bench leaderboards, open replications |
| 6 | Adversarial verification and critic ensembles | LLM-as-judge reliability studies — the repo's own mechanism |

Angle 6 exists because the repo's core claim is narrower than "multi-agent is good." It is "isolated skeptics kill errors." That deserved its own angle and its own attack.

## Step 2 — the canary

One agent got the shared brief with the angle field left as `[ANGLE INSERTED HERE]` and one instruction: *before searching, list every fact or constraint you would need that this brief does not give you.* It was told not to search. It returned in 31 seconds, 24k tokens, zero tool calls. Verbatim, its top gaps and its closing verdict:

> 1. **No operational definition of "multi-agent LLM system."** The term spans wildly different architectures — debate/voting ensembles, orchestrator-worker pipelines, mixture-of-agents, the requester's own search-dedupe-skeptic pattern. Papers report gains/losses for architecturally incompatible things. Without a shared definition, each of the six agents will silently pick a different referent and the results won't actually be measuring the same thing.
>
> 2. **No definition of "single-agent baseline."** Which model, which prompting strategy (plain, CoT, ReAct, tool-augmented, extended thinking)? "Multi-agent beats single-agent" claims are meaningless without knowing what the single agent was allowed to do — a single strong model with tools+CoT often erases gains attributed to "multi-agent."
>
> 3. **No model-generation / recency cutoff.** Agent benchmarks move fast... Without a cutoff, agents will mix 2023 and 2026 results as if comparable.
>
> 4. **No units for "cost multiplier."** Tokens? Dollars? Wall-clock latency? API calls? ...without a target unit, (a) becomes six incompatible numbers that can't be aggregated or even compared to each other.
>
> 5. **No scope boundary on "research and analysis tasks."** Could include coding benchmarks, deep-research benchmarks (BrowseComp, GAIA, HLE), QA, or agentic SWE tasks.
>
> 6. **No source-credibility/COI weighting rule.** Vendor-published benchmarks (e.g., a lab's own multi-agent product blog) vs. independent academic evals need different epistemic weight.
>
> 7. **No canonical claim-ID/format for downstream dedupe.**
>
> **The one gap that would silently corrupt all six outputs at once:** the undefined scope of "multi-agent LLM system" (#1) — because it's the central construct of the entire research question, each agent will independently and plausibly operationalize it differently, and since every downstream step (dedupe, skeptic verification, report) trusts that the six angles are talking about the same thing, nothing in the pipeline is positioned to catch the mismatch.

That last paragraph is the whole argument for the canary step in one sentence. The brief was patched with all seven — explicit definitions of both sides, a compute-matched flag on every claim, a token-preferred cost unit, a January 2024 cutoff, a vendor/independent COI tag with a MEDIUM cap on vendor self-reports, and a fixed five-field output shape. Cost: one round trip, 31 seconds, 24k tokens. Without gap #2 patched, six agents would have compared multi-agent systems against six different notions of "one agent," and the dedupe step would have merged them as if they matched.

Gap #6 turned out to matter more than expected: it is the only reason Anthropic's own 90.2% figure entered this report capped at MEDIUM rather than as a headline.

## Step 3 — merge and dedupe (deterministic, no agent)

Six angles returned **36 raw claims**. Mechanical merge of semantic duplicates, all sources kept:

- Anthropic's 90.2% figure arrived from angles 1, 2, and 4 → 2 merges
- Anthropic's 15x token multiplier from angles 1 and 4 → 1 merge
- Tran & Kiela equal-token-budget from angles 3 and 4 → 1 merge
- Smit et al. "Should we be going MAD?" from angles 3 and 6, via two different URLs (arXiv and PMLR) → 1 merge
- Verga et al. PoLL from angles 2 and 6 → 1 merge

**36 raw → 5 merges → 31 unique claims. 20 tagged high importance.**

One angle-5 finding is worth recording because it is a non-result: that agent tried to pull architecture-labeled numbers from DeepResearch Bench's live leaderboard, got internally inconsistent tables (an "Overall" value of 111.21), and discarded the source rather than report it. It also flagged that an aggregator site was serving GAIA numbers for model names it could not corroborate against the official leaderboard, and excluded them. Two potential sources went in the bin at the search stage, before any verifier saw them.

## Step 4 — adversarial verification, under an explicit cap

**The cap: 8 claims, not 25.** The repo's guidance forbids silent caps, so: 20 claims qualified as high importance; 8 were verified; **12 high-importance claims were never adversarially tested.** Selection rule, applied by the orchestrator: the 8 whose falsity would most change the repo's premise, split deliberately across pro-premise and anti-premise claims so the round could not only shoot at one side.

24 verifiers, 3 per claim, dispatched in two parallel batches of 12. Each verifier received only the claim text and its source URL — never the search agent's reasoning, never another verifier's output, never the other seven claims. The template's skeptic instruction was forwarded verbatim, with one formatting addition (250-word cap, VERDICT/EVIDENCE/NOTE shape) so the vote could be tallied mechanically. Kill rule as written: 2+ REFUTED of 3 → the claim dies.

### The tally

| # | Claim (abbreviated) | Votes | Outcome |
|---|---|---|---|
| 1 | Anthropic multi-agent beat single-agent Opus 4 by 90.2% | 0 REFUTED / 3 could not refute | survives |
| 2 | Multi-agent ≈15x chat tokens; single agent ≈4x | 0 / 3 | survives |
| 3 | Token usage alone explains 80% of BrowseComp variance | 0 / 3 | survives |
| 4 | Single-agent *consistently* ≥ multi-agent at equal thinking tokens | **3 REFUTED** / 0 | **dies** |
| 5 | Automatic MAS *consistently* underperform CoT-SC at up to 10x cost | **3 REFUTED** / 0 | **dies** |
| 6 | 41–86.7% failure rate across 7 SOTA open-source MAS | 1 REFUTED / 2 | survives, wounded |
| 7 | 9 judges from 7 families = ~2.18 effective votes | 0 / 3 | survives |
| 8 | Multi-agent scaffold moved Opus GAIA-L2 by up to 28 pp | 1 REFUTED / 2 | survives, wounded |

**6 of 8 survived, 2 died. 25% rejection rate** — inside the template's stated 16–40% field baseline.

The direction is the story. **Both claims that died were arguments against multi-agent systems.** The verifiers killed them by opening the papers' own tables and finding multi-agent winning in cells the abstracts had smoothed over.

## Step 5 — stop-loss check

No sub-question hit zero survivors, so nothing triggers the two-round stop-loss. One flag: angle 3 (null and negative results) contributed 2 of the 8 verified claims and **both died**. Angle 3's remaining evidence is entirely UNVERIFIED. If a second round is run, angle 3 is where the verification budget should go — not because it is weak, but because the two pieces of it that were tested both turned out to be overstated by their own authors' abstracts.

---

# Report

## 1. Executive summary

Multi-agent systems do beat single-agent baselines on research tasks, but almost every large reported gain is measured against a baseline that was given less compute, and the only lab number that survived verification (90.2%) comes with a 15x token multiplier from the same page. Under compute-matched conditions the advantage shrinks toward zero and flips sign depending on model family, benchmark, and budget — nobody has shown a stable architecture-level effect. The two strongest published refutations of multi-agent value both died in this round for overclaiming the word "consistently," which is itself evidence that the honest answer is "it depends," not "single-agent wins." For the repo's specific mechanism — isolated skeptics — the sharpest finding is adverse: nine judges across seven model families deliver ~2.18 independent votes, so three same-family verifiers are worth substantially less than three votes. The cost multiplier is real and large; the accuracy multiplier is contested and conditional.

## 2. Findings

### 2.1 What the labs report about themselves

**VERIFIED (3–0, could not refute) — confidence MEDIUM (vendor-reported, not compute-matched).** Anthropic's multi-agent research system (Opus 4 lead, Sonnet 4 subagents) outperformed single-agent Opus 4 by 90.2% on an internal research eval. All three verifiers opened the page, confirmed the sentence verbatim and unrevised as of 2026-08-03, and all three independently attacked the same soft spot. Verifier 1b:

> The same page discloses the grading method: an LLM judge emitting "scores from 0.0-1.0 and a pass-fail grade" against a rubric, on an eval that began as "a set of about 20 queries." No dataset, no n for the 90.2% run, no baseline score, no confidence interval, no human-verified head-to-head. […] "90.2%" is a relative delta on an undisclosed, LLM-judged, ~20-query internal set against a 15x-cheaper baseline — precise-sounding but unfalsifiable and uncontrolled for compute.

Verifier 1b also surfaced, unprompted, that Cognition published the opposite architectural conclusion one day earlier ([Don't Build Multi-Agents](https://cognition.com/blog/dont-build-multi-agents), June 2025). Verifier 1c added that both models in the comparison were retired on 2026-06-15.

**VERIFIED (3–0) — confidence MEDIUM (vendor-reported).** Multi-agent systems use about 15x more tokens than chat interactions; single agents with tools about 4x. The quote is exact. All three verifiers refused to let the generalization stand, and one found a case where the ratio inverts — verifier 2c:

> AssetOpsBench retrospective (arXiv 2605.08518, May 2026): "Single-agent executions consume nearly twice the tokens of multi-agent executions on average (121K vs. 63K, t = 7.18, p < 0.001)... The result cautions against a common assumption... that multi-agent evaluations are inherently more expensive."

Verifier 2b found measured cross-architecture overheads of 1.18x (parallel), 1.39x (hierarchical), and 2.20x (reflexive) in [arXiv 2603.22651](https://arxiv.org/html/2603.22651), and concluded: "treat 4×/15× as one vendor's 2025 telemetry, not a constant, since measured ratios range from 1.18× to 1000× depending on task."

**VERIFIED (3–0) — confidence MEDIUM (vendor-reported, no methodology released).** Token usage by itself explains 80% of performance variance in Anthropic's BrowseComp analysis. Verified against a Wayback snapshot from 2025-08-07 to rule out a silent edit. This is the single most load-bearing number in the entire report, because it is the vendor's own admission that most of its 90.2% may be purchased rather than architected. Verifier 3c's caveat is the one to carry forward:

> Anthropic's own post undercuts a causal reading by noting that upgrading to Sonnet 4 beat doubling Sonnet 3.7's token budget, so "80% of variance" should never be restated as "tokens are 80% of what matters."

**UNVERIFIED (single source, not tested under the cap).** Google DeepMind's AI co-scientist reaches 78.4% top-1 on GPQA diamond, while the same paper concedes that o3-mini-high and DeepSeek R1 "demonstrated competitive performance while requiring significantly less compute and reasoning time" ([arXiv 2502.18864](https://arxiv.org/abs/2502.18864)). Same paper, same angle: "Note that Elo metric is auto-evaluated and not based on the ground truth."

**UNVERIFIED (single source).** Anthropic states its multi-agent architecture is a poor fit for coding and for any domain requiring shared context or many inter-agent dependencies.

**UNVERIFIED (single source, weak baseline).** Microsoft's Magentic-One scored 38% GAIA / 27.7% AssistantBench / 32.8% WebArena against a tool-less GPT-4 baseline at roughly 7% / 16% / 15% — a comparison the brief's own definition disqualifies, since the baseline had no browsing.

### 2.2 Compute-matched evidence — the part that actually decides the question

**REJECTED (3–0).** See ledger item R1. The claim that single agents *consistently* win at equal thinking-token budgets did not survive.

**REJECTED (3–0).** See ledger item R2.

**VERIFIED (1 REFUTED / 2 could not refute) — confidence LOW-MEDIUM.** 41–86.7% failure rates across open-source multi-agent systems, from Cemri et al., [Why Do Multi-Agent LLM Systems Fail?](https://arxiv.org/abs/2503.13657) (NeurIPS 2025 D&B). Survives the vote but arrives badly damaged, and all three verifiers found the same defect independently: the paper says "7 systems," its own Figure 5 caption says **six** and plots six. Verifier 6c:

> Version instability: v1 (Mar 2025) charted five systems with AG2 on GSM-Plus at 15.2% failure — a 15%–86.7% band. The 41% floor exists only because v3 swapped AG2's benchmark to OlympiadBench.

Verifier 6b voted REFUTED on a stronger ground — that the "gains often remain minimal" sentence is not this paper's finding at all:

> The "minimal gains" line is not this paper's result. Refs [18]/[19] in v3's bibliography are Xia et al. 2024 (Agentless) and Kapoor et al. 2024 (AI Agents That Matter) — third-party citations in the intro, never tested here.

That is correct and it survived only because the other two verifiers scored the same observation as a caveat rather than a refutation. **Cite the failure-rate range as a spread of six non-comparable benchmark scores with no single-agent control, never as a measured multi-agent failure rate.**

**INFERENCE (this report's interpretation).** Across findings 2.1 and 2.2 together: no source in this round demonstrates an architecture-level multi-agent advantage that holds across model families at matched compute. Anthropic's 80%-of-variance disclosure and the two rejected papers point the same way from opposite directions — the measured effect tracks spend and configuration, not topology.

### 2.3 Independent third-party evaluation

**VERIFIED (1 REFUTED / 2 could not refute) — confidence LOW (single-author preprint, v1, unreplicated).** In a pre-registered controlled GAIA comparison ([arXiv 2606.08529](https://arxiv.org/abs/2606.08529)), a multi-agent Planner-Actor-Rater scaffold moved Claude Opus's Level-2 accuracy by up to 28 points, while showing no statistically significant advantage over single-agent ReAct for Gemini 3.1 Pro (+0.058) or GPT-5.5 (+0.016) — both CIs spanning zero. Two verifiers confirmed the numbers verbatim; the third voted REFUTED on scope. All three found the same confound, which the paper itself discloses:

> "Contrasts involving s1, including the H3 test, therefore measure loop structure **and tool surface jointly**" (ReAct got `web_browser` and no text editor; PAR got `web_search` + `text_editor`) — and 28 points is the favorable *robust* slice, versus **+0.140** in the pre-registered primary slice.

Verifier 8c pushed further: the robust slice drops units carrying a `provider_serialization_bug` that "selectively suppressed s2 and s3 accuracies" and "did not affect" the cross-provider cells — meaning **the Anthropic-vs-others split the claim rests on is partly an artifact of an SDK defect touching only Anthropic runs.** Use +14 points, not +28.

**UNVERIFIED (single source).** Hugging Face's Open Deep Research — a genuine multi-agent replication — reaches 55% pass@1 on GAIA validation against 67% for the closed original. A documented case of a multi-agent architecture losing to a stronger system by 12 points.

**UNVERIFIED (single source).** Hugging Face's "naive" multi-agent orchestration scored 44.2% on GAIA validation vs 40% for a more elaborate Autogen multi-agent system, while bare GPT-4-Turbo scored under 7% — suggesting agentic tool access, not multi-agent complexity, carried most of the gain.

### 2.4 The repo's own mechanism: isolated verifiers

**VERIFIED (3–0) — confidence MEDIUM (single-author preprint, hosted by Apple ML Research, unreplicated).** A panel of 9 frontier LLM judges from 7 different model families provides only ~2.18 effective independent votes (Kish n_eff, 95% CI [2.07, 2.31]; independence ratio 24.2%), and the best single judge matches or outperforms the full panel across all conditions tested ([arXiv 2605.29800](https://arxiv.org/abs/2605.29800)). Verifier 7b recomputed the statistic from scratch rather than trusting it:

> I recomputed the statistic. Kish formula with the paper's reported φ̄=0.391, k=9: 9/(1+8×0.391) = **2.1802**, ratio **24.22%**. Matches to the digit.

Verifier 7c found the caveat that keeps this from being fatal to panels: "identifying the best individual also requires oracle access to gold labels" — the "best judge" is chosen post-hoc on the test set, so it is not actionable prospectively. Verifiers 7a and 7b both noted 2.18 is the MNLI-specific low end (SNLI 2.35, AlphaNLI 2.48), and that on MNLI the panel actually edged the best judge by +0.2 points, so "matches" is quietly counting a loss as a tie.

**This is the finding that most directly indicts this very run.** Seven model families collapse to ~2.18 effective votes. This round used three verifiers, all Claude, all one family. The template asks for at least one verifier from a different model family; **this run could not satisfy that**, and by this paper's own metric the three-verifier votes above are worth meaningfully less than three.

**UNVERIFIED (single source, vendor-reported).** OpenAI's CriticGPT: model critiques preferred over human critiques 63% of the time, but on outputs contractors rated "flawless" the critic flagged a substantive problem in 24% of cases against a ~6% human baseline — a roughly 4x over-flagging rate. The critic ensemble's own false-positive mode, documented by its own vendor.

**UNVERIFIED (single source, vendor-reported, cost-inverted).** A 3-model Panel-of-LLM-evaluators beat a single GPT-4 judge on agreement with humans (κ 0.906 vs 0.841 on TriviaQA) at 7–8x *lower* cost — the one case in this round where multi-agent is both more accurate and cheaper.

**UNVERIFIED (single source).** Huang et al.: on GSM8K at 9 responses, multi-agent debate reached 83.0% while plain self-consistency reached 88.2% — a 5.2-point gap favoring the single model at equal sample count.

### 2.5 Cost economics

**UNVERIFIED (single source).** A single agent simulating a multi-agent workflow in one context matched homogeneous multi-agent accuracy at lower cost via KV-cache reuse: GSM8K $0.387 vs $0.623, MATH $0.677 vs $0.819 ([arXiv 2601.12307](https://arxiv.org/abs/2601.12307)).

**UNVERIFIED (single source).** Optimal test-time compute allocation within a single model improves efficiency >4x over best-of-N, and in FLOPs-matched evaluation lets a smaller model beat one 14x larger ([arXiv 2408.03314](https://arxiv.org/abs/2408.03314)).

**UNVERIFIED (single source, out-of-core).** Repeated sampling from a cheap open model beat single attempts from frontier models on SWE-bench Lite at under a third the dollar cost ([arXiv 2407.21787](https://arxiv.org/abs/2407.21787)). Coding benchmark, included as contrast only.

**UNVERIFIED (single source).** Self-MoA — ensembling a single top model with itself — beat standard mixed-model Mixture-of-Agents by 6.6% on AlpacaEval 2.0 ([arXiv 2502.00674](https://arxiv.org/abs/2502.00674)), because mixing in weaker models drags the average down.

## 3. Rejected — do not cite

Next round's dedupe ledger. Both kills were unanimous, and both were anti-multi-agent claims that died on the word "consistently."

**R1 — 3 REFUTED / 0. "Under equalized reasoning-token budgets on FRAMES and 4-hop MuSiQue, single-agent systems *consistently* match or outperform five multi-agent architectures across three model families."** Source: [arXiv 2604.02460](https://arxiv.org/abs/2604.02460), Tran & Kiela. The paper is real and the sentence is a faithful quote of its abstract — but three verifiers independently opened the tables and found the abstract contradicts them. Verifier 4c:

> **Table 8 (MuSiQue 4-hop, Gemini-2.5-Pro): Debate beats SAS with non-overlapping CIs at all six budgets** — SAS 0.308/0.391/0.413/0.412/0.419/0.428 vs Debate 0.412/0.435/0.444/0.448/0.470/0.458. […] Budgets were not actually equalized on the third family. Table 8 at "10k": SAS self-counts 1522 thinking tokens, Debate 6615. The paper concedes "artifacts in API-based budget control (particularly in Gemini 2.5)."

Verifier 4b recomputed all 240 head-to-heads: "**MAS beat single-agent in 66/240 (27.5%)**… At the 100-token budget, averaged over all 8 cells, **four of five MAS architectures beat SAS**." Verifier 4a independently found the same Gemini/MuSiQue cell. One-line reason: *the paper's own tables show multi-agent winning an entire model-family cell at every budget, and its "equal budgets" were caps on spend, not matched spend.*

**R2 — 3 REFUTED / 0. "Automatically-designed multi-agent systems *consistently* underperform CoT-SC across reasoning datasets and interactive workflows including BrowseComp-Plus, despite being up to 10x more expensive."** Source: [arXiv 2606.13003](https://arxiv.org/abs/2606.13003), "The Illusion of Multi-Agent Advantage." Again a verbatim abstract quote; again contradicted by the paper's own table. Three verifiers recomputed it separately and got 16/71, 21/81, and a clean sweep in one cell — different table versions, same conclusion. Verifier 5b:

> **On Gemini-2.5-Pro / GPQA-Diamond, every automatic MAS beats CoT-SC** (CoT-SC 83.13; DyLAN 87.35, MaAS 86.95, MAS-Zero 86.14, ADAS 84.52, AFlow 83.33) — a clean sweep in the wrong direction. […] **The cost half fails too.** GPQA / GPT-OSS-120B: DyLAN scores 75.70 at $0.90 vs CoT-SC 71.48 at $1.90 — *higher accuracy at half the cost*.

Verifier 5c found the paper's own figure caption carving out the exception the claim drops — "except on HLE-Math" — and that "up to 10x" is wrong in the other direction too: "AFlow/HLE-Maths/GPT-4o is 33.6× CoT-SC cost." Verifier 5a noted the paper's introduction states the far weaker "automatic MAS do **not consistently outperform** SAS." One-line reason: *"consistently underperform" holds on exactly one of four benchmarks; on the two traditional reasoning sets multi-agent wins roughly a quarter of cells, sometimes at lower cost.*

**Do not re-derive either claim next round. Both papers remain citable for the weaker statement they actually support: multi-agent advantage is inconsistent, not absent.**

## 4. Coverage notes

- **The cap, stated plainly.** 20 high-importance claims qualified; **8 were verified; 12 were not.** The template calls for up to 25. Everything labeled UNVERIFIED above rests on a single search agent's reading of a single source and has survived no adversarial test whatsoever. The 25% rejection rate observed on the tested 8 is the best available estimate of how many of the untested 12 would also die — roughly three of them, identity unknown.
- **Sub-questions with zero verification coverage:** angle 4 (cost economics) and angle 5's leaderboard claims other than the GAIA scaffold study. Every number in §2.5 is unverified.
- **Numbers resting on a single source:** Anthropic's 90.2%, 15x, and 80% all trace to one blog post — the verifiers confirmed there is no independent replication of any of them, and that every secondary citation traces back to that same page.
- **Model family diversity: not achieved.** All 24 verifiers were Claude Opus, all 6 search agents Claude Sonnet, one vendor. The template asks for at least one verifier from a different family, and finding 2.4 quantifies exactly what that costs.
- **One verifier produced a false negative on source existence.** Verifier 3b reported that arXiv 2604.02460 "returns zero entries from the arXiv API — I could not confirm those papers exist," and declined to use it as counter-evidence. Five other agents opened the same paper successfully, including its full HTML tables. Had that verifier been the only one checking, a real paper would have been recorded as nonexistent.
- **arXiv's HTML endpoint served the wrong paper, twice.** Verifier 7c: "`arxiv.org/html/2605.29800v1` served me a different paper entirely ('Scaffold Effects on GAIA'). The PDF is correct; treat the HTML endpoint as unreliable." Verifier 7b independently hit the same glitch and caught it by refetching and comparing md5 hashes. Two isolated agents hitting and diagnosing the same infrastructure fault is the clearest case in this run of redundancy paying for itself.
- **Two verifiers exhausted their web-search budget** (8a and 8b, both on the GAIA scaffold claim) and could not search for independent replication. Their verification is primary-source-only. Recorded rather than smoothed over.

## 5. Open questions

1. **Is there any compute-matched, cross-family, multi-round study showing a stable multi-agent advantage on open-ended research tasks?** Two candidates surfaced *during verification* and were never themselves verified: [arXiv 2605.01566](https://arxiv.org/abs/2605.01566) (Wunderlich et al., "With an equal computing budget, debate and mixture-of-agents outperform self-consistency by 1.3% and 2.7% points") and [arXiv 2512.08296](https://arxiv.org/html/2512.08296v1) ("On BrowseComp-Plus… Decentralized achieves +9.2% (0.347 vs. SAS 0.318)" under matched token budgets). Both point *for* multi-agent under matched compute. Both are unverified. **These are the highest-value targets for round 2** and they exist only because skeptics went looking for counter-evidence.
2. **What is the effective-vote count for 3 same-family verifiers?** The 2.18-of-9 figure is for 7 families. Nobody has published the number for 3 instances of one model, which is what this repo's templates actually deploy.
3. **What is the false-refutation rate of an adversarial verifier ensemble?** CriticGPT's 24%-vs-6% over-flagging is the only measurement found, and it is on code, vendor-reported, and out-of-core. The kill rule in this template assumes REFUTED votes are more trustworthy than COULD-NOT-REFUTE votes. Nothing in this round tested that assumption.
4. **Does the fan-out step or the verification step carry the value?** Every source studied whole architectures. Nobody has ablated a research pipeline into "parallel search only" vs "verification only" vs "both." This is answerable with first-party experiments and is not in public data.
5. Angle 3's untested claims (Smit et al., CopMAD debate hacking, non-monotonic LLM-call scaling) all need verification before reuse, given that both of its tested claims were overstated by their own abstracts.

## 6. Sources

### Primary

- Anthropic, *How we built our multi-agent research system* — https://www.anthropic.com/engineering/multi-agent-research-system (vendor-reported)
- Cemri et al., *Why Do Multi-Agent LLM Systems Fail?*, NeurIPS 2025 D&B — https://arxiv.org/abs/2503.13657
- Tran & Kiela, *Single-Agent LLMs Outperform Multi-Agent Systems… Under Equal Thinking Token Budgets* — https://arxiv.org/abs/2604.02460 **(claim rejected, see R1)**
- Jwalapuram et al., *The Illusion of Multi-Agent Advantage* — https://arxiv.org/abs/2606.13003 **(claim rejected, see R2)**
- Kohli, *Nine Judges, Two Effective Votes* — https://arxiv.org/abs/2605.29800 (also hosted at https://machinelearning.apple.com/research/correlated-llm-evaluation-panels)
- Starace, *Scaffold Effects on GAIA: A Controlled Comparison* — https://arxiv.org/abs/2606.08529
- Du et al., *Improving Factuality and Reasoning… through Multiagent Debate* — https://arxiv.org/abs/2305.14325
- Li et al., *More Agents Is All You Need* — https://arxiv.org/abs/2402.05120
- Wang et al., *Mixture-of-Agents* — https://arxiv.org/abs/2406.04692 (vendor-reported, Together AI)
- Verga et al., *Replacing Judges with Juries* (PoLL) — https://arxiv.org/abs/2404.18796 (vendor-reported, Cohere)
- Huang et al., *Large Language Models Cannot Self-Correct Reasoning Yet* — https://arxiv.org/abs/2310.01798
- Smit et al., *Should we be going MAD?*, ICML 2024 — https://arxiv.org/abs/2311.17371 / https://proceedings.mlr.press/v235/smit24a.html
- *Stop Overvaluing Multi-Agent Debate* — https://arxiv.org/abs/2502.08788
- Chen et al., *Are More LLM Calls All You Need?*, NeurIPS 2024 — https://arxiv.org/abs/2403.02419
- *When and Why Does Multi-Agent Debate Fail…* — https://arxiv.org/abs/2510.20963
- Brown et al., *Large Language Monkeys* — https://arxiv.org/abs/2407.21787
- Snell et al., *Scaling LLM Test-Time Compute Optimally…* — https://arxiv.org/abs/2408.03314
- Xu et al., *Rethinking the Value of Multi-Agent Workflow* — https://arxiv.org/abs/2601.12307
- Li et al., *Rethinking Mixture-of-Agents* — https://arxiv.org/abs/2502.00674
- McAleese et al., *LLM Critics Help Catch LLM Bugs* — https://arxiv.org/html/2407.00215v1 (vendor-reported, OpenAI)
- Google DeepMind, *Towards an AI co-scientist* — https://arxiv.org/abs/2502.18864 (vendor-reported)
- Microsoft Research, *Magentic-One* — https://www.microsoft.com/en-us/research/articles/magentic-one-a-generalist-multi-agent-system-for-solving-complex-tasks/ (vendor-reported)
- Hugging Face, *Open Deep Research* — https://github.com/huggingface/smolagents/tree/main/examples/open_deep_research
- Hugging Face, *Beating GAIA* — https://huggingface.co/blog/beating-gaia (vendor-reported)

### Surfaced by verifiers as counter-evidence (not independently verified)

- *AssetOpsBench* retrospective — https://arxiv.org/pdf/2605.08518
- Kulkarni & Kulkarni, agent architecture overheads — https://arxiv.org/html/2603.22651
- Bai et al., *How Do AI Agents Spend Your Money?* — https://arxiv.org/abs/2604.22750
- Wunderlich et al., *Multi-Agent Reasoning Improves Compute Efficiency* — https://arxiv.org/abs/2605.01566
- Kim, Gu, Park et al., *Towards a Science of Scaling Agent Systems* — https://arxiv.org/html/2512.08296v1
- Zhao et al., single-vs-multi non-replication — https://arxiv.org/abs/2602.04234
- *Benchmark Test-Time Scaling of General LLM Agents* — https://arxiv.org/html/2602.18998v1

### Secondary

- Cognition, *Don't Build Multi-Agents* — https://cognition.com/blog/dont-build-multi-agents
- Simon Willison, note on the Anthropic post — https://simonwillison.net/2025/Jun/14/multi-agent-research-system/
- MarkTechPost on Magentic-One — https://www.marktechpost.com/2024/11/06/microsoft-researchers-introduce-magentic-one-a-modular-multi-agent-system-focused-on-enhancing-ai-adaptability-and-task-completion-across-benchmark-tests/
- Anthropic model deprecations — https://platform.claude.com/docs/en/about-claude/model-deprecations

---

## What this run shows

- **The canary was the highest-leverage 31 seconds in the round.** Its top gap — nobody had defined "multi-agent system" — would have produced six angles silently measuring different things, and the deterministic dedupe would have merged them without complaint, because dedupe matches text, not meaning. One agent, no tools, 24k tokens, caught a failure that would have been invisible in the final report.

- **Isolated convergence held up, and produced things one agent would not have.** Three verifiers, none seeing the others, each downloaded arXiv 2604.02460 and each found multi-agent winning the Gemini/MuSiQue cell. Two independently hit the same arXiv HTML fault serving the wrong paper, and both diagnosed it rather than reporting the wrong content. Verifier 7b recomputed the Kish formula by hand and matched to four digits. That is what the redundancy is for.

- **The method attacked its own thesis and the thesis partly lost — but so did the attack.** Going in, the interesting outcome was "multi-agent doesn't work." What actually happened is stranger and more useful: the two most quotable *anti*-multi-agent papers both died, unanimously, because their abstracts overstated their own tables. Meanwhile the pro-multi-agent flagship number survived 3–0 while every verifier documented that it is not compute-matched, is LLM-judged on ~20 queries, and comes with a 15x token bill from the same page. **The honest state of the evidence is that nobody has cleanly demonstrated an architecture-level effect in either direction, and both camps' headline sentences are stronger than their data.** The repo's premise is not refuted. It is unproven, and its most-cited supporting number is a spending comparison.

- **Where the multi-agent approach did not pay for itself here.** Claims 1, 2, 3, and 7 each cost three verifiers and returned 3–0 unanimous COULD-NOT-REFUTE. For those four, one verifier would have produced the same verdict at a third the cost — twelve agents bought four votes' worth of information. Concretely: roughly 460k tokens spent on unanimous confirmations. The value concentrated entirely in claims 4, 5, 6, and 8, where verifiers disagreed or overturned the search agent. Ex ante there was no way to know which four were which — but that is a real cost, not a rounding error, and it is precisely the shape the Kish result predicts.

- **Finding 2.4 indicts this transcript.** Nine judges across seven model families are worth ~2.18 independent votes. This run used three verifiers from one family. Every "3–0" above should be read as substantially less than three independent confirmations, and the correct fix — a non-Anthropic verifier — was unavailable in this harness. That is a limitation of the run, not a caveat to be waved at.

- **The kill rule's asymmetry is untested and it shaped this result.** Two claims died on 3-0 REFUTED votes. Claim 6 survived 2-1 even though the dissenting verifier was, on inspection, right about the strongest point — the "gains remain minimal" sentence is a citation of two other papers, not a finding of the one cited. The template counts votes; it does not weigh the quality of the reasoning behind them. In this round, majority rule preserved a claim whose minority objection was the more accurate one.

- **What went wrong operationally.** One verifier declared a real paper nonexistent based on an arXiv API query that five other agents contradicted. Two verifiers exhausted their search budget and could only check the primary source against itself. Two candidate sources were discarded at the search stage for serving internally inconsistent numbers. None of this was fatal because there were other agents — which is an argument for the topology, and simultaneously a reminder that a single-agent version of this run would have shipped at least one confident falsehood.

Before this run, "multi-agent beats single-agent by 90.2%" was a number this repo could quote. It still is — it just now comes with the sentence three rooms down the same page that says token spend explains 80% of the variance.
