# ML Engineering Portfolio — Summary

Four end-to-end projects, each covering a different core ML problem
type, all deployed and verified live on AWS (not just locally). Built to
demonstrate breadth (supervised classification, time-series forecasting,
unsupervised anomaly detection, NLP) plus the deployment/production
skills that separate an ML engineer from someone who only builds
notebooks.

---

## The four projects at a glance

| | Time-Series Forecasting | Credit Risk Classification | GL Anomaly Detection | Financial NLP |
|---|---|---|---|---|
| **Problem type** | Regression / forecasting | Supervised classification (imbalanced) | Unsupervised anomaly detection | NLP: sentiment classification + real-world signal test |
| **Data** | Apple's real SEC filings (35 quarters) | Real Lending Club data (1.05M rows, 425K resolved) | Synthetic GL data (20K entries, statistically verified against Benford's Law) | Financial PhraseBank (2,264 real sentences) + Apple's real 10-K filings (2015-2025) |
| **Models compared** | Seasonal naive, SARIMA, Prophet, XGBoost, LSTM | Logistic regression, Random Forest, XGBoost (class-weighted + SMOTE) | Isolation Forest, autoencoder, 2 ensemble variants | TF-IDF+LogReg, FinBERT (zero-shot), DistilBERT (fine-tuned) |
| **Core finding** | Simpler models with explicit seasonal structure beat flexible/deep models on small data | Class weighting beats SMOTE; XGBoost wins on PR-AUC | A hypothesis was tested and refuted — models have complementary, mechanistically-explained strengths | A hypothesis was tested and completely confirmed (15/15); a "strong" real-world correlation was shown to be statistically fragile via leave-one-out analysis |
| **Deployment** | ECS Fargate batch task + FastAPI/Streamlit (Docker-tested) | ECS Fargate batch task, 525MB S3 data pipeline | ECS Fargate batch task, self-contained (generates own data) | ECS Fargate, 4 separate sub-pipeline task runs, cross-project S3 dependency |
| **Real bugs found & fixed** | `uvicorn` import path gotcha, container filesystem isolation, S3 client timeout | Windows/Linux case-sensitivity, stale Docker CMD, IAM role mixup, missing log group permission | Same case-sensitivity/CMD lessons applied *proactively*; stale local files caught via S3 path verification | Broken/deprecated public dataset infrastructure (3 layers deep), a fiscal-year calculation bug caught before it corrupted results, a slow Docker build diagnosed and fixed, a transient ECR pull failure, a Windows shell-escaping problem |

---

## What this portfolio actually demonstrates

**Breadth across ML problem types.** Not four variations on the same
technique — a genuine regression/forecasting problem, a genuine
imbalanced classification problem, a genuine unsupervised problem with
no labels at training time, and a genuine NLP problem spanning classical
ML, zero-shot and fine-tuned transformers. Each required different
modeling approaches, different evaluation metrics (MAPE vs.
precision/recall/PR-AUC vs. ROC-AUC/top-K precision vs. per-class F1),
and different interpretation techniques (confidence intervals vs. SHAP
vs. z-score deviation explanations vs. linear model coefficients).

**Rigor that goes beyond running a model.** Every project includes:
- A **specific, falsifiable hypothesis tested against real evidence** —
  not just "I tried X and it worked." The forecasting project predicted
  and confirmed that flexible models would underperform on small data.
  The anomaly project predicted an autoencoder would fix Isolation
  Forest's weak spot — and was *wrong*, which led to a deeper,
  mechanistically-verified understanding than confirmation would have.
  The NLP project predicted a transformer would fix a specific TF-IDF
  failure pattern — and was *completely right*, confirmed 15/15 against
  the exact sentences that motivated the hypothesis.
- **Real production bugs, found and fixed with direct evidence**, not
  glossed over. The case-sensitivity bug (Windows vs. Linux filesystems),
  the IAM role-vs-execution-role mixup, the S3 prefix bug that got
  progressively better across the first three projects (documented as a
  known limitation in project 1, still unresolved in project 2, and
  *actually fixed* by project 3, then built correctly from the start in
  project 4) — this shows learning compounding across the whole
  portfolio, not four isolated exercises.
- **Honest limitations, stated plainly.** SARIMA's undershoot bias.
  The credit-risk project's incomplete leakage-column audit on a
  145-column real dataset. The anomaly project's own interpretability
  tool producing a technically-correct-but-misleading explanation
  (caught by actually reading the output, not just generating it). The
  NLP project's real-world correlation (r=-0.728) shown via leave-one-out
  analysis to be driven almost entirely by a single extraordinary year
  — reported as "too small a sample to draw a reliable conclusion,"
  not oversold as a discovered signal.

**Real deployment experience, not "should work in theory."** All four
run live on AWS ECS Fargate, verified via actual CloudWatch logs and S3
listings — not just a clean exit code, which was directly proven
insufficient partway through this work (a silently-stale Docker image
exited cleanly while running an outdated, incomplete pipeline). The
fourth project's deployment surfaced a genuinely new class of real
issues — a slow Docker build correctly diagnosed to its root cause, a
confusing AWS task-visibility mystery worked through systematically
rather than guessed at, a transient infrastructure failure correctly
distinguished from a real one, and a Windows-specific shell-escaping
problem solved by removing the fragility entirely rather than fighting
it.

---

## Resume bullets (pick and adapt per application)

- Built and deployed four end-to-end ML pipelines (time-series
  forecasting, imbalanced classification, unsupervised anomaly
  detection, NLP sentiment analysis) on real and realistic data, each
  containerized with Docker and deployed live on AWS ECS Fargate with
  S3-backed output storage.
- Compared 5+ modeling approaches per project with rigorous, metric-
  appropriate evaluation (MAPE, PR-AUC, ROC-AUC, top-K precision, per-
  class F1); diagnosed and explained *why* each model won or lost using
  direct statistical evidence (z-scores, calibration checks, per-
  category recall breakdowns, leave-one-out sensitivity analysis) rather
  than reporting metrics alone.
- Diagnosed and resolved real production deployment issues including a
  cross-platform file-casing bug, an IAM role misconfiguration, a
  silently-incomplete containerized pipeline, a mis-configured Docker
  build causing 30+ minute build times, and transient cloud
  infrastructure failures — verified fixes using direct AWS CLI evidence
  (CloudWatch logs, S3 listings) rather than assuming success from a
  clean exit code.
- Built a financial sentiment pipeline spanning classical ML (TF-IDF) to
  zero-shot and fine-tuned transformer models (FinBERT, DistilBERT),
  including real SEC EDGAR data extraction across a decade of
  inconsistently-formatted filings, and a statistically rigorous
  real-world signal test (Pearson correlation + leave-one-out
  sensitivity analysis) against actual company financial data.
- Built a synthetic data generator for general ledger anomaly detection
  with six distinct, realistic fraud/error patterns, statistically
  validated against Benford's Law before use — enabling rigorous
  quantitative evaluation of unsupervised models against ground truth,
  which is normally unavailable in real anomaly detection.
- Engineered a full credit risk pipeline on 1M+ real loan records,
  correctly excluding 14+ leakage columns, handling class imbalance via
  weighted vs. SMOTE comparison, and delivering a cost-justified
  decision threshold (not just a default 0.5 cutoff) based on stated
  business assumptions.

---

## Interview talking points (STAR-style, drawn from real events)

### "Tell me about a time you found a bug in production"
*The stale-Dockerfile bug (credit-risk project).* After adding three new
model scripts, the container kept exiting cleanly with code 0 —
seemingly successful. But comparing CloudWatch logs across two runs
showed they stopped at the *exact same point* every time, regardless of
a memory increase from 8GB to 16GB (initially suspected as an
out-of-memory kill). That non-result — memory doubling changed nothing
about *where* it failed — was the actual clue: it ruled out OOM and
pointed toward a static, code-level cause. Checked the local Dockerfile
directly and found it still only ran the original 4-script pipeline; the
new scripts were never being invoked at all. Fixed it, then hit a
second, subtler bug from the same root cause (Windows/Linux file-casing
mismatch, invisible locally, breaking immediately in a case-sensitive
container) — good story about testing a wrong hypothesis rigorously
enough to *learn* from its failure, not just move to the next guess.

### "Tell me about a hypothesis that didn't pan out"
*The autoencoder vs. Isolation Forest test (anomaly detection project).*
After finding Isolation Forest struggled on one anomaly type
(`unusual_pair`) because its signal was diluted across many
uninformative features, predicted an autoencoder — which reconstructs
all features jointly — would handle it better. It didn't; it did worse.
Rather than treat that as a dead end, investigated why with direct
measurements: confirmed the two models' rare correct catches were
literally the same entries, not complementary ones, and quantified that
`unusual_pair`'s statistical signal (-2.1 std devs) was categorically
weaker than the signal driving the autoencoder's other successes (e.g.
`rare_user` at -37.4 std devs). That refutation led to a specific,
actionable recommendation — this anomaly type needs a dedicated rule,
not a better general-purpose model — that a confirmed hypothesis
wouldn't have produced.

### "Tell me about a hypothesis that turned out to be right"
*The transformer vs. TF-IDF test (financial NLP project).* A TF-IDF
sentiment baseline systematically misclassified plain factual sentences
reporting financial figures ("net sales were EUR 39.2 million") as
positive — no evaluative language present, just a spurious correlation
between financial vocabulary and positive sentiment. Hypothesized that a
transformer, capable of weighing full sentence context rather than a
largely bag-of-words treatment, would resolve this specific failure
mode. Rather than just compare aggregate accuracy, re-ran the exact same
15 sentences through the fine-tuned model directly — all 15 were
correctly reclassified. A clean, complete confirmation is just as
valuable a story as a refutation, as long as it's tested precisely
rather than just assumed from an overall accuracy improvement.

### "Tell me about validating a business signal you found"
*The sentiment-vs-revenue correlation (financial NLP project).* After
scoring a decade of Apple's real 10-K Risk Factors sections with FinBERT
and testing the sentiment trend against real revenue growth, a strong
lagged correlation appeared (r=-0.728) — but with only 7 data points, a
single unusual year could easily be driving it. Ran a leave-one-out
sensitivity analysis: excluding one specific fiscal year alone dropped
the correlation to near zero (r=-0.057), while excluding a different
year pushed it to statistical significance (r=-0.893, p=0.017). That
instability — the reported result swinging from "no relationship" to
"significant" depending on which single point you removed — was itself
the real finding: the correlation is real, but the sample is too small
to trust as a discovered signal. Reporting that honestly, rather than
citing whichever exclusion looked best, is the difference between doing
statistics and doing statistics correctly.

### "How do you handle imbalanced data?"
*Credit risk project.* Compared class-weighting against SMOTE
oversampling on both Random Forest and XGBoost, rather than defaulting
to one. Class weighting won consistently on both models — a real,
reproducible finding, not a coin flip — and reported *why* it's the
more defensible default in future write-ups rather than treating SMOTE
as the assumed-correct answer.

### "Walk me through choosing a decision threshold"
*Credit risk project.* Rejected the default 0.5 cutoff and instead
computed actual confusion-matrix costs across a threshold grid, given a
stated assumption (missing a default costs 5x a wrongly-rejected good
loan). Caught and fixed a subtler bug along the way — an earlier
approximation-based cost formula degenerated to a nonsensical extreme
threshold, only caught by actually running it, not by inspection.

### "Tell me about validating your own tooling, not just trusting it"
*Anomaly detection project.* Building a per-entry SHAP-style explanation
tool, noticed two flagged "round-dollar fraud" entries were explained
primarily by an unrelated feature (`duplicate_count`). Investigated
directly rather than assuming the explanation was simply wrong: found a
genuine, low-probability coincidental data collision (two entries
sharing the same amount/account/user by chance, in a combinatorial space
of only 528 possibilities for 80 draws). The explanation was technically
accurate but could easily mislead a reader without that context —
documented it rather than letting the tool's output stand unexamined.

### "Tell me about debugging something you couldn't reproduce"
*The AWS task-visibility mystery (financial NLP project).* A live ECS
task launched successfully (a real task ARN, no launch errors), but
subsequently could not be found through five independent verification
methods — direct task lookup, task listing filtered by RUNNING, by
STOPPED, with no filter at all, and even the AWS Console UI directly.
Rather than keep guessing at increasingly obscure explanations,
relaunched the identical task and monitored it in real time, checking
every 15-20 seconds instead of waiting minutes between checks — the
second attempt succeeded cleanly and was caught immediately, strongly
suggesting the first likely also succeeded but was missed to an AWS-side
indexing delay during an extended troubleshooting window rather than a
real infrastructure fault. Knowing when a mystery has enough evidence to
stop chasing and move forward — rather than needing a fully closed
explanation for every anomaly — is its own kind of engineering judgment.

### "Tell me about relying on a public dataset or library that turned out to be broken"
*Financial PhraseBank loading (financial NLP project).* The commonly
documented way to load this well-known academic sentiment benchmark is
fully broken — the short dataset identifier used in most tutorials
404s, and even the correct identifier fails via the standard library
call because the dataset repository still contains a deprecated legacy
loading script. A promising-looking fix based on search results (a
Parquet-converted version of the same repo) turned out to be based on
stale information once checked directly against the repo's actual
current file listing. Resolved by bypassing the fragile library
abstraction entirely — downloading and parsing the raw source file
directly — which also turned out to be a more durable fix long-term,
since it no longer depends on that library's script-execution support
existing at all.

---

## Where each project's full write-up lives

- **Time-series forecasting**: [github.com/lordadrianbold/ts-forecast-project]([https://github.com/lordadrianbold/ts-forecast-project](https://github.com/lordadrianbold/time-series-forecast-project))
- **Credit risk classification**: [github.com/lordadrianbold/credit-risk-project]([https://github.com/lordadrianbold/credit-risk-project](https://github.com/lordadrianbold/credit-risk-project))
- **GL anomaly detection**: [github.com/lordadrianbold/gl-anomaly-project]((https://github.com/lordadrianbold/gl-anamoly-project)](https://github.com/lordadrianbold/gl-anamoly-project))](https://github.com/lordadrianbold/gl-anamoly-project)
- **Financial NLP**: [github.com/lordadrianbold/financial-nlp-project](https://github.com/lordadrianbold/financial-nlp-project)

*(Double-check these URLs match your actual repo names exactly — GitHub
usernames/repo names are case-sensitive in the sense that a typo won't
resolve, even though GitHub itself lowercases display.)*

Each project's `notes.md` is the detailed, week-by-week technical record;
each `README.md` is the front-door summary a reviewer sees first.




