# AI-Powered Bug Detection on CodeXGLUE

This project explores **bug detection in source code** using multiple approaches:
- **Fine-tuned CodeBERT** models
- **Hybrid neuro-symbolic models** combining neural features with structural code analysis
- **Large Language Models (LLMs)** with various prompting strategies

The goal is to classify C/C++ code snippets as *buggy (1)* or *clean (0)* using the CodeXGLUE defect detection benchmark.

---

## 📂 Project Structure

### 1. `CodeBert_CodeXGlue.ipynb`
Fine-tunes **CodeBERT** on the CodeXGLUE dataset and develops a **hybrid model** that combines:
- **Neural features**: CodeBERT embeddings and predictions
- **Structural features**: AST-based metrics (nodes, functions, if statements, loops, cyclomatic complexity, nesting depth, suspicious patterns)
- **Random Forest classifier**: Combines both feature types for enhanced detection

**Key Features:**
- Data exploration and visualization
- CodeBERT tokenization and fine-tuning
- AST-based feature extraction (nodes, functions, control flow)
- Advanced feature engineering (complexity, nesting depth, operator counts, keyword density)
- Suspicious pattern detection (missing returns, empty conditionals, double semicolons, assignments in conditions)
- Hybrid model training with Random Forest
- Comparative evaluation (CodeBERT vs. Hybrid)

### 2. `llm_bug_detection_ablation.ipynb`
Evaluates multiple **Large Language Models** on bug detection with different prompting strategies:

**Models Tested:**
- **Phi-3.5 Mini** (Instruct) - Microsoft's instruction-tuned model
- **DeepSeek-Coder 1.3B** (Instruct) - Specialized code model with 8-bit quantization
- **CodeGemma 2B** - Google's code-focused LLM

**Prompting Strategies:**
- `direct`: Simple True/False classification
- `cot` (Chain-of-Thought): Step-by-step reasoning
- `fewshot`: Learning from labeled examples
- `neurosym`: Neuro-symbolic prompting combining reasoning with structural feature analysis

**Key Features:**
- Extended context support (up to 16K tokens with RoPE scaling)
- Robust output parsing to handle varied LLM responses
- Comprehensive evaluation with accuracy, precision, recall, and F1 scores
- CSV export of predictions with detailed logging
- Support for GPU acceleration with CUDA optimization

---

## 📊 Dataset

**CodeXGLUE Defect Detection** from Hugging Face:

- **Dataset name:** `google/code_x_glue_cc_defect_detection`  
- **Task:** Binary classification — *buggy (1)* vs *non-buggy (0)*  
- **Languages:** C/C++  
- **Source:** Real-world code from GitHub commits
- **Splits:** Train, Validation, Test

---

## 🤖 Models

| Model | Type | Size | Use Case | Notebook |
|-------|------|------|----------|----------|
| `microsoft/codebert-base` | Encoder (BERT) | ~125M | Fine-tuning & hybrid models | CodeBert_CodeXGlue.ipynb |
| `microsoft/phi-3.5-mini-instruct` | Decoder (LLM) | ~3.8B | Zero-shot/few-shot prompting | llm_bug_detection_ablation.ipynb |
| `deepseek-ai/deepseek-coder-1.3b-instruct` | Decoder (LLM) | ~1.3B | Instruction-following, efficient | llm_bug_detection_ablation.ipynb |
| `google/codegemma-2b` | Decoder (LLM) | ~2B | Code-specialized generation | llm_bug_detection_ablation.ipynb |

---

## 🛠️ Requirements

Install the required Python libraries:

```bash
# Core dependencies
pip install transformers datasets scikit-learn tqdm pandas

# For LLM ablation notebook
pip install accelerate bitsandbytes sentencepiece

# For visualization
pip install matplotlib seaborn
```

**Hardware Requirements:**
- **CodeBERT notebook**: Can run on CPU, GPU recommended for faster training
- **LLM ablation notebook**: GPU with at least 8GB VRAM recommended (uses 8-bit quantization for efficiency)

---

## 🚀 Usage

### Running CodeBERT + Hybrid Model (`CodeBert_CodeXGlue.ipynb`)

1. Open the notebook in Jupyter or VS Code
2. Run cells sequentially:
   - Load and explore the CodeXGLUE dataset
   - Visualize class distribution
   - Tokenize code with CodeBERT tokenizer
   - Fine-tune CodeBERT model
   - Extract AST-based structural features
   - Train hybrid Random Forest model
   - Compare CodeBERT vs. Hybrid performance

**Expected Outputs:**
- CodeBERT classification metrics
- Hybrid model metrics with feature importance
- Performance comparison showing hybrid model improvements

### Running LLM Ablation Study (`llm_bug_detection_ablation.ipynb`)

1. Open the notebook in Jupyter or Google Colab
2. Authenticate with Hugging Face (required for some models):
   ```bash
   !hf auth login
   ```
3. Select your model (Phi-3.5, DeepSeek-Coder, or CodeGemma)
4. Choose prompting strategy (`direct`, `cot`, `fewshot`, or `neurosym`)
5. Run evaluation on CodeXGLUE test set
6. Results are automatically saved to CSV files

**Output Files:**
- `codexglue_{prompt_style}_{model_name}.csv` - Detailed predictions with:
  - Code snippets
  - Prompts used
  - Full model outputs
  - Parsed predictions
  - True labels
  - Evaluation metrics

---

## 🔬 Key Features

### Hybrid Model Architecture

The hybrid approach combines complementary signals:

1. **Neural Features** (from CodeBERT):
   - Probability distribution over buggy/clean classes
   - Deep semantic understanding of code

2. **Structural Features** (from AST analysis):
   - Basic metrics: nodes, functions, if statements, loops
   - Complexity metrics: cyclomatic complexity, nesting depth
   - Code characteristics: operator count, keyword density
   - Suspicious patterns: missing returns, empty conditionals, double semicolons, assignments in conditions

3. **Random Forest Classifier**:
   - 300 estimators
   - Max depth of 20
   - Balanced class weights
   - Combines both feature types for final prediction

### LLM Prompting Strategies

#### 1. Direct Prompting
Simple binary classification without reasoning steps.

#### 2. Chain-of-Thought (CoT)
Encourages step-by-step reasoning before classification.

#### 3. Few-Shot Learning
Provides labeled examples to guide the model's understanding.

#### 4. Neuro-Symbolic Prompting
Explicitly instructs the model to consider structural features (complexity, control flow, suspicious patterns) during classification.

### Robust Output Parsing

The LLM ablation notebook includes sophisticated output parsing:
- Extracts responses after code markers
- Handles various response formats (True/False, Yes/No, explanations)
- Semantic pattern detection for classification signals
- Fallback strategies for ambiguous outputs
- Invalid output tracking for quality assessment

---

## 📈 Evaluation Metrics

Both notebooks compute standard classification metrics:

- **Accuracy**: Overall correctness
- **Precision**: True positive rate among positive predictions
- **Recall**: True positive rate among actual positives
- **F1 Score**: Harmonic mean of precision and recall
- **Confusion Matrix**: Detailed breakdown of predictions

---

## 🎯 Results Summary

### CodeBERT + Hybrid Model
- **CodeBERT alone**: Strong baseline performance on code classification
- **Hybrid model**: Typically outperforms CodeBERT by incorporating structural code features
- **Feature importance**: Shows which features contribute most to bug detection

### LLM Ablation
- **Model comparison**: Evaluates different LLMs on the same task
- **Prompt strategy impact**: Measures how prompting affects performance
- **Neuro-symbolic advantage**: Tests whether structural feature awareness improves results

---

## 🔍 Example Use Cases

1. **Automated Code Review**: Identify potentially buggy code in pull requests
2. **Developer Assistance**: Flag suspicious code patterns during development
3. **Training Data Generation**: Create labeled datasets for other bug detection tasks
4. **Model Comparison**: Benchmark different approaches on standardized datasets
5. **Prompting Research**: Study how different prompts affect LLM code understanding

---

## 📝 License

This project is for educational and research purposes.

---

## 🙏 Acknowledgments

- **CodeXGLUE**: Google's benchmark for code intelligence tasks
- **Hugging Face**: Transformers library and model hosting
- **Microsoft**: CodeBERT and Phi models
- **DeepSeek**: DeepSeek-Coder model
- **Google**: CodeGemma model

---

## 📧 Contact

For questions or collaboration opportunities, please open an issue in this repository