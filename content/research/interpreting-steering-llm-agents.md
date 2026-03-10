---
title: "Interpreting and Steering LLM Agents for Social Simulations"
date: 2026-02-15
summary: "We compare prompting, sparse autoencoders, and linear probes for interpreting and controlling LLM agent behavior in social science simulations. SAE-based steering outperforms prompting, offering fine-grained, predictable control over preferences and capabilities."
---

**Jiayue Gaveal Fan\*, Arul Murugan\*, Shreyas Krishnan, Abhishek Nagaraj** | UC Berkeley | February 2026

*Preprint coming soon.*

LLM-powered agents are becoming a serious tool for computational social science -- running simulated experiments at a fraction of the cost of human studies. But there's a fundamental problem: these agents are black boxes. Social scientists need two things from their experimental subjects: **interpretability** (why did the agent behave that way?) and **controllability** (can we reliably shift behavior along specific dimensions?). Prompting alone falls short on both counts.

This paper asks: can we do better by going *inside* the model?

### The Setup

We ran four classic social science experiments on **Llama-3.3-70B-Instruct** agents, spanning two core dimensions of human behavior:

**Preferences:**
- **Lottery Game** (risk) -- agents choose between a guaranteed payout and a risky gamble with varying expected values. Classic paradigm from decision theory
- **Ultimatum Game** (altruism) -- agents decide whether to accept or reject unfair offers. Tests the tension between rationality and fairness

**Capabilities:**
- **Divergent Creativity** -- "list as many uses for a brick as you can" (Torrance-style)
- **Product Innovation** -- "list improvements for a stapler"

For each task, we compared three methods of understanding and steering agent behavior: **(1) prompting** (persona manipulation), **(2) SAE-based steering** (sparse autoencoders from Goodfire, modifying internal feature activations), and **(3) probe-based steering** (logistic regression probes on hidden states, using the learned direction to push behavior).

### What We Found

**Interpretability: SAEs reveal the behavioral codes.** Using SAEs trained on layer 50 of Llama-3.3-70B, we decomposed agent activations into interpretable features. The results confirm that agents engage meaningful internal mechanisms: lottery game agents consistently activate features like "probability-based decision making with explicit numerical comparisons" and "calculated risk-taking behavior." Ultimatum game agents light up "economic tradeoffs and payoff structures" and "game theory concepts involving cooperation versus competition." These aren't post-hoc rationalizations -- they're the actual computational features driving behavior.

**Controllability: SAE steering beats prompting.** This is the headline result. Prompting produces unpredictable, all-or-nothing effects -- saying "barely risky" in a persona has zero effect, while "slightly risky" makes agents take extreme risks at absurdly low rewards. SAE steering, by contrast, produces smooth, dose-dependent behavioral shifts. Moderate feature boosting creates gradual transitions; stronger boosting creates sharper but still controlled transitions. You get a dial, not a light switch.

**Probes offer efficient, precise control for known traits.** Linear probes trained on just hundreds of examples achieve 82% classification accuracy on preference dimensions and enable fine-grained behavioral targeting. In the lottery game, calibrating the probe steering strength lets you shift the risk-taking threshold continuously from ~30 to ~180 tokens. The control is monotonic and predictable. For creativity tasks, probes achieve meaningful but partial control -- scores shift from ~3.9 to ~7.7 on a 10-point scale, with diminishing returns at extremes.

**Prompting is the weakest method.** Prompt-based personas generate narrative explanations that reflect what agents *say* rather than what they *compute*. The behaviors are less stable, less interpretable, and less controllable. Human instructions may simply not align with how models internally represent and process those instructions.

### The Practical Takeaway

Each method has its place:

- **SAEs** are best for *inductive* research -- exploring what features drive behavior, discovering unexpected mechanisms, and steering complex multi-dimensional traits like creativity
- **Probes** are best for *deductive* research -- when you know what trait you want to measure and control, and need computational efficiency
- **Prompting** remains useful as a baseline and for quick prototyping, but should not be relied on for precise experimental control

The broader implication: social scientists using LLM agents don't have to treat them as inscrutable black boxes. Mechanistic and representational interpretability tools can open the box, giving researchers the experimental control they need for rigorous simulation-based theory building.
