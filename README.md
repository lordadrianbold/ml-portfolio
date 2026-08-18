# ML Engineering Portfolio — Summary

Three end-to-end projects, each covering a different core ML problem
type, all deployed and verified live on AWS (not just locally). Built to
demonstrate breadth (supervised classification, time-series forecasting,
unsupervised anomaly detection) plus the deployment/production skills
that separate an ML engineer from someone who only builds notebooks.

---

## The three projects at a glance

| | Time-Series Forecasting | Credit Risk Classification | GL Anomaly Detection |
|---|---|---|---|
| **Problem type** | Regression / forecasting | Supervised classification (imbalanced) | Unsupervised anomaly detection |
| **Data** | Apple's real SEC filings (35 quarters) | Real Lending Club data (1.05M rows, 425K resolved) | Synthetic GL data (20K entries, statistically verified against Benford's Law) |
| **Models compared** | Seasonal naive, SARIMA, Prophet, XGBoost, LSTM | Logistic regression, Random Forest, XGBoost (class-weighted + SMOTE) | Isolation Forest, autoencoder, 2 ensemble variants |
| **Core finding** | Simpler models with explicit seasonal structure beat flexible/deep models on small data | Class weighting beats SMOTE; XGBoost wins on PR-AUC | A hypothesis was tested and refuted — models have complementary, mechanistically-explained strengths |
| **Deployment** | ECS Fargate batch task + FastAPI/Streamlit (Docker-tested) | ECS Fargate batch task, 525MB S3 data pipeline | ECS Fargate batch task, self-contained (generates own data) |
| **Real bugs found & fixed** | `uvicorn` import path gotcha, container filesystem isolation, S3 client timeout | Windows/Linux case-sensitivity, stale Docker CMD, IAM role mixup, missing log group permission | Same case-sensitivity/CMD lessons applied *proactively*; stale local files caught via S3 path verification |

---

## What this portfolio actually demonstrates

**Breadth across ML problem types.** Not three variations on the same
technique — a genuine regression/forecasting problem, a genuine
imbalanced classification problem, and a genuine unsupervised problem
with no labels at training time. Each required different modeling
approaches, different evaluation metrics (MAPE vs. precision/recall/PR-AUC
vs. ROC-AUC/top-K precision), and different interpretation techniques
(confidence intervals vs. SHAP vs. z-score deviation explanations).

**Rigor that goes beyond running a model.** Every project includes:
- A **specific, falsifiable hypothesis tested against real evidence** —
  not just "I tried X and it worked." The forecasting project predicted
  and confirmed that flexible models would underperform on small data.
  The anomaly project predicted an autoencoder would fix Isolation
  Forest's weak spot — and was *wrong*, which led to a deeper,
  mechanistically-verified understanding than confirmation would have.
- **Real production bugs, found and fixed with direct evidence**, not
  glossed over. The case-sensitivity bug (Windows vs. Linux filesystems),
  the IAM role-vs-execution-role mixup, the S3 prefix bug that got
  progressively better across all three projects (documented as a known
  limitation in project 1, still unresolved in project 2, and *actually
  fixed* by project 3) — this last one specifically shows learning
  compounding across the portfolio, not three isolated exercises.
- **Honest limitations, stated plainly.** SARIMA's undershoot bias.
  The credit-risk project's incomplete leakage-column audit on a
  145-column real dataset. The anomaly project's own interpretability
  tool producing a technically-correct-but-misleading explanation
  (caught by actually reading the output, not just generating it).

**Real deployment experience, not "should work in theory."** All three
run live on AWS ECS Fargate, verified via actual CloudWatch logs and S3
listings — not just a clean exit code, which was directly proven
insufficient partway through this work (a silently-stale Docker image
exited cleanly while running an outdated, incomplete pipeline).

---

## Resume bullets (pick and adapt per application)

- Built and deployed three end-to-end ML pipelines (time-series
  forecasting, imbalanced classification, unsupervised anomaly
  detection) on real and realistic data, each containerized with Docker
  and deployed live on AWS ECS Fargate with S3-backed output storage.
- Compared 5+ modeling approaches per project with rigorous, metric-
  appropriate evaluation (MAPE, PR-AUC, ROC-AUC, top-K precision);
  diagnosed and explained *why* each model won or lost using direct
  statistical evidence (z-scores, calibration checks, per-category
  recall breakdowns) rather than reporting metrics alone.
- Diagnosed and resolved real production deployment issues including a
  cross-platform file-casing bug, an IAM role misconfiguration, and a
  silently-incomplete containerized pipeline — verified fixes using
  direct AWS CLI evidence (CloudWatch logs, S3 listings) rather than
  assuming success from a clean exit code.
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

---

## Where each project's full write-up lives

- **Time-series forecasting**: [github.com/lordadrianbold/ts-forecast-project](https://github.com/lordadrianbold/ts-forecast-project)
- **Credit risk classification**: [github.com/lordadrianbold/credit-risk-project](https://github.com/lordadrianbold/credit-risk-project)
- **GL anomaly detection**: [github.com/lordadrianbold/gl-anomaly-project](https://github.com/lordadrianbold/gl-anamoly-project)

*(Double-check these URLs match your actual repo names exactly — GitHub
usernames/repo names are case-sensitive in the sense that a typo won't
resolve, even though GitHub itself lowercases display.)*

Each project's `notes.md` is the detailed, week-by-week technical record;
each `README.md` is the front-door summary a reviewer sees first.




