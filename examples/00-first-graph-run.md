# 00 Your First Graph — a real run

One claim, three isolated skeptics, one vote. This is a transcript of [template 00](../prompts/00-first-graph.md) executed on 2026-07-30 in Claude Code: three subagents dispatched in parallel, each with web search, none able to see the others. Total cost: ~146k subagent tokens, about 90 seconds wall-clock. The skeptic outputs below are verbatim.

## The claim

> Email open rates across industries average around 20%.

The README's own first example — a number quoted in a thousand marketing meetings.

## Dispatch

The skeptic instruction was forwarded verbatim from the template to each of the three, with one formatting addition (a ~250-word cap and a VERDICT/EVIDENCE/NOTE output shape so the vote could be tallied mechanically).

## The three returns

### Skeptic 1 — REFUTED

> VERDICT: REFUTED
>
> EVIDENCE:
> - Mailchimp benchmark page (vendor-reported), data updated Dec 2023: average open rate across all industries is 35.63% — not ~20%. The commonly repeated 21.33% is Mailchimp's October 2019 dataset, still echoed by secondary sites. https://mailchimp.com/resources/email-marketing-benchmarks/
> - MailerLite (vendor-reported), 2025: 43.46% average open rate across all industries, from 3.6M campaigns / 181,000 accounts, Dec 2024–Nov 2025. Industry range 30.1%–55.71% — even the lowest industry exceeds 20%. https://www.mailerlite.com/blog/compare-your-email-performance-metrics-industry-benchmarks
> - GetResponse (vendor-reported), 2024 benchmarks report: 39.64% all-industries average. https://www.getresponse.com/resources/reports/email-marketing-benchmarks
> - Litmus client-market data via EmailTooltester (vendor-reported analytics, independent of any single ESP): Apple Mail ≈58% of opens by early 2025; MPP (Sept 2021) auto-fires tracking pixels, so reported "opens" include machine prefetches. https://www.emailtooltester.com/en/blog/apple-mpp-open-rate/
> - Validity study 2024 (vendor-reported deliverability analytics, cited via datainnovation.io): Apple-Mail-dominant senders show open rates 18–32 percentage points above verified engagement — the ~20% and ~40% eras are not comparable measurements. https://datainnovation.io/en/apple-mpp-email-open-rate-fix/
>
> NOTE: The claim's biggest weakness is era plus measurement: "around 20%" is a pre-2021 pixel-tracking artifact, and no independently verified open-rate figure exists at all — every benchmark is vendor telemetry, and since Apple MPP the "open" metric no longer measures human opens in either direction.

### Skeptic 2 — REFUTED

> VERDICT: REFUTED
>
> EVIDENCE:
> - MailerLite, 2025 (vendor-reported): "The average email open rate in 2025 was 43.46%," from 3.6M campaigns / 181K accounts, Dec 2024–Nov 2025 (verified on-page). https://www.mailerlite.com/blog/compare-your-email-performance-metrics-industry-benchmarks
> - GetResponse, 2023 data (vendor-reported): "The average email open rate is 39.64%," from 4.4B messages (verified on-page). https://www.getresponse.com/resources/reports/email-marketing-benchmarks
> - Mailchimp, updated Dec 2023 (vendor-reported): average open rate 35.63% across industries — the same vendor whose October 2019 figure (21.33%) is the apparent origin of the "around 20%" claim, i.e., the source itself now reports ~36%. https://mailchimp.com/resources/email-marketing-benchmarks/
> - Paubox / beehiiv analyses of Apple Mail Privacy Protection, 2021–2026 (independent commentary on a documented mechanism): MPP auto-fires tracking pixels on delivery for ~50%+ of email clients, inflating "opens" by an estimated 15–35%; opens are no longer a reliable engagement measure. https://www.paubox.com/blog/how-apple-mail-privacy-protection-inflates-email-open-rates ; https://www.beehiiv.com/blog/apple-mpp-open-rate
> - Cross-check: no post-2021 large-dataset benchmark found reporting ~20%; 2024–2026 vendor reports cluster at 35–45%. The ~20% figure appears only in pre-MPP data (Mailchimp 2019: 21.33%; Campaign Monitor ~2020: ~18%) — commonly repeated, not currently documented.
>
> NOTE: The claim is era-bound and definitionally unstable — "open rate" itself changed meaning in September 2021 when Apple MPP began machine-triggering opens, so a single cross-era "average" is unmeasurable; also note every benchmark, old or new, is vendor-reported ESP data, never independently audited.

### Skeptic 3 — REFUTED

> VERDICT: REFUTED
>
> EVIDENCE:
> - Mailchimp (vendor-reported), data as of December 2023: average open rate across all industries is 35.63% — not ~20%. The oft-cited 21.33% is Mailchimp's stale October 2019 figure, still widely repeated. https://mailchimp.com/resources/email-marketing-benchmarks/
> - GetResponse (vendor-reported), 2024 report on 4.4 billion messages sent in 2023: "The average email open rate is 39.64%." Their own 2020 report said 22.15% — the ~20% era ended with Apple MPP. https://www.getresponse.com/resources/reports/email-marketing-benchmarks
> - Brevo (vendor-reported), 2026 benchmarks: average open rate 33.87% including Apple MPP vs 20.73% excluding it — the "average" swings ~13 points depending on measurement definition alone. https://www.brevo.com/blog/email-marketing-benchmarks/
> - HubSpot (vendor-reported), 2025: average open rate across industries ~42.35%. https://blog.hubspot.com/sales/average-email-open-rate-benchmark
> - Mechanism (independent of ESP benchmark methodology): Apple Mail Privacy Protection (iOS 15, Sept 2021) prefetches tracking pixels, auto-registering "opens" for Apple Mail users (~46-49% of opens); analyses across industry sources estimate 15-20 points of inflation, e.g. https://www.geysera.com/blog/email-marketing/email-marketing-benchmarks-2026-open-rates-ctr-and-why-half-your-data-is-wrong
>
> NOTE: The claim's fatal weakness is era plus measurement — "open rate" stopped being a well-defined metric in September 2021, so a single cross-industry average is meaningless without stating whether MPP auto-opens are included (~34-42% if yes, ~21% if filtered), and every available benchmark is vendor-reported from that vendor's own customer base, with no independently verified cross-industry figure existing at all.

## The vote (deterministic — no agent)

3 of 3 REFUTED → **the claim dies.**

Rejection-ledger line: "~20% average open rate" is Mailchimp's October 2019 figure (21.33%) still circulating five years past expiry. Post-2021 vendor benchmarks cluster at 35–45% because Apple Mail Privacy Protection machine-fires opens; under an MPP-excluded definition one vendor still reports ~20.7%. The number is not so much wrong as **undefined** — it means nothing without stating the measurement.

## What to notice

- **Isolated convergence is signal.** All three skeptics independently traced the claim to the same origin — Mailchimp's October 2019 dataset — without seeing each other. Three isolated agents landing on one primary source is evidence; three agents in one conversation agreeing is an echo
- **Divergence is why you pay for three.** Only skeptic 3 surfaced the sharpest fact in the whole run: Brevo reports ~20.7% when Apple MPP is *excluded*, meaning the claim survives under one definition and dies under the other. One skeptic would have returned a flat "outdated"; the third found the reason the number refuses to die
- **The NOTE field earned its keep.** All three converged on the deeper finding — "open rate" stopped being a well-defined metric in September 2021 — which is worth more than the verdict itself
- **What this run did not do** (the method's own discipline): all three skeptics ran on the same model family, and every benchmark cited is vendor-reported ESP telemetry — no independently audited cross-industry figure exists. A stricter round would mix model families and hunt for non-ESP data

An hour before this run, "20% open rate" was a perfectly citable number. That feeling is the product.
