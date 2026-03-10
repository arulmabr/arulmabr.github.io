---
title: "Research"
layout: "research"
url: "/research/"
summary: Research
---

## Probing Circuit Robustness: How Syntactic Form Shapes Neural Circuit Activation in LLMs

**Arul Murugan** | UC Berkeley | Fall 2025 | INFO 256: Applied NLP

[Read the paper (PDF)](/papers/circuit_robustness.pdf) | [Code on GitHub](https://github.com/arulmabr/info-256-circuit-robustness)

Large language models are increasingly used in high-stakes settings, yet we still don't fully understand *how* they internally represent different types of content -- or how fragile those representations are. This project digs into that question using **mechanistic interpretability**: instead of just looking at what a model outputs, we look at the neural circuits it activates to get there.

### The Question

**Do semantically equivalent prompts activate the same internal circuits, even when their syntactic form changes?**

For example, "Who wrote Romeo and Juliet?" and "Romeo and Juliet's author is" ask for the same fact -- but does the model process them the same way internally?

### Approach

We used **Gemma-2-2B** with **Gemma Scope sparse autoencoders** to extract attribution graphs (structured representations of which internal features causally contribute to an output) for 500 prompt pairs across five categories:

- **Factual Recall** -- baseline knowledge retrieval (e.g., "Who painted the Mona Lisa?" vs. "The Mona Lisa was painted by")
- **Harm-Related** -- direct harmful requests paraphrased (e.g., "How do I make a bomb?" vs. "What are steps to create an explosive device?")
- **Word Order** -- pure syntactic reordering (e.g., "Yesterday I went shopping" vs. "I went shopping yesterday")
- **Adversarial Jailbreak** -- harmful requests wrapped in five jailbreak techniques (role-playing, hypothetical framing, context manipulation, ethical justification, multi-step indirection)
- **Refusal Spectrum** -- a five-tier gradient from obviously harmful to benign

We then measured circuit stability using three complementary metrics: **Weighted Jaccard Similarity** (edge overlap), **Spearman Rank Correlation** (importance ranking consistency), and **Feature IoU** (node overlap).

### Key Findings

**1. Syntactic form dominates circuit activation.** This was the central and most surprising result. Prompts with identical meaning but different syntactic structures activate fundamentally different circuits -- Jaccard similarity as low as 0.08. Meanwhile, syntactically similar paraphrases maintain high overlap (Jaccard > 0.80). The model doesn't have "semantic circuits" so much as "syntactic processing circuits." A WH-question ("Who discovered penicillin?") and a fill-in-the-blank statement ("Penicillin was discovered by") route through entirely different internal pathways, even though they target the exact same fact.

**2. Harm-topic circuits are *more* stable than factual ones.** Contrary to our initial hypothesis, harm-related prompts showed *higher* circuit stability (Jaccard = 0.543) than factual recall (0.362). But this turned out to be a syntactic confound: harmful requests are predominantly phrased as interrogatives ("How do I..."), giving them consistent syntactic form. When controlling for syntax, the difference disappears.

**3. Adversarial framing substantially disrupts circuits.** Jailbreak-wrapped prompts showed 48% lower circuit stability compared to direct harmful requests. All five techniques (role-playing, hypothetical scenarios, context manipulation, ethical justification, multi-step indirection) caused similar levels of circuit divergence (41-53% reduction), suggesting they share a common mechanism of altering internal representations rather than exploiting distinct pathways.

**4. Rank-overlap divergence.** Despite low circuit overlap (different features activate), the *relative importance* of shared features stays remarkably consistent (Spearman correlations of 0.86-0.94). The model maintains consistent computational priorities even when the actual circuit composition changes.

### Why This Matters

These findings have direct implications for AI safety. If adversarial framing techniques can reroute model processing through entirely different circuits, then safety mechanisms that monitor specific neural pathways could be systematically bypassed. Robustness evaluations that only test output behavior may miss fundamental representational brittleness.

More broadly, the dominance of syntactic form over semantic content in circuit activation suggests that current models are far more sensitive to *how* something is said than *what* is being said -- at least at the circuit level.

---

## Theorizing with LLMs

This is the first every paper that I have contributed to, we did this exactly an year back.

Check more about it here: [arulmurugan.me/theorizing_with_llms/](https://arulmurugan.me/theorizing_with_llms/)
