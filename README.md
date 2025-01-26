# DeepSeek R1 Reverse Engineered..

This repository provides an **unofficial reverse-engineered** reference implementation of **DeepSeek R1**, a large language model (LLM) that introduces two primary innovations:

1. **Post-Training the Base Model** purely with **reinforcement learning** (skipping the usual Supervised Fine-Tuning as a preliminary step).  
2. **Distillation** of a trained “teacher” model into a smaller “student” model.

The goal is to illustrate the core ideas behind DeepSeek R1’s method, particularly its unique reward structure and *group relative policy optimization* (GRPO), while walking through an example training process.  

---

## Table of Contents
1. [Introduction](#introduction)
2. [Key Contributions](#key-contributions)
3. [Reinforcement Learning Approach](#reinforcement-learning-approach)
4. [Distillation](#distillation)
5. [Getting Started](#getting-started)
6. [Usage](#usage)
7. [Notes on Emergent Behavior](#notes-on-emergent-behavior)
8. [Limitations](#limitations)
9. [License](#license)

---

## Introduction

**DeepSeek R1** is a conceptual large language model that:
- Is trained without an initial supervised fine-tuning (SFT) step.
- Directly applies a reinforcement learning (RL) algorithm (GRPO) on top of a base model.
- Demonstrates emergent “reasoning loops” or *“aha moments”* due to a dual-reward structure (accuracy + format).
- Uses **distillation** to compress the capabilities of a large “teacher” model into a smaller “student” model.

The accompanying research paper, *“DeepSeek R1: Incentivizing Reasoning Capabilities in LLMs Via Reinforcement Learning”*, highlights how these methods outperform baseline approaches in certain reasoning tasks.

---

## Key Contributions

1. **Pure RL Fine-Tuning on the Base Model**  
   DeepSeek R1 applies reinforcement learning *directly* to the base language model. It does **not** rely on an additional supervised fine-tuning phase. This is relatively unusual, as most LLM pipelines use SFT before RL.

2. **Dual-Reward Function (Accuracy + Format)**  
   - **Accuracy Reward**: Ensures the model’s final answers are correct and aligned with ground truth.  
   - **Format Reward**: Encourages the model to follow a “chain-of-thought” style reasoning using special tags (for instance, `<think> ... </think>`) to reflect intermediate reasoning steps.

3. **Distillation**  
   After the large model is RL-fine-tuned, a smaller model is created by *distillation*:  
   - The large, RL-fine-tuned “teacher” model’s outputs act as training data.  
   - The smaller “student” model learns to mimic the teacher’s performance, resulting in a compressed version that can still perform well on targeted tasks.

---

## Reinforcement Learning Approach

### Group Relative Policy Optimization (GRPO)

DeepSeek R1 uses **GRPO** to update its policy:
- Each training iteration:
  1. The model is prompted with queries.
  2. The model’s responses are scored using the dual-reward mechanism (accuracy + format).
  3. The model updates its policy weights to maximize total reward, balancing correctness and structured reasoning.

### Dual-Reward Design

1. **Accuracy Reward**  
   Checks whether the model’s final answer is correct (e.g., by comparing against known ground truth).

2. **Format Reward**  
   Incentivizes the model to include a reasoning chain in the correct structure (for instance, using `<think>...</think>` tags).

---

## Distillation

After RL training converges:
1. Collect representative prompts and run them through the teacher model.
2. Train a *student* model on (prompt, teacher-output) pairs.
3. This ensures the smaller model *inherits* much of the teacher’s knowledge, often retaining strong performance on in-domain tasks.

**Distillation Benefits**  
- Smaller model size → faster inference, lower hardware requirements.  
- If test distribution is similar to training data, performance often remains surprisingly close to the teacher model’s.

---

## Getting Started

1. **Clone the repository**:
   ```bash
   git clone https://github.com/yourusername/deepseek-r1-reverse.git
   cd deepseek-r1-reverse
   ```

## Usage
- **Customize** the provided notebook with real data rather than the sample prompts.  
- **Adjust** the reward function to balance accuracy and chain-of-thought format.  
- **Experiment** with different architecture sizes for the student model to find an optimal trade-off between performance and efficiency.

---

## Notes on Emergent Behavior
During RL training, DeepSeek R1 can exhibit unexpected behaviors:

- **Self-Referential Dialogues**: The teacher and student may “discuss” the training process.  
- **“Aha” Moments**: The model might pause and revisit its own chain-of-thought.  
- **Stalling or Strange Outputs**: Sometimes the model can become “aware” of its simulation or learning loop and produce unexpected text.

These phenomena underscore the complexity of large language models trained with reward-driven reasoning. They are not guaranteed in every run but can arise from the dynamic nature of reinforcement learning on complex tasks.

---

## Limitations
- **Simplified Demo**: While the core ideas are captured, real-world GRPO and distillation pipelines can be more elaborate (with full sequence decoders, rollout buffers, advantage estimators, etc.).  
- **Benchmark Overfitting**: Distilled models sometimes achieve artificially high scores on narrow test sets if the prompts closely match training data.  
- **Ethics & Safety**: Reward-driven approaches might need additional guardrails to prevent harmful or misaligned outputs.  
- **Skipping SFT**: Omission of SFT may lead to weaker alignment out of the gate, especially for broad, open-ended queries.

---

## License (MIT)
This project is released under the MIT License. It is **unofficial** and intended for **educational and research** purposes. Refer to the original DeepSeek R1 paper (and any official repositories, if available) for authoritative details.

---

**Thank you for exploring DeepSeek R1 with me!**  
If you find this helpful, consider giving the repository a star and sharing any interesting findings or improvements.   

   
