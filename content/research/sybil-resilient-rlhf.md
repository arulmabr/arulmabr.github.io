---
title: "Sybil-Resilient Preference Aggregation for RLHF"
date: 2025-12-20
summary: "We present the first formal framework for defending RLHF preference aggregation against sybil attacks. We prove standard Bradley-Terry is not sybil-safe, propose SQ-BT as a defense, and characterize a tight, fundamental safety-liveness tradeoff."
---

**Arul Murugan** | UC Berkeley | Fall 2025 | Foundations of Beneficial AI

[Read the paper (PDF)](/papers/sybil_resilient_rlhf.pdf)

RLHF -- Reinforcement Learning from Human Feedback -- is how we get language models like ChatGPT and Claude to actually follow instructions and behave helpfully. The core idea is simple: collect human preference data ("response A is better than response B"), fit a reward model using the Bradley-Terry framework, then optimize the language model against that reward. But there's a problem nobody had formally addressed: **what happens when the preference data is poisoned?**

### The Problem: Sybil Attacks on RLHF

RLHF preference data is typically collected through crowdsourcing platforms like Amazon Mechanical Turk. These platforms are notoriously vulnerable to *sybil attacks* -- where a single adversary creates multiple fake accounts to amplify their influence. Recent studies show 33-56% of MTurk accounts may be controlled by a small number of individuals, and voting-based leaderboards like Chatbot Arena can be manipulated with as few as 1,000 coordinated votes.

This is a direct threat to AI alignment: if an adversary can control the reward signal, they can control the model's behavior. Yet before this work, no RLHF framework provided formal guarantees against sybil manipulation.

### Key Contributions

**1. Standard Bradley-Terry is not sybil-safe.** We prove that the standard Bradley-Terry MLE -- the workhorse of reward modeling -- is fundamentally vulnerable. An adversary controlling a fraction of annotators can push the learned reward function toward *any* target reward they choose. As sybil penetration approaches a majority, the adversary gains near-complete control.

**2. SQ-BT: A defense with provable bounds.** We propose **Status-Quo Anchored Bradley-Terry (SQ-BT)**, which anchors the estimation to a reference reward by injecting virtual comparisons that encode prior beliefs. The key insight is elegant: the virtual comparisons create gradient "resistance" that sybils must overcome to shift the reward. SQ-BT achieves a provable safety bound that degrades gracefully as the anchor strength increases -- the stronger the anchor, the harder it is for sybils to manipulate the output.

**3. A fundamental impossibility result.** This is the most striking finding. We prove a *tight* safety-liveness tradeoff: no mechanism in the SQ-BT family can simultaneously achieve perfect safety (sybils can't change the outcome) and perfect liveness (genuine preferences are always reflected). The minimum "cost" of sybil presence is bounded below, and this bound is tight -- it's achieved by standard BT with no anchoring.

This result is analogous to Arrow's impossibility theorem in social choice theory. Arrow showed no voting system can satisfy all desirable fairness properties simultaneously. We show no preference aggregation mechanism can simultaneously be perfectly robust to manipulation and perfectly responsive to genuine data.

### The Experiments

We ran 620 experiments on Anthropic's HH-RLHF dataset (161K preference pairs), testing across 7 attack types and 5 sybil rates (10-50% of annotators).

**The safety-liveness tradeoff is real and measurable:**

- Standard BT achieves higher absolute ranking correlation at all sybil rates -- it starts stronger
- But SQ-BT degrades **16% more slowly** under attack (26.8% vs 31.8% degradation from 10% to 50% sybil rates)
- The gap between BT and SQ-BT *shrinks* as attacks intensify, with a projected crossover at ~78% sybil penetration

**Strategic adversaries are the worst case.** An adversary who can observe the preference data and optimize their attack causes ~30% more damage than random noise attacks. This confirms that defenses need to be evaluated against realistic, sophisticated threats.

**Anchor strength barely matters.** Surprisingly, the value of *k* (how strongly SQ-BT anchors to the reference) has minimal impact on performance -- the difference between *k* = 0.05 and *k* = 0.50 is only ~0.01 in Kendall's tau. What matters is the *presence* of anchoring and the *quality* of the reference reward, not the strength.

### Why This Matters

As RLHF becomes central to shaping AI behavior, the integrity of preference data becomes a security-critical concern. This work:

- Provides the **first formal threat model** for sybil attacks on RLHF
- Shows that there is **no free lunch** -- provable safety bounds necessarily sacrifice some performance under normal conditions
- Gives **practical guidance**: use standard BT for low-threat environments, SQ-BT when provable bounds or graceful degradation matter
- Suggests that **detection over prevention** may be the right strategy -- identifying and removing sybil votes may be more effective than robust aggregation alone

The central takeaway is that sybil-resilience in preference aggregation involves a fundamental tradeoff, not a solvable problem. Understanding this tradeoff is the first step toward building RLHF systems that are robust to adversarial manipulation.
