ESG Fine-Tuning Dataset (README)

This dataset supports fine-tuning language models for ESG-related tasks such as metadata extraction, KPI identification, and guideline alignment from full ESG reports.

---

1. Dataset Format

Each training example follows this structure:

{
  "task": "Task Name",
  "input": "<report chunk or full text>",
  "output": { /* structured result */ }
}

Tasks include:
- ESG metadata extraction
- Quantitative ESG data extraction
- Guideline alignment

---

2. Preparation Steps

1. ESG reports were downloaded from Hugging Face:
   - Dataset: DataNeed/company-reports
   - Saved in /reports/

2. PDFs converted to JSON using PyMuPDF (fitz):
   - Extracted: page-wise text, company name (from filename), report year

3. Cleaning:
   - Removed empty, short, or broken files

4. Task Generation:
   - Metadata: extracted company name, report year, ESG topics
   - KPIs: parsed Scope 1 emissions, renewable energy %, etc., with GRI fuzzy linking
   - Guideline Alignment: TF-IDF matching between GRI text and ESG report chunks

---

3. Output Files

- esg_metadata_all.jsonl — company, year, topics
- esg_kpi_all.jsonl — numerical ESG metrics with disclosure links
- esg_guideline_all.jsonl — matched GRI disclosures with confidence
- esg_multi_task_dataset.jsonl — all tasks combined (merged)

---

4. Usage

For evaluation and fine-tuning:
- Format is compatible with OpenAI's GPT fine-tuning, Gemini, Claude, or similar
- Train using supervised instruction-style prompts
- Compare base vs. fine-tuned outputs for each model

---

5. Tools Used

- Python 3.10+
- PyMuPDF
- scikit-learn (TF-IDF)
- Hugging Face Datasets
- tqdm

---

6. Author Notes

This dataset was auto-generated from raw PDF ESG reports without manual annotation. The goal is to enable model supervision on real-world disclosures at scale.