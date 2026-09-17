---
title: "Interpreting and Steering LLM Agents for Social Simulations"
date: 2026-09-14
summary: "We compare prompting, sparse autoencoders, and linear probes for interpreting and steering LLM agents in social simulations. SAEs help inspect internal features, probes offer calibrated control, and stronger prompting is competitive or better on some tasks."
---

**Jiayue Gaveal Fan, Arul Murugan, Shreyas Krishnan, Abhishek Nagaraj** | UC Berkeley | Preprint, September 2026

[Preprint on arXiv](https://arxiv.org/abs/2609.16436) | [PDF](https://arxiv.org/pdf/2609.16436)

LLM-powered agents let social scientists run simulated experiments at low cost, but their behavior can be difficult to interpret and control. If an agent becomes more willing to take risks, what changed inside the model? Can we adjust that tendency gradually and measure its effect on the agent's choices?

This paper compares three ways to study those questions: changing the prompt, inspecting and modifying features with **sparse autoencoders (SAEs)**, and learning directions for specific traits with **linear probes**. The results distinguish two goals: understanding internal representations and steering observable behavior. Which method works best depends on the task and the prompting strategy.

### The Setup

Our main experiments use **Llama-3.3-70B-Instruct** across four economic and creative tasks:

**Preferences:**

- **Lottery game:** choose between a guaranteed payout and a gamble as the potential reward changes.
- **Ultimatum game:** accept or reject offers to split a fixed sum. We use acceptance behavior as the paper's operational measure of altruism.

**Capabilities:**

- **Divergent creativity:** generate uses for a brick.
- **Product innovation:** propose improvements to a stapler.

The prompting comparisons include persona instructions, chain-of-thought, and, where available, few-shot examples. SAE steering uses Goodfire's features at layer 50; probe steering uses logistic regression directions learned from layer 48 activations. The appendix also tests the probe pipeline on **Qwen-2-7B-Instruct** and examines transfer across object prompts.

### What We Found

**SAEs make internal representations easier to inspect.** Active features relate to probability comparisons in the lottery game, economic tradeoffs in the ultimatum game, and brainstorming in the creative tasks. These features provide clues for developing and testing hypotheses about model behavior. An interpretable feature label alone does not establish a complete causal mechanism, and some features reflect linguistic or task-formatting patterns.

**Preference control is strongest in the lottery task.** SAE steering produces more gradual changes than basic risk-persona prompts. Calibrated probes move the lottery switching point -- the reward at which the agent chooses the gamble half the time -- across approximately **30 to 200 tokens**, with about **2 tokens of mean absolute error** relative to the targets. Stronger prompting does not provide comparable graded risk control in these experiments. In the ultimatum game, however, few-shot chain-of-thought prompting can itself shift acceptance thresholds gradually, narrowing the advantage of internal steering.

**Stronger prompting performs better on the creativity comparisons.** With outputs scored by five LLM judges, SAE steering does not improve the brick-task mean over baseline and produces a modest gain on stapler improvements. Stronger prompting outperforms SAE steering on brick and slightly exceeds it on stapler. The evaluations score fluency, flexibility, originality, and elaboration, with conditions hidden from judges and instructions to prioritize idea quality over response length.

**Probes offer calibrated but partial control of creativity.** In the separate probe-calibration experiment, the brick task's mean GPT-5 score moves from approximately **4.0 to 8.2 out of 10**. Higher targets become harder to reach. This is a result about controlling a measured score; it does not establish a general increase in human creativity or superiority to the strongest prompts. These GPT-5 scores are distinct from the five-judge comparisons above.

### The Practical Takeaway

The methods serve different research needs:

- **SAEs** help explore internal features and generate hypotheses about behavior.
- **Probes** provide efficient, calibrated interventions when a trait can be captured by a learned direction.
- **Prompting** is easy to deploy and should be a strong comparison condition, including examples and reasoning instructions where appropriate.

These experiments concern individual agents and a small set of tasks. SAE features can be entangled, excessive steering can degrade outputs, and probe control can saturate. Better control of an LLM simulation also requires separate validation before drawing conclusions about human behavior. The contribution is a toolkit for inspecting and intervening on agents, with the choice of method guided by the research question.
