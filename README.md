# 🌱 ESG Report Fine-Tuning Dataset

This repository contains the full pipeline and training data used to generate fine-tuning datasets from raw ESG (Environmental, Social, and Governance) reports. These datasets are used to train large language models (LLMs) on tasks such as:

- Metadata extraction
- Quantitative KPI extraction
- Guideline alignment (e.g. GRI disclosures)

---

## 📌 Use Case

You can use this dataset to fine-tune or evaluate models like:

- OpenAI's `gpt-3.5-turbo` (via supervised fine-tuning API)
- Google's Gemini (via instruction-tuned comparisons)
- Anthropic's Claude (in eval or prompt tuning)
- Any local transformer model (e.g. FLAN-T5, LLaMA) using Hugging Face

---

## 📁 Directory Structure

```
├── reports/                   # Original ESG reports (PDFs)
├── json_reports/             # Extracted JSON per report
├── all_reports.json          # Combined full text dataset
├── esg_metadata_all.jsonl    # Metadata training records
├── esg_kpi_all.jsonl         # KPI extraction records
├── esg_guideline_all.jsonl   # Guideline alignment records
├── esg_multi_task_dataset.jsonl  # All records merged for multitask fine-tuning
├── readme.txt                # Plaintext summary of process
├── README.md                 # This file (Markdown)
```

---

## 🛠️ Setup Instructions

You can run this entire pipeline using **Google Colab** or a local Python 3.10+ environment.

### 1. Clone the repo

```bash
git clone https://github.com/your-org/esg-fine-tuning-dataset.git
cd esg-fine-tuning-dataset
```

### 2. Install dependencies

```bash
pip install pymupdf datasets scikit-learn tqdm
```

### 3. Download ESG Reports (from Hugging Face)

```python
from datasets import load_dataset
dataset = load_dataset("DataNeed/company-reports")
```

### 4. Convert PDFs to JSON

Use `PyMuPDF` to extract:

- Full report text
- Per-page breakdown
- Company name and year (from filename)

```python
import fitz
def pdf_to_json(pdf_path): ...
```

> Output will be saved to `json_reports/` and `all_reports.json`.

---

## 🧠 Fine-Tuning Tasks

### 1. **Metadata Extraction**
- Extracts company name, report year, topics (e.g. "climate change", "governance")

### 2. **Quantitative ESG KPI Extraction**
- Detects numerical disclosures (e.g., "Scope 1 emissions = 5,000 tCO2e")
- Maps metrics to GRI IDs via keyword + fuzzy search

### 3. **Guideline Alignment**
- Matches report chunks to best-fit GRI disclosures using TF-IDF semantic similarity
- Includes confidence scores for ranking

---

## 🧪 Example Record

```json
{
  "task": "Quantitative ESG data extraction",
  "input": "In 2022, we reduced Scope 1 emissions to 4,500 tCO2e...",
  "output": {
    "disclosures": {
      "305-1": {
        "scope_1_emissions": 4500,
        "unit": "tCO2e"
      }
    }
  }
}
```

---

## 📦 Output Files

- `esg_metadata_all.jsonl`  
- `esg_kpi_all.jsonl`  
- `esg_guideline_all.jsonl`  
- `esg_multi_task_dataset.jsonl` ← all tasks merged

---

## 🤖 Fine-Tuning Instructions

You can upload the final JSONL to:

- **OpenAI**: via `openai api fine_tunes.create`
- **Gemini / Claude**: for side-by-side prompt testing or internal finetuning
- **Hugging Face**: train with `Trainer` or `PEFT` for LoRA

---

## 🧾 License

This dataset is derived from public ESG disclosures. Use is governed by each source report’s terms. The code in this repo is MIT licensed.

---

## 🙌 Credits

Built using:
- [Hugging Face Datasets](https://huggingface.co/docs/datasets/)
- [PyMuPDF](https://pymupdf.readthedocs.io/)
- [GRI Guidelines](https://www.globalreporting.org/)

Contributors: Your Name, Your Organization