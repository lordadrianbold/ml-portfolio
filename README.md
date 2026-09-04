# ML & AI Engineering Portfolio — Summary

This portfolio has seven end-to-end projects. Five are core ML
projects. Two are newer AI Engineering projects.

Together they cover supervised classification, time-series
forecasting, unsupervised anomaly detection, NLP, retrieval-augmented
agents, structured-output reliability engineering, and multi-agent
orchestration. Every project was deployed and verified, not just
built and left in a notebook.

The two AI Engineering projects close a real, current market gap.
Live research into current hiring data showed RAG, vector databases,
agent orchestration, and structured-output reliability engineering as
the highest-premium skill cluster. This was verified directly, not
assumed. None of the five original ML projects touch these skills.

---

## The five ML projects at a glance

| | Time-Series Forecasting | Credit Risk Classification | GL Anomaly Detection | Financial NLP | Financial RAG Agent |
|---|---|---|---|---|---|
| **Problem type** | Regression / forecasting | Supervised classification (imbalanced) | Unsupervised anomaly detection | NLP: sentiment classification + real-world signal test | Retrieval-augmented generation + agentic tool use |
| **Data** | Apple's real SEC filings (35 quarters) | Real Lending Club data (1.05M rows, 425K resolved) | Synthetic GL data (20K entries, statistically verified against Benford's Law) | Financial PhraseBank (2,264 real sentences) + Apple's real 10-K filings | 19 real 10-K filings across Apple, Microsoft, JPMorgan Chase |
| **Core technique** | SARIMA, Prophet, XGBoost, LSTM | Logistic regression, Random Forest, XGBoost (class-weighted + SMOTE) | Isolation Forest, autoencoder | TF-IDF+LogReg, FinBERT (zero-shot), DistilBERT (fine-tuned) | ChromaDB + sentence-transformers retrieval, LangChain `create_agent()`, Claude Haiku 4.5 |
| **Core finding** | Simpler models with explicit seasonal structure beat flexible/deep models on small data | Class weighting beats SMOTE. XGBoost wins on PR-AUC | A hypothesis was tested and refuted. Models have complementary, mechanistically-explained strengths | A hypothesis was tested and completely confirmed (15/15). A "strong" real-world correlation was shown to be statistically fragile via leave-one-out analysis | 100% agent routing accuracy, including 2 deliberately adversarial "trap" questions. A mechanical groundedness check found and precisely explained a real, bounded hallucination |
| **Deployment** | ECS Fargate batch task + FastAPI/Streamlit (Docker-tested) | ECS Fargate batch task, 525MB S3 data pipeline | ECS Fargate batch task, self-contained (generates own data) | ECS Fargate, 4 separate sub-pipeline task runs, cross-project S3 dependency | A live ECS Service. First project with real inbound networking and a real managed secret |
| **Real bugs found & fixed** | `uvicorn` import path gotcha, container filesystem isolation, S3 client timeout | Windows/Linux case-sensitivity, stale Docker CMD, IAM role mixup, missing log group permission | Same case-sensitivity/CMD lessons applied proactively. Stale local files caught via S3 path verification | Broken/deprecated public dataset infrastructure (3 layers deep), a fiscal-year calculation bug, a slow Docker build diagnosed and fixed | 4 distinct real HTML parsing bugs, a silently-wrong extraction caught only by manual verification, and 5 distinct deployment/IAM/networking issues in a single live deployment session |

## The two AI Engineering projects at a glance

| | Financial Doc Extraction | Month-End Close Assistant |
|---|---|---|
| **Problem type** | Structured extraction + output reliability engineering | Multi-agent orchestration |
| **Data** | 987 real SROIE receipts (ICDAR 2019) + 160 synthetic B2B invoices | Multi-month synthetic account balances, randomized realistic anomalies |
| **Core technique** | LangChain structured output (function calling). A 4-signal reliability system: reconciliation, field plausibility, semantic similarity, header validation | LangGraph `StateGraph` directly. 5 specialized agents, including a real parallel fan-out |
| **Core finding** | 7 distinct reliability blind spots found and fixed through direct investigation, not designed correctly upfront. Final result (100% precision on "trust this" predictions) confirmed to generalize on a genuinely fresh holdout set | 100% accuracy on every real anomaly type, confirmed on two independent holdout runs. A real model fabrication was caught by an independent validator, after a prompt-only fix was tested and found insufficient |
| **Deployment** | A live ECS Service, deployed cleanly on the first attempt with zero new bugs. Three real fixes from the Month-End Close Assistant's deployment were applied proactively — direct evidence those fixes addressed real root causes, not just one project's symptoms | A live ECS Service with a real multi-agent graph and a live demo page. 3 distinct real deployment bugs were diagnosed and fixed with direct evidence at each step |
| **Real bugs found & fixed** | A real data-format mismatch (documentation vs. actual downloaded data), a Parquet schema quirk silently hiding missing values, a currency-comparison bug under-reporting real accuracy, and 4 distinct reliability-system blind spots — each found by refusing to accept a good-looking number at face value | A genuine uvicorn/Docker import-path issue, an unexplained file silently dropped from a broader folder copy, and a real IAM permission gap — each diagnosed with direct evidence (inspecting the built container's actual filesystem, checking the image's real existence) before being fixed |

---

## What this portfolio actually demonstrates

**Breadth across ML problem types and current AI Engineering skill
clusters.** This portfolio includes a genuine regression/forecasting
problem, a genuine imbalanced classification problem, a genuine
unsupervised problem with no labels at training time, a genuine NLP
problem spanning classical ML through fine-tuned transformers, a
genuine retrieval-augmented agentic system, genuine structured-output
reliability engineering, and genuine multi-agent orchestration. That
is seven different core techniques and seven different evaluation
approaches. These are not variations on the same idea.

**A portfolio that responds to real market signal, not just personal
interest.** The two AI Engineering projects exist because live
research into current hiring data showed RAG/agent skills,
structured-output reliability engineering, and multi-agent
orchestration carrying the highest wage premium of any specific
technical cluster. They were scoped and built directly in response to
that finding. Framework and technique choices were verified against
real, current, verifiable sources, not assumed from possibly-stale
training knowledge. Examples: LangChain's `create_agent()` for the RAG
project, LangGraph's `StateGraph` directly for the Month-End Close
Assistant, and `all-mpnet-base-v2` as the real published best-performer
for paraphrase detection, with its own cited optimal threshold.

**Rigor that goes beyond running a model.** Every project includes a
specific, falsifiable hypothesis tested against real evidence. Every
project has real production bugs found and fixed with direct evidence.
Honest limitations are stated plainly, not glossed over. The two AI
Engineering projects each add a further form of rigor beyond even the
RAG project's mechanical groundedness checking: iteratively-built
reliability systems, honestly holdout-validated. A strong result
measured on the same data used to develop a system cannot be fully
trusted. So a completely fresh dataset — different random seed,
confirmed genuinely different data — was generated and evaluated with
the unmodified system each time. This tests whether a result is real,
or just an artifact of fitting to the tuning sample. In both projects,
the result held up on the metric that mattered most. Specific
sub-findings did not always hold, and that was reported honestly
rather than hidden. The Month-End Close Assistant's holdout run went
further: it found a real, previously-invisible data-generation flaw
that the smaller tuning sample had never exposed. The flaw was fixed,
and the fix was re-verified with a second, independent holdout run
before it was trusted.

**Real deployment experience, including a real, reasoned scope
decision that was later reconsidered.** All seven projects run live
on AWS. This was verified via actual CloudWatch logs and direct
file/S3 checks, never trusted from a clean exit code alone. The
Financial Doc Extraction project was initially, deliberately left
undeployed, with real, stated reasoning at the time: its
differentiator was reliability-engineering depth, not infrastructure
already proven elsewhere. That decision was later reconsidered. When
the deployment was actually carried out, it applied three real,
hard-won fixes from the Month-End Close Assistant's own deployment
proactively. The task reached `RUNNING` on the first attempt, with
zero new bugs. That is direct evidence those fixes generalized to a
real root cause, not just one project's specific symptoms.

---

## Resume bullets (pick and adapt per application)

- Built and deployed five end-to-end ML pipelines (time-series
  forecasting, imbalanced classification, unsupervised anomaly
  detection, NLP sentiment analysis, and a retrieval-augmented agentic
  system) on real and realistic data. Each was containerized with
  Docker and deployed live on AWS (ECS Fargate batch tasks and a live
  ECS Service) with S3-backed output storage.
- Built a retrieval-augmented generation system and LangChain-based
  agent (`create_agent()`) with three distinct tools over real SEC
  10-K filings. Achieved 100% agent tool-routing accuracy on a
  held-out test set, including deliberately adversarial questions
  designed to test genuine reasoning over surface keyword matching.
- Designed and implemented a mechanical groundedness verification
  system for LLM-generated financial answers. It automatically
  extracts and validates every quoted claim against real source text.
  It precisely diagnoses failure causes rather than relying on manual
  review alone.
- Built a four-signal output reliability system for LLM-based
  structured document extraction (financial reconciliation, field
  format validation, semantic similarity via published research,
  header validation). Iteratively discovered and closed genuine blind
  spots through direct investigation of real failures. Validated the
  final result on a genuinely held-out dataset never used in
  development, confirming the system's core reliability guarantee
  (100% precision on trusted predictions) generalizes rather than
  reporting a number fitted to the tuning data.
- Built a genuine multi-agent system with LangGraph's `StateGraph`
  directly, not a single agent choosing between tools. Five
  specialized agents, including a real parallel fan-out, achieved 100%
  accuracy on every real anomaly type across two independent holdout
  evaluations. Diagnosed a real model reliability failure — a
  fabricated causal link between unrelated accounts. Confirmed a
  prompt-only fix was insufficient through direct testing, and closed
  the issue completely with an independent, rule-based validator
  instead.
- Diagnosed and resolved real production deployment issues across
  seven projects: a cross-platform file-casing bug, IAM role
  misconfigurations, a silently-incomplete containerized pipeline, a
  secrets-management gap, cross-identity IAM permission issues, a
  networking/security-group misconfiguration, a Docker/uvicorn
  import-path issue, an unexplained file silently dropped from a build
  context, and a missing CloudWatch log-group permission. Each was
  verified fixed using direct evidence (AWS CLI output, or inspecting
  a built container's actual filesystem), not assumed from success.
- Compared 5+ modeling approaches per project with rigorous,
  metric-appropriate evaluation: MAPE, PR-AUC, ROC-AUC, top-K
  precision, per-class F1, retrieval accuracy, agent routing accuracy,
  reliability precision/recall. Diagnosed and explained why each
  approach won or lost, using direct statistical and mechanistic
  evidence rather than reporting metrics alone.

---

## Interview talking points (STAR-style, drawn from real events)

### "Tell me about a time you found a bug in production"
*The stale-Dockerfile bug (credit-risk project).* Three new model
scripts were added to a pipeline. The container kept exiting cleanly
with code 0. It looked successful. CloudWatch logs from two separate
runs were compared. Both runs stopped at the exact same point, every
time, regardless of a memory increase from 8GB to 16GB. That
non-result was the real clue. It ruled out an out-of-memory error and
pointed to a static, code-level cause instead. The local Dockerfile
was still only running the original 4-script pipeline.

### "Tell me about a hypothesis that didn't pan out"
*The autoencoder vs. Isolation Forest test (anomaly detection
project).* The prediction was that an autoencoder would fix Isolation
Forest's weak spot on one anomaly type. It did worse instead. This was
investigated directly rather than treated as a dead end. The two
models' rare correct catches turned out to be the exact same entries.
The weak anomaly type's statistical signal was categorically weaker
than what drove the autoencoder's actual successes. This was a
specific, actionable finding — one a confirmed hypothesis would never
have produced.

### "Tell me about a hypothesis that turned out to be right"
*The transformer vs. TF-IDF test (financial NLP project).* A TF-IDF
baseline systematically misclassified plain factual financial
sentences as positive. The hypothesis was that a transformer's
context-awareness would fix this specific failure mode. The exact same
15 failing sentences were re-run through the fine-tuned model
directly. All 15 were corrected.

### "Tell me about validating a business signal you found"
*The sentiment-vs-revenue correlation (financial NLP project).* A
strong lagged correlation (r=-0.728) appeared between filing sentiment
and real revenue growth — with only 7 data points. A leave-one-out
sensitivity analysis was run. Excluding one specific fiscal year
dropped the correlation to near zero. Excluding a different year
pushed it to statistical significance instead. That instability was
the real finding: the sample was too small to trust as a discovered
signal. This was reported honestly, rather than citing whichever
exclusion looked best.

### "Tell me about debugging something you couldn't reproduce"
*The AWS task-visibility mystery (financial NLP project).* A live ECS
task launched successfully but could not be found, through five
independent verification methods. Rather than keep guessing, the
identical task was relaunched and monitored in real time. The second
attempt succeeded and was caught immediately. This suggested the first
attempt likely also succeeded, but was missed due to an AWS-side
indexing delay, not a real infrastructure fault.

### "Tell me about building an agent, not just a chatbot"
*The financial RAG agent's tool-routing evaluation.* Three tools were
built: single-company search, cross-company comparison, and revenue
lookup. Routing was tested with 9 questions, including two deliberate
traps. One question contained the word "compare" while naming only one
real company — this should NOT trigger the comparison tool. Another
asked about revenue for a company with no loaded data — this SHOULD
still route to the revenue tool, which then honestly reports no data
rather than fabricating a number. Routing accuracy was 100% on both
traps specifically. This is real evidence the agent reasons about tool
constraints, rather than just matching keywords.

### "Tell me about verifying an LLM's output wasn't hallucinating"
*The RAG agent's groundedness investigation.* Real generated answers
relied heavily on direct quotation. This enabled a genuinely mechanical
check: extract every quoted claim, and verify it is a real substring
of its cited source. 5 of 9 initial quotes failed a strict check.
Rather than accept or dismiss that at face value, this was investigated
through four escalating, evidence-confirmed hypotheses. Final result:
8 of 9 were fully vindicated as genuine. 1 of 9 was a real, bounded
hallucination. It was precisely diagnosed and directly addressed in
the system prompt, rather than ignored or overstated as a systemic
failure.

### "Tell me about a deployment that didn't go smoothly"
*The financial RAG agent's live ECS Service deployment.* Five
genuinely distinct issues came up in one deployment session. Each was
diagnosed with direct CLI evidence, not guesswork: a secrets-management
gap, two separate IAM identities each needing their own distinct
permission grant, a permission change that silently didn't apply the
first time (caught only by verifying via CLI directly, not by trusting
the console's confirmation), a cross-platform tooling gap, and a
networking gap. The service had landed on the VPC's default security
group by omission, which permits no public inbound traffic at all.
Final result: a real HTTP request, from a local machine, over the
internet, correctly answered by a live container on AWS.

### "Tell me about building a system you didn't fully trust, and how you made it trustworthy"
*The document extraction project's reliability system.* A two-signal
"trust this or don't" system was built for structured invoice
extraction: confidence plus financial reconciliation. The first real
test showed the signal wasn't discriminating anything at all — 100%
agreement across every invoice, none flagged for review. Rather than
accept that, this was investigated directly. The reconciliation check
had a real, structural blind spot: it could only ever detect errors
that disturbed the financial math. A wrong line-item description, or a
wrong PO number, with every number still correct, sailed through
undetected. Three more blind spots were closed the same way, each
discovered by investigating a specific real failure, not designed in
advance: a discount that legitimately made line items not sum to
subtotal, corrupted GL codes and PO numbers from realistic OCR noise,
and a description paraphrased differently than expected. The final,
four-signal system reached 100% precision on every extraction it
recommends trusting. That number was then tested the hard way: a
completely fresh dataset the system had never seen in any form was
generated, and the result held exactly, on the metric that mattered
most.

### "Tell me about relying on a public dataset or library that turned out to be broken"
*Financial PhraseBank loading (financial NLP project).* The commonly
documented way to load this benchmark is fully broken. This was
resolved by bypassing the fragile library abstraction entirely —
downloading and parsing the raw source file directly. This is also a
more durable fix long-term, since it no longer depends on deprecated
script-execution support existing at all.

### "Tell me about a time research or documentation was wrong and you had to verify reality directly"
*The SROIE data format mismatch (document extraction project).* The
official documentation for a real academic benchmark dataset described
one file format. The actually-downloaded data, from a specific real
mirror, was structured completely differently — Parquet, not raw text
files. Rather than debug against assumptions, the real downloaded file
was inspected directly, and the loader was rebuilt to match reality.
This caught a second, non-obvious bug in the process: Parquet's
columnar format silently represents a missing value as a present key
holding `None`, not an absent key. This was confirmed only by testing
against a real Parquet round-trip, not synthetic Python dicts.

### "Tell me about testing whether a fix actually worked, not just assuming it did"
*The month-end close assistant's prompt-only fix, tested and found
insufficient.* A multi-agent close-analysis system fabricated a causal
link between two unrelated accounts, inventing a plausible-sounding
explanation that wasn't actually supported by anything it was given.
An explicit prompt instruction was added, telling the model not to do
exactly this. Rather than trust that it worked, the identical real
scenario was re-run. The model made the exact same mistake again, word
for word the same fabricated pattern. An independent, rule-based
validator was built instead — one that does not rely on the model's
own wording or confidence. It caught the issue completely. Holdout
evaluation later surfaced a second, different problem the same
disciplined way: a real per-category accuracy drop (one-time-event
classification fell to 43% on fresh data), traced directly to a
data-generation flaw, not a model weakness. It was fixed, and
re-verified with a second, independent holdout run before the fix was
trusted.

### "Tell me about a really difficult, frustrating debugging session"
*Deploying the month-end close assistant live.* Three genuinely
distinct real bugs stacked on top of each other during one deployment
session. First, a Docker/uvicorn import-path issue, fixed by switching
to `python -m uvicorn`, run from inside the source directory directly.
Second, a harder problem, where the identical error persisted after
that fix. This was investigated by ruling out every plausible cause in
sequence, with direct evidence at each step: confirming the Dockerfile
content itself, confirming a completely fresh `--no-cache` rebuild,
confirming the image genuinely existed, confirming the working
directory was correct. The real cause was found by inspecting the
built container's actual filesystem directly. One specific file was
being silently dropped from a broader folder copy, for reasons never
fully explained. It was fixed with an explicit, targeted copy of that
one file. Third, a real AWS IAM permission gap on the first live
deployment attempt, diagnosed directly from the real CloudWatch error
text, not guessed at. Each fix was verified with direct, real evidence
before moving to the next step, never assumed from a clean-looking
command. By the end, the service was live and correctly answering real
requests over the internet.

---

## Where each project's full write-up lives

- **Time-series forecasting**: [github.com/lordadrianbold/ts-forecast-project](https://github.com/lordadrianbold/ts-forecast-project)
- **Credit risk classification**: [github.com/lordadrianbold/credit-risk-project](https://github.com/lordadrianbold/credit-risk-project)
- **GL anomaly detection**: [github.com/lordadrianbold/gl-anomaly-project](https://github.com/lordadrianbold/gl-anomaly-project)
- **Financial NLP**: [github.com/lordadrianbold/financial-nlp-project](https://github.com/lordadrianbold/financial-nlp-project)
- **Financial RAG Agent**: [github.com/lordadrianbold/financial-rag-agent](https://github.com/lordadrianbold/financial-rag-agent)
- **Financial Document Extraction**: [github.com/lordadrianbold/financial-doc-extraction](https://github.com/lordadrianbold/financial-doc-extraction)
- **Month-End Close Assistant**: [github.com/lordadrianbold/month-end-close-assistant](https://github.com/lordadrianbold/month-end-close-assistant)

Double-check these URLs match your actual repo names exactly.

Each project's `notes.md` is the detailed, week-by-week technical
record. Each `README.md` is the front-door summary a reviewer sees
first.




