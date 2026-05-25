# Lanna-Global-MMLU-Lite

A small, hand-translated benchmark dataset for evaluating LLMs on **Lanna script** (ᨲᩫ᩠ᩅᨾᩮᩬᩥᨦ / Tua Mueang) — the traditional script of the Lanna Kingdom of northern Thailand.

> **⚠️ Use at your own risk.** This is a personal research project currently on hiatus. Translations were done by hand to the best of my ability, cross-checked against dictionaries and academic resources, but mistakes are expected. Please consult a professional linguist or build your own dataset if precision is critical.

---

## Background & Story

This project started as a research topic I was developing for a **MEXT scholarship** application (Japanese government scholarship). The idea was to investigate LLM understanding of Lanna — a low-resource, historical script still used by some communities in northern Thailand and neighboring regions. Spoiler: the scholarship didn't work out. 😅

However, I was admitted to **JAIST** (Japan Advanced Institute of Science and Technology), where I had applied. This dataset and the evaluations in `results/` are a product of my **weekly research progress at JAIST** — something I kept chipping away at while searching for a more formal thesis topic that my professor would approve of.

Progress halted about 1–2 months ago, so consider this a work-in-progress snapshot. I'm sharing it in case anyone finds it useful.

---

## A Personal Note on the Script

I am **a native speaker of Northern Thai / Lanna**, so this project is close to home — literally. Lanna script is part of my cultural heritage, even though most Northern Thai people today (myself included) are more fluent in the modern Thai script. This dataset is my attempt to see what, if anything, today's AI systems can make of this writing system.

---

## What Is Lanna Script?

Lanna (also called *Tua Mueang*, *Tai Tham*, or *Northern Thai script*) is an abugida script historically used throughout the ancient Lanna Kingdom (present-day northern Thailand, Shan State of Myanmar, Laos, and Yunnan, China). While it is still used in Buddhist religious contexts and by some cultural communities, it is considered a low-resource script with virtually no LLM training data.

---

## Dataset

This dataset is a small subset of **[Global MMLU Lite](https://huggingface.co/datasets/CohereForAI/Global-MMLU-Lite)** translated into Lanna script.

### Translation Methodology

1. **English → Thai**: Translated using ChatGPT
2. **Thai → Lanna**: Translated using Gemini (3.1 Pro), with basic chain-of-thought prompting that provided grammar conventions and script rules
3. **Human validation**: All ~30 questions were validated by hand, with corrections made where needed. Cross-referenced against online Lanna dictionaries and academic guides.

### Files

| File | Description |
|------|-------------|
| [`annotated_question.txt`](./annotated_question.txt) | ~30 questions in **Lanna script only**. English/Thai translations are not included (lost), but `#sample_id` tags allow you to map questions back to the original Global MMLU Lite. |
| [`unannotated_question.txt`](./unannotated_question.txt) | ~30 questions in **all three languages** (English → Thai → Lanna), output directly from the LLM translation pipeline. This file has not been hand-validated. |
| [`results/`](./results/) | Evaluation results from running several LLMs on the annotated Lanna questions and the original English questions. |

### Subject Coverage

Questions span 6 subject categories across 23 topics from the MMLU taxonomy:

| Category | Subjects |
|----------|---------|
| **Business** | Business Ethics, Management, Professional Accounting |
| **Humanities** | High School European History, Prehistory, Logical Fallacies, Moral Disputes, Jurisprudence |
| **Medical** | College Medicine, Human Aging, Nutrition |
| **Other** | Global Facts, Miscellaneous |
| **STEM** | Astronomy, High School Statistics, High School Physics |
| **Social Sciences** | High School Geography, Sociology, Professional Psychology, Public Relations |

---

## Evaluation Results

LLMs were evaluated on both the original **English** questions and the **Lanna script** questions (23 questions each, `xhigh` reasoning effort). The evaluation used **no advanced system prompt and no additional context** — just basic LLM calling with the question text. Results are in [`results/`](./results/).

### Overall Accuracy

| Model | English Accuracy | Lanna Accuracy | Δ Drop |
|-------|:----------------:|:--------------:|:------:|
| **google/gemini-3.1-pro-preview** | **91.3%** | 69.6% | −21.7% |
| **google/gemini-3-flash-preview** | **91.3%** | ⚠️ N/A | — |
| **deepseek/deepseek-v3.2** | **91.3%** | 36.4% | −54.9% |
| **openai/gpt-5.4** | 87.0% | 60.9% | −26.1% |
| **anthropic/claude-opus-4.6** | 87.0% | 61.9% | −25.1% |
| **google/gemini-3.1-flash-lite-preview** | 82.6% | 47.8% | −34.8% |
| **qwen/qwen3.5-397b-a17b** | 82.6% | ⚠️ N/A | — |
| **qwen/qwen3.5-9b** | 81.0% | 23.8% | −57.1% |
> ⚠️ **Note on missing Lanna results**: Two result files (`gemini-3-flash-preview_lanna` and `qwen3.5-397b-a17b_lanna`) are unreadable due to disk corruption on the evaluation PC. The files are unfortunately corrupted and lost. If you'd like to reproduce these results, the evaluation is straightforward — basic LLM API calls with no special prompting; the question text from `annotated_question.txt` fed directly to the model.


### Key Findings

- **All models perform significantly worse on Lanna** than on English, which is expected for a truly low-resource script.
- **Gemini-3.1-pro-preview** performs best on Lanna (69.6%), suggesting Google's models may have more exposure to Tai Tham script through multilingual training data.
- **DeepSeek-v3.2** scores highest on English (91.3%) but drops dramatically on Lanna (36.4%), suggesting strong general reasoning that doesn't extend to unseen scripts.
- **Qwen3.5-9B** scores lowest on Lanna (23.8%), barely above random chance (25%).
- **The Lanna task is genuinely hard** — even the best model drops ~22 percentage points from its English baseline.
- Models take **3–10× longer** to process Lanna questions than English ones, suggesting real character-level decoding rather than guessing from structural cues.

---

## Limitations

- **Small sample size**: Only 23 evaluated questions — don't over-interpret small differences.
- **Translation quality**: Hand-validated by a non-expert. Lanna orthography conventions vary by region and tradition.
- **Lost source data**: The English and Thai parallel texts for the annotated (hand-corrected) version were lost. Only the unannotated file has all three language versions.
- **No special prompting**: Results reflect zero-shot performance with no Lanna-specific context or grammar hints given to the model.
- **No test-train leakage check**: The original Global MMLU Lite questions may appear in model training data.

---

## Usage

Map questions to the original Global MMLU Lite dataset using the `#sample_id` tags in each file:

```
#sample_id: high_school_european_history/test/9
```

This corresponds to the `high_school_european_history` subject, `test` split, question index `9` in the [Global MMLU Lite](https://huggingface.co/datasets/CohereForAI/Global-MMLU-Lite) dataset.

---

## See Also

- 📝 [Lanna and LLM Investigation — t3m.dev](https://t3m.dev/blog/lanna-and-llm-investigation/) — A blog post with more context on this investigation. (My blog)
- 🤗 [Global MMLU Lite (CohereForAI)](https://huggingface.co/datasets/CohereForAI/Global-MMLU-Lite) — The original benchmark this dataset is derived from.

## Final Thoughts & Vision 💭

> **A Note on the Future of Tua Mueang (Lanna Script) in AI:**
>
> This project was entirely independent and self-funded, completed without any external backing or institutional support. While this is just a humble, initial step, my deepest hope is to see this effort carried forward by researchers, linguists, and academic institutions in Northern Thailand who are far more expert and experienced than I am. 
> 
> Language is the living vessel of culture. My ultimate dream is that AI, LLMs, and language technology can be used to preserve, revitalize, and extend the lifespan of minority and low-resource languages. I hope this work inspires deeper, more comprehensive research into Lanna script within the modern NLP and AI communities.

---


## Citation & Attribution


If you use this dataset, please also cite the original **Global MMLU Lite**:

```bibtex
@dataset{GlobalMMLULite,
  title  = {Global MMLU Lite},
  author = {CohereForAI},
  url    = {https://huggingface.co/datasets/CohereForAI/Global-MMLU-Lite}
}
```

---

## Contributing & Contact

This project is currently on pause, but I would love to hear from you! If you want to discuss anything, share ideas, or collaborate, you are extremely welcome to reach out:

- 📬 **Get in touch**: Connect with me via **[LinkedIn](https://www.linkedin.com/in/thiti-phuttaamart/)** or [**Via email**](mailto:[p.thiti.jaist.ac.jp]) p.thiti@jaist.ac.jp.
- 🛠️ **Participate**: Open a **GitHub Issue** or submit a **Pull Request** (corrections to Lanna translations are especially welcome!).

---

## License

The translations in this repository are released under **CC BY 4.0**. The underlying questions are from Global MMLU Lite — please check their license for usage terms.

---

*Built with curiosity, a MEXT scholarship dream, weekly JAIST progress reports, and a deep respect for the writing system of my hometown.* 🙏
