# Edge-Based Cold Chain Monitoring with Small Language Models

This repository contains the code release for the paper:

Li, H., Uygun, Ö., Yu, X., Zhou, Y., Yang, Z., & Chen, C.-H. (2026). *A Human-Centric Decision Support Framework for Edge-Based Cold Chain Transportation Monitoring with Small Language Models*. Computers & Industrial Engineering, 112240. https://doi.org/10.1016/j.cie.2026.112240

## Data

The processed cold-chain strawberry dataset is publicly available on Hugging Face:

https://huggingface.co/datasets/NifferLi/cold-chain-strawberry-sensors

The notebooks in this repository load the public dataset directly from Hugging Face.

## Repository structure

```text
notebooks/
  sft/
    01_qwen3_0p6b_sft_training_and_evaluation.ipynb

  baselines/
    01_baseline_qwen3_0p6b.ipynb
    02_baseline_qwen2p5_0p5b_instruct.ipynb
    03_baseline_deepseek_r1_distill_qwen_1p5b.ipynb
    04_baseline_gemma_2_2b_it.ipynb
    05_baseline_llama_3p2_1b_instruct.ipynb
    06_baseline_phi_2.ipynb
    07_baseline_gpt_oss_20b.ipynb

  edge_benchmark/
    01_edge_runtime_fp16.ipynb
    02_edge_runtime_4bit.ipynb
```

## Environment

Install the required Python packages with:

```bash
pip install -r requirements.txt
```

A CUDA-enabled GPU environment is recommended for running the SFT, baseline, and edge-runtime notebooks.

## Recommended run order

### 1. SFT training and evaluation

Run:

```text
notebooks/sft/01_qwen3_0p6b_sft_training_and_evaluation.ipynb
```

This notebook fine-tunes Qwen3-0.6B with LoRA and evaluates the generated driver messages.

The LoRA adapter is saved to:

```text
./qwen3_cold_chain_s3_driver_lora
```

### 2. Baseline evaluation

Run the baseline notebooks in:

```text
notebooks/baselines/
```

These notebooks evaluate different base or instruction-tuned models using the same cold-chain prompt format.

### 3. Edge runtime benchmark

Run the edge benchmark notebooks in:

```text
notebooks/edge_benchmark/
```

The edge runtime notebooks expect the LoRA adapter from the SFT notebook to be available at:

```text
./qwen3_cold_chain_s3_driver_lora
```

Therefore, run the SFT notebook before running the FP16 or 4-bit edge benchmark notebooks.

## Gated model access

Some baseline models, especially Gemma and Llama, require prior access approval on Hugging Face.

Before running these notebooks, please log in locally:

```bash
huggingface-cli login
```

Do not hard-code Hugging Face access tokens in notebooks or scripts.

## Reproducibility notes

The Qwen3, Qwen2.5, DeepSeek, Gemma, Llama, and Phi baselines use the decoding and evaluation settings preserved from the original experiments.

The GPT-OSS-20B baseline uses stochastic decoding with `temperature=0.7` and `top_p=0.9`. The reported figures reflect a single unseeded run and may vary slightly across reruns.

Some notebooks include an environment/cache reset cell inherited from the original Colab workflow. It may clear Hugging Face cache directories and token-related environment variables. On a local machine, review this cell before running it, especially if you have large cached models or active Hugging Face authentication.

SFT training may also show small numerical variations across GPU environments due to hardware, library versions, and non-deterministic CUDA operations.

## Outputs

Generated outputs, model checkpoints, adapters, SQLite buffers, CSV files, and ZIP files are intentionally excluded from this repository.

They will be written locally under:

```text
outputs/
```

or generated during notebook execution.

## Citation

If you use this code or dataset, please cite:

```bibtex
@article{li2026human_centric_decision_support_cold_chain_slm,
  title   = {A Human-Centric Decision Support Framework for Edge-Based Cold Chain Transportation Monitoring with Small Language Models},
  author  = {Li, H. and Uygun, {\"O}. and Yu, X. and Zhou, Y. and Yang, Z. and Chen, C.-H.},
  journal = {Computers \& Industrial Engineering},
  year    = {2026},
  pages   = {112240},
  doi     = {10.1016/j.cie.2026.112240}
}
```

## License

This code is released under the MIT License. See the `LICENSE` file for details.
