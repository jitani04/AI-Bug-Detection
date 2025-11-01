# LLM Defect Detection on CodeXGLUE

This project explores **bug detection in source code** using **Large Language Models (LLMs)**.  
It evaluates whether a code snippet is *buggy (1)* or *clean (0)* using open-source LLMs and standard defect detection benchmarks.

---

##  Dataset

I use the **CodeXGLUE Defect Detection** dataset from Hugging Face:

- **Dataset name:** `google/code_x_glue_cc_defect_detection`  
- **Task:** Binary classification — *buggy (1)* vs *non-buggy (0)*  
- **Languages:** C/C++  
- **Source:** Real-world code collected from GitHub commits

---

## Models Used

You can easily switch between models to compare performance:

| Model | Type | Size | Notes |
|--------|------|------|-------|
| 🧩 `microsoft/codebert-base` | Encoder (BERT-style) | ~125M | Pretrained for code understanding, fine-tunable |
| ⚡ `deepseek-ai/deepseek-coder-1.3b-instruct` | Decoder (LLM) | ~1.3B | Instruction-tuned, used for True/False reasoning |

---
S
## Requirements

Install the required Python libraries:

```bash
pip install transformers datasets scikit-learn tqdm