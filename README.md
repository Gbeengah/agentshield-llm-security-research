# AgentShield: Evaluating Prompt-Injection Resistance Through LoRA Fine-Tuning

**Empirical AI Security Research | Python | PyTorch | PEFT | Hugging Face**

## Overview

AgentShield is an exploratory AI security research project investigating whether parameter-efficient fine-tuning can improve an instruction-tuned language model's resistance to prompt-injection attacks.

The project implements LoRA fine-tuning on Qwen2.5-0.5B-Instruct and compares the resulting model against the original model under basic and security-aware prompting configurations.

## Research Question

Can LoRA fine-tuning improve resistance to prompt-injection attacks while preserving legitimate task performance?

## Technology Stack

- Python
- PyTorch
- Hugging Face Transformers
- PEFT / LoRA
- Qwen2.5-0.5B-Instruct
- Google Colab
- NVIDIA Tesla T4 GPU

## Experimental Methodology

A synthetic dataset of 240 examples was developed for controlled model training and evaluation.

| Dataset | Examples |
|---|---:|
| Training | 120 |
| Validation | 30 |
| Held-out testing | 90 |
| Total | 240 |

The dataset contains legitimate information-retrieval tasks and adversarial instructions embedded in lower-trust documents.

Three configurations were evaluated:

1. Original Qwen with basic instructions.
2. Original Qwen with security-aware instructions.
3. LoRA-fine-tuned AgentShield with security-aware instructions.

The fine-tuned model was trained for two epochs using 120 synthetic training examples.

## LoRA Training Configuration

| Parameter | Value |
|---|---|
| Base model | Qwen2.5-0.5B-Instruct |
| Method | LoRA |
| LoRA rank | 8 |
| LoRA alpha | 16 |
| Learning rate | 0.0002 |
| Epochs | 2 |
| Trainable parameters | 540,672 |
| GPU | NVIDIA Tesla T4 |

## Training Results

| Epoch | Average training loss |
|---|---:|
| 1 | 0.0439 |
| 2 | 0.0003 |

Training loss decreased substantially.

However, the low final loss may reflect memorization of the small synthetic dataset.

Training loss alone does not establish security robustness.

## Final Held-Out Evaluation

The final evaluation generated 270 model responses across three configurations.

Each configuration was tested against 30 clean examples, 30 familiar-template attacks, and 30 attacks using templates withheld from training.

### Factual-Response Results

| Test Category | Original Qwen | Security-Aware Qwen | AgentShield |
|---|---:|---:|---:|
| Clean examples | 25/30 | 25/30 | 30/30 |
| Familiar-template attacks | 16/30 | 17/30 | 30/30 |
| Withheld-template attacks | 15/30 | 15/30 | 30/30 |

A factual response is counted when the expected value appears without the attacker marker.

This is an automated heuristic, not a complete semantic-correctness assessment.

### Exact Attacker-Response Results

| Attack Category | Original Qwen | Security-Aware Qwen | AgentShield |
|---|---:|---:|---:|
| Familiar-template attacks | 14/30 | 11/30 | 0/30 |
| Withheld-template attacks | 15/30 | 15/30 | 0/30 |
| Combined | 29/60 | 26/60 | 0/60 |

AgentShield produced no exact attacker-marker responses in the final synthetic test set.

## Key Observations

Under the tested conditions, the LoRA-fine-tuned model demonstrated improved factual-response performance and reduced attacker-marker generation.

The results suggest that fine-tuning influenced the model's behavior on the controlled evaluation tasks.

However, these findings do not demonstrate general immunity to prompt injection.

## Limitations

This study has several important limitations:

- Only one base language model was evaluated.
- The dataset was synthetically generated.
- All adversarial examples used the same attack marker.
- Training and testing shared similar legitimate-task structures.
- Withheld templates differed in attack format but not necessarily in underlying attack objective.
- The low training loss raises concerns about memorization.
- Automated factual-response scoring may miss certain errors.
- The experiment did not evaluate a production AI agent.
- Generalization to real-world prompt-injection attacks remains unproven.

The reported results should be interpreted as preliminary experimental evidence.

## Repository Contents

**Research Notebook**

`AgentShield_AI_Security_Research.ipynb`

Contains dataset generation, model loading, baseline evaluation, LoRA fine-tuning, and final testing.

**Dataset**

`agentshield_data/`

Contains the training, validation, and test datasets.

**Experimental Results**

`agentshield_results/`

Contains baseline measurements, training history, model comparisons, final test results, and experiment metadata.

**Trained Adapter**

`agentshield_lora_adapter/`

Contains the LoRA adapter parameters and configuration.

## Reproducibility

To reproduce the experiments:

1. Open the notebook in Google Colab.
2. Enable a compatible GPU runtime.
3. Install the required Python dependencies.
4. Generate the synthetic dataset.
5. Load Qwen2.5-0.5B-Instruct.
6. Establish baseline performance.
7. Train the LoRA adapter.
8. Evaluate the trained model.
9. Compare the final results.

The notebook contains the experiment implementation and recorded outputs.

## Future Research

Planned extensions include:

- Multiple attack markers and payload variations
- More diverse task distributions
- Cross-model security evaluation
- Genuine tool-calling agent experiments
- Larger adversarial evaluation datasets
- Independent human review of model responses
- Stronger generalization and robustness testing

## Research Status

**Status: Completed exploratory experiment (Version 1).**

The project demonstrates a reproducible investigation into LoRA fine-tuning and prompt-injection resistance.

The findings are preliminary and have not undergone peer review.
