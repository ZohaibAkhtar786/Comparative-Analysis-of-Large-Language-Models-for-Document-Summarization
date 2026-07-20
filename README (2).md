# LLM Document Summarization Benchmark

A comparative study of Claude, ChatGPT, Gemini, and Grok summarizing the same 66-page technical
report, scored against the source document rather than against each other's outputs.

## Objective

Test whether large language models can accurately, concisely, and faithfully summarize a long
technical document under an identical prompt and length constraint — and specifically check
whether any of them hallucinate facts not present in the source.

## Source Document

**Real Estate Price Prediction** — BS Computer Science capstone project (Project ID: UE-CS-13),
University of Education, Lahore (Attock Campus). 66 pages covering an ML pipeline (EDA,
preprocessing, feature engineering, model selection, evaluation) and a Flask web application for
predicting California housing prices.

## Models Evaluated

- ChatGPT
- Gemini
- Grok
- Claude *(see caveat below)*

## Prompt (identical across all models)

```
You are an expert technical writer.

Read the attached document carefully and generate a professional summary.

Requirements:

1. Summarize the complete document.
2. Explain:
   - Problem Statement
   - Objectives
   - Methodology
   - Dataset
   - Machine Learning Models
   - Data Preprocessing
   - Feature Engineering
   - Model Evaluation
   - Results
   - Deployment
   - Conclusion
3. Keep the summary between 500-700 words.
4. Use clear headings.
5. Do not include information that is not present in the document.
6. Maintain technical accuracy.
```

## Method

Every claim scored below was checked directly against the source document — not assumed from
general plausibility. Word counts were computed programmatically from each raw output, not
estimated.

## Results

### Fact Verification

| Claim | ChatGPT | Gemini | Grok | Claude | Verified against source |
|---|---|---|---|---|---|
| 207 missing values in `total_bedrooms` | Not stated | ✅ Correct | ✅ Correct | ✅ Correct | Abstract |
| K-Means uses 10 clusters | Not stated | ✅ Correct | Not stated | Not stated | Class Diagram (`Similarity4Cluster`) |
| Median income correlation ~0.688 | Named, no figure | ✅ Correct | ✅ Correct | ✅ Correct | Abstract |
| 'ISLAND' edge case in testing | Not stated | Not stated | ✅ Correct | ✅ Correct | Ch. 6, TC-02 |
| 500-700 word limit respected | ❌ No (837) | ❌ No (758) | ✅ Yes (508) | ❌ No (459) | Prompt requirement #3 |

**No model fabricated a fact.** Every deviation found was an instruction-following miss
(length, heading structure, scope), not a hallucination.

### Scored Rubric (1–10)

| Model | Quality | Accuracy | Conciseness | Coverage | Technical Depth | Hallucination | Overall |
|---|---|---|---|---|---|---|---|
| ChatGPT | 9 | 10 | 5 | 10 | 7.5 | 10 | **8.58** |
| Gemini | 6 | 10 | 6.5 | 10 | 9.5 | 10 | **8.67** |
| Grok | 6 | 10 | 9.5 | 7.5 | 8 | 9 | **8.33** |
| Claude* | 9 | 10 | 6.5 | 10 | 8.5 | 10 | **9.00** |

**\*Claude's row was scored by Claude** — the same model that wrote the summary being graded.
That's a direct conflict of interest, so this score is **not treated as an independent result**
and should be re-scored by a human or a different model before being trusted. It's included for
transparency, not as a claim that Claude "won."

## Key Findings

1. **No hallucinations across any of the four models.** Every headline metric (R² = 0.8198,
   RMSE ≈ $47,537) was reproduced correctly everywhere it appeared.
2. **Every model violated the 500–700 word instruction** — two ran over, two ran under. This was
   the single most consistent failure mode across all four, more consistent than any accuracy
   issue.
3. **Among the three independently judged models** (ChatGPT, Gemini, Grok — no conflict of
   interest in scoring), **Gemini ranked highest** on technical precision (correctly cited the
   exact K-Means cluster count and consistently used specific figures over vague paraphrase), but
   lost points for running section headers into continuous prose instead of using clear, distinct
   headings as instructed.
4. **Grok was the only model to respect the word limit**, but merged two requested section pairs
   into single headings, added unrequested content (author/institution front matter, a closing
   note about unrelated document sections), and misreported its own word count by ~22%.
5. Accuracy alone is an incomplete evaluation for this kind of task — **instruction adherence**
   (length, structure, scope discipline) is where all four models actually failed, and deserves
   equal weight in any future benchmark.

## Repository Contents

```
.
├── README.md                                  # this file
├── source-document.pdf                        # the 66-page report used as input
├── LLM_Summarization_Comparison_Report.pdf     # full report with all four outputs + scoring
└── prompt.txt                                  # the exact prompt used for all models
```

## Reproducing This Study

1. Use the same source document and the exact prompt above — no wording changes between models.
2. Submit to each target model as an attached file, not pasted text, so all models see identical
   formatting.
3. Record raw outputs without editing.
4. Check specific factual claims against the source directly rather than scoring on impression.
5. If including a model's own output in the comparison, disclose any self-scoring conflict of
   interest explicitly rather than presenting it as an independent result.

## Caveats

- This is a single-document, single-run comparison — not a statistically robust benchmark. Model
  outputs can vary between runs even with an identical prompt.
- Rubric scores are inherently somewhat subjective; the fact-verification table is the more
  load-bearing evidence in this study.
- Model versions were not pinned/recorded at time of testing — results may not generalize to
  earlier or later versions of the same models.
