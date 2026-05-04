# Low-Cost LLM Inference Providers — 2026 Reference

A working reference of LLM inference providers organized by business model. Companion to ["The Consumer Inference Stack: 26 LLM Providers Worth Knowing"](#) — this doc goes deeper, covers more providers, and explains *why* you'd pick each one.

**Last updated:** May 3, 2026
**Maintainer:** [Kevin Simback](https://x.com/ksimback)
**License:** CC BY 4.0 — fork, share, contribute via PR

---

## Table of contents

- [Methodology](#methodology)
- [TL;DR — pick by use case](#tldr--pick-by-use-case)
- [Tier 1 — Per-token serverless](#tier-1--per-token-serverless)
- [Tier 2 — Flat-rate, infrastructure-limited](#tier-2--flat-rate-infrastructure-limited)
- [Tier 3 — Flat-rate, credit-limited (frontier + OSS bundles)](#tier-3--flat-rate-credit-limited-frontier--oss-bundles)
- [Tier 4 — Genuinely free tiers](#tier-4--genuinely-free-tiers)
- [Tier 5 — Local / self-hosted](#tier-5--local--self-hosted)
- [Tier 6 — Decentralized / crypto-native](#tier-6--decentralized--crypto-native)
- [Multi-provider gateways](#multi-provider-gateways)
- [Hidden cost dimensions](#hidden-cost-dimensions)
- [Comparative pricing snapshot](#comparative-pricing-snapshot)
- [Sources](#sources)
- [Contributing](#contributing)

---

## Methodology

This list is organized by **business model**, not alphabetically — because the model determines the relevant comparison.

The most useful distinction within the flat-rate space isn't "open-source vs frontier" — it's **what kind of limit caps your usage**:

- **Infrastructure-limited subscriptions** charge a fixed fee for N parallel slots / X concurrent requests / Y GPU-hours. Heavy use doesn't drain anything; the limit is the queue. These can only host OSS models because frontier providers won't let anyone resell their tokens at flat rates.
- **Credit-limited subscriptions** charge a fixed fee that buys credits/points which deplete at different rates depending on which model you call. Cheap models barely move the needle; calling Opus 4.7 or GPT-5.5 Pro can drain 30-50K points per query. The advantage is model breadth — frontier closed models bundled with OSS.

Tiers used here:

1. **Per-token serverless** — pay-as-you-go API access to OSS models on the provider's infrastructure
2. **Flat-rate, infrastructure-limited** — monthly fee, queue-based caps, OSS-only by economic necessity
3. **Flat-rate, credit-limited** — monthly fee, credits deplete by model cost, frontier + OSS bundles
4. **Free** — persistent free tiers requiring no card
5. **Local / self-hosted** — zero variable cost, capital cost up front
6. **Decentralized** — crypto-native settlement, novel tokenomics
7. **Gateways** — meta-layer that routes across the above

Each provider entry covers:
- **Pricing model** — how you're billed
- **Sample pricing** — point-in-time snapshot, may be stale by the time you read this
- **Hardware / infrastructure** — what they run on, where it's located
- **Compliance & data handling** — SOC2, HIPAA, EU residency, training-on-prompts policy
- **What makes them interesting** — why you'd pick them over the next-cheapest competitor

Listing inclusion requires that a consumer can actually call an inference endpoint today. Pre-testnet projects, even well-funded ones, are excluded.

---

## TL;DR — pick by use case

| Use case | Top picks |
|---|---|
| Cheapest per-token frontier OSS (Kimi, GLM, DeepSeek) | CrofAI, DeepInfra, Parasail, Avian |
| Flat-rate, infrastructure-limited (predictable for agent loops) | Featherless ($10–75), Arli AI, Synthetic ($30), OpenCode Zen, Ollama Cloud ($20/$100), Mancer |
| Flat-rate, credit-limited (frontier + OSS in one bundle) | Abacus.AI ChatLLM ($10), Nous Portal ($20–200), Poe ($5–20), You.com Pro ($15–20) |
| Crypto / decentralized | Venice (VVV/DIEM), Chutes (Bittensor SN64), Eternal AI (onchain verifiable), Targon, Phala |
| Best free tier, no card | Google AI Studio, Cerebras (1M tokens/day), Groq, Cloudflare Workers AI |
| Speed kings | Cerebras (~2,600 tps), Groq (~315 tps), Avian (489 tps on DeepSeek) |
| Multi-provider gateway | OpenRouter (300+ models, 60+ providers, single API key) |
| Self-hosted gateway | LiteLLM (open source, 100+ providers) |
| Roleplay / uncensored | Mancer, Arli AI, Featherless, Venice |
| Coding subscription as Claude Code alternative | Z.AI GLM Coding Plan ($10–80), Synthetic ($30), OpenCode Zen |
| Function-calling fidelity | Fireworks (FireFunction), Synthetic (Synbad evals), OpenCode Zen (curated catalog) |
| EU data residency | Nebius, Scaleway, Mistral La Plateforme |
| US-only infra commitment | Avian (Azure US), Cerebras, SambaNova, Groq, Abacus |
| Specialized fine-tuning + serving | Inference.net, Together (full pipeline), Predibase (LoRA) |

---

## Tier 1 — Per-token serverless

Pay-as-you-go OpenAI-compatible endpoints. Margins are razor-thin and prices change weekly. The interesting comparison isn't price-per-million — those mostly cluster within a 20-30% band — but what each provider does *besides* compete on price.

### CrofAI
- **Pricing model:** Pay-per-token, OpenAI-compatible at `https://crof.ai/v1`
- **Sample pricing (Kimi K2.6):** $0.55/M input, $0.11/M cached, $2.70/M output
- **Special offer:** $100 lifetime unlimited (was capped at first 20 users — likely full)
- **Behind it:** Solo operator (nahcrof) running custom infrastructure; building toward custom on-prem
- **Compliance:** None disclosed
- **What makes them interesting:** Markets itself as "the cheapest inference provider in the world" and on the most popular open-weight models, the published price is genuinely the floor of the market — not a teaser. The single-operator angle means new models often appear within hours of release because there's no procurement chain to navigate. The downside of the same setup is no SLA, no support team, and your workload sharing capacity with whoever else is on the platform that day.
- **Best for:** Hobbyist or individual builders who want to test new OSS frontier models the moment they drop, or batch workloads where occasional flakiness is fine.

### Together AI
- **Pricing model:** Per-token serverless ($0.05–$7/M depending on model) + dedicated GPU + fine-tuning
- **Free credit:** $25 for new users, no card required (one of the most generous trials)
- **Speed:** ~95 tps on Llama 70B, 0.71s TTFT
- **Hardware:** Operates 25+ data centers across US, EU, Asia, Middle East — *not* US-only
- **Compliance:** SOC2; specific certifications negotiated at enterprise tier
- **Funding & traction:** ~$315M ARR as of Feb 2026 per Sacra; named customers include Cursor, Perplexity, Notion, Sourcegraph, Uber, DoorDash, Shopify, Upwork
- **What makes them interesting:** The breadth play. 271+ models hosted, full fine-tuning pipeline, batch processing at 50% off, and ATLAS speculative decoding for latency. They've positioned themselves as the place you go to *experiment* across the OSS landscape, identify a winner, then either stay (with a dedicated endpoint and reserved capacity) or graduate to a price-leader. The team's research credentials (ThunderKittens, FlashAttention contributors) show up in custom kernel work that delivers genuine performance gains over generic vLLM hosting.
- **Best for:** Teams doing model evaluation across many OSS models, or running fine-tuning workloads they want to deploy on the same platform.

### Fireworks AI
- **Pricing model:** Per-token serverless ($0.10/M for <4B models up to $0.90/M for >16B) + dedicated GPU ($2.90–$9/hr)
- **Free credit:** $1 (notably stingy compared to Together's $25)
- **Hardware:** Custom CUDA kernels on top of vLLM/SGLang/TGI; FP8 and FP4 quantization tiers available
- **Compliance:** SOC2 Type II, HIPAA available, regional deployment options (their docs explicitly call this out as a differentiator vs. US-only inference providers)
- **What makes them interesting:** Three things genuinely separate them from the per-token crowd. First, **function-calling reliability** — their FireFunction model family hits 92%+ accuracy on multi-tool function calling per TokenMix benchmarks, beating Together and Groq. For agentic systems where tool-call failures cascade, this matters more than raw price. Second, **latency consistency under load** — P99 is only 3.9× their P50, where competitors often blow out 10×+ during traffic spikes. Third, **founder pedigree** — built by ex-Meta PyTorch researchers, which shows in the engineering. Customer list (Cursor, Perplexity, Sourcegraph) reads like "AI products that need to actually work."
- **Best for:** Production agentic systems with strict JSON/tool-call requirements, or any user-facing app where consistent tail latency matters more than a few cents per million tokens.

### DeepInfra
- **Pricing model:** Aggressively cheap pay-per-token with explicit tier pricing
- **Sample pricing:** Llama 3.1 8B at $0.03/$0.05; DeepSeek V3 at $0.34 blended/M; gpt-oss-120B at ~$0.08/M; **Kimi K2.6 cached input at $0.15/M** (rare to see published)
- **Free credit:** $5 on signup, no card required
- **Hardware:** All H100/A100 with auto-scaling
- **Compliance:** Standard cloud security; not heavily marketed as enterprise-grade
- **Catch:** 200 concurrent request cap per account silently enforced — surprise 429s at scale unless you explicitly request more
- **What makes them interesting:** Quietly one of the only providers that **publishes explicit cached-token pricing**, which directly maps to real savings on agent loops where the system prompt and tool schemas get resent on every turn. For OpenClaw-style or coding-copilot workloads, this beats raw token-price comparisons substantially. They also support **JSON mode across every text model** in their catalog (most providers enable it on a subset only), so you don't have to model-shop when your downstream parser needs guaranteed structured output. Often shows up as the price floor on Artificial Analysis benchmarks, especially for the most popular OSS frontier models.
- **Best for:** Cost-optimized batch workloads, high-volume RAG, agent loops with repeated context where caching pays off.

### Parasail
- **Pricing model:** Per-token serverless + dedicated instances + batch processing (80–90% cheaper than realtime)
- **Funding:** $32M Series A April 2026 (total $42M), led by Touring Capital and Kindred Ventures
- **Sample pricing:** Kimi K2.6 at $1.15/M blended (cheapest provider for that model); DeepSeek V3.2 at $0.32/M
- **Founder:** Mike Henry — former CPO of Groq, built Groq Cloud, also founded Mythic
- **Hardware:** "AI Supercloud" orchestrating rented GPU time across 40 data centers in 15 countries; H200 currently most advanced GPU offered
- **Compliance:** Standard, with regional deployment available on dedicated tier
- **What makes them interesting:** Built specifically for the **agent paradigm, not just LLM chat**. The product is "inference-as-code" — declare your tokenization, retrieval, prompting, and fine-tune steps once, get identical behavior in dev and prod. Customer testimonials emphasize **zero-day support for new model releases** — when Moonshot drops a new Kimi or Z.AI ships GLM-6, Parasail typically has it deployed before competitors. Customer list includes Weights & Biases, Elicit, Rasa, Everpilot, and Oumi. The Groq pedigree means they think about throughput and tail latency at a different level than typical CRUD-API providers.
- **Best for:** Token-maxxing batch (think "process 100K papers overnight"), or teams that want production-grade agent infrastructure without negotiating with hyperscalers.

### Avian
- **Pricing model:** Per-token, no subscriptions, no rate limits
- **Sample pricing:** From $0.23/M; DeepSeek V3.2 at $0.38/M output, claims 489 tps (4× GPT-4o)
- **Hardware:** B200 Blackwell GPUs hosted on Microsoft Azure
- **Compliance:** **SOC/2 approved infrastructure on Azure**, zero data retention, full GDPR & CCPA, models pre-loaded so zero cold starts
- **Customers:** Bank of America, Boeing, Google, eBay, Intel, Salesforce, General Motors
- **What makes them interesting:** The **enterprise-credentials story** is what sets them apart from the cheap-token crowd. SOC2 + Azure-hosted + zero-retention is a procurement-friendly stack — when your buyer asks "do they have SOC2?" you can say yes, where most cheap providers can't. First inference platform to deploy DeepSeek R1 at scale (Jan 2025), and the customer list reads more like Anthropic's enterprise roster than a typical OSS-inference startup. Speed is a real number too — 489 tps on DeepSeek V3.2 is genuinely fast B200 territory.
- **Best for:** Regulated industries, large companies where procurement compliance is the gating factor, or any workload where speed + enterprise-grade trust both matter.

### Hyperbolic
- **Pricing model:** Per-token, primarily FP8 quantized
- **Sample pricing:** GPT-OSS 20B at $0.10/M; DeepSeek V3 around $1.25/M output
- **Free credit:** $1 trial
- **Hardware:** H100/H200 with FP8 quantization throughout
- **Compliance:** Privacy-positioned but no major certifications publicly listed
- **Catch:** 600 RPM cap on Pro tier; FP8 quantization may degrade quality on coding/reasoning workloads (5-10pp drop on some Aider benchmarks)
- **Side business:** GPU rental at competitive rates ($1.99/hr for H100, well below typical hyperscaler pricing)
- **What makes them interesting:** Two-product company — inference API plus GPU rental marketplace. The GPU rental side is where they're most competitive (some of the cheapest H100/H200 hours available without long-term commitment). On the inference side, the FP8-everywhere strategy means they can publish dramatically lower prices than competitors but with a quality tax that varies by workload.
- **Best for:** Workloads where you've tested the FP8 version and confirmed it's good enough, or anyone wanting cheap GPU rental on demand.

### Nebius
- **Pricing model:** Per-token serverless ("Nebius Token Factory") + dedicated endpoints + GPU cloud + fine-tuning pipeline
- **Sample pricing:** DeepSeek V3.2 at $0.80/M blended on Nebius Fast (135-200 tps); $0.75/M on standard tier
- **Background:** Ex-Yandex spinoff after 2022 divestiture; Amsterdam HQ; publicly traded (NASDAQ: NBIS)
- **Hardware:** NVIDIA GB300 NVL72, GB200 NVL72, B300, B200, H200, H100; InfiniBand interconnect; in-house data center and rack design
- **Compliance:** **SOC 2 Type II, HIPAA, ISO 27001, ISO 27799** — far above what cheap-inference providers typically carry. Zero-retention inference selectable in EU or US datacenters.
- **Notable:** $17.4B multi-year deal with Microsoft signed Sept 2025; raising $3B to expand
- **What makes them interesting:** The **EU sovereignty play with enterprise teeth**. Most cheap providers either skip compliance entirely or buy a single SOC2 cert as table stakes. Nebius treats governance as the product — Token Factory has SSO, RBAC, team management, audit-friendly workspaces, and fine-grained access control built in. They also sell **fine-tuning + deployment as one workflow** — fine-tune with LoRA or full-parameter, one-click deploy to a dedicated endpoint, claimed up to 70% cost/latency reductions on the resulting model. Hugging Face integrated them as a primary inference backend specifically for the EU performance angle (4.5× faster TTFT in Europe vs. competitors per their published benchmarks).
- **Best for:** EU companies with data residency requirements, regulated industries (healthcare, finance, public sector), or any team wanting fine-tuning + serving + governance from one vendor.

### Inference.net
- **Pricing model:** Per-token serverless + custom fine-tuned model deployment + full lifecycle platform
- **Sample pricing:** Claims 90% lower than competitors on like-for-like serving
- **Free credit:** $25 (matches Together's most generous trial)
- **Proprietary models:** **Schematron** (state-of-the-art structured output model), **ClipTagger** (12B vision-language model)
- **Compliance:** Standard, varies by deployment
- **What makes them interesting:** Doesn't really compete on commodity inference — their actual product is **"build a specialized model that's 50× cheaper than the frontier model you're currently paying for."** They take your production traces, fine-tune a small specialist (often on Nemotron 2 Nano), evaluate it against your baseline, and deploy via OpenAI-compatible endpoints. Their **Catalyst Evaluate** product turns production traces into continuous training data so the model improves with use. The pitch is fundamentally different from "we're 5 cents cheaper per million tokens" — it's "you're paying GPT-5 prices for a task a fine-tuned 12B could nail at 90% lower cost and 5× lower latency, and we'll build it for you in days."
- **Best for:** Teams running a stable, repeatable workload (structured extraction, classification, summarization in a fixed format) on a frontier model that's eating their margins.

### Novita AI
- **Pricing model:** Per-token, both standard and "Turbo" (quantized) tiers
- **Sample pricing:** DeepSeek V3 at $0.48/M blended; DeepSeek V3.2 at $0.30/M
- **Compliance:** Standard
- **Catch:** Doesn't always support function calling on cheapest tier; quality varies by model and tier
- **What makes them interesting:** Aggressive pricing across a broad catalog. Notable for speed/price tradeoff transparency — they explicitly tier their hosting (standard vs Turbo) so you can pick your point on the curve rather than discovering quality issues mid-deployment.
- **Best for:** Budget batch workloads where tools/JSON aren't required, or as a fallback in a multi-provider router.

### SiliconFlow
- **Pricing model:** Per-token, very cheap; persistent free tier
- **Sample pricing:** DeepSeek V3.2 at $0.31/M blended (FP8); free tier with 1K RPM, 50K TPM
- **Free models:** 13 models including Qwen3-8B, DeepSeek R1 distills, GLM-4.1V Thinking
- **Hosting:** China-hosted
- **Catch:** High TTFT (~3s); China hosting means data residency concerns for many use cases
- **What makes them interesting:** One of the most generous *persistent* free tiers — not a $5 trial, but ongoing access to 13 useful models with real rate limits. Useful for prototyping when you don't want to burn through Together's $25 credit. The China hosting is a hard no for many enterprise buyers but a non-issue for hobbyist work.
- **Best for:** Free prototyping with no budget, batch workloads where TTFT doesn't matter and China hosting is acceptable.

### Kluster.ai
- **Pricing model:** "Adaptive Inference" — realtime, async, and batch tiers
- **Free credit:** $5 for new users
- **Models:** Llama 4 Maverick FP8, DeepSeek R1, Qwen3-235B, others
- **Compliance:** Standard
- **What makes them interesting:** Three-tier latency model (realtime / async / batch) lets you trade speed for cost more granularly than typical providers. The async tier specifically targets LLM workloads where you'd otherwise build a queue + worker yourself.
- **Best for:** Synthetic data generation, batch dataset processing, async pipelines where you can tolerate seconds-to-minutes latency for substantial savings.

### Other notable per-token providers

- **Replicate** — Pay-per-second billing on broader ML (not just LLMs); strongest for image/video/audio. Higher per-token cost on LLMs but the right call when you need a specific model the big providers don't host yet.
- **Baseten** — Production-grade enterprise focus; raised $300M Series E at $5B valuation Feb 2026. Strong on custom model serving with self-hosted/hybrid VPC deployment for compliance-heavy accounts. NVIDIA Blackwell partner.
- **Modal** — Serverless GPU; good for custom deployments where you want to write Python and have it run on rented GPUs.
- **Lambda Labs** — Primarily GPU rental; some inference endpoints exist as a secondary product.
- **RunPod** — GPU rental at very low rates; serverless inference is secondary. Popular with hobbyists for spot pricing.
- **Scaleway Generative APIs** — French/EU; strong data residency, smaller catalog. Pairs naturally with Nebius for EU-only stacks.
- **Alibaba Model Studio** — China + International tiers; primary path to Qwen models from the source.
- **Volcengine Ark** — ByteDance; primary path to Doubao models.
- **Tencent Cloud LKE** — China; Hunyuan family.
- **Baidu Qianfan** — China; ERNIE family.
- **PPIO** — Distributed cloud inference.
- **Eigen AI** — DeepSeek V3.2 at $0.91/M blended, 131 tps; smaller but transparent pricing.
- **Clarifai** — Kimi K2.6 at 154 tps (fastest measured for that model); broader vision/multimodal background.
- **OctoAI** — Acquired by NVIDIA late 2024; less actively positioned for new customers.
- **Anyscale** — Ray-based; HIPAA / SOC2 / EU data residency by default; premium pricing 1.5-2× Together but strongest enterprise contract terms.

---

## Tier 2 — Flat-rate, infrastructure-limited

Heavy use doesn't drain anything — the limit is the queue. Fixed fee, queue-based caps (concurrency, parallel slots, GPU-time, requests per window). Limited to OSS models because frontier providers won't let anyone resell their tokens at flat rates. The right shape for **agent workloads** that run in loops or make thousands of small tool calls.

### Featherless.ai
- **Pricing:** $10 Basic / $25 Premium / $75+ Scale / Enterprise
- **Limits:** Concurrency-capped (2 / 4 / 8+ concurrent at each tier) — *not* token-capped
- **Catalog:** **6,700+ Hugging Face models** — by far the broadest in any tier
- **Hardware/stack:** Custom hot-swap inference engine with sub-250ms cold start; built by Recursal.AI (the RWKV team)
- **Compliance:** Privacy-positioned, explicit no-log policy
- **Distribution:** Now powers Hugging Face's "Featherless AI" inference endpoints option — meaning when you click "Inference Endpoints" on a HF model page, this is what's serving it
- **What makes them interesting:** The **catalog breadth is unmatched** — if a model exists on Hugging Face, Featherless can probably serve it, even rare community fine-tunes that no commercial provider would bother with. The hot-swap engine is genuinely novel: rather than keeping every model warm (impossible at 6,700+ scale) or cold-starting on demand (slow), they built infrastructure that loads model weights into pre-warmed compute slots in under 250ms. For roleplay communities (SillyTavern, JanitorAI), creative writing, exotic merges, and indie experimentation, no other provider comes close.
- **Best for:** Roleplay, exotic fine-tunes, model exploration across the HF ecosystem, indie dev work where you want to try 20 different models without 20 different bills.

### Arli AI
- **Pricing tiers:** Starter / Core / Advanced / Professional / Ultimate / Continuous (separate pricing for LLM and image plans)
- **Limits:** **Truly unlimited tokens & requests** — parallel request count is the only limit. Run an agent loop overnight, the bill stays the same.
- **Hardware/stack:** INT8 base models with FP16 LoRAs hot-swapped on top — efficient way to host many fine-tunes
- **Compliance:** **Zero-log policy** (servers don't write to disk), 30-day money-back guarantee
- **HQ:** Indonesia; uses Midtrans payment processor (region matters for billing setup)
- **Notable models:** Their proprietary RpR (Roleplay-with-Reasoning) finetunes have a cult following on SillyTavern; also hosts up to 200K context, GLM-4.7, Qwen 3.5 vision
- **What makes them interesting:** The **truly-unlimited framing is rare**. Most "unlimited" providers cap concurrency or have soft fair-use ceilings. Arli's only meaningful limit is how many parallel requests you can fire. The zero-log policy is architectural, not just a privacy statement — they don't write prompts/completions to disk. Indonesia HQ means they're outside US/EU jurisdictional reach for subpoenas, which some users specifically want.
- **Best for:** SillyTavern / JanitorAI users, Roo Code / Kilo Code coding agents running at high concurrency, anyone who wants strong privacy without crypto rails.

### Synthetic (synthetic.new)
- **Pricing:** $30/mo flat (was $20, raised Feb 2026)
- **Limits:** Request-based with 5-hour windows; **tool-call requests count as 0.1**, so ~500 small tool calls don't burn your quota
- **Models:** Kimi K2.5, MiniMax M2.5, GLM 5.1, GLM 4.7 Flash, Qwen3 Coder 480B, DeepSeek 3.1 — **all unquantized**
- **Compliance:** Standard, no enterprise-specific claims
- **Frontend compatibility:** Works with Roo, Cline, OpenCode, Crush, KiloCode, Octofriend — no platform restrictions
- **What makes them interesting:** Two unique commitments. First, **"never quantize"** — explicitly promises to serve full-precision models, where Hyperbolic, Together Turbo, DeepInfra FP4, and SiliconFlow FP8 all serve quantized variants on cheapest tiers. Second, the team open-sourced **Synbad**, an eval suite that specifically tests whether providers serve the model they claim — directly checking for the silent quality drops that quantized hosting introduces. The 0.1× tool-call weighting is the cleanest pricing structure I've seen for agentic workloads — most providers count every API call equally, which punishes agents that make many small calls.
- **Best for:** Coding agents (Roo, Cline, OpenCode) where unquantized quality matters and tool-heavy workflows would otherwise burn through quota.

### OpenCode Zen
- **Pricing:** Free tier with 8 exclusive models; paid tier (verify current pricing at opencode.ai)
- **Models:** Big Pickle, MiniMax M2.5 Free, MiMo V2, plus other curated models tested specifically for coding-agent workloads
- **Distribution:** Default-recommended inference path inside OpenCode (open-source coding agent with 150K+ GitHub stars, 6.5M monthly developers)
- **Compliance:** Privacy-first (OpenCode's stated principle: "does not store any of your code or context data")
- **What makes them interesting:** The **curation-as-product** angle is unique. Where every other provider serves whatever models customers want, OpenCode Zen explicitly hosts only models that pass the OpenCode team's internal eval suite for coding-agent workloads — meaning models with verified function-calling fidelity, reasonable tool-use patterns, and consistent quality. They publish their pick list specifically because the OpenCode team hit quality cliffs running random OSS models and decided to build the inference layer themselves. Pairs naturally with Synthetic (the other "we tested these specifically") provider — both compete on quality assurance rather than pure price.
- **Important context:** OpenCode-the-product is the agent (analogous to Claude Code, Cline, Cursor); OpenCode Zen is their bundled inference offering. The agent works with any provider you point it at — Zen is just the recommended default.
- **Best for:** Anyone running OpenCode as their coding agent; users who've been burned by quality drops on commodity providers and want pre-vetted models.

### Z.AI GLM Coding Plan
- **Pricing:** Lite ~$10/mo (was $3 promo before Feb 11, 2026), Pro ~$30/mo, Max ~$80/mo (billed quarterly; periodic discounts)
- **Limits:** 5-hour and weekly windows; **GLM-5.1 burns 3× quota at peak** (UTC+8 14:00–18:00), 2× off-peak; GLM-4.7 is 1×
- **Models:** GLM-5.1 (Opus-class reasoning), GLM-5-Turbo, GLM-4.7, GLM-4.5-Air; free tier on GLM-4.7-Flash and GLM-4.5-Flash
- **Bonuses:** Includes web search MCP, web reader MCP, Zread MCP, vision MCP — each of which would otherwise be a separate subscription
- **Compliance:** Standard; China-affiliated (Zhipu AI is Chinese)
- **Catch:** Restricted to "officially supported tools" (Claude Code, Cline, OpenCode) — third-party SDK access throttled; some countries have access restrictions
- **What makes them interesting:** Genuinely cheap access to **GLM-5.1, which benchmarks competitively with Claude Opus 4.5** on reasoning and coding. The bundled MCP tooling (web search, vision, document reading) is the hidden value — if you'd otherwise pay for Firecrawl or similar separately, the $10 Lite tier is essentially free once you account for those tools. The peak-hour multiplier is honest pricing for a constrained resource (their data center is in China; peak hours align with Chinese business hours).
- **Best for:** Coding work with Claude Code or Cline as the frontend, where you want frontier-quality reasoning without the Anthropic price tag.

### Ollama Cloud
- **Pricing:** Free / Pro $20/mo / Max $100/mo (Sept 2025 launch, GA in 2026)
- **Limits:** 5-hour session limits + 7-day weekly limits; usage measured in actual GPU time
- **Models:** DeepSeek V3.2, Qwen 3.5, Kimi K2.5, plus 17+ others — full Ollama catalog
- **Compliance:** Standard
- **What makes them interesting:** **Zero-friction transition from local Ollama to cloud.** Same OpenAI-compatible HTTP surface as your local install — change one base URL and you're cloud-hosted. For users who've already built workflows around `ollama run`, this is genuinely a drop-in upgrade for when you want to access big models that don't fit on your hardware. Fixed subscription means no overage anxiety if you leave Claude Code or OpenClaw running; predictable pricing is the explicit value.
- **Best for:** Developers who already standardize on Ollama locally and want to spill over to cloud for big models without changing tooling.

### Mancer.tech
- **Pricing:** Free tier + credit-based paid models
- **Stance:** Explicitly uncensored ("Prompt responsibly. You're an adult.")
- **Models:** Magnum (Claude-style fine-tunes on Qwen 72B), Goliath 120B, MythoMax, Weaver
- **Sample pricing on OpenRouter:** Magnum v2 72B at $3/$5; Weaver alpha at $0.75/$1
- **Compliance:** Privacy-focused, explicitly no-log
- **What makes them interesting:** The **uncensored positioning is architectural, not marketing.** They host models specifically chosen for minimal safety training (Magnum, Goliath), and they're explicit that the platform is for adults exercising their own judgment. For creative writing, narrative agents, and SillyTavern users, this matters because most commercial providers either refuse those requests or insert safety preambles that break immersion. Note: this also means using Mancer for production user-facing apps requires *your own* moderation layer — they don't add one for you.
- **Best for:** Creative writing without refusals, narrative agents, SillyTavern, anyone building adult-themed products who needs an inference provider that won't refuse mid-generation.

### Targon
- **Pricing:** Bittensor-affiliated; pricing structure varies as a SN64 component
- **Hardware:** "Ultra fast InfiniBand inference" — high-bandwidth interconnect optimized for large parallel context
- **Compliance:** Decentralized, no traditional certifications
- **What makes them interesting:** Less consumer-facing than Chutes (also Bittensor) — Targon positions itself as researcher-grade infrastructure rather than a polished API product. The InfiniBand-backed parallel inference matters for workloads that need genuinely large context windows or many concurrent inferences against the same model.
- **Best for:** Researchers running large parallel context workloads, or Bittensor-native builders preferring direct subnet access over Chutes' aggregation layer.

---

## Tier 3 — Flat-rate, credit-limited (frontier + OSS bundles)

Heavy use of expensive models drains the bucket fast. Same flat-rate framing as Tier 2, but the limit is credits/points that deplete at different rates depending on which model you call. Cheap models barely move the needle; calling Opus 4.7 or GPT-5.5 Pro can drain 30-50K points per query. The advantage is **model breadth** — frontier closed models bundled with OSS, all under one bill. Best for **bursty, varied workloads** that include expensive frontier reasoning.

### Abacus.AI ChatLLM Teams
- **Pricing:** $10/mo per user (Pro tier +$10 for unrestricted Abacus Studio + DeepAgent autonomous agent)
- **Models:** GPT-5.5, GPT-5.5 Thinking, Codex 5.3, GPT-5.5 Pro, o3, Sonnet 4.6, Opus 4.7, Gemini 3.1 Pro, Grok 4.2, Qwen 3.6, DeepSeek v4, Kimi 2.6 thinking, GLM 5.1 + 20 others
- **Allowance:** 20K credits/month; **Llama / Gemini Flash / GPT Mini / Kimi K2 / GLM 4.7 unrestricted** even after credits run out
- **Includes:** **RouteLLM API** — auto-picks best model per prompt; developer-grade unified API at consumer prices
- **Compliance:** **SOC-2 Type 2, HIPAA, no training on user data**, all LLMs hosted on US infrastructure (per their FAQ)
- **Image gen:** Nano Banana Pro, Flux Ultra, Grok Imagine
- **Video gen:** Sora 2, Veo 3.1, Kling AI v3, Seedance 2.0 (Pro tier only)
- **What makes them interesting:** **The most underpriced product in this entire reference.** $10/mo replaces ChatGPT Plus ($20) + Claude Pro ($20) + Gemini Advanced ($20) — you get all three frontier models plus 30+ open-source models, a developer API (RouteLLM), and image/video generation, for a sixth of the combined cost. New frontier models appear within 24-48 hours of release. SOC-2 + HIPAA + US-only infrastructure is rare at this price point — most consumer AI bundles either skip compliance entirely or charge enterprise rates for it.
- **Best for:** Anyone currently paying for multiple frontier subscriptions; small teams wanting unified frontier access with developer API in the same bill; compliance-sensitive users (healthcare, finance) who can't use vague "consumer AI products."

### Poe (by Quora)
- **Pricing:** $4.99 → $19.99 → $249.99/mo tiers; **Premium ($19.99)** is the standard pick
- **Premium allowance:** ~1M compute points/month
- **Models:** GPT-5.4, Claude Sonnet 4.5/Opus 4.5, Gemini 3 Pro, Grok 4 + ~200 community bots
- **Image gen:** Nano Banana Pro, Ideogram, FLUX
- **Video gen:** Veo 3.1, Pika, Hailuo, Runway
- **TTS:** ElevenLabs, Cartesia
- **Context:** Up to 2M tokens
- **What makes them interesting:** **Strongest custom-bot ecosystem in the consumer aggregator space.** Poe pioneered the "build a bot from a prompt + model + persona, share with the community" pattern. They have multi-bot conversations (run two bots against each other), creator monetization (bot makers earn from subscriber engagement), and the largest library of pre-built personas. Mobile experience (iOS + Android) is genuinely best-in-class with strong voice mode integration.
- **Catch:** Heavy Opus / GPT-5 Pro use drains 30K-50K points per query — a serious developer can burn through Premium's 1M points in a few hours. The points system is opaque enough that you don't realize you're rate-limited until you hit the wall.
- **Best for:** Casual to moderate users wanting frontier model variety, bot creators, mobile-first AI users.

### You.com Pro
- **Pricing:** $15/mo annual ($180), $20/mo monthly
- **Models:** GPT, Claude, Gemini, DeepSeek R1, Llama 4 — unlimited
- **Differentiators:** **ARI deep research feature**, 2M-token context windows, unlimited file uploads
- **Max tier:** Adds shared workspaces, multi-GB file uploads, faster support
- **Customers:** Stanford, Salesforce, Mimecast, Hugging Face, Institute for Advanced Study
- **Compliance:** Enterprise-tier features available
- **What makes them interesting:** **Research-focused frontier aggregator.** Where Abacus is "frontier API at consumer prices" and Poe is "frontier chat with bots," You.com is "frontier AI for serious research workflows." The ARI deep research feature does multi-source synthesis with citation tracking that's genuinely better than ChatGPT's web search or Claude's research tools. 2M-token context lets you paste entire codebases or PDFs without chunking. Customer list (Stanford, IAS, Hugging Face) reflects the research positioning.
- **Best for:** Knowledge workers doing research-heavy tasks where citation tracking and long-context analysis matter; academic users.

### Nous Portal
- **Tiers:**
  - Free: $0/mo, free models only
  - Plus: **$20/mo** → $22 credits (10% bonus), $10 rollover cap, 300+ models including frontier closed, **Tool Gateway**
  - Super: **$100/mo** → $110 credits, $50 rollover, Tool Gateway
  - Ultra: **$200/mo** → $220 credits, $100 rollover, Tool Gateway
- **Tool Gateway:** Bundles **Firecrawl** (web search), **FAL** (image gen), **OpenAI** (TTS), **Browser Use** into the subscription — no separate API keys needed
- **Hermes models hosted at $0.05/$0.20 per M tokens (70B) and $0.09/$0.37 (405B) through the credit system
- **Powers Hermes Agent** — persistent server-resident agent that learns over time
- **What makes them interesting:** **The Tool Gateway is the unique pitch.** Most credit-limited aggregators just bundle models. Nous bundles models *plus* the agent tools you'd otherwise integrate separately — Firecrawl alone runs $20/mo on its own subscription, so the Plus tier essentially gives you frontier-model credits for free if you'd be paying for Firecrawl anyway. Hermes Agent is a different product than the chat UI most aggregators ship — it's a server-resident agent meant to run continuously, not an ephemeral chat interface. Ideal for builders who want frontier API access *and* the agent infrastructure layer in one bill.
- **Best for:** Builders running persistent agents (Hermes Agent or custom), Nous Hermes / Nomos model fans, anyone who'd otherwise be cobbling together Firecrawl + FAL + TTS subscriptions separately.

### Merlin AI
- **Pricing:** Starts $12/mo, $19/mo for full tier
- **Form factor:** **Browser-extension-first** ("ChatGPT in every webpage" — Gmail, Google Docs, X, Notion)
- **Models:** GPT, Claude, Llama 3 — 26+ in total
- **Catch:** "Unlimited Pro" capped at $100/month of compute under fair-use; aggressive throttling above that
- **What makes them interesting:** **The form factor is the differentiator, not the model catalog.** Where Abacus and Poe are standalone products you visit, Merlin lives in your existing tools — write an email in Gmail with Claude, summarize a Google Doc with GPT, draft a tweet with Llama, all without leaving the page. For users whose AI use is mostly "improve this thing I'm already writing," this beats context-switching to a chat tab. Less feature-dense than Abacus or Poe but lighter weight.
- **Best for:** Knowledge workers who mainly want frontier access embedded in their existing browser workflow rather than as a separate destination.

### TypingMind
- **Pricing:** $39 lifetime (one-time payment)
- **Model:** Bring your own API keys — pay only API rates underneath
- **Strength:** Polished frontend for OpenAI / Anthropic / OpenRouter / any OpenAI-compatible endpoint
- **What makes them interesting:** **Different product entirely from Tier 3 aggregators** — TypingMind sells you the *frontend*, you bring the inference. For users who already have API keys (or want to use Abacus's RouteLLM as the backend), $39 once gets you a chat UI with multi-model switching, prompt libraries, and conversation organization that's better than ChatGPT.com's. The lifetime pricing means it pays for itself in two months vs. a typical $20/mo subscription if you don't use that subscription's bundled allowance.
- **Best for:** Power users with existing API keys who want a better chat UI than vendor-default frontends.

### MSTY (desktop app)
- **Pricing:** Free + paid tiers
- **Form:** Local-first desktop app (macOS/Windows) with optional cloud routing
- **What makes them interesting:** **Single UI for both local Ollama and cloud providers.** For users who run Ollama on their machine for sensitive work and switch to cloud for heavy reasoning, MSTY is the only well-known interface that handles both seamlessly. Privacy-focused — your local prompts never touch a server.
- **Best for:** Privacy-focused users wanting a single UI across local + cloud inference; power users who don't want to maintain two separate chat tools.

### Other consumer aggregators worth knowing

- **Nelima** — Model-agnostic chat with custom personas; smaller but actively maintained.
- **HuggingChat** — Free HuggingFace-hosted multi-model chat; limited features but no signup friction.
- **Perplexity Pro** ($20/mo) — Research-focused with web search baked in. Overlaps Tier 3 conceptually but is more search-product than aggregator. Strongest for citation-heavy research.
- **Kagi Assistant** — Bundled with Kagi search subscription; underrated for users already paying for Kagi.

---

## Tier 4 — Genuinely free tiers

Persistent free tiers requiring no credit card. These renew daily/monthly forever — not "$5 trial" free.

### Google AI Studio
- **Models:** Gemini 2.5 Flash, Pro, Flash-Lite + Gemma family
- **Limits:** ~500–1,500 requests/day, 5–15 RPM, **1M-token context**
- **Bonus:** Native multimodal in a single call (text + image + audio + video + PDFs)
- **Catch:** Free-tier prompts may be used for model improvement
- **What makes it interesting:** The **most generous free tier from any major lab**, period. Google subsidizes Gemini access aggressively, and the multimodal handling in a single API call is genuinely better than competitors who require separate vision/audio endpoints. The 1M-token context is unmatched at the free tier — useful for "summarize this 500-page PDF" workloads.
- **Best for:** Daily driver, long-context work, multimodal apps, prototyping anything Google-adjacent.

### Groq
- **Hardware:** LPU (Language Processing Unit) — purpose-built inference chip, 300–1,000+ tokens/sec
- **Free tier:** Every model accessible, no card; rate-limited (1K–14.4K req/day)
- **Paid:** $0.05–$3/M; Developer tier (add card, no min) gets 10× rate limits + 25% off
- **Models:** Llama 3.3 70B at 315 tps, Mixtral, Qwen, GPT-OSS — open-source only; no Claude/GPT/Gemini
- **What makes it interesting:** **Speed is the entire product.** LPUs are custom silicon designed specifically for transformer inference, not general-purpose GPUs. The result is the fastest open-source model serving available — 315 tokens/sec on Llama 70B is genuinely 5-10× what GPU-based providers deliver on the same model. For voice agents (where every additional second of TTFT shows up as awkward silence) or real-time chat UX, the speed difference is the difference between feeling responsive and feeling laggy. Free tier is no-friction, paid tier is competitive on price.
- **Best for:** Latency-critical UX, voice agents, real-time chat, anything where the user is *waiting*.

### Cerebras
- **Hardware:** Wafer-Scale Engine (WSE-3) — single chip the size of a dinner plate, ~2,600 tokens/sec; fastest measured throughput in the industry
- **Free tier:** **1M tokens/day**, 30 RPM, 14,400 RPD — most generous hard cap of any free tier
- **Models:** Llama 3.3 70B, Qwen3 32B, Qwen3 235B, GPT-OSS 120B
- **Notable:** Signed $10B inference deal with OpenAI January 2026
- **What makes it interesting:** **Even faster than Groq at the high end.** 2,600 tps means a complete 1,000-token response in under half a second. The wafer-scale design avoids the GPU-to-GPU communication overhead that limits speed on larger models — a single Cerebras chip can host an entire 70B model without sharding. The 1M-tokens/day free allocation is the most generous free tier in this entire reference; legitimate batch synthetic-data-generation workloads can run entirely on free credits. The OpenAI deal is structural — it suggests Cerebras has scaled the hardware enough that frontier labs trust it for production inference.
- **Best for:** Batch dataset curation, synthetic data generation, multi-step agents where throughput dominates per-token cost.

### SambaNova
- **Hardware:** RDU (Reconfigurable Dataflow Unit), 186–294 tps
- **Free tier:** $5 credit + persistent free tier on Llama 3.3 70B and Qwen 2.5 72B (10–30 RPM)
- **Strength:** Llama 3.1 405B at $5/M input — **cheapest 405B available anywhere**
- **What makes it interesting:** The **405B specialist.** Most providers either don't host Llama 405B at all or charge premium pricing for it. SambaNova's RDU architecture handles very large models efficiently, and they pass that through as the lowest 405B pricing in the market. For workloads where you specifically need 405B-class capability (long reasoning chains, complex code, high-stakes generation), they're the obvious choice.
- **Best for:** Anyone needing Llama 405B specifically; users wanting persistent free tier on substantial models (Llama 70B, Qwen 72B).

### Cloudflare Workers AI
- **Free tier:** 10,000 Neurons/day (~5–10K requests depending on model)
- **Catalog:** 47+ models including Llama 3.3 70B, Llama 3.2 (incl. vision), Mistral, DeepSeek R1 distills, FLUX.2, Whisper
- **Edge deployment:** Inference runs at whatever Cloudflare data center is closest to the user
- **Paid:** $0.011/1,000 Neurons overage
- **What makes it interesting:** **Edge inference is the unique product.** Where every other provider routes you to a centralized data center, Cloudflare runs models on their existing 300+ POP CDN footprint. For globally distributed apps, this means 50ms TTFT in Mumbai or São Paulo without the 200ms+ trans-pacific latency tax. Pairs naturally with anyone already on Cloudflare Workers — you get inference from the same compute fabric serving your application code, with no cross-region egress charges.
- **Best for:** Anyone already on Cloudflare; globally distributed apps where edge latency matters; serverless apps wanting to skip a separate AI vendor.

### Mistral La Plateforme (Experiment plan)
- **Free models:** Mistral Large 3, Small 3.1, Ministral 8B + 3 more
- **Limits:** 1 req/s, **1B tokens/month**
- **Catch:** Phone verification required; data may be used for training
- **EU data residency**
- **What makes it interesting:** **1 billion tokens/month** on Mistral Large 3 is an absurd amount of free quota — most users won't approach it. Combined with EU data residency (Mistral is French, hosts in Europe), it's the obvious free tier for European users or anyone who wants to evaluate Mistral models without committing.
- **Best for:** EU users wanting GDPR-clean inference; anyone evaluating Mistral models pre-purchase; high-volume free prototyping.

### Cohere
- **Free trial:** 1,000 API calls/month, 20 RPM
- **Catalog:** Command A, Command R+, Aya Expanse 32B, Embed 4, Rerank 3.5
- **What makes it interesting:** **Best free option for the full RAG pipeline.** Where most free tiers give you a chat model, Cohere gives you generation + embeddings + reranking — the three components every production RAG system needs. The Aya Expanse models are also notably strong on multilingual workloads.
- **Best for:** RAG and search apps; multilingual products; teams evaluating the full Cohere stack.

### GitHub Models
- **Free:** 50 chat + 2K completions per month; 30+ models including GPT-4o, Llama 3.3 70B, DeepSeek R1
- **Catch:** Restrictive token limits per request; tied to GitHub Copilot tier
- **What makes it interesting:** **Multi-frontier-model access tied to your GitHub account.** No new signup, no new key, no new bill. The token limits per request are tight enough that you can't use it for production, but for "let me try this prompt against five different models real quick," it's the lowest-friction option available.
- **Best for:** Quick model comparison testing; anyone who already has a GitHub Copilot subscription.

### NVIDIA NIM (build.nvidia.com)
- **Free:** 91 endpoint models — language, vision, biology, simulation, safety
- **Limits:** 40 RPM
- **What makes it interesting:** **Specialty models you can't get elsewhere.** NVIDIA's catalog includes drug discovery models (BioNeMo), protein folding (RoseTTAFold), simulation models, and other domain-specific tools that commercial inference providers don't host. Free tier is generous enough for legitimate research use.
- **Best for:** Researchers needing specialty scientific models; biotech/pharma; anyone evaluating NVIDIA's AI Foundry stack.

### Hugging Face Inference Providers
- **Model:** Routes to multiple providers (Featherless, Together, Fireworks, etc.)
- **Free tier:** Limited but useful for prototyping
- **Catch:** Serverless inference limited to <10GB models on some routes
- **What makes it interesting:** **Lowest friction to test any HF-hosted model.** Click "Inference API" on any HF model page, get a curl command, you're testing in 30 seconds. The routing happens automatically; you don't pick the underlying provider.
- **Best for:** Model evaluation, prototyping, anyone evaluating HF-hosted models without committing to a specific provider.

### Vercel AI Gateway
- **Free models** with curated provider set
- **Catch:** May use data for improvement
- **What makes it interesting:** Native to Vercel AI SDK — zero-config inference for Vercel-hosted apps. If you're already deploying on Vercel, this is probably the lowest-friction inference path for your AI features.

### Other persistent-free notes
- **Zhipu/Z.AI Flash:** GLM-4.7-Flash and GLM-4.5-Flash genuinely free with reasonable limits.
- **LLM7.io** (UK): 30+ models including DeepSeek R1, Qwen2.5 Coder; 30 RPM free, 120 with token.
- **Kluster AI free:** DeepSeek-R1, Llama 4 Maverick, Qwen3-235B (limits undocumented).

### Trial-credit-only providers (not really free, but worth knowing)
- Fireworks: $1 credit
- Together: $25 credit (most generous)
- Hyperbolic: $1 trial
- SambaNova: $5 for 3 months on top of free tier
- AI21: $10 for 3 months
- Inference.net: $25 credits
- Baseten: trial credits
- Upstage / NLP Cloud: small trial credits

---

## Tier 5 — Local / self-hosted

Zero variable cost; capital cost up front. Breakeven analysis: an RTX 4090 build (~$2K) crosses Ollama Cloud Pro Max ($100/mo) at roughly 25K daily requests, per Pooya Golchian's March 2026 calculation.

- **Ollama** — Local runtime; same HTTP API as Ollama Cloud (drop-in). The de facto local LLM standard for developers.
- **LM Studio** — Desktop UI for HF models. Polished UX, easier than Ollama for non-CLI users.
- **vLLM / SGLang / TGI** — Production inference engines for self-hosted GPU. What every commercial provider builds on top of.
- **llama.cpp / koboldcpp** — CPU + lightweight GPU. Runs surprisingly well on Apple Silicon and modern CPUs.
- **Jan.ai** — Open-source ChatGPT-style desktop app; bridges local and cloud cleanly.

**Why local is interesting beyond cost:** privacy is architectural (your prompts never leave your machine), latency is bounded by your hardware (predictable), and you're immune to provider price hikes, deprecations, or outages. The breakeven math favors local for any sustained workload above ~25K requests/day, and the privacy math favors local for any workload involving sensitive data — regardless of cost.

---

## Tier 6 — Decentralized / crypto-native

### Venice.ai (VVV / DIEM)
- **Founded:** May 2024 by Erik Voorhees (ShapeShift founder)
- **Mechanic:** **Stake-for-inference.** Stake VVV on Base → pro-rata share of daily compute capacity perpetually
- **DIEM token (Aug 2025):** Each DIEM = $1/day API credit forever; mint by locking staked VVV, burn to unlock
- **VVV staking yield:** ~18% with monthly buy-and-burn from revenue
- **Consumer Pro:** $18/mo — unlimited chat, **uncensored**, on-device encryption, no server-side prompt storage
- **API:** OpenAI-compatible from 10 credits per 1M tokens
- **Catalog:** GPT-5.2 (added Dec 2025), DeepSeek, Llama, Qwen, FLUX
- **Scale:** 1M+ users, 50K DAU, 15K req/hour
- **Notable partnership:** OpenClaw selected Venice as primary AI model provider March 2, 2026
- **What makes them interesting:** **The cleanest crypto-native inference economics.** Most crypto-AI projects bolt a token onto a centralized service. Venice built the staking-for-capacity primitive from the ground up — VVV staking is genuinely tied to compute capacity, not just governance theater. The DIEM token is the more interesting innovation: tokenized perpetual compute as a tradeable asset means you can speculate on future inference demand without ever using inference yourself, or lock in $1/day forever for a one-time DIEM purchase. Privacy is architectural (Base L2 + on-device encryption + no server-side storage) and uncensored is the explicit policy. Erik Voorhees's involvement gives the project crypto-native credibility most AI startups lack.
- **Best for:** Crypto-native builders, privacy-focused users, anyone who wants frontier model access without a recurring subscription, OpenClaw users (now the official default).

### Chutes (Bittensor SN64)
- **Tiers:** $3 / $10 / $20+ monthly + pay-per-token
- **Important update:** **Killed free tier Feb 27, 2026** after users were burning 100–324× their subscription value. Subscriptions now capped at 5× their PAYG value.
- **Cost:** ~85% lower than AWS for comparable inference
- **Scale:** ~$5.5M annualized revenue; integrations with Vercel SDK, n8n, OpenClaw
- **Models:** Llama 3.3 70B, DeepSeek R1 distills, Qwen3 family, Mixtral, Codestral
- **What makes them interesting:** **Highest-revenue Bittensor subnet by far** — Chutes is what proves the decentralized-inference thesis can actually generate meaningful real-world revenue. The subnet's miner economics created genuinely cheap inference (85% below AWS) until the free tier abuse forced the recent rationalization. For Bittensor-native builders, Chutes is the default; for everyone else, they're worth knowing about as the dominant proof-of-concept that decentralized inference can scale.
- **Catch:** Decentralized = variable reliability; data passes through whoever wins the auction (no cryptographic guarantees on standard tier; TEE tier exists). Subnet emissions and governance shifts can change pricing/availability quickly.
- **Best for:** Crypto-native builders comfortable with the Bittensor ecosystem; non-critical workloads where 85% cost savings outweigh occasional flakiness.

### Eternal AI
- **Founded:** 2024 by Cypher Labs
- **Architecture:** **Proof-of-Compute consensus** — prompts submitted onchain to an Inference smart contract, randomly distributed to three nodes, with a separate verifier set continuously checking outputs
- **Verification:** pBFT committee — 2/3+1 of miners must produce the same deterministic response
- **Storage:** Models packaged as standardized containers, stored on Filecoin/Arweave, deployed as smart contracts ("Eternals")
- **Multichain:** Bitcoin, Ethereum, Solana, Base, Avalanche, Polygon, BNB Chain, Arbitrum — explicitly designed to be callable from dapps on any chain
- **Models:** DeepSeek-R1-Distill-Llama-70B, Llama 3.1 405B, FLUX, Dobby-Llama-3.3-70B (uncensored, by SentientAGI)
- **API:** OpenAI-compatible Decentralized Inference API at eternalai.org/api; permissionless, no KYC
- **Token:** EAI — micro-cap (~$747K market cap, ~$5K daily volume)
- **What makes them interesting:** **Onchain-verifiable AI is the unique property.** Every inference is recorded onchain and independently verifiable by anyone — no other live provider in this reference offers that. For compliance-driven use cases (auditable AI for regulated industries), trust-minimized agents (DAO operations, smart contract execution), or research into verifiable computation, Eternal AI is the only option that ships today. The multichain design explicitly answers Bittensor's single-network limitation — your dapp on Solana or Base can call inference without bridging.
- **Catch:** Async programming model — submit prompt onchain, poll PromptScheduler contract for response. Not suitable for latency-sensitive work. Requires a wallet with gas tokens. Token is micro-cap, so the economic flywheel is still early.
- **Best for:** Crypto-native builders who specifically need verifiable inference (compliance, auditability); dapp developers wanting AI capabilities callable directly from smart contracts.

### Phala Network
- **Architecture:** Confidential AI inference using **TEE (Trusted Execution Environment)** — hardware-enforced encryption
- **Strength:** Zero-knowledge guarantees on data privacy
- **What makes them interesting:** **Different threat model from every other decentralized provider.** Where Eternal AI uses cryptoeconomic verification (miners can't fake outputs because logits don't lie) and Venice uses on-device encryption + decentralized staking, Phala uses hardware-level TEE — your prompts and outputs are encrypted such that even Phala operators can't read them. For privacy-paranoid use cases (legal work, M&A documents, medical records, opposition research), TEE-based inference is the strongest privacy guarantee available short of self-hosting.
- **Best for:** Privacy-paranoid workloads; legal/medical/financial work where even the inference provider shouldn't see your prompts; users specifically preferring hardware-attested privacy over cryptoeconomic privacy.

### Other decentralized notes
- **Sentient** (via Fireworks AI infra) — Multi-agent reasoning, agentic-native; not technically decentralized but agentic-native and crypto-adjacent.
- **Akash Network** — Decentralized GPU rental (mostly training-oriented).
- **Render (RNDR)** — Decentralized GPU rendering (less LLM-focused).
- **Bittensor (TAO)** broader network — Chutes and Targon are subnets within it.

---

## Multi-provider gateways

Gateways are a different product category from the providers above. They route requests across other inference providers' infrastructure — one API key, but the actual compute happens on Fireworks/DeepInfra/Together/etc.

### OpenRouter
- **Coverage:** 300+ models, 60+ providers
- **Free tier:** 50 req/day → 1,000/day with $10 lifetime topup
- **Strength:** Automatic failover, BYOK supported, single billing
- **Free models:** DeepSeek R1, Llama 3.3 70B, Gemma 3, Qwen3 Coder 480B, Hermes 4 405B, GPT-OSS 120B, MiniMax M2.5
- **Caveat:** Some "free" backends serve quantized variants (Aider tests showed up to 10pp completion-quality gap on Qwen3 Coder between official Alibaba API and OpenRouter round-robin)
- **What makes them interesting:** **The de facto default for model-agnostic apps.** When DeepInfra goes down, your request fails over to Together. When Together's Llama 70B endpoint is throttled, traffic routes to whichever Llama 70B backend is healthy. This kind of automatic resilience is impossible with a single direct provider, and building it yourself across 60 providers is a multi-engineer-quarter project.
- **Best for:** A/B testing, model-agnostic apps, automatic failover. *De facto* default for most agent harnesses.

### LiteLLM
- **Form:** Open-source proxy you self-host
- **Coverage:** 100+ providers
- **Strength:** Full control of routing logic and logs; fits self-hosted infra patterns
- **What makes it interesting:** **The "build your own gateway" option.** Where OpenRouter is a service you call, LiteLLM is code you run. Same OpenAI-compatible interface, but you control the routing rules, the logging, the rate limits, and the failover behavior. For production deployments wanting BYO observability or strict data-handling requirements (no third-party gateway sees your prompts), this is the obvious choice.
- **Best for:** Production deployments wanting BYO observability and routing logic; teams with strict no-third-party-pass-through data policies.

### Portkey
- **Strength:** Observability-first gateway
- **Coverage:** 250+ models
- **Features:** Built-in guardrails, PII filtering, request caching, budget tracking
- **What makes it interesting:** **Observability + guardrails + caching as the product, not the gateway.** Where OpenRouter optimizes for failover and LiteLLM optimizes for control, Portkey optimizes for "what's actually happening in production." PII filtering and budget alerts come built-in — useful for teams where AI spend or compliance is on the audit list.
- **Best for:** Teams needing audit trails, regulated industries, production AI systems with budget governance requirements.

### Vercel AI Gateway
- **Form:** Native to Vercel AI SDK
- **Strength:** Zero-config for Vercel-hosted apps
- **What makes it interesting:** Tightest integration with Vercel deployment patterns. If you're already on Vercel, the gateway is essentially free observability for your AI calls.
- **Best for:** Vercel ecosystem.

### Helicone
- **Form:** Observability layer that doubles as gateway via proxy mode
- **What makes it interesting:** Started as observability for AI apps, added gateway features later. Strongest if your primary need is "what's our AI doing in production" rather than "how do I route between providers."
- **Best for:** Teams wanting analytics + routing in one tool.

### Other gateways
- **Requesty** — Newer aggregator focused on price routing per request.
- **Eden AI** — Multi-provider including non-LLM (vision, OCR, translation).
- **Unify** — Auto-routing based on per-prompt benchmarks of cost/quality/latency.
- **Martian** — The OG "model router" — routes per-request to optimal model.
- **Abacus RouteLLM** — Included with Abacus ChatLLM $10 sub; closed-source but unified API across frontier + OSS.

---

## Hidden cost dimensions

When "cheapest" looks too good, check these. Most cause production failures, not visible bills.

1. **Quantization.** "$0.10/M for GPT-OSS 20B" usually means FP8 or INT8. Hyperbolic, Together (Turbo), DeepInfra (FP4), SiliconFlow (FP8) all serve quantized variants on cheapest tiers. For coding agents this can drop completion quality 5–10pp. Synthetic and OpenCode Zen explicitly promise "never quantize."

2. **Concurrency caps that aren't advertised.** DeepInfra silently caps at 200 concurrent. Hyperbolic at 600 RPM Pro. Featherless at 2/4/8 concurrent. These create surprise 429s under traffic.

3. **Subsidies running out.** Three providers raised prices in Feb 2026 alone (Chutes killed free tier, Z.AI hiked 30%+, Synthetic +$10). Anyone offering "unlimited" below cost will rationalize eventually. Transparent providers (Synthetic, Chutes) showed math; less transparent ones sent vague "investing in the platform" emails.

4. **"Unlimited" with hidden caps.** Merlin AI Pro is "unlimited" with a $100/mo fair-use ceiling. Poe Premium points drain in hours under heavy Opus use. Read the fair-use clause before assuming flat-rate means flat-rate.

5. **Free-tier training data leakage.** Mistral Experiment, Google AI Studio free, Gemma — most free tiers may train on your prompts. Hard no for sensitive workloads. Featherless and Arli AI explicitly don't log; Hyperbolic markets privacy; Abacus, Venice, OpenAI/Anthropic paid tiers don't train on user data.

6. **Cold starts.** Featherless ~250ms; serverless providers vary 3–15s for rare models; Avian explicitly pre-loads models for zero cold start; dedicated endpoints zero.

7. **Function-calling fidelity.** Aider tests showed a 10pp gap between official Qwen3 Coder and OpenRouter round-robin. Synthetic's `Synbad` and OpenCode Zen's curated catalog are the only public guarantees that the served model behaves like the published model on tool calls.

8. **Geography assumptions.** "We're SOC2!" doesn't mean US-hosted. Together AI runs across 25+ data centers globally, including non-US. Ask explicitly if data residency matters. Avian (Azure US), Cerebras, SambaNova, Groq, and Abacus all explicitly commit to US infrastructure; Nebius and Scaleway offer EU residency on dedicated tiers; most others split traffic globally.

9. **Egress and bandwidth.** Most providers don't charge egress; some do (especially batch/large-output workloads). Check.

10. **Token counting differences.** Some providers count completion tokens, some count "billable tokens," some count tool-call tokens differently (Synthetic explicitly weights them at 0.1×). Two providers at "$1/M output" can produce 30% different bills on the same workload.

---

## Comparative pricing snapshot

For Llama 3.3 70B (the most-hosted reference model), April 2026:

| Provider | Pricing model | Input ($/M) | Output ($/M) | Speed (tps) | Notes |
|---|---|---|---|---|---|
| Cerebras | Free tier | $0 | $0 | ~2,200 | 1M tokens/day free |
| Groq | Free tier | $0 | $0 | ~315 | Rate-limited |
| DeepInfra | Per-token | $0.23 | $0.40 | ~80 | 200 concurrent cap |
| Together AI | Per-token | $0.88 | $0.88 | ~95 | Broad catalog |
| Fireworks | Per-token | $0.90 | $0.90 | ~110 | FireFunction option |
| SambaNova | Per-token | $0.60 | $1.20 | ~294 | RDU hardware |
| Avian | Per-token | $0.40 | $0.50 | ~315 | SOC2/Azure |
| Anyscale | Per-token | $1.00 | $1.00 | ~85 | Enterprise compliance |
| OpenRouter | Pass-through | varies | varies | varies | Routes to above |
| Featherless Premium | Flat $25/mo | (unlimited*) | (unlimited*) | varies | 4 concurrent cap |
| Synthetic | Flat $30/mo | (unlimited*) | (unlimited*) | varies | Request-based, unquantized |

*Within request quota; tool calls count as 0.1×ed; concurrency-capped.

Caveats: prices change weekly; speed measurements depend on context length and concurrent load; "unlimited" means within the queue/concurrency limit, not literally infinite. Cross-check current rates at [Artificial Analysis](https://artificialanalysis.ai) before making decisions.

---

## Sources

- [Artificial Analysis](https://artificialanalysis.ai) — per-model provider benchmarks
- [pricepertoken.com](https://pricepertoken.com), [costgoat.com](https://costgoat.com), [costbench.com](https://costbench.com) — pricing trackers
- [TokenMix.ai](https://tokenmix.ai), Featherless, Inference.net blogs — provider-side comparisons (read with bias awareness)
- [LMSpeed](https://lmspeed.com), ComputePrices, [Infrabase.ai](https://infrabase.ai) — independent comparators
- [cheahjs/free-llm-api-resources](https://github.com/cheahjs/free-llm-api-resources) — community-maintained free-tier list
- [Sulat](https://sulat.dev), [Patshead](https://patshead.com) — independent practitioner reports on Synthetic, Chutes, Z.AI
- [Sacra](https://sacra.com) — Fireworks revenue/customer analysis
- Provider docs and pricing pages directly

---

## Contributing

Found an outdated price, missing provider, or factual error? Open a PR or issue. This doc is meant to be living — the market moves quarterly.

**Inclusion criteria:** A consumer must be able to call an inference endpoint *today*. Pre-testnet projects, even well-funded ones, are excluded regardless of how architecturally interesting they are. If a provider goes from "tracking only" to "live," PRs to add them are welcome.
