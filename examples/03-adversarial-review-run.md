# 03 Adversarial Review — a real run, on this repo's own article

Twenty-six frozen conclusions, five isolated attackers, no mercy for the author. This is a transcript of [template 03](../prompts/03-adversarial-review.md) executed on 2026-08-03 in Claude Code, with [ARTICLE.md](../ARTICLE.md) — this repository's own companion essay — as the document under attack. The method turned on the thing that describes it. Cost: roughly 300k subagent tokens across six dispatches (one canary, five attackers), about 50 minutes wall-clock end to end — of which the last 25 were spent recovering two attackers that had silently stalled. Attacker outputs below are verbatim. Prep, arbitration, and the arithmetic recomputation were done in the main loop, because the template says that layer cannot be outsourced, and this run is a demonstration of why.

The short version: all 26 conclusions were eventually adjudicated. **Zero survived unqualified.** Six were overturned outright, two are unverifiable by construction, sixteen came back weakened, and two survived only with a correction attached. The article's single showcase example of adversarial verification working correctly turned out to be an example of it failing — and the article's one explicit rebuttal to the multi-agent counter-camp turned out to be backwards.

## Prep — the frozen conclusion list

The article's load-bearing claims, extracted into assertions that can be overturned. Twenty-six of them, grouped by professional domain. This list is what the attackers saw; they never saw the article, its framing, its author, or each other.

**Research-methods history and decision science (H)**

1. **H1** — Isolated skeptics descend from the Delphi method (RAND, 1950s), in which experts answer independently and never meet.
2. **H2** — Later experimental work confirmed the reason Delphi works: social influence wrecks the accuracy of group judgment.
3. **H3** — "Designated attack" descends from Devil's Advocacy, a 1970s management-science technique; the finding is that structured attack exposes hidden assumptions better than expert consensus does.
4. **H4** — The open question is the Premortem (Gary Klein, HBR, 2007), and it empirically digs up about 30% more risks than a straight review.
5. **H5** — The rejection ledger descends from Analysis of Competing Hypotheses (CIA, Heuer 1999); refuted hypotheses are kept so they cannot resurrect, and there is an official standard for labeling confidence.
6. **H6** — Knight & Leveson (1986) established that independently written programs still make correlated errors, which justifies "one model, copied, is one blind spot, copied" and the prescription to bring in a counter-examiner from another model family.

**LLM multi-agent and debate literature (M)**

7. **M1** — AI Safety via Debate (Irving, Christiano & Amodei, 2018) argued that models attacking each other gets closer to truth than models grading themselves.
8. **M2** — Anthropic published in 2025 an engineering account of its multi-agent research system reaching the same conclusion: multi-agent wins clearly on breadth, at ten-something times the token cost.
9. **M3** — Since 2024 a counter-literature argues the gains from multi-agent debate are sometimes just more compute, not more perspectives (Chen et al., 2024).
10. **M4** — That criticism targets debate-to-consensus architectures; independent attackers with a human arbitrator sit outside its range.
11. **M5** — An agent never grades its own homework; when an agent iterates on its own work, wrong assumptions reinforce themselves, whereas isolated skeptics carry no such baggage.
12. **M6** — Isolation buys independence and buys noise, so collation must filter line by line; item-by-item checks produce corrections, while the open question produces reversals.

**Technology regulation and AI-industry facts (R)**

13. **R1** — The EU AI Act's high-risk obligations were scheduled for August 2026 but were amended and pushed to December 2027.
14. **R2** — "A frontier lab spends about $1B a year buying RL environments" is media hearsay with no independent sourcing, and should not be cited.
15. **R3** — "A startup grew revenue 15x in a year" was self-reported with no independent source, and should not be cited.
16. **R4** — "Graph Engineering" became widely used in late July 2026, immediately after "Loop Engineering," and was current enough by 2026-07-30 to be described as having just taken off.

**Quantitative methodology and measurement (Q)**

17. **Q1** — Across three research rounds: 313 agents, about 13 million subagent tokens, 332 falsifiable claims, 75 into verification.
18. **Q2** — The three rounds had claim rejection rates of 16%, 40%, and 28%.
19. **Q3** — "A 40% rejection rate means nearly half of the public data in my field can't survive three skeptics."
20. **Q4** — In the adversarial review, about one fifth of a hand-revised plan's conclusions were overturned or corrected in one round, and the single biggest number was overstated by almost 5x.
21. **Q5** — Replacing a loop-style rejection ledger with a graph-structured lookup dropped lookup cost by about 12x.

**Agent engineering: cost, context, architecture (E)**

22. **E1** — Model tiering is a real saving: search and fetch are repetitive work a cheap model handles fine; judgment belongs in verification and synthesis. Running all ~300 agents on the most expensive model was waste.
23. **E2** — Most "then"s in hand-written agentic flows are false edges, and the single planning-time test is whether you can name the data flowing along the arrow.
24. **E3** — A single chat session cannot hold 20-some sources open at once because the context window will not take it; the graph works because each agent carries only its own share.
25. **E4** — Arithmetic should never be outsourced to an agent; math is plumbing, keep it in the main loop.
26. **E5** — A graph amplifies search but cannot manufacture answers absent from public data; two questions returned zero surviving claims round after round, and a hundred more agents would return the same nothing.

## The domain split

Five domains, mutually exclusive, 4–6 conclusions each. Each attacker got a de-identified background brief (the operator described as "one independent operator," the field run as private and unpublished), its own slice of the conclusion list, 2–3 mandatory attack questions aimed at the points I judged weakest, and the template's closing open question. None was told whether the author wanted them to succeed or fail.

| Attacker | Domain | Conclusions | Returned |
|---|---|---|---|
| 1 | Research methodology and decision science | H1–H6 | Yes, first pass |
| 2 | ML literature: multi-agent, debate, self-critique | M1–M6 | **Stalled; recovered on resume** |
| 3 | Technology regulation and AI-industry fact-checking | R1–R4 | Yes, first pass |
| 4 | Quantitative methodology and measurement | Q1–Q5 | Yes, first pass |
| 5 | LLM application engineering: context, tiering, cost | E1–E5 | **Stalled; recovered on resume** |

All five ran on the same model family, on a strong reasoning tier. That is a direct violation of the article's own prescription (H6) and of the template's step 5, and it is recorded honestly at the bottom.

## The canary, and the isolation breach

Template step 6 says: before fan-out, send one attacker the brief and ask it to list every fact the brief does not give it. I ran that on the quantitative group. It came back with sixteen missing facts, which is why the fan-out brief explicitly told attackers what was *not* available rather than letting five agents independently reason around the same hole.

It also did something I did not ask for and did not want:

> Checked the local repo first: the source is at `/Users/.../ARTICLE.md`. Its quantitative passages (lines 100, 106, 112, 118, 151) restate the same figures with no definitions, denominators, or units — so the gaps below are missing from the document itself, not just from the brief.

The canary had filesystem access and used it to find the document it was supposed to be blind to. De-identification protects against an attacker *guessing*; it does nothing against an attacker *reading the disk*. Every subsequent attacker prompt carried an explicit no-filesystem constraint. This is a containment failure that the template does not currently warn about, and it is a real finding about running this method inside a coding harness rather than in fresh chat windows.

The canary's most useful output was its unverifiability triage:

> **Q1, Q2, Q4, Q5** — all rest on unpublished logs, claim lists, token accounting, and a private planning document. No external research can confirm or refute them; they can only be attacked for internal consistency, definitional vacuity, and non-reproducibility. Q4's "5x" is doubly unverifiable (private document *and* private corrected source).
>
> **Q3 is the exception** — it is an inference from Q2, not a measurement, and is fully attackable on logic alone (denominator, selected sample, undefined population, uncalibrated verifiers). Attack Q3 at full strength; attack the rest as unfalsifiable-as-stated.

## Attacker 1 — research methods history (H1–H6)

> **H1. Delphi / isolated skeptics — PARTIALLY HOLDS (high confidence).**
> Attribution is right: Project Delphi at RAND, early 1950s; first journal publication Dalkey & Helmer, *Management Science* 9(3):458–467 (1963), from RAND RM-727. But "never meet" is not the definition. The four classical features are anonymity of *response*, iteration, controlled feedback, and statistical group response. Anonymity attaches to the answers, not to the people, and RAND itself markets modified-Delphi designs — the RAND/UCLA Appropriateness Method inserts a face-to-face discussion round between rating rounds. The essay has hardened a design parameter into a law. Fix: "answers are collected before they are compared," not "experts never meet."
>
> **H2. Social influence wrecks group accuracy — PARTIALLY HOLDS, trending OVERTURNED (high).**
> Lorenz, Rauhut, Schweitzer & Helbing, *PNAS* 108(22):9020–9025 (2011), N=144, is real and says mild social influence narrows diversity without improving accuracy. It was contested in *PNAS* immediately (Farrell, "Social influence benefits the wisdom of individuals in the crowd," 2011) and reversed in scope by Becker, Brackbill & Centola, *PNAS* 114(26):E5070–E5076 (2017): in *decentralized* networks social influence reliably *improves* crowd accuracy. The finding is topology-conditional — aggregate feedback to everyone harms; local exchange helps. Also note Woudenberg (1991): Delphi convergence is driven by conformity pressure, which cuts against Delphi rather than for it. "Confirmed" is the wrong verb.
>
> **H3. Devil's Advocacy — PARTIALLY HOLDS (medium-high).**
> "1970s management science" is wrong on origin (the *advocatus diaboli* is a canonization office, formalized 1587 — verify before printing) and imprecise on lineage: the management-science root is Mason, "A dialectical approach to strategic planning," *Management Science* (1969), with Cosier/Schwenk experiments through the 1970s–80s. The empirical clause survives: Schwenk, *OBHDP* 47(1):161–176 (1990), 20 studies, finds DA and dialectical inquiry beat consensus on decision quality. The damaging counter-primary is Nemeth, Brown & Rogers, *EJSP* 31:707–720 (2001): a *role-played* devil's advocate underperformed authentic dissent, generated more thoughts *supporting* the initial position, and may entrench it. A "designated attacker" node is contrived dissent by construction.
>
> **H4. Premortem, "~30% more risks" — OVERTURNED (high).**
> See attack question 1. Number is misattributed and mis-described. Klein's HBR framing ("introduced 2007") is also imprecise — he described the technique in earlier books (check *Sources of Power*, 1998; *The Power of Intuition*, 2003).
>
> **H5. ACH / rejection ledger / confidence standard — PARTIALLY HOLDS (high).**
> Two of three claims survive; the third is a provenance splice. See attack question 3.
>
> **H6. Knight & Leveson — HOLDS as diagnosis, UNSUPPORTED as prescription (high).**
> Knight & Leveson, *IEEE TSE* SE-12(1):96–109 (1986): 27 independently written versions (UVa, UC Irvine), 1M tests, independence-of-failure rejected. "One model, copied, is one blind spot, copied" is well-founded, and now directly replicated on agents: Ron, Baudry & Monperrus, "N-Version Programming with Coding Agents," arXiv:2606.20158 (June 2026) — 48 agent implementations, substantial common-mode failure clustering where the *spec* is ambiguous. But the essay cites a negative result to license a positive fix. Kim, Garg, Peng & Garg, "Correlated Errors in LLMs," ICML 2025 (arXiv:2506.07962), >350 models: errors stay correlated *across distinct architectures and providers*, and correlation rises with capability. Different model family reduces, does not restore, independence.

On the 30% figure specifically:

> **1. The 30%.** It is not Klein's finding and it is not about risks. It traces to Mitchell, Russo & Pennington, "Back to the future: Temporal perspective in the explanation of events," *Journal of Behavioral Decision Making* 2(1):25–38 (1989), as relayed by Klein's HBR piece and by Veinott, Klein & Wiggins (ISCRAM 2010). What the study measured was the *number of reasons generated* — it did not assess reason quality or correctness. Klein's gloss ("increases the ability to *correctly* identify reasons") adds a correctness claim the study does not support, and the essay adds a second distortion ("risks"). I could not open the 1989 full text (Wiley/ResearchGate blocked); accessible summaries indicate Experiment 1 found temporal perspective had *little* influence, with outcome certainty doing the work — so even the ~30% may belong to the certainty manipulation, not the "assume it failed" frame. N and effect size: **unverified** pending the paper. Defensible replacements: Veinott, Klein & Wiggins (2010) — premortem cut overconfidence roughly twice as much as a pros/cons list; Gallop, Willy & Bischoff, *Journal of Enterprise Transformation* 6(2):87–106 (2016) — N=101 program managers/engineers, premortem teams found significantly more quality risks and more "black swan" risks than brainstorming. Cite Gallop, drop the 30%.

On the ACH confidence standard:

> (c) The confidence standard exists but is **not Heuer's and not part of ACH**: it is ODNI **ICD 203, Analytic Standards** (technical amendments signed 2 January 2015), which sets likelihood bands (01–05% "almost no chance" … 95–99% "nearly certain") and forbids combining a confidence level and a likelihood in the same sentence. Attributing it to ACH/Heuer is the essay's clearest citation error.

And its answer to the open question:

> The active ingredient doesn't transfer. Delphi, devil's advocacy, and the premortem all work by defeating *social* pathology among humans who each hold **private, partially independent evidence**. Freshly dispatched agents sampled from one training distribution have neither: no social pressure to break, and no independent evidence to pool. What they have is shared priors — which is exactly what Kim et al. and Ron et al. measure. So the essay may be right about the graph and wrong about why, and every historical citation is decorative rather than load-bearing. That is the failure mode a hostile reader will find.
>
> Most likely cause of failure a year out, ranked: (1) **capability drift** — a single long-context agent with native tool use makes fan-out overhead unjustifiable and the fixed graph becomes ceremony; multi-agent token costs already run several times single-agent for small accuracy deltas. (2) **The bottleneck is upstream of the reviewers**: Cemri et al., "Why Do Multi-Agent LLM Systems Fail?" (arXiv:2503.13657), 7 frameworks, 200+ tasks, 14 failure modes — **41.8% are specification issues**, i.e. the briefs on your edges, not the blind spots in your nodes. Ron et al. found the same: common-mode failures cluster where the *spec* is ambiguous. (3) **Citation laundering** — one reader checks the 30% and the provenance section discredits the engineering.

## Attacker 3 — regulation and industry facts (R1–R4)

> **R1 — PARTIALLY HOLDS (confidence: high).**
> The delay is now adopted law, not a proposal: **Regulation (EU) 2026/1744 of 8 July 2026**, amending Reg. (EU) 2024/1689, 2018/1139 and 2023/1230; published OJ L, **24 July 2026**; in force **27 July 2026**. But "pushed to December 2027" is right for only one subset. **Annex III standalone high-risk** systems (Art. 6(2)): Chapter III Sections 1–3 now apply **2 December 2027** (was 2 Aug 2026). **Annex I Section A product-embedded** systems (Art. 6(1)): now **2 August 2028** — and their original date was **2 August 2027**, never August 2026. So R1 mis-states the baseline for half the regime and omits the second date entirely. Also: the June 2026 provisional agreement dropped the Commission's proposed *conditional* trigger for **fixed** dates — so "conditional stop-the-clock" framings are stale. And Article 50 transparency duties still bite **2 August 2026**.
>
> **R2 — OVERTURNED as reasoned; the underlying number is still misquoted (confidence: high).**
> "No independent sourcing" is wrong. **The Information** (Sept 2025) reported the figure; **TechCrunch, 21 Sept 2025**, cites it by name and links it; **Epoch AI** repeats and contextualises it against ~$19bn OpenAI 2026 R&D compute. It is named, datable, attributable trade reporting — citable with attribution. The real error is different and R2 misses it: the source says **"leaders at Anthropic have *discussed* spending *more than* $1 billion on RL environments *over the next year*"** — a deliberation, forward-looking from Sept 2025, a floor not a point estimate, about RL-environment spend generally, not "buying" from vendors. "A frontier lab spends about $1bn a year buying RL environments" is a four-way distortion of a real report, not an unsourced rumour. Killing it for the wrong reason is itself a finding.
>
> **R3 — UNVERIFIABLE (confidence: low-medium).**
> The company is de-identified, so I cannot test it. Context: the mid-2026 AI ARR genre is almost entirely self-reported (TechCrunch, 8 July 2026 compiles Mercor $2bn, Sierra $100m→$200m, Glean, Clio — all company announcements). So the *default* is R3's default. But the blanket rule fails on the biggest cases: Anthropic's ~$4bn→$47bn trajectory and Cursor's ~20x are corroborated through investor materials reported by Bloomberg/The Information, i.e. third-party sourced even though unaudited. If the essay's startup is one of those, "no independent source" is false. **This is the one conclusion I would not ship without the company name.**
>
> **R4 — PARTIALLY HOLDS (confidence: medium-high).**
> Datable: **Peter Steinberger's X post "Are we still talking loops or did we shift to graphs yet?" ~17–18 July 2026** (2.6–2.9M views; sources disagree between 17, 18 and 20 July). **LangChain, 22 July 2026**: "'Graph engineering' surfaced this weekend." So by **30 July 2026** the term was ~10–13 days old and heavily circulated — "just took off" is defensible. Two corrections: (a) **not immediately after Loop Engineering** — that was Steinberger 7 June 2026 + Osmani's essay the next day, a ~6-week gap; (b) LangChain, in the same post, says "**Graph engineering isn't a new idea. It's the latest name for a well established approach**." "Widely used" describes blog/X volume, not adopted practice, and the term already existed in graph-database/data-engineering usage.

Its answer to the open question is the sharpest thing in the whole run:

> The failure mode a year out is **not a wrong fact — it's the method's false-positive rate**. R2 and R3 both kill a claim on the ground "single-sourced / self-reported." Applied consistently, that filter discards most true facts in this industry, because AI financial and internal-spend facts *originate* in The Information and in company ARR announcements and are never independently auditable. A reader who knows the TechCrunch/Epoch chain reads R2 and concludes the essay's verifier cannot distinguish "unsourced" from "sourced to a paywalled outlet I didn't check." That inverts the demonstration more damagingly than a factual error would, because it impeaches the process, not one example.
>
> Second missed risk: **decay asymmetry.** R1 was a *proposal* until 8 July 2026 and law only from 27 July — the essay is correct by three days. R4's currency claim is the most perishable thing in the document; buzzword cycles in this space are running ~6 weeks. An essay whose evidence of rigour is four timestamped claims needs a visible "verified as of" date and a re-verification cadence, or it self-refutes on schedule.
>
> Third: **reflexivity.** The essay's own subject is the term whose novelty it asserts, and the most credible primary party (LangChain) publicly called it a rename. That is a conflict a hostile reader will name.

## Attacker 4 — quantitative methodology (Q1–Q5)

> **Q1 (313 agents / 13M tokens / 332 claims / 75 verified) — PARTIALLY HOLDS. Confidence: high (on the arithmetic), medium (on cause).**
> The stated topology yields per round: 1 splitter + 5 searchers + 0 (merge is code) + 25×3 verifiers + 1 writer = **82**. Three rounds = **246**. Stated: 313. Gap = **67 agents (21.4% of the total) with no node in the published graph**. 313/3 = 104.33, so rounds cannot have been identical either. Adding a per-angle extractor (+5/round → 261) or exercise 2's 8 attackers still leaves 44–52 unexplained. Separately, **75 = 3 × 25 is forced by the design cap**, not an empirical finding — it is the top-25 rule restated. 257 of 332 claims (77.4%) were never tested. 13M/313 = 41.5k tokens/agent average across wildly heterogeneous loads; with no per-node breakdown this is unfalsifiable, and "subagent tokens" excludes orchestrator context, so true spend is higher.
>
> **Q2 (16% / 40% / 28%) — PARTIALLY HOLDS as description, OVERTURNED as a trend. Confidence: high.**
> 16/40/28% of 25 = **4.0, 10.0, 7.0 exactly** — confirming n=25 per round, 21 kills total. Test of homogeneity across rounds (kills 4/10/7, expected 7 each): **χ² = 3.571, df = 2, p ≈ 0.17**. The three rates are statistically indistinguishable from one constant 28% rate. Wilson 95% CIs: 16% → [6.4%, 34.7%]; 40% → [23.4%, 59.3%]; they overlap almost entirely. Any narrative of "round 2 hit worse sources" is noise at n=25.
>
> **Q3 ("40% means nearly half the public data in my field can't survive three skeptics") — OVERTURNED. Confidence: high.**
> Four defects, any one fatal: 40% is not "nearly half"; it is the **maximum of three noisy estimates** (aggregate 28%, 21/75); the denominator is 25 agent-ranked claims, not sources, not "public data"; and 21/332 = **6.3%** of extracted claims were actually killed. The instrument is uncalibrated (see Two).
>
> **Q4 (one fifth overturned; biggest number overstated ~5x) — PARTIALLY HOLDS. Confidence: medium.**
> The 5x arithmetic error is the **only genuinely checkable result in the document** — arithmetic has external ground truth, and freezing + de-identifying before attack was correct design. But "about one fifth" has no stated denominator (conclusions? 8? 40?), so its CI is uninterpretable; and "three rounds of hand-revision never caught it" is not a controlled comparison — hand revision optimized prose/coherence, and self-checking one's own arithmetic is a known blind spot, so this shows *a different modality caught it*, not that the graph is superior.
>
> **Q5 (12x lookup-cost drop) — UNVERIFIABLE. Confidence: medium.**
> No units, no A/B, no baseline. The magnitude is achievable only under one accounting (per-claim re-read of a 21-row ledger: 25×21 vs 25×1 ≈ 21x) and impossible under another (per-round reads over 3 rounds: 1+2+3 = 6 vs 3 = **2x**). Regardless of which, the absolute saving is a few hundred to a few thousand tokens — **under 0.1% of the 13M** — so a real structural insight is being reported as a cost result it cannot support.

On whether a rejection rate measures anything about the world:

> **2. Rate measures the instrument, not the sources.** Established findings: LLM judges match human agreement ~80% at best but carry position, verbosity and self-enhancement biases (Zheng et al., NeurIPS 2023). Judges are wildly asymmetric — TPR 96% but **TNR <25%** on code feedback (Jain et al., arXiv 2510.11822). [...] Under conditional independence, a 2-of-3 rule implies single-verifier kill propensity q where 3q²−2q³ = P: q ≈ 0.25 (round 1) vs **q ≈ 0.435** (round 2). Correlated verifiers collapse 2-of-3 toward 1-of-1 at 3x cost. **Defensible interpretation: 16/40/28% is the kill propensity of one prompt on one model family on one date, with no measured false-kill rate. It is a reading with no zero point.** Critically, "refute-only" with no promote-agent makes *failure to find support within one session's budget* indistinguishable from *falsity* — the instrument is structurally biased against true-but-hard-to-source claims.

And its answer to the open question, which is the deepest architectural finding of the run:

> **Source independence is assumed and never measured.** Five parallel searchers pulling ~100 sources per round may be reading 3–5 upstream originals via syndication, aggregators, and increasingly AI-generated restatements. The dedupe step is "one line of code" — it dedupes *text*, not *lineage*. Parallelism reduces variance only under independent draws; correlated retrieval means the diamond's fan-out buys breadth of wording, not breadth of evidence, and the verifier can "confirm" a claim against a descendant of the claim's own source. This silently inflates both survival and confidence.
>
> **The kill criterion selects for hedging.** A refute-only verifier kills specific, quantified, falsifiable statements more easily than vague ones. Run at scale, this filter systematically promotes unfalsifiable prose — the opposite of the essay's stated goal — and any longitudinal drop in the rejection rate would be read as "sources improved" when it means "sources got vaguer."
>
> **Cheapest fix, highest value:** plant N known-true and N known-false claims into each verification round. That yields FPR/FNR, converts 16/40/28% into a calibrated measurement, and costs ~10 extra verifier sessions.

## The stall — and three integrator errors worth recording

Attackers 2 and 5 did the work and then failed to hand it over. Both ran to completion internally — 56 and 53 turns, transcripts of 224KB and 1.15MB — and both produced full, high-quality reports. Neither delivered. They sat finished-but-undelivered for over forty minutes, and only surfaced after being explicitly resumed and asked for their final report; one of the two then needed a manual relay to reach the integrator at all. **The failure mode was a stall in delivery, not a crash in reasoning.** That distinction matters for anyone running fan-out at scale: the expensive part had already been paid for and was sitting on disk the whole time.

Three errors of mine, in order of severity:

**One — I declared the two attackers dead on evidence that proved nothing.** I checked for a `"type":"result"` line in their transcript files, found none, and reported "verified three independent ways" that they had returned nothing. Then I ran the same check against attackers 1, 3, and 4 — whose full reports I was already quoting in this file — and they showed exactly the same empty result. The detection method could not distinguish a stalled agent from a successful one. My evidence had zero diagnostic value, and I had presented it as conclusive. This is precisely what attacker 4 warned about in Q2: *a reading with no zero point.* I ran an instrument I had never calibrated against a known-positive control, and it told me what I already half-believed.

**Two — I wrote sentences describing what attackers 2 and 5 had found before either had returned anything.** Mid-session I asserted that they disagreed on a token multiple and that one had an internal pricing inconsistency. There was no output to describe. Those statements were discarded and none reached this file, but the impulse is the finding: the integrator filling a gap with a plausible expectation instead of an observation. When the real reports arrived, attacker 5 *did* flag the Anthropic token figure as unreachable and unverified — so my invention was close enough to the truth to have survived casual review, which is the dangerous kind of wrong.

**Three — I nearly shipped a transcript that under-reported the article's problems.** The version written before recovery said 15 conclusions adjudicated, 3 overturned. The true figures are 26 and 6. Both of the recovered attackers overturned conclusions, including the sharpest single finding in the entire run. Had I stopped when the harness suggested stopping, this document would have been accurate about everything it said and wrong about the thing that mattered most: how much of the article survives.

On vote math: every conclusion here was assigned to exactly one attacker, by design — mutually exclusive domains mean no redundancy and no vote to take. Template 00 votes 3-of-3 on one claim; template 03 assigns 1-of-1 on many. That is cheaper and covers far more ground, and it means each verdict is a single point of failure. A round that loses 40% of its attackers loses 42% of its conclusions with no partial coverage from anyone else. The fix is not more attackers per conclusion — it is a delivery check, because in this run nothing was actually lost except my knowledge of it.

## Attacker 2 — multi-agent and debate literature (M1–M6)

> **M1 — OVERTURNED as attributed (high).**
> The 2018 paper never compares debate to self-grading. Its comparison class is direct human judging: "debate with optimal play can answer any question in PSPACE given polynomial time judges (direct judging answers only NP questions)." The guarantee is conditional on optimal play; the authors flag "potential weaknesses as the model scales up"; the only evidence is a 6-pixel MNIST game (59.4%→88.9%). The honest-advantage assumption was later broken by the obfuscated-arguments problem (Barnes & Christiano 2020) and conceded by Irving's own group in 2025: recursive debate hits obfuscation and the repair holds only "under certain stability assumptions" (arXiv:2506.13609). Defensible weaker claim: debate is a *proposal* for scalable oversight, not a demonstrated win over self-review.
>
> **M2 — PARTIALLY HOLDS (high).**
> Quote is real, baseline is wrong. Anthropic: "agents typically use about 4× more tokens than chat interactions, and multi-agent systems use about 15× more tokens than chats." Against a *single agent* — your actual comparison — the multiple is ~3.75×, not "ten-something." The 90.2% figure is one config (Opus 4 lead, Sonnet 4 subagents) on an undisclosed internal eval. The post reports losses too: domains "that require all agents to share the same context or involve many dependencies between agents are not a good fit"; "most coding tasks involve fewer truly parallelizable tasks"; "LLM agents are not yet great at coordinating." It also reports token usage alone explains 80% of BrowseComp variance — Anthropic's own evidence for M3's compute hypothesis.
>
> **M3 — PARTIALLY HOLDS, citation misattributed (high).**
> Chen et al. (NeurIPS 2024) studies Vote and Filter-Vote — majority voting over repeated calls of one model. It never compares multi-agent debate to single-model sampling. Its headline is different: performance "can first increase but then decrease as a function of the number of LM calls," because more calls help easy queries and hurt hard ones. M3's actual claim is supported by Smit et al. (ICML 2024): MAD systems "do not reliably outperform other prompting strategies, such as self-consistency and ensembling," and are hyperparameter-fragile. Swap the citation or the sentence; as printed, the source does not say what it is said to say.
>
> **M4 — OVERTURNED (high).**
> Backwards. Chen et al.'s scope is exactly majority voting over K independent calls — "3 refuters, 2 of 3 kills" *is* a Vote system with K=3, inside the paper's range, not outside it. Their result predicts your specific failure: as K rises, kill-rate rises on easy claims and accuracy falls on hard ones — hard claims being the valuable ones. Human arbitration doesn't exempt you: critique helps a judge only "when the critic provides a usable advantage" over that judge (arXiv:2605.27483, May 2026), with null effects when critic and judge discriminate comparably. A solo operator arbitrating inside his own expertise may have no such gap.
>
> **M5 — PARTIALLY HOLDS (medium-high).**
> First half supported: Huang et al. (ICLR 2024) find intrinsic self-correction fails and "at times, their performance even degrades." The stated mechanism is wrong, and that matters. Recent work (arXiv:2606.05976, June 2026) finds the deficit is largely a *role-labeling* artifact — relabeling self-generated errors as external raised explicit-correction rates by 23–93 percentage points in 10 of 12 model-domain settings. If that replicates, most of the benefit is available by re-presenting output as external input, without N dispatched agents. Second half — "isolated skeptics carry no such baggage" — is false: fresh context strips conversational priors, not pretraining priors.
>
> **M6 — HOLDS, understates the failure (medium).**
> Noise is worse than "filter line by line" implies. Critics hallucinate: "the rate of nitpicks and hallucinated bugs is much higher for models than for humans" (OpenAI, CriticGPT 2024). An agent whose only instruction is to refute inherits instruction-sycophancy; FlipFlop (Laban et al. 2023) shows a bare "are you sure?" flips answers 46% of the time with a 17-point accuracy drop. And isolation does not buy independence: model mistakes "are becoming more similar with increasing capabilities," which "could undermine benefits from using LM juries by compromising independence and amplifying collective biases" (Goel et al., ICML 2025). The corrections/reversals asymmetry is n=1 with no baseline — label it anecdote.

Its answer to the open question:

> The likeliest one-year failure is **silent false kills**. Three refuters sampled from one model family share blind spots, so the 2-of-3 gate kills claims that are *unusual* rather than wrong, and passes errors the shared prior endorses. Nothing is logged, so the loss is invisible: the process feels rigorous while its selection pressure runs against exactly the non-consensus claims that made the essay worth writing. Compounding it — the rule is symmetric but the costs are not (killing a true claim ≠ passing a false one), and there is no abstention channel.
>
> The missed opportunity is the **independence axis**. Verga et al. (2024) get their gains from *disjoint model families*, not fresh context, at 7× lower cost than one large judge. Three refuters from three vendors is a cheap upgrade you never took. Second: Chen et al. provide a method to compute the optimal number of calls from a small sample — your "3" is a guess where a measurement exists.

## Attacker 5 — agent engineering: context, tiering, cost (E1–E5)

> **E1 (model tiering is a real saving; 300 agents on the top tier was waste) — PARTIALLY HOLDS. Confidence: high.**
> Direction right, magnitude wrong, risk unpriced. Anthropic's published ladder is Opus 5 $5/$25, Sonnet 5 $3/$15, Haiku 4.5 $1/$5 per MTok (platform.claude.com models overview, fetched 2026-08-03) — a **5× spread, not the ~60× of the Opus-3/Haiku-3 era**. At ~300 agents × ~50–80K input plus a few K output, the whole exercise plausibly cost **$100–250**; tiering saves perhaps $60–150. That is not decision-relevant "waste." Haiku 4.5 also caps at 200K context with no adaptive thinking, so it is not a drop-in fetch node. Restate E1 as a latency/rate-limit argument, or drop it.
>
> **E2 (most "then"s are false edges; exactly one planning-time test) — PARTIALLY HOLDS. Confidence: medium.**
> "Name the data on the arrow" is a sound *necessary* test, not sufficient. It misses decision coupling without data flow, and the empirical failure distribution: MAST annotates 1600+ traces across 7 frameworks, attributing ~41.8% of failures to specification problems and ~36.9% to inter-agent misalignment, not missing edges (arXiv:2503.13657). Add a second test: *would knowing A's result change how you would phrase B?* If yes, that is a dependency the byte-flow test declares absent.
>
> **E3 (a chat session can't hold 20-some sources — the window won't take it) — OVERTURNED as stated; conclusion survives on a different premise. Confidence: high.**
> The capacity claim is false today (see below). The practical claim is rescued by effective-context degradation, not capacity. Publish the corrected reason or the claim is falsifiable by anyone with a token counter.
>
> **E4 (never outsource arithmetic; no agent for a one-liner) — PARTIALLY HOLDS. Confidence: medium-high.**
> Right conclusion, wrong mechanism. The main loop is also an LLM; moving arithmetic there doesn't make it deterministic. Defensible rule: *arithmetic runs in code, not in tokens* — code execution is a first-class server-side tool in 2026 (`code_execution_20260521`). As phrased, E4 reads as "do it in your head, just not over there."
>
> **E5 (a graph amplifies search but can't manufacture absent answers) — HOLDS, with a scope correction. Confidence: medium-high.**
> True for agent *count*; not for search *channel*. Deep-research agents are structurally confined to open-access, index-visible material and cannot retrieve paywalled content ([JMIR 2026](https://www.jmir.org/2026/1/e88195)). "Zero surviving claims" is evidence about your index, not the world. Reframe: fan-out has a coverage ceiling set by the retrieval channel; more agents move you along it, not through it.

On the context-window claim specifically:

> No, not for the stated reason. Current windows: Claude Fable 5 / Opus 5 / Sonnet 5 = **1M tokens**, 128K max output; Haiku 4.5 = **200K**. [...] Token cost of 20-some sources: median web page ≈ **3.2K tokens**, mean ≈ 10.4K across 44,684 pages; markdown-extracted pages average ~8K vs ~70K raw HTML. So 25 sources ≈ **80–200K tokens extracted** — comfortably inside 1M, and inside even Haiku's 200K. Raw HTML (~1.75M) would not fit. **E3's capacity claim is false unless you were feeding raw HTML.**
>
> Effective context rescues the conclusion differently. Chroma's *Context Rot* (18 models, 14 Jul 2025): non-uniform degradation with length; single distractors depress accuracy; models score *better on shuffled than logically coherent haystacks*; LongMemEval focused ~300-token prompts beat ~113K full prompts. NoLiMa: 10–11 of 12 long-context models fall below 50% of their short-context baseline at **32K** (ICML 2025). Twenty sources in one window is *possible* and *worse*. Say that.

On tiering and its hidden danger:

> Documented poisoning mode: **yes.** Most RAG hallucinations are extraction errors; cascading hallucination in agentic RAG shows early errors propagating silently while staying locally coherent, with RAGAS catching only 41.7% of cascades (CHARM, arXiv 2606.04435). This makes **E1 incomplete, not wrong** — and specifically dangerous in your design: a node that reads its own sources and returns only its conclusion destroys the artifact verification needs. Tiering is safe only where verification can re-open the primary text. Yours cannot.

And its answer to the open question:

> **The unpriced lever is prompt caching, not tiering.** A shared brief fanned to ~300 agents is a textbook cache prefix; cache reads bill ~0.1× input, writes 1.25×. That beats a 5× tier swap on the shared span **at zero quality cost** — and the essay never mentions it. Batch API (50% off) applies to non-interactive fan-out.
>
> **Most likely cause of failure a year out: the return contract, not the topology.** "Return only your conclusion" is the essay's load-bearing move and its single point of failure. Citation quality and factual accuracy are the weakest axes of deep-research systems (best ~65%/68%), and 3–13% of cited URLs in some evaluations do not exist. A conclusion-only node is unfalsifiable after the fact. Fix: nodes return a typed tuple — claim, verbatim quote, URL, retrieval timestamp — not prose.
>
> **Second-order:** search-time contamination. Round-over-round comparison assumes a stable index; by round three the index may contain AI-generated echoes of the essay itself.

## Integration — the part I did not outsource

**Arithmetic, recomputed by hand.** I ran this in the main loop before dispatch; attacker 4 reproduced it independently from a brief that contained the architecture but not my working. Both arrive at the same place:

- Stated diamond per round: 1 splitter + 5 searchers + (25 claims × 3 verifiers) + 1 writer = **82**. Three rounds = **246**. Stated total 313. **67 agents unaccounted** — 59 if the 8 adversarial-review attackers are inside the 313, which the article does not say.
- 313 / 3 = 104.33, so the three rounds cannot have been identical.
- 16%, 40%, 28% of 25 = exactly 4, 10, 7. That confirms n=25 per round and **21 total kills across all three rounds**.
- 21 / 75 = 28.0%, which is also the unweighted mean of 16/40/28. The 40% is the worst round, not the typical one.
- 21 / 332 = **6.3%** of extracted claims were actually killed. The article's generalizing sentence is built on the maximum of three observations from a pre-ranked subset.
- 40% is not "nearly half." It is 10 percentage points short — a 25% relative overstatement.
- 13,000,000 / 313 = 41,533 tokens per agent, which is plausible but unfalsifiable without a per-node breakdown.

**False-positive filter.** Checking each "what we missed" item against the original text:

| Attacker item | Ruling |
|---|---|
| Historical methods work on humans with private evidence; agents have neither | **Truly missed.** The article uses the lineage as validation and never asks whether the mechanism transfers. |
| Specification quality is 41.8% of multi-agent failures | **Truly missed by the article** — though the repo's own template 03 addresses it (the canary step). Article and templates are out of sync. |
| Verifier cannot distinguish "unsourced" from "sourced to a paywalled outlet" | **Truly missed.** The article's "isolation buys noise" line covers collation noise, not verifier false-positives on true claims. |
| Needs a visible "verified as of" date and re-verification cadence | **Truly missed.** |
| Source independence assumed, never measured; dedupe dedupes text, not lineage | **Truly missed.** The strongest architectural finding in the run. |
| Refute-only kill criterion selects for hedged, unfalsifiable prose | **Truly missed.** |
| Plant known-true/known-false claims to calibrate the verifier | **Truly missed**, and cheap enough that its absence is the notable part. |
| R3 cannot be tested without the company name | **The attacker could not know.** The article deliberately does not name the startup; this is an artifact of my de-identification, not a flaw in the document. Not a finding. |
| Requests for the run manifest, ranker prompt, token export | **The attacker could not know** these are unpublished — but the request stands as a real disclosure gap, not as a missed item. |

**Disagreements to arbitrate: none surfaced.** With mutually exclusive domains and two attackers dead, no two returns touched the same conclusion. The nearest thing to an overlap is that attacker 1 and attacker 4 independently reached the same place on correlated model errors by different routes — attacker 1 via Kim et al. (arXiv:2506.07962, ICML 2025) and Ron et al. (arXiv:2606.20158), attacker 4 via a paper it cited as Agarwal (arXiv:2604.19049). Two isolated agents converging on "same-family verifiers do not give you independent draws" is the one piece of genuine isolated convergence this round produced. It also happens to contradict the configuration this round was run in.

**Single-source, high-impact — do not cite externally without pulling the original.** Every one of these is one attacker's finding, and in four cases the attacker itself flagged that it could not reach the primary:

- **R2's sourcing chain.** The Information (Sept 2025) is paywalled and was **not read directly**. The overturn rests on TechCrunch's and Epoch AI's characterization of it. The verbatim quote attributed to the original ("leaders at Anthropic have *discussed* spending *more than* $1 billion... *over the next year*") must be confirmed against the source before anyone repeats it.
- **R1's Regulation (EU) 2026/1744.** The attacker's EUR-Lex fetches returned empty (JS-rendered); the article/section/date structure was reconstructed from law-firm briefings. High impact, primary unread.
- **R4's coinage date.** The attacker could not open the originating X post; secondary datings conflict across 17, 18, and 20 July 2026.
- **H4's 30% provenance.** Mitchell, Russo & Pennington (1989) full text was blocked. The claim that the figure measures *count of reasons generated* rather than risks found rests on secondary summaries plus Klein's own gloss. The attacker labelled N and effect size **unverified**. This one matters because H4 is the conclusion most likely to be quoted back.
- **Attacker 4's arXiv identifiers** 2603.00539, 2604.19049, 2605.00914 and 2606.29270 were not verified by me, and the attacker explicitly flagged two of them as abstract-only or not independently checked. Treat as unconfirmed.

**One external number I verified myself,** because it was load-bearing and cheap: current published model pricing and context windows. Opus-tier $5/$25 per MTok, Sonnet-tier $3/$15, Haiku-tier $1/$5 with a 200K window against 1M on the current Opus, Sonnet and Fable tiers. That is a 5x input-price spread across tiers on one vendor and a 5x context-window spread, which is the factual basis E1 and E3 would have been tested against — had attacker 5 returned.

## Review report — verdict table

Impact is my judgment: how much of the article moves if the verdict stands.

| # | Conclusion | Verdict | Impact |
|---|---|---|---|
| H1 | Delphi = experts never meet | PARTIALLY HOLDS | Low — wording |
| H2 | Social influence wrecks group accuracy, confirmed | PARTIALLY HOLDS, trending OVERTURNED | Medium — "confirmed" is wrong; finding is topology-conditional |
| H3 | Devil's Advocacy, 1970s, beats consensus | PARTIALLY HOLDS | Medium — origin wrong; role-played dissent may entrench |
| H4 | Premortem digs up ~30% more risks | **OVERTURNED** | High — number is misattributed and mis-described |
| H5 | ACH keeps corpses; official confidence standard | PARTIALLY HOLDS | High — the confidence standard is ODNI ICD 203, not Heuer |
| H6 | Knight & Leveson licenses cross-family counter-examiner | HOLDS as diagnosis, UNSUPPORTED as prescription | High — cross-family reduces, does not restore, independence |
| M1 | Debate paper: models attacking each other beats self-grading | **OVERTURNED** | High — the paper's comparison class is direct human judging, not self-review |
| M2 | Anthropic: multi-agent wins on breadth at ~10x token cost | PARTIALLY HOLDS | High — 15x is versus *chat*; versus a single agent it is ~3.75x |
| M3 | Counter-camp says gains are just more compute (Chen et al.) | PARTIALLY HOLDS, citation misattributed | High — Chen et al. never compares MAD to single-model sampling; cite Smit et al. |
| M4 | That criticism does not apply to independent attackers + human arbiter | **OVERTURNED** | High — a 2-of-3 refuter vote is the canonical in-scope case |
| M5 | Agents can't grade their own homework; isolated skeptics carry no baggage | PARTIALLY HOLDS | Medium — mechanism is role-labeling, and fresh context does not strip pretraining priors |
| M6 | Isolation buys independence and noise; open question produces reversals | HOLDS, understates the failure | Medium — critic hallucination and sycophancy are worse than "filter line by line" |
| R1 | EU AI Act high-risk pushed to Dec 2027 | PARTIALLY HOLDS | Medium — right for Annex III only; wrong baseline for Annex I |
| R2 | $1B RL environments is media hearsay | **OVERTURNED** | High — it is sourced, datable trade reporting |
| R3 | 15x revenue, self-reported, uncitable | UNVERIFIABLE | Low — de-identified by construction |
| R4 | Graph Engineering took off right after Loop Engineering | PARTIALLY HOLDS | Medium — ~6-week gap, not "immediately"; novelty disputed by LangChain |
| Q1 | 313 agents / 13M tokens / 332 claims / 75 verified | PARTIALLY HOLDS | High — 67 agents unaccounted; 75 is a design cap restated |
| Q2 | Rejection rates 16% / 40% / 28% | PARTIALLY HOLDS as description, OVERTURNED as trend | Medium — χ²=3.571, p≈0.17; indistinguishable from one 28% rate |
| Q3 | 40% means nearly half of public data fails | **OVERTURNED** | High — four independent defects; true figure is 6.3% of extracted claims |
| Q4 | ~1/5 overturned; biggest number 5x off | PARTIALLY HOLDS | Medium — no denominator; not a controlled comparison |
| Q5 | Lookup cost dropped ~12x | UNVERIFIABLE | Medium — no units, no A/B; absolute saving <0.1% of spend |
| E1 | Model tiering is a real saving; 300 top-tier agents was waste | PARTIALLY HOLDS | High — the spread is 5x, not ~60x; whole run likely cost $100–250 |
| E2 | Most "then"s are false edges; one planning-time test | PARTIALLY HOLDS | Medium — necessary but not sufficient; misses decision coupling |
| E3 | A session can't hold 20-some sources; the window won't take it | **OVERTURNED as stated** | High — 25 extracted sources ≈ 80–200K tokens, inside even a 200K window |
| E4 | Never outsource arithmetic; keep math in the main loop | PARTIALLY HOLDS | Medium — the main loop is also an LLM; the rule is "in code, not in tokens" |
| E5 | A graph amplifies search but can't manufacture absent answers | HOLDS, with a scope correction | Medium — "zero claims" is evidence about the index, not the world |

Adjudicated: **26 of 26.** Overturned outright: **6** (H4, R2, Q3, M1, M4, E3). Weakened: **16**. Survived with a correction attached: **2** (M6, E5). Unverifiable by construction: **2** (R3, Q5). **Survived unqualified: 0.**

## Rejected — do not cite

- **"The premortem digs up about 30% more risks than a straight review."** The figure traces to Mitchell, Russo & Pennington (1989), where it measured the *number of reasons generated*, not risks found and not correctness. Klein's HBR gloss added the correctness claim; the article added "risks." Accessible summaries suggest the effect may belong to the outcome-certainty manipulation rather than the temporal frame. Cite Gallop, Willy & Bischoff (2016, N=101) instead, or drop the number.
- **"There's even an official standard for labeling confidence" (attributed to ACH/CIA/Heuer).** The standard is ODNI ICD 203, Analytic Standards. It is not Heuer's and not part of ACH.
- **"A lab spends $1B a year buying RL environments — media hearsay. Killed."** The underlying report is real, named, and datable (The Information, Sept 2025; relayed by TechCrunch 21 Sept 2025 and Epoch AI). It was killed for the wrong reason. The genuine error is quantitative: the source describes leaders having *discussed* spending *more than* $1B *over the next year* — a forward-looking floor, on RL-environment spend generally, not an annual run-rate for "buying."
- **"A 40% rejection rate means nearly half of the public data in my field can't survive three skeptics."** 40% is the worst of three rounds; the pooled rate is 28%; the denominator is 25 model-ranked claims, not sources and not "public data"; and 21 of 332 extracted claims (6.3%) were actually killed.
- **"Experts answer independently and never meet"** as the definition of Delphi. Anonymity attaches to responses, not people; modified-Delphi designs with face-to-face rounds are standard RAND practice.
- **"AI Safety via Debate argued that models attacking each other gets closer to truth than models grading themselves."** The paper's comparison class is direct human judging, not self-review; the guarantee is conditional on optimal play; the honest-advantage assumption was later broken by the obfuscated-arguments problem and conceded by Irving's own group in 2025.
- **"That criticism targets debate-to-consensus architectures; independent attackers with human arbitration sit outside its range."** Backwards. A 2-of-3 refuter vote is exactly the Vote architecture Chen et al. studies — the canonical in-scope case, not an exception to it.
- **"Multi-agent wins clearly on breadth, at ten-something times the token cost."** The 15x in the source is measured against *chat*, not against a single agent. Against the single-agent baseline the article is actually comparing to, the multiple is ~3.75x.
- **"Chen et al., Are More LLM Calls All You Need?"** as the citation for "gains are just more compute, not more perspectives." The paper never compares multi-agent debate to single-model sampling. Cite Smit et al. (ICML 2024) instead.
- **"A single chat session can't hold 20 sources open at once — the context window won't take it."** Twenty-five extracted sources run roughly 80–200K tokens, which fits inside a 1M window and inside a 200K one. The claim is only true if you are feeding raw HTML. The conclusion survives on effective-context degradation, which is a different argument and must be stated as such.

## Decision dependency graph

**Gates — unknowns that block a decision.**
- Can the 313 be reconciled to the published architecture? Blocks any external citation of the field-run numbers. Settled only by a run manifest that does not exist publicly.
- Is the verifier's false-kill rate nonzero and how large? Blocks every claim built on rejection rates. Settled by planting known-true and known-false claims — about ten extra verifier sessions.
- Did the five search agents read independent sources or descendants of the same originals? Blocks the claim that fan-out buys evidentiary breadth. Settled by a provenance graph over the ~300 sources.
- What do M1–M6 and E1–E5 actually hold up to? Blocks the article's literature section and its entire engineering-advice section. Settled by re-running attackers 2 and 5.

**Clocks — irreversible deadlines.**
- R4 (the term's currency) decays on a ~6-week cycle and was already ~10–13 days old at publication. It is the most perishable sentence in the article.
- R1's regulatory dates move by amendment; the article was correct by three days at publication and Article 50 obligations bite 2 August 2026 regardless.
- Every field number is pinned to unnamed model versions on an unnamed date. Silent model updates make the run unreproducible by construction.

**No-regret moves — valid under every scenario, doable now.**
- Add a "verified as of" date to the article.
- Replace the 30% with Gallop et al. or delete it.
- Re-attribute the confidence standard to ODNI ICD 203.
- Change "experts never meet" to "answers are collected before they are compared."
- Rewrite the RL-environments bullet to describe the real error (a distorted quantity) instead of a false one (no sourcing).

**False dependencies — things that look sequential but can run in parallel.**
- Fixing the citations does not depend on resolving the field numbers. The provenance section and the methodology section fail independently and can be repaired independently.
- Calibrating the verifier does not depend on publishing the logs. Planted claims measure the instrument without disclosing anything about past runs.

## What changes as a result — recommendations only

Per instruction, nothing in ARTICLE.md was modified. These are recommendations:

1. Drop the "about 30% more risks" figure or replace it with Gallop, Willy & Bischoff (2016).
2. Move the confidence-labeling standard from ACH/Heuer to ODNI ICD 203.
3. Soften Delphi from "never meet" to "answers are collected before they are compared."
4. Correct Devil's Advocacy's origin, and add Nemeth et al. (2001) — role-played dissent can entrench the position it attacks, which is a direct caution about designated-attacker nodes.
5. Split H6: keep Knight & Leveson as diagnosis, stop citing it as a license for the cross-family fix, and add Kim et al. (2025) — cross-provider errors stay correlated, and correlation rises with capability.
6. Rewrite the RL-environments bullet to name the real distortion instead of claiming no sourcing.
7. Split R1 into its two regimes (Annex III → 2 Dec 2027; Annex I Section A → 2 Aug 2028, from a 2027 baseline) and note Article 50 still applies 2 Aug 2026.
8. Add the ~6-week gap between Loop Engineering and Graph Engineering, and quote LangChain's own "isn't a new idea" line rather than leaving a hostile reader to find it.
9. Replace "a 40% rejection rate means nearly half of public data..." with the pooled 28% of verified claims and the 6.3% of extracted claims, and state that verification ran on a ranked top-25 subset.
10. Disclose plainly that 313/13M/332/75/16-40-28%/one-fifth/5x/12x come from a private run with no published logs and cannot be checked by anyone else.
11. Add units and a baseline to the 12x, or remove the number and keep the structural point.
12. Add a "verified as of 2026-07-30" line.
13. Add the two architectural gaps the attackers found and the article does not mention: source-lineage independence is assumed rather than measured, and a refute-only verifier selects for hedged prose.
14. Add the calibration fix — plant known-true and known-false claims per round — to the templates.
15. Correct the token multiple: cite ~3.75x against a single agent, or state plainly that the 15x figure is measured against chat.
16. Re-cite the counter-camp to Smit et al. (ICML 2024), and **delete the sentence claiming independent attackers sit outside that critique** — a 2-of-3 vote is squarely inside it.
17. Rewrite the debate-paper sentence as "a proposal for scalable oversight," not a demonstrated win over self-grading.
18. Fix the context-window claim: 25 extracted sources fit comfortably; the real argument is effective-context degradation (Context Rot, NoLiMa), which is stronger and currently unstated.
19. Re-scope the model-tiering advice. The tier spread is 5x, not the ~60x of an earlier model era, and the whole 313-agent run plausibly cost $100–250 — so tiering saves tens of dollars, not a meaningful fraction. Restate it as a latency and rate-limit argument, and note Haiku's 200K cap makes it not a drop-in fetch node.
20. Add prompt caching to the cost section. A shared brief fanned to ~300 agents is a textbook cache prefix at ~0.1x read cost — a larger and safer saving than tiering, and the article never mentions it.
21. Change the return contract. "Return only your conclusion" is the article's load-bearing move and makes every node unfalsifiable after the fact. Have nodes return a typed tuple: claim, verbatim quote, URL, retrieval timestamp.
22. Re-scope the ceiling claim: "zero surviving claims" is evidence about the retrieval channel, not about the world. Deep-research agents cannot reach paywalled material at all.

## What survived, what died, what this says about reviewing your own work

**What survived.** Nothing unqualified, out of twenty-six. Two conclusions — M6 and E5 — came back marked HOLDS, and both carry a correction: M6 understates how noisy isolation is, and E5's "zero surviving claims" turns out to describe the retrieval channel rather than the world. The closest thing to a clean survivor is H6's *diagnosis*: Knight & Leveson really did show that independently written programs make correlated errors, and a 2026 replication on coding agents found the same. The article's instinct there is sound. Its prescription is not.

**What died.** Six conclusions outright, and they cluster in a way that is worse than the count suggests. Three are the article's own supporting citations misreporting what their sources say (M1, M3's attribution, H4). One is a factual claim about model capability that any reader with a token counter can falsify today (E3). One is the showcase demonstration (R2). And one is the article's only line of defense against the strongest published objection to its entire method (M4).

**Two findings share the top spot.**

The first is R2. The article offers the $1B RL-environments claim as proof that adversarial verification catches errors — "media hearsay. Killed." It was killed, and it should not have been, at least not on those grounds. There *was* a named outlet, a datable report, and a relaying chain through TechCrunch and Epoch AI. The claim was wrong for a different reason (it distorts a forward-looking deliberation about a floor into an annual run-rate for purchasing), and the verifier found the wrong reason. As attacker 3 put it: that "inverts the demonstration more damagingly than a factual error would, because it impeaches the process, not one example." A method's advertisement for itself is the last place you want a false positive.

The second is M4, and on reflection it is the more structurally damaging of the two. The article acknowledges the counter-camp — the line of work arguing that multi-agent gains are just more compute — and then dismisses it in one sentence: that criticism targets debate-to-consensus architectures, while independent attackers with human arbitration sit outside its range. Attacker 2 found this exactly backwards. Chen et al.'s scope *is* majority voting over K independent calls; "3 refuters, 2 of 3 kills" is a Vote system with K=3, squarely inside the paper's range rather than outside it. Worse, the paper predicts the specific failure this architecture should exhibit: as K rises, kill-rate rises on easy claims while accuracy falls on hard ones — and the hard claims are the valuable ones. The article's single defensive sentence does not merely fail; it points at the one published result that most precisely describes how its own diamond could be quietly degrading. And the human-arbitration escape hatch does not hold either: critique helps a judge only when the critic has a usable advantage over that judge, which a solo operator arbitrating inside his own expertise may not have.

**What is unverifiable by construction, and readers deserve to know it.** Every field number in the article — 313 agents, 13 million tokens, 332 claims, 75 verified, 16/40/28%, one fifth overturned, 5x overstated, 12x lookup drop — comes from a private run with no published logs, no claim list, no token export, and no planning document. Nobody outside the author can check any of it. Two of those figures also fail internal consistency checks that require no external data at all: 67 agents have no node in the published architecture, and the 12x has no units and an absolute saving under 0.1% of the run's own token spend. Unverifiable *and* internally unreconciled is a worse position than either alone.

**Did the author's proximity show up as a bias the attackers caught?** Yes, and it has a signature: **the errors all lean the same way.** The premortem number is inflated in the direction that flatters the technique. A contested finding is described as "confirmed." A confidence standard is credited to the method's own claimed ancestor rather than to the agency that actually published it. A rejection rate is generalized from the highest of three rounds rather than the pooled figure, and rounded up from 40% to "nearly half." A real, sourced report is written off as hearsay in the one bullet where the method needs a clean kill. None of these is a fabrication; each is a small lean. But six small leans in the same direction is not noise, and that directionality is exactly what a hand-revising author cannot see and an indifferent stranger finds in twenty minutes. The article's own line about debugging with the same eyes turns out to be true about the article.

There is a second proximity effect the attackers did not have to find, because it happened in the harness: the canary went and read the source document off the disk. Proximity is not only a property of the author's mind; it is a property of the environment the review runs in. If the reviewer can reach the artifact, isolation is a story you are telling yourself.

**What this round did not do,** stated plainly as next round's starting point:

- **All attackers ran on one model family.** The article prescribes crossing families; this run did not. Two independent returns in this very round (Kim et al. via attacker 1, and attacker 4's correlated-verdict citations) say that is the configuration most likely to produce correlated blind spots. Whatever these three attackers all failed to see, they probably failed to see together.
- **Two of five attackers stalled silently for over forty minutes**, and only delivered after being explicitly resumed — with a manual relay needed to get their output back to the integrator. Nothing was lost in the end, but for most of this run I believed 11 of 26 conclusions were unadjudicated, and I nearly shipped a transcript that said so. **A stall is indistinguishable from a death from the outside.** The orchestrator that gives up early does not lose an attacker; it throws away work that was already finished and sitting on disk. Both recovered attackers overturned conclusions, including the sharpest finding in the run. Poll for delivery, and resume before concluding.
- **No second-round cross-verification.** Every high-impact overturn rests on a single attacker, and in four cases that attacker could not reach the primary source it was overturning against.
- **No probability weighting of scenarios**, and no attempt to price which recommendation is worth doing first.
- **No verification of four arXiv identifiers** cited by attacker 4, two of which the attacker itself flagged as unconfirmed.

An hour before this run, ARTICLE.md was a document I would have cited without hesitation. That feeling is the product — and it is noticeably less comfortable when the document is your own.
