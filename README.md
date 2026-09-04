# ML & AI Engineering Portfolio — Summary

Seven end-to-end projects spanning both core ML problem types and
current AI Engineering skill clusters — built to demonstrate genuine
breadth (supervised classification, time-series forecasting,
unsupervised anomaly detection, NLP, retrieval-augmented agents,
structured-output reliability engineering, and genuine multi-agent
orchestration) plus the deployment and verification discipline that
separates a real engineer from someone who only builds notebooks.

The fifth, sixth, and seventh projects specifically close a real,
current market gap: live research into current ML/AI engineering
hiring data showed RAG, vector databases, agent orchestration, and
structured-output reliability engineering as the highest-premium
skill cluster — verified directly, not assumed — and none of the
first four projects touch them.

---

## The seven projects at a glance

| | Time-Series Forecasting | Credit Risk Classification | GL Anomaly Detection | Financial NLP | Financial RAG Agent | Financial Doc Extraction | Month-End Close Assistant |
|---|---|---|---|---|---|---|---|
| **Problem type** | Regression / forecasting | Supervised classification (imbalanced) | Unsupervised anomaly detection | NLP: sentiment classification + real-world signal test | Retrieval-augmented generation + agentic tool use | Structured extraction + output reliability engineering | Genuine multi-agent orchestration |
| **Data** | Apple's real SEC filings (35 quarters) | Real Lending Club data (1.05M rows, 425K resolved) | Synthetic GL data (20K entries, statistically verified against Benford's Law) | Financial PhraseBank (2,264 real sentences) + Apple's real 10-K filings | 19 real 10-K filings across Apple, Microsoft, JPMorgan Chase | 987 real SROIE receipts (ICDAR 2019) + 160 synthetic B2B invoices | Multi-month synthetic account balances, randomized realistic anomalies |
| **Core technique** | SARIMA, Prophet, XGBoost, LSTM | Logistic regression, Random Forest, XGBoost (class-weighted + SMOTE) | Isolation Forest, autoencoder | TF-IDF+LogReg, FinBERT (zero-shot), DistilBERT (fine-tuned) | ChromaDB + sentence-transformers retrieval, LangChain `create_agent()`, Claude Haiku 4.5 | LangChain structured output (function calling), 4-signal reliability system (reconciliation, field plausibility, semantic similarity, header validation) | LangGraph `StateGraph` directly — 5 specialized agents, including a real parallel fan-out |
| **Core finding** | Simpler models with explicit seasonal structure beat flexible/deep models on small data | Class weighting beats SMOTE; XGBoost wins on PR-AUC | A hypothesis was tested and refuted — models have complementary, mechanistically-explained strengths | A hypothesis was tested and completely confirmed (15/15); a "strong" real-world correlation was shown to be statistically fragile via leave-one-out analysis | 100% agent routing accuracy including 2 deliberately adversarial "trap" questions; a mechanical groundedness check found and precisely explained a real, bounded hallucination | 7 distinct reliability blind spots found and fixed via direct investigation, not designed correctly upfront; final result (100% precision on "trust this" predictions) confirmed to generalize on a genuinely fresh holdout set | 100% accuracy on every real anomaly type, confirmed on two independent holdout runs; a real model fabrication caught by an independent validator after a prompt-only fix was tested and found insufficient |
| **Deployment** | ECS Fargate batch task + FastAPI/Streamlit (Docker-tested) | ECS Fargate batch task, 525MB S3 data pipeline | ECS Fargate batch task, self-contained (generates own data) | ECS Fargate, 4 separate sub-pipeline task runs, cross-project S3 dependency | A genuine live ECS Service — first project with real inbound networking and a real managed secret | Not deployed — this project's focus was extraction reliability engineering and holdout validation discipline, not infrastructure; a genuine, honest scope difference from the others | A genuine live ECS Service with a real multi-agent graph and a live demo page — 3 distinct real deployment bugs diagnosed and fixed with direct evidence at each step |
| **Real bugs found & fixed** | `uvicorn` import path gotcha, container filesystem isolation, S3 client timeout | Windows/Linux case-sensitivity, stale Docker CMD, IAM role mixup, missing log group permission | Same case-sensitivity/CMD lessons applied *proactively*; stale local files caught via S3 path verification | Broken/deprecated public dataset infrastructure (3 layers deep), a fiscal-year calculation bug, a slow Docker build diagnosed and fixed | 4 distinct real HTML parsing bugs, a silently-wrong extraction caught only by manual verification, and 5 distinct deployment/IAM/networking issues in a single live deployment session | A real data-format mismatch (documentation vs. actual downloaded data), a Parquet schema quirk silently hiding missing values, a currency-comparison bug under-reporting real accuracy, and 4 distinct reliability-system blind spots — each found by refusing to accept a good-looking number at face value | A genuine uvicorn/Docker import-path issue, an unexplained file silently dropped from a broader folder copy, and a real IAM permission gap — each diagnosed with direct evidence (inspecting the built container's actual filesystem, checking the image's real existence) before being fixed |

---

## What this portfolio actually demonstrates

**Breadth across ML problem types AND current AI Engineering skill
clusters.** A genuine regression/forecasting problem, a genuine
imbalanced classification problem, a genuine unsupervised problem with
no labels at training time, a genuine NLP problem spanning classical ML
through fine-tuned transformers, a genuine retrieval-augmented agentic
system, genuine structured-output reliability engineering, and now
genuine multi-agent orchestration — seven different core techniques
and seven different evaluation approaches, not variations on the same
idea.

**A portfolio that responds to real market signal, not just personal
interest.** The fifth, sixth, and seventh projects exist specifically
because live research into current hiring data showed RAG/agent
skills, structured-output reliability engineering, and multi-agent
orchestration carrying the highest wage premium of any specific
technical cluster — scoped and built directly in response to that
finding, including verifying framework and technique choices
(LangChain's `create_agent()` for the fifth project, LangGraph's
`StateGraph` directly for the seventh, `all-mpnet-base-v2` as the real
published best-performer for paraphrase detection with its own cited
optimal threshold) against real, current, verifiable sources rather
than trusting possibly-stale training knowledge.

**Rigor that goes beyond running a model.** Every project includes a
specific, falsifiable hypothesis tested against real evidence, real
production bugs found and fixed with direct evidence, and honest
limitations stated plainly rather than glossed over. The sixth and
seventh projects each add a genuinely new form of rigor beyond even
the fifth's mechanical groundedness checking: **iteratively-built
reliability systems, honestly holdout-validated** — rather than trust
a strong result measured on the same data used to develop it, a
completely fresh dataset (different random seed, confirmed genuinely
different data) was generated and evaluated with the unmodified
system, specifically to test whether the result was real or an
artifact of fitting to the tuning sample. In both cases it held up on
the metric that mattered most; specific sub-findings didn't always,
and that was reported honestly rather than hidden — the seventh
project's holdout run went further, actually finding a real,
previously-invisible data-generation flaw that the smaller tuning
sample had never exposed, fixing it, and re-verifying with a *second*
independent holdout run before trusting the fix.

**Real deployment experience where deployment was the point — and
honest scope boundaries where it wasn't.** Six of seven projects run
live on AWS, verified via actual CloudWatch logs and direct file/S3
checks — never trusted from a clean exit code alone. The sixth project
deliberately did not pursue deployment, focusing instead on a different
kind of rigor (reliability engineering, holdout validation) — a real,
stated scope choice, not an omission glossed over as if it weren't
there.

---

## Resume bullets (pick and adapt per application)

- Built and deployed five end-to-end ML pipelines (time-series
  forecasting, imbalanced classification, unsupervised anomaly
  detection, NLP sentiment analysis, and a retrieval-augmented agentic
  system) on real and realistic data, each containerized with Docker
  and deployed live on AWS (ECS Fargate batch tasks and a live ECS
  Service) with S3-backed output storage.
- Built a retrieval-augmented generation system and LangChain-based
  agent (`create_agent()`) with three distinct tools over real SEC
  10-K filings, achieving 100% agent tool-routing accuracy on a
  held-out test set including deliberately adversarial questions
  designed to test genuine reasoning over surface keyword matching.
- Designed and implemented a mechanical groundedness verification
  system for LLM-generated financial answers — automatically
  extracting and validating every quoted claim against real source
  text, precisely diagnosing failure causes rather than relying on
  manual review alone.
- Built a four-signal output reliability system for LLM-based
  structured document extraction (financial reconciliation, field
  format validation, semantic similarity via published research,
  header validation), iteratively discovering and closing genuine
  blind spots through direct investigation of real failures — and
  validated the final result on a genuinely held-out dataset never
  used in development, confirming the system's core reliability
  guarantee (100% precision on trusted predictions) generalizes rather
  than reporting a number fitted to the tuning data.
- Built a genuine multi-agent system with LangGraph's `StateGraph`
  directly (not a single agent choosing between tools) — five
  specialized agents including a real parallel fan-out, achieving 100%
  accuracy on every real anomaly type across two independent holdout
  evaluations. Diagnosed a real model reliability failure (a
  fabricated causal link between unrelated accounts), confirmed a
  prompt-only fix was insufficient through direct testing, and closed
  it completely with an independent, rule-based validator instead.
- Diagnosed and resolved real production deployment issues across six
  projects, including a cross-platform file-casing bug, IAM role
  misconfigurations, a silently-incomplete containerized pipeline, a
  secrets-management gap, cross-identity IAM permission issues, a
  networking/security-group misconfiguration, a Docker/uvicorn import-
  path issue, an unexplained file silently dropped from a build
  context, and a missing CloudWatch log-group permission — each
  verified fixed using direct evidence (AWS CLI output, or inspecting
  a built container's actual filesystem) rather than assuming success.
- Compared 5+ modeling approaches per project with rigorous, metric-
  appropriate evaluation (MAPE, PR-AUC, ROC-AUC, top-K precision, per-
  class F1, retrieval accuracy, agent routing accuracy, reliability
  precision/recall); diagnosed and explained *why* each approach won
  or lost using direct statistical and mechanistic evidence rather
  than reporting metrics alone.

---

## Interview talking points (STAR-style, drawn from real events)

### "Tell me about a time you found a bug in production"
*The stale-Dockerfile bug (credit-risk project).* After adding three new
model scripts, the container kept exiting cleanly with code 0 —
seemingly successful. Comparing CloudWatch logs across two runs showed
they stopped at the *exact same point* every time, regardless of a
memory increase from 8GB to 16GB — that non-result was the actual clue,
ruling out OOM and pointing toward a static, code-level cause. The
local Dockerfile still only ran the original 4-script pipeline.

### "Tell me about a hypothesis that didn't pan out"
*The autoencoder vs. Isolation Forest test (anomaly detection project).*
Predicted an autoencoder would fix Isolation Forest's weak spot on one
anomaly type — it did worse. Investigated directly rather than treating
it as a dead end: confirmed the two models' rare correct catches were
literally the same entries, and quantified that the weak anomaly type's
statistical signal was categorically weaker than what drove the
autoencoder's actual successes — a specific, actionable finding a
confirmed hypothesis wouldn't have produced.

### "Tell me about a hypothesis that turned out to be right"
*The transformer vs. TF-IDF test (financial NLP project).* A TF-IDF
baseline systematically misclassified plain factual financial sentences
as positive. Hypothesized a transformer's context-awareness would fix
this specific failure mode — re-ran the exact same 15 failing sentences
through the fine-tuned model directly: all 15 were corrected.

### "Tell me about validating a business signal you found"
*The sentiment-vs-revenue correlation (financial NLP project).* A
strong lagged correlation (r=-0.728) between filing sentiment and real
revenue growth appeared — with only 7 data points. A leave-one-out
sensitivity analysis showed excluding one specific fiscal year dropped
the correlation to near zero, while excluding a different year pushed
it to statistical significance — that instability was itself the real
finding: too small a sample to trust as a discovered signal, reported
honestly rather than citing whichever exclusion looked best.

### "Tell me about debugging something you couldn't reproduce"
*The AWS task-visibility mystery (financial NLP project).* A live ECS
task launched successfully but couldn't be found through five
independent verification methods. Rather than keep guessing,
relaunched the identical task and monitored it in real time — the
second attempt succeeded and was caught immediately, suggesting the
first likely also succeeded but was missed to an AWS-side indexing
delay, not a real infrastructure fault.

### "Tell me about building an agent, not just a chatbot"
*The financial RAG agent's tool-routing evaluation.* Built three tools
(single-company search, cross-company comparison, revenue lookup) and
tested routing with 9 questions — including two deliberate traps: one
containing the word "compare" while naming only one real company (which
should NOT trigger the comparison tool), and one asking about revenue
for a company with no loaded data (which SHOULD still route to the
revenue tool, which then honestly reports no data rather than
fabricating a number). 100% routing accuracy on both traps specifically
— real evidence the agent reasons about tool constraints, not just
keyword-matches.

### "Tell me about verifying an LLM's output wasn't hallucinating"
*The RAG agent's groundedness investigation.* Real generated answers
relied heavily on direct quotation, enabling a genuinely mechanical
check: extract every quoted claim, verify it's a real substring of its
cited source. 5 of 9 initial quotes failed a strict check — rather than
accepting or dismissing that at face value, investigated through four
escalating, evidence-confirmed hypotheses. Final result: 8 of 9 fully
vindicated as genuine; 1 of 9 was a real, bounded hallucination —
precisely diagnosed and directly addressed in the system prompt,
rather than either ignored or overstated as a systemic failure.

### "Tell me about a deployment that didn't go smoothly"
*The financial RAG agent's live ECS Service deployment.* Five genuinely
distinct issues in one deployment session, each diagnosed with direct
CLI evidence rather than guesswork: a secrets-management gap, two
separate IAM identities each needing their own distinct permission
grant, a permission change that silently didn't apply the first time
(caught only by verifying via CLI directly rather than trusting the
console's confirmation), a cross-platform tooling gap, and a networking
gap (the service landed on the VPC's default security group by
omission, which permits no public inbound traffic at all). Final
result: a real HTTP request, from a local machine, over the internet,
correctly answered by a live container on AWS.

### "Tell me about building a system you didn't fully trust, and how you made it trustworthy"
*The document extraction project's reliability system.* Built a
two-signal "trust this or don't" system for structured invoice
extraction — confidence plus financial reconciliation. First real test
showed the signal wasn't discriminating anything at all (100%
agreement across every invoice, none flagged for review). Rather than
accept that, investigated directly and found the reconciliation check
had a real, structural blind spot: it could only ever detect errors
that disturbed the financial math, so a wrong line-item description or
a wrong PO number — with every number still correct — sailed through
undetected. Closed three more blind spots the same way, each
discovered by investigating a specific real failure, not designed in
advance: a discount that legitimately made line items not sum to
subtotal, corrupted GL codes and PO numbers from realistic OCR noise,
and a description paraphrased differently than expected. Final,
four-signal system: 100% precision on every extraction it recommends
trusting. Then tested that number the hard way — generated a
completely fresh dataset the system had never seen in any form, and
confirmed the result held. It did, exactly, on the metric that
mattered most.

### "Tell me about relying on a public dataset or library that turned out to be broken"
*Financial PhraseBank loading (financial NLP project).* The commonly
documented way to load this benchmark is fully broken. Resolved by
bypassing the fragile library abstraction entirely — downloading and
parsing the raw source file directly — also a more durable fix
long-term, since it no longer depends on deprecated script-execution
support existing at all.

### "Tell me about a time research/documentation was wrong and you had to verify reality directly"
*The SROIE data format mismatch (document extraction project).* The
official documentation for a real academic benchmark dataset described
one file format; the actually-downloaded data (from a specific real
mirror) was structured completely differently — Parquet, not raw text
files. Rather than debug against assumptions, inspected the real
downloaded file directly, rebuilt the loader to match reality, and
caught a second, non-obvious bug in the process: Parquet's columnar
format silently represents a missing value as a present key holding
`None`, not an absent key — confirmed only by testing against a real
Parquet round-trip, not synthetic Python dicts.

### "Tell me about testing whether a fix actually worked, not just assuming it did"
*The month-end close assistant's prompt-only fix, tested and found
insufficient.* A multi-agent close-analysis system fabricated a causal
link between two unrelated accounts, inventing a plausible-sounding
explanation that wasn't actually supported by anything it was given.
Added an explicit prompt instruction telling the model not to do
exactly this — then, rather than trust that it worked, re-ran the
identical real scenario. The model made the exact same mistake again,
word for word the same fabricated pattern. Built an independent,
rule-based validator instead — one that doesn't rely on the model's
own wording or confidence — which caught it completely. Holdout
evaluation later surfaced a second, different problem the same
disciplined way: a real per-category accuracy drop (one-time-event
classification fell to 43% on fresh data) traced directly to a data-
generation flaw, not a model weakness, fixed, and re-verified with a
second independent holdout run before trusting the fix.

### "Tell me about a really difficult, frustrating debugging session"
*Deploying the month-end close assistant live.* Three genuinely
distinct real bugs stacked on top of each other during one deployment
session. A Docker/uvicorn import-path issue, fixed by switching to
`python -m uvicorn` run from inside the source directory directly. A
second, harder problem where the identical error persisted after that
fix — investigated by ruling out every plausible cause in sequence
with direct evidence (confirming the Dockerfile content itself,
confirming a completely fresh `--no-cache` rebuild, confirming the
image genuinely existed, confirming the working directory was
correct) before finding the real cause by inspecting the built
container's actual filesystem directly: one specific file was being
silently dropped from a broader folder copy, for reasons never fully
explained, fixed with an explicit, targeted copy of that one file.
Then a real AWS IAM permission gap on the first live deployment
attempt, diagnosed directly from the real CloudWatch error text rather
than guessed at. Each fix was verified with direct, real evidence
before moving to the next step — not assumed from a clean-looking
command — and the service was live and correctly answering real
requests over the internet by the end.

---

## Where each project's full write-up lives

- **Time-series forecasting**: [github.com/lordadrianbold/ts-forecast-project](https://github.com/lordadrianbold/ts-forecast-project)
- **Credit risk classification**: [github.com/lordadrianbold/credit-risk-project](https://github.com/lordadrianbold/credit-risk-project)
- **GL anomaly detection**: [github.com/lordadrianbold/gl-anomaly-project](https://github.com/lordadrianbold/gl-anomaly-project)
- **Financial NLP**: [github.com/lordadrianbold/financial-nlp-project](https://github.com/lordadrianbold/financial-nlp-project)
- **Financial RAG Agent**: [github.com/lordadrianbold/financial-rag-agent](https://github.com/lordadrianbold/financial-rag-agent)
- **Financial Document Extraction**: [github.com/lordadrianbold/financial-doc-extraction](https://github.com/lordadrianbold/financial-doc-extraction)
- **Month-End Close Assistant**: [github.com/lordadrianbold/month-end-close-assistant](https://github.com/lordadrianbold/month-end-close-assistant)

*(Double-check these URLs match your actual repo names exactly.)*

Each project's `notes.md` is the detailed, week-by-week technical record;
each `README.md` is the front-door summary a reviewer sees first.




