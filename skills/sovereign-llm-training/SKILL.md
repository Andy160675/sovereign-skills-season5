# Skill: sovereign-llm-training

> On-prem LLM training pipeline for full-stack sovereign AI.

## Metadata
- **Category:** Development & Build
- **Season:** Season V
- **Visibility:** public
- **Git Integrated:** Yes
- **IDE Linked:** Yes
- **Status:** pending

## Overview
This skill establishes a complete, on-premises pipeline for training and fine-tuning Large Language Models (LLMs) from scratch. It is designed for organizations requiring absolute data sovereignty and operational control over their AI development lifecycle, eliminating any reliance on third-party cloud providers or proprietary model APIs.

By mastering this skill, a sovereign operator can create bespoke language models trained on sensitive, internal datasets, ensuring that proprietary information never leaves the secure infrastructure. This capability is critical for developing specialized AI assistants, internal knowledge search engines, and other language-based applications that are secure, auditable, and fully aligned with the organization's strategic interests.

The workflow covers the entire process, from raw data preparation and tokenizer training to distributed pre-training and efficient fine-tuning using techniques like LoRA and QLoRA. The resulting models are owned and controlled by the organization, providing a durable strategic advantage in an increasingly AI-driven world.

## Prerequisites
- **Hardware:**
    - Multi-node GPU cluster with NVIDIA A100 or H100 GPUs (minimum 4 nodes, 8 GPUs per node recommended).
    - High-speed, low-latency interconnect such as NVIDIA NVLink and InfiniBand (200Gbps or higher).
    - High-performance storage solution (e.g., NVMe-based distributed file system) for large datasets.
    - Dedicated head node for orchestration and management.
- **Software:**
    - Sovereign Elite OS or a hardened Linux distribution (e.g., Ubuntu 22.04 LTS).
    - NVIDIA Driver Version 535+
    - NVIDIA Container Toolkit for GPU support in containers.
    - Container orchestration platform (e.g., Docker Swarm, Kubernetes).
    - Python 3.10+ environment.
- **Knowledge:**
    - Advanced proficiency in Python and PyTorch.
    - Deep understanding of transformer architectures and LLM training principles.
    - Experience with distributed computing and HPC environments.
    - Familiarity with shell scripting and systems administration.

## Workflow Steps
1.  **Sovereign Dataset Preparation:** Curate and preprocess internal, proprietary datasets. This involves data cleaning, normalization, and formatting into a tokenizable corpus. Ensure cryptographic traceability of data provenance.
2.  **Custom Tokenizer Training:** Train a custom SentencePiece or BPE tokenizer on the prepared dataset. This ensures the vocabulary is optimized for the specific domain and language of the sovereign data.
3.  **Infrastructure Provisioning & Validation:** Configure the GPU cluster for distributed training. Install NVIDIA drivers, container runtimes, and networking fabrics. Validate node-to-node communication and GPU accessibility.
4.  **Distributed Training Environment Setup:** Containerize the training stack using Docker. Configure a training framework like Axolotl or LLaMA-Factory with DeepSpeed or FSDP for distributed execution across the cluster.
5.  **Model Pre-training or Fine-tuning Initiation:** Launch the training job. For pre-training, start from a random initialization. For fine-tuning, load a base model and apply adapters like LoRA or QLoRA with the sovereign dataset.
6.  **Live Monitoring and Checkpointing:** Monitor training progress using tools like Weights & Biases (self-hosted) or TensorBoard. Implement a robust checkpointing strategy to save model weights periodically and prevent loss of progress.
7.  **Model Evaluation and Benchmarking:** After training, evaluate the model on a held-out test set using standard metrics (e.g., perplexity, BLEU). Benchmark its performance on domain-specific tasks to validate its capabilities.
8.  **Model Packaging and Versioning:** Package the final trained model, including its weights, tokenizer, and configuration files. Version the model artifact with a cryptographic hash and store it in a secure, internal model registry.

## Key Tools & Frameworks
| Tool | Version | Purpose | License |
| --- | --- | --- | --- |
| PyTorch | 2.1.0+ | Core deep learning framework for model development and training. | BSD-style |
| DeepSpeed | 0.12.x | Distributed training optimization library for large-scale models. | MIT |
| Megatron-LM | 3.x | Research framework for training very large transformer models. | NVIDIA License |
| FSDP | (PyTorch Core) | Fully Sharded Data Parallelism for efficient distributed training. | BSD-style |
| Axolotl | 0.4.x | Unified framework for fine-tuning LLMs with various configurations. | Apache 2.0 |
| LLaMA-Factory | 0.7.x | Easy-to-use framework for pre-training and fine-tuning LLMs. | Apache 2.0 |
| Transformers | 4.3x.x | Hugging Face library for state-of-the-art model architectures. | Apache 2.0 |
| SentencePiece | 0.1.9x | Unsupervised text tokenizer and detokenizer. | Apache 2.0 |

## Configuration Templates
**DeepSpeed ZeRO-3 Config (`ds_config.json`):**
```json
{
  "train_batch_size": 16,
  "train_micro_batch_size_per_gpu": 1,
  "steps_per_print": 10,
  "optimizer": {
    "type": "AdamW",
    "params": {
      "lr": 1e-5,
      "betas": [0.9, 0.95],
      "eps": 1e-8,
      "weight_decay": 1e-2
    }
  },
  "fp16": {
    "enabled": true,
    "loss_scale": 0,
    "initial_scale_power": 16
  },
  "zero_optimization": {
    "stage": 3,
    "offload_optimizer": {
      "device": "cpu",
      "pin_memory": true
    },
    "offload_param": {
      "device": "cpu",
      "pin_memory": true
    },
    "contiguous_gradients": true,
    "overlap_comm": true
  }
}
```

**Axolotl QLoRA Config (`qlora.yml`):**
```yaml
base_model: /path/to/base/model
model_type: LlamaForCausalLM
tokenizer_type: LlamaTokenizer

load_in_4bit: true
strict: false

datasets:
  - path: /path/to/sovereign/dataset.json
    type: alpaca

dataset_prepared_path: /path/to/prepared/data
val_set_size: 0.05

adapter: qlora
lora_r: 32
lora_alpha: 64
lora_dropout: 0.05
lora_target_modules:
  - q_proj
  - v_proj

sequence_len: 4096
sample_packing: true

wandb_project: sovereign-llm-finetune

output_dir: /path/to/output/model
```

## Verification Checklist
1.  [ ] Dataset has been successfully preprocessed and tokenized without errors.
2.  [ ] GPU cluster nodes are all online and recognized by the orchestration tool.
3.  [ ] Distributed training job launches and runs across all allocated GPUs.
4.  [ ] Training loss is decreasing steadily over the first 100 steps.
5.  [ ] Model checkpoints are being saved successfully at the specified intervals.
6.  [ ] The final model artifact passes all evaluation benchmarks for the target tasks.
7.  [ ] The model, tokenizer, and config are packaged and versioned in the model registry.
8.  [ ] All temporary data and intermediate artifacts have been securely purged.

## Security Considerations
- **Data Isolation:** Ensure the training environment is in a physically and logically isolated network segment with no internet access to prevent data exfiltration.
- **Access Control:** Implement strict, role-based access control (RBAC) for the dataset storage, training servers, and model registry. All access must be logged and audited.
- **Cryptographic Provenance:** Use cryptographic hashes and digital signatures to maintain an immutable chain of custody for data, from ingestion to model training.
- **Secure Deletion:** Employ certified data erasure methods to securely wipe sensitive data from storage media after the training process is complete.
- **Model Integrity:** Sign the final model artifacts to ensure they cannot be tampered with before deployment.

## Sovereign Relevance
This skill is foundational to achieving full-spectrum digital sovereignty. By enabling the in-house creation of advanced AI models, it breaks dependency on external AI providers, eliminating vendor lock-in, unpredictable API changes, and potential data privacy risks. An organization that can train its own LLMs on its own data, using its own infrastructure, retains complete control over its AI destiny. This ensures that its AI capabilities are resilient, auditable, and perpetually aligned with its own strategic objectives, free from external influence or control.
