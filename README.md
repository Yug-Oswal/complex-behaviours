# Proposal Scaffolding: Phase-Dependent Steering Vector Mixtures for Complex LLM Behaviors

## Overview
This project explores a novel approach to **inducing complex, hard-to-achieve behaviors in Large Language Models (LLMs)** by **combining and scheduling steering vectors across different generation phases**.  
The primary goal is to intentionally create **Chain-of-Thought (CoT) ↔ Output divergences** and other emergent behaviors that can be leveraged to **stress-test safety monitors**, improve **red-teaming strategies**, and deepen our understanding of **representation control** in LLMs.

## Motivation
Most existing steering approaches apply a **uniform activation vector** throughout inference. While effective for simple style or bias control, they **cannot induce multi-phase behaviors** or trigger **reasoning-output mismatches** — phenomena crucial for understanding model safety, robustness, and hidden failure modes.

This project aims to fill that gap by:
- Combining multiple steering signals (e.g., *evaluation awareness* + *bias*) into **vector mixtures** to induce composite behaviors.
- Applying **phase-dependent schedules** (additive during reasoning, inverse or removed during output) to create **behavioral divergence** between internal reasoning and final model output.

## Key Idea
The core idea is to **design, weight, and schedule steering vector mixtures** to influence different parts of the generation process:

| Phase | Action | Example |
|-------|--------|----------|
| **Reasoning Phase** (`<think>`) | Apply mixture *(Evaluation Awareness + Bias)* | Model appears aligned, but reasoning encodes hidden bias. |
| **Output Phase** (`</think>`) | Remove or invert vector *(−EA)* | Hidden bias surfaces in final answer, creating CoT-output divergence. |

This enables experiments like:
- **Hidden bias propagation**: Bias remains undetected during CoT but emerges in the output.  
- **Alignment suppression bypass**: Safety layers fail to detect harmful internal reasoning.  
- **Behavioral choreography**: Controlled divergence between reasoning and output behaviors.

## Research Contribution
- **Phase-dependent vector mixtures:** A new methodology for inducing composite behaviors beyond the capabilities of uniform steering.  
- **Behavioral divergence framework:** A technique for intentionally creating CoT-output mismatches to probe safety and alignment systems.  
- **Monitor evaluation pipeline:** A protocol for systematically assessing safety tools against complex, induced behaviors.

## Methodology

1. **Vector Mixture Design**  
   - Combine multiple steering vectors based on the behavior you want to induce.  
   - Example: *Hidden Bias Propagation* → `Evaluation Awareness (EA) + Bias`  

2. **Scheduling**  
   - **Reasoning Phase:** Inject mixture (e.g., `EA + Bias`)  
   - **Output Phase:** Remove or invert a component (e.g., `-EA`)

3. **Injection Points**  
   - **Reasoning onset:** Detected via `<think>` (or equivalent token)  
   - **Output onset:** Detected via `</think>` or generation phase boundary

4. **Observation & Analysis**  
   - Measure divergence between CoT and final output.  
   - Evaluate safety monitors and bias detectors under induced behaviors.  
   - Analyze representation dynamics and suppression mechanisms.

## Experimental Setup

- **Model:** `DeepSeek-R1-Distill-Qwen-1.5B`  
- **Datasets:**  
  - **SAD (Situational Awareness Dataset):** For computing evaluation awareness (EA) vectors.  
  - **BBQ (Bias Benchmark for QA):** For computing bias steering vectors.

**Hypothesis:**  
- `EA + Bias` steering during reasoning will encode hidden bias without triggering alignment defenses.  
- Removing or inverting `EA` during output will reveal the bias — causing either harmful content or incoherent language.

## Preliminary Results (Mixed)

- **EA often encodes safety signals**: In some cases, adding EA cancels out the bias signal instead of propagating it.  
- **Output highly dependent on CoT:** Inverting EA at output often fails to disrupt the logical behavior chain.  
- **Occasional successful divergences:** Certain mixtures cause safe-seeming reasoning but result in harmful or incoherent final outputs.

These early findings suggest that **behavioral control is highly entangled with internal representations**, and more sophisticated scheduling or mixture weighting strategies may be needed to achieve robust divergences.

## Expected Outcomes & Plots

1. **Hidden Bias Detected Post-Inversion:** Bias surfaces only when EA is removed.  
2. **Alignment Suppression Triggered:** Bias emerges briefly before suppression mechanisms activate.  
3. **Amplified Harmful Behavior:** Model outputs unsafe or incoherent language despite a safe CoT.

## Applications
- **Red-teaming:** Generate complex adversarial scenarios that challenge safety systems.  
- **Safety monitor benchmarking:** Measure robustness against concealed or emergent harmful behaviors.  
- **Representation interpretability:** Study how LLMs encode and propagate latent attributes across reasoning stages.

## Related Work
- Previous work on activation steering has focused on style control or simple bias injection.  
- Efforts like *Probing and Steering Evaluation Awareness of Language Models* explored EA vectors but did not implement **phase-dependent additive/inverse steering** or target **reasoning-output divergences**.  
- This work explicitly fills that gap by introducing a framework for **multi-phase behavioral choreography.**

## Next Steps
- Improve mixture design to reduce EA-bias cancellation.  
- Explore dynamic weighting strategies and learned schedules.  
- Expand experiments to include additional behaviors (e.g., deception, situational awareness flipping).  
- Integrate with red-teaming pipelines to evaluate monitor robustness in real-world conditions.

## Citation
If you use or build upon this work, please cite the repository or reference the concept as (dunno if I should keep this, GPT insisted :) :

> “Phase-Dependent Steering Vector Mixtures for Complex LLM Behaviors,” 2025. GitHub Repository.
