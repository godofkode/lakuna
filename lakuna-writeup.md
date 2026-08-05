# Lakuna: Technical Background

## The Proposal

Lakuna initiates with most traffic directed to the frontier model. The organization defines quality standards, maps actual workloads, evaluates local model candidates through deterministic verification and selective A/B testing, constructs a work-cluster × model capability matrix, and progressively migrates qualified traffic from frontier to local models.

The matrix remains economical to maintain:
- New models undergo testing against existing cluster cases
- Novel clusters add rows without system-wide disruption
- Changes to models, prompts, or runtime only invalidate affected cells
- Deterministic outcomes and user preferences strengthen or weaken confidence
- A/B testing diminishes once sufficient evidence accumulates
- Testing resumes following performance drift or promising new model releases

The objective establishes trust before transferring traffic. Version 1 routes already-qualified work while continuing measurement. This matrix subsequently indicates precisely where local models underperform, enabling targeted training for expanded coverage.

## Why Technical Backing Exists

Moslem and Kelleher's survey organizes the field into six paradigms: difficulty-aware routing, human preference alignment, clustering-based methods, reinforcement learning, uncertainty quantification, and cascading. Its primary discovery supports this proposal: "routing systems can outperform even the strongest individual model" through complementarity and specialization.

LLMRouterBench provides measurement vocabulary covering 400,000+ instances, 21 datasets, and 33 models. Key definitions include best single, cheapest qualified single, oracle performance, router results, cost savings, performance gains, and routing overhead.

Conformal LLM Routing calibrates routing around accepted violation rates rather than maximizing averages. For example, routing locally maintains fewer than 2% qualified request failures. FLARE demonstrates economic benefits: up to 68% latency reduction and 75% cost reduction while preserving accuracy.

## Local Models Foundation

A local model comprises model weights, typically packaged as GGUF files. An inference engine loads weights and performs generation. Quantization stores weights at lower precision, enabling larger models on smaller hardware.

## Consumer Inference Engines

**llama.cpp** operates GGUF models on CPUs, consumer GPUs, and Apple Silicon.

**LM Studio** provides recognizable desktop software for discovering, downloading, loading, and serving models.

**Ollama** supplies popular developer-oriented runtime and API, though its convenience layer may obscure model templates and performance decisions.

## Enterprise Inference Engines

Enterprise-local typically indicates models running on company-controlled GPU servers, private cloud, or on-premises systems.

**vLLM** serves high-throughput shared serving with batching and concurrent requests. Inferact, founded by vLLM's creators, raised $150M at $800M valuation in January 2026, signaling inference serving as distinct infrastructure.

**SGLang** and **NVIDIA TensorRT-LLM** represent alternative enterprise-serving options.

## Gateways

Gateways sit between applications and model providers, handling credentials, retries, and usage records.

**LiteLLM** provides unified API across cloud and self-hosted providers.

**OpenRouter** offers hosted API for cloud models, though routing occurs in their hosted service.

**Portkey** and **Cloudflare AI Gateway** supply enterprise request infrastructure with policy, caching, and fallbacks.

## Current Routing Approaches

**Semantic routing** embeds prompts and compares with predefined examples without additional LLM calls.

**Classifier routing** employs small classifiers predicting intent, domain, or difficulty for threshold-based routing.

**Cascade routing** calls cheap models first, escalating after timeout, failed checks, or low confidence.

Lakuna combines semantic clustering with measured model outcomes.

## Existing Routers and the Gap

**Not Diamond** learns optimal models and trains custom routers from evaluation data.

**OpenRouter** functions primarily as hosted marketplace and gateway, requiring cloud routing for every request.

**vLLM Semantic Router** combines semantic, capability, privacy, and policy signals.

**Conifer** offers local inference, cloud escalation, and consumer-friendly runtime.

**SmarterRouter** and **Nordlys** demonstrate clustering and per-cluster statistics feasibility at small scale.

The gap: existing tools run models or classify requests; Lakuna determines which local combinations meet quality requirements, progressively moving qualified work away from frontier models.

## Current Company Assembly

Companies typically:
1. Run local models through Ollama, LM Studio, or vLLM
2. Implement local proxy or OpenAI-compatible gateway
3. Write semantic rules or complexity classifier
4. Send remaining requests to frontier models
5. Retry frontier when local inference fails

Serious enterprises add scripts for clustering work, calculating success rates, comparing outputs, maintaining matrices, and detecting staleness.

Lakuna packages this measurement-and-transition system comprehensively.

## Learning Loop Extension

Once the matrix exists, it identifies which clusters the local tier cannot handle, targeting improvements.

**Moda** converts production agent traces into proposed changes.

**Trajectory** builds continual-learning platforms transforming usage into model, prompt, and harness improvements.

Lakuna can route initially, let frontier cover unqualified work, convert frontier results into cluster-specific evaluation material, improve local prompts or adapters, and require crossing the quality threshold before local expansion.

Version 1 focuses on routing and qualification. Training local models represents later extension using the same information matrix.

## Unknowns Remaining

The author proposes companies need simpler measurement and maintenance of local tiers but acknowledges limited enterprise experience. Outstanding questions include:

1. What infrastructure currently bridges evaluation results and production routing rules?
2. Do cluster-level model results predict performance on new company work?
3. At what traffic volume does frontier cost savings justify local tier operations?

---

Source: [gist](https://gist.github.com/chinmayi-r/42c1534807381fb33dbd3dad8fc8396f)
