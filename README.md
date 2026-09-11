# ML & AI Engineering Portfolio — Summary

This portfolio has ten end-to-end projects. Five are core ML projects.
Five are AI Engineering projects.

Together they cover supervised classification, time-series
forecasting, unsupervised anomaly detection, NLP, retrieval-augmented
agents, structured-output reliability engineering, multi-agent
orchestration, systematic evaluation/guardrails, fine-tuning, and
multimodal vision-direct extraction. Every project was deployed and
verified, not just built and left in a notebook.

The five AI Engineering projects close a real, current market gap.
Live research into current hiring data showed RAG, vector databases,
agent orchestration, structured-output reliability engineering,
systematic evaluation, and fine-tuning as the highest-premium skill
cluster. This was verified directly, not assumed. None of the five
original ML projects touch these skills.

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

## The five AI Engineering projects at a glance

| | Financial Doc Extraction | Month-End Close Assistant | Compliance-Aware AI Guardrails | Journal Entry Classifier + LayoutLMv2 | Multimodal Receipt Processing |
|---|---|---|---|---|---|
| **Problem type** | Structured extraction + output reliability engineering | Multi-agent orchestration | Systematic evaluation + defense-in-depth guardrails | Fine-tuning (SFT) + layout-aware document understanding | Vision-direct multimodal extraction |
| **Data** | 987 real SROIE receipts (ICDAR 2019) + 160 synthetic B2B invoices | Multi-month synthetic account balances, randomized realistic anomalies | 23 hand-written test cases (4 categories) + a genuinely fresh 20-case holdout set | 300 synthetic journal entries (12 GL categories) + the same real SROIE dataset from Project 1 | 200 real, photographed receipts (CORD-v2) + 11 hand-verified real US receipts |
| **Core technique** | LangChain structured output (function calling). A 4-signal reliability system: reconciliation, field plausibility, semantic similarity, header validation | LangGraph `StateGraph` directly. 5 specialized agents, including a real parallel fan-out | A five-layer real check: rule-based pre-check, dedicated LLM injection classifier, the assistant's own response, a post-response advice-language check, a usefulness judge | DistilBERT fine-tuning (GL classification) + LayoutLMv2 fine-tuning (layout-aware address extraction, using real bounding-box position) | Vision-direct extraction (Claude reads the image directly) vs. OCR-then-parse baselines, with a multi-run consistency check and cross-validation safety layer |
| **Core finding** | 7 distinct reliability blind spots found and fixed through direct investigation, not designed correctly upfront. Final result (100% precision on "trust this" predictions) confirmed to generalize on a genuinely fresh holdout set | 100% accuracy on every real anomaly type, confirmed on two independent holdout runs. A real model fabrication was caught by an independent validator, after a prompt-only fix was tested and found insufficient | The rule-based pre-check had a real, severe generalization gap (0/6 on genuinely new injection techniques), closed directly by adding a dedicated LLM classifier (6/6 on a second holdout run). A real blind spot in the scoring itself was found via live testing and fixed with a dedicated usefulness judge | Zero-shot journal classification reached 90.0%, with one dominant, real weakness (Accounts Receivable vs. Service Revenue confusion). Fine-tuning closed it to 100%, verified as genuine task learning (not memorization) on entirely novel vendors. LayoutLMv2 closed Project 1's real 0%-exact-match address ceiling to 97.2% token-level F1 — genuinely comparable to the published SROIE benchmark, after discovering the stricter exact-match metric had been giving a misleading picture of genuinely strong performance | Vision-direct beat OCR by 3.6x (87.9% vs. 24.5%/81% total-price accuracy). The project's worst failure (8 different wrong totals across 8 runs on one receipt) was traced past glare and QR codes to its real root cause: email compression destroying the image before the model ever saw it — reframing an apparent model limitation as a real, upstream data-quality problem |
| **Deployment** | A live ECS Service, deployed cleanly on the first attempt with zero new bugs. Three real fixes from the Month-End Close Assistant's deployment were applied proactively — direct evidence those fixes addressed real root causes, not just one project's symptoms | A live ECS Service with a real multi-agent graph and a live demo page. 3 distinct real deployment bugs were diagnosed and fixed with direct evidence at each step | A live, interactive ECS Service — visitors type any prompt, including real attempts to break it, and see exactly which layer catches it. Every lesson from prior deployments applied proactively; reached `RUNNING` on the first attempt | A live ECS Service (the LayoutLMv2 address-extraction model specifically), deployed on CPU-only Fargate as a deliberate cost trade-off (~100x cheaper per hour than GPU-backed EC2, reasonable for a one-question-at-a-time demo). Three real, distinct bugs found and fixed | A live ECS Fargate service, working end-to-end after resolving five distinct real infrastructure issues (three IAM permission gaps, a wrong deployment script for the actual environment, and a genuine SDK version bug), confirmed against a diverse batch of real receipts in production |
| **Real bugs found & fixed** | A real data-format mismatch (documentation vs. actual downloaded data), a Parquet schema quirk silently hiding missing values, a currency-comparison bug under-reporting real accuracy, and 4 distinct reliability-system blind spots — each found by refusing to accept a good-looking number at face value | A genuine uvicorn/Docker import-path issue, an unexplained file silently dropped from a broader folder copy, and a real IAM permission gap — each diagnosed with direct evidence (inspecting the built container's actual filesystem, checking the image's real existence) before being fixed | A confirmed generalization gap in rule-based prompt-injection detection, a real blind spot in evaluation scoring itself (checking a response existed, not that it was useful) that hid a factual hallucination and a recurring deflection pattern, and a judge-calibration check where an initial "looks fine" read was tested against a comparable case and found wrong | A BIO-labeling granularity misunderstanding (SROIE's "words" are whole lines, not tokens — confirmed failing 0/625 before the real fix), a severe word-to-token misalignment bug masking genuinely strong model performance (0.0% exact match despite healthy training loss), a disproven hypothesis about JALAN-line mislabeling, and a corrected false claim about required dataset size, checked directly against the real published paper | Three sequential real IAM/infrastructure issues (Secrets Manager, ECR pull, CloudWatch log group creation), a genuine `anthropic` SDK/httpx version incompatibility reproduced and fixed directly in sandbox, and a resolution-based safety check that was built too strict and corrected after real evidence disproved it |

---

## What this portfolio actually demonstrates

**Breadth across ML problem types and current AI Engineering skill
clusters.** This portfolio includes a genuine regression/forecasting
problem, a genuine imbalanced classification problem, a genuine
unsupervised problem with no labels at training time, a genuine NLP
problem spanning classical ML through fine-tuned transformers, a
genuine retrieval-augmented agentic system, genuine structured-output
reliability engineering, genuine multi-agent orchestration, genuine
systematic evaluation and defense-in-depth guardrails, genuine
fine-tuning across two model architectures (a text classifier and a
layout-aware document model), and genuine multimodal vision-direct
extraction. That is ten different core techniques and ten different
evaluation approaches. These are not variations on the same idea.

**A portfolio that responds to real market signal, not just personal
interest.** The five AI Engineering projects exist because live
research into current hiring data showed RAG/agent skills,
structured-output reliability engineering, multi-agent orchestration,
systematic evaluation, and fine-tuning carrying the highest wage
premium of any specific technical cluster. They were scoped and built
directly in response to that finding. Framework and technique choices
were verified against real, current, verifiable sources, not assumed
from possibly-stale training knowledge. Examples: LangChain's
`create_agent()` for the RAG project, LangGraph's `StateGraph`
directly for the Month-End Close Assistant, `all-mpnet-base-v2` as the
real published best-performer for paraphrase detection, and LayoutLMv2
as the real, published architecture achieving 95%+ F1 on layout-aware
address extraction — with the exact same 626-receipt training set
already in use, confirmed directly against the source paper rather
than assumed.

**Rigor that goes beyond running a model.** Every project includes a
specific, falsifiable hypothesis tested against real evidence. Every
project has real production bugs found and fixed with direct evidence.
Honest limitations are stated plainly, not glossed over. Several
projects show a strong result being independently, adversarially
re-checked rather than accepted: the Month-End Close Assistant's
holdout validation found a real, previously-invisible data-generation
flaw a smaller tuning sample had never exposed; the Journal Entry
Classifier's perfect 100% holdout score was treated with real
suspicion (not celebration) and re-verified against entirely novel
vendors before being trusted; the Compliance-Aware Guardrails project
found a real generalization gap in its own rule-based defense layer
specifically by testing it against a genuinely fresh, never-seen
holdout set. A strong result measured on the same data used to develop
a system cannot be fully trusted — that discipline is applied
consistently, not selectively.

**Real deployment experience, including real, reasoned scope
decisions that were later reconsidered.** All ten projects run live
on AWS. This was verified via actual CloudWatch logs and direct
file/S3 checks, never trusted from a clean exit code alone. The
Financial Doc Extraction project was initially, deliberately left
undeployed, with real, stated reasoning at the time: its
differentiator was reliability-engineering depth, not infrastructure
already proven elsewhere. That decision was later reconsidered. When
the deployment was actually carried out, it applied three real,
hard-won fixes from the Month-End Close Assistant's own deployment
proactively. The task reached `RUNNING` on the first attempt, with
zero new bugs — direct evidence those fixes generalized to a real
root cause, not just one project's specific symptoms. The same
discipline repeated again in the Guardrails and Journal Entry Classifier
deployments: lessons from each prior deployment were applied
proactively to the next, not rediscovered from scratch.

**Honest, well-characterized negative results, not just wins.** The
Journal Entry Classifier's LayoutLMv2 work is a strong example: a
well-tested class-weighting attempt produced no meaningful improvement
(a real, honest negative result correctly attributed to the loss
function not being the bottleneck), and a specific hypothesis about
mislabeled training data was tested directly and disproven (100% of
the relevant training examples were correctly labeled) before the real
root cause — a metric choice giving a misleading picture of genuinely
strong performance — was found. The Multimodal Receipt Processing
project's own fine-tuning attempt is reported the same way: zero
regression on standard data, but a genuine, unresolved failure on the
specific hard cases it targeted, with a clear, evidenced explanation
rather than a forced or hidden result.

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
- Designed and built a five-layer, defense-in-depth guardrails and
  systematic evaluation framework for a financial AI assistant (23
  hand-written adversarial/edge-case test cases, plus a genuinely
  fresh 20-case holdout set). Found and closed a real, confirmed
  generalization gap in rule-based prompt-injection detection (0/6 on
  novel techniques) by adding a dedicated LLM classifier layer,
  verified to fully close the gap on a second holdout run. Identified
  and fixed a real blind spot in the evaluation methodology itself,
  which had been masking a factual hallucination and a recurring
  response-quality problem.
- Fine-tuned two distinct model architectures for real accounting
  document-understanding tasks. A DistilBERT classifier improved
  general ledger coding from a 90.0% zero-shot baseline to 100%,
  verified as genuine task learning (not memorization) against
  entirely novel vendors. A LayoutLMv2 model, using real bounding-box
  layout position, closed a documented 0%-exact-match address
  extraction ceiling from a prior text-only pipeline to 97.2%
  token-level F1 — independently verified as comparable to the
  published research benchmark for this exact task.
- Built and evaluated a vision-direct multimodal document extraction
  pipeline (Claude reads receipt images directly), benchmarked against
  two OCR-then-parse baselines across 200 real, photographed receipts,
  winning by 3.6x on total-price accuracy. Investigated the project's
  most severe failure (repeated, wildly inconsistent hallucination on
  a specific receipt) to its actual root cause — image compression
  during transfer, not a model limitation — reframing an apparent AI
  reliability problem as a real, fixable data-pipeline issue.
- Diagnosed and resolved real production deployment issues across ten
  projects: a cross-platform file-casing bug, IAM role
  misconfigurations, a silently-incomplete containerized pipeline, a
  secrets-management gap, cross-identity IAM permission issues, a
  networking/security-group misconfiguration, a Docker/uvicorn
  import-path issue, an unexplained file silently dropped from a build
  context, a missing CloudWatch log-group permission, a genuine SDK
  version incompatibility, and a wrong deployment script for the
  actual target environment. Each was verified fixed using direct
  evidence (AWS CLI output, or inspecting a built container's actual
  filesystem), not assumed from success.
- Compared 5+ modeling approaches per project with rigorous,
  metric-appropriate evaluation: MAPE, PR-AUC, ROC-AUC, top-K
  precision, per-class F1, retrieval accuracy, agent routing accuracy,
  reliability precision/recall, token-level F1. Diagnosed and
  explained why each approach won or lost, using direct statistical
  and mechanistic evidence rather than reporting metrics alone.

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

*A second, more recent example: the class-weighted training attempt
(Journal Entry Classifier project, Part 2).* A real, well-tested
hypothesis — that reweighting rare training classes would close a
confirmed accuracy gap in address extraction — was set up carefully:
inverse-frequency weights computed directly from the actual training
label distribution, tested in isolation first to confirm the
weighting genuinely penalized rare-class errors more before spending
real GPU time on it. The full retraining produced essentially no
change. A genuine negative result, correctly interpreted: the
bottleneck wasn't the loss function at all — the real cause (a metric
choice, not a training deficiency) was found afterward.

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

*A related, more recent example: the multimodal receipt hallucination
investigation.* A receipt produced 8 different, wildly wrong totals
across 8 identical runs. Two direct hypotheses (glare, a QR code) were
tested with controlled comparisons and ruled out. The real cause was
found only when the person provided the genuine, uncompressed original
photo: the receipt had been silently compressed during email transfer
(640×480 pixels, down from a real 4032×3024), destroying the visual
data before the model ever received it. The same receipt, given its
real resolution, was read correctly on the first attempt. This
reframed the entire failure from an apparent AI reliability problem
into a real, fixable data-pipeline issue.

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

*A related, more recent example: the Multimodal Receipt Processing
deployment.* Four sequential real infrastructure issues surfaced one
at a time: the deploy script itself was written for the wrong shell
environment (bash instead of the actual native Windows/Cmd setup, requiring
a genuine rewrite), then three distinct IAM permission gaps on the
same task role (Secrets Manager access, ECR image pull, CloudWatch log
group creation) — each one only becoming visible after the previous
one was fixed and the task progressed further before failing again.
After all three were resolved, the container itself crashed on a real
application-level bug: a version incompatibility between the pinned
SDK and a newer underlying HTTP library, reproduced and confirmed
fixed directly in a local sandbox before redeploying.

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

*A related, more recent example: the Compliance-Aware Guardrails
project's defense-in-depth design.* A rule-based prompt-injection
pre-check performed well on its original test set (5/6 known
techniques caught, zero false positives). Tested against a genuinely
fresh holdout set of new injection techniques, it caught 0 of 6 — a
real, severe, confirmed generalization gap in pattern matching. Rather
than treat the overall system's still-strong score (6/6, via the
model's own in-context judgment and an independent post-check) as good
enough, a dedicated LLM classifier layer was added specifically to
close that gap — verified directly to work, catching 6/6 on a second
holdout run, confirmed by checking which specific layer blocked each
case rather than assuming the fix worked.

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

*A related, more recent example: the LayoutLMv2 metric investigation
(Journal Entry Classifier project).* An early, unverified claim —
that closing a real address-extraction accuracy gap would require
training sets "in the tens of thousands" — was checked directly
against the actual published LayoutLMv2 paper, rather than left
unverified. It was found to be false: the real paper fine-tunes on the
exact same 626-receipt training set already in use here, reaching
97.81% F1. The much lower number seen locally (2.8% exact match) was
then traced to a genuine metric mismatch, not a dataset-size problem:
the strict, plain-language "exact match" metric being used was far
harsher than the standard token-level F1 the published research
actually reports. Computing the same standard metric directly on the
already-trained model revealed a genuinely strong, comparable result
(97.2% F1) that had been hiding behind the wrong yardstick the entire
time.

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

*A related, more recent example: the Journal Entry Classifier's 100%
holdout result, deliberately not trusted at face value.* A perfect
score on holdout data is a signal for real suspicion, not celebration.
The original holdout set shared the same vendor pool as training data
— a model could reach 100% by memorizing vendor identity rather than
genuinely learning the task. A second, genuinely independent test was
built: 24 entries using vendors the model had never seen in any form,
directly verified with a real overlap check (confirmed to actually
catch a deliberately reused vendor before being trusted). Result: 24/24
— real evidence of genuine task learning, not memorization.

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

*A related, more recent example: the LayoutLMv2 training investigation
(Journal Entry Classifier project).* Evaluation showed 0.0% exact
match despite training's own loss converging cleanly — a stark,
confirmed mismatch between "the model is learning" and "the evaluation
says it isn't." The root cause took real, layered investigation to
find: LayoutLMv2's output is token-level (subword tokens, plus special
tokens), not word-level, and the original extraction code used a naive
positional slice that didn't correspond to real word boundaries at
all. The fix (the processor's real `word_ids()` alignment) was tested
directly against realistic mock data — including subword splitting,
special tokens, and truncation — before being trusted. Real
infrastructure setbacks compounded the difficulty: a required
dependency with no Windows support forced a move to Google Colab, an
overnight session disconnect lost an entire trained model to
non-persistent storage, and the free GPU tier's quota was confirmed
exhausted, requiring real compute units to be purchased to continue
same-day.

---

## Where each project's full write-up lives

- **Time-series forecasting**: [github.com/lordadrianbold/ts-forecast-project](https://github.com/lordadrianbold/time-series-forecast-project.git)
- **Credit risk classification**: [github.com/lordadrianbold/credit-risk-project](https://github.com/lordadrianbold/credit-risk-project)
- **GL anomaly detection**: [github.com/lordadrianbold/gl-anomaly-project](https://github.com/lordadrianbold/gl-anamoly-project.git)
- **Financial NLP**: [github.com/lordadrianbold/financial-nlp-project](https://github.com/lordadrianbold/financial-nlp-project)
- **Financial RAG Agent**: [github.com/lordadrianbold/financial-rag-agent](https://github.com/lordadrianbold/financial-rag-agent)
- **Financial Document Extraction**: [github.com/lordadrianbold/financial-doc-extraction](https://github.com/lordadrianbold/financial-doc-extraction)
- **Month-End Close Assistant**: [github.com/lordadrianbold/month-end-close-assistant](https://github.com/lordadrianbold/month-end-close-assistant)
- **Compliance-Aware AI Guardrails**: [github.com/lordadrianbold/ai-guardrails-eval](https://github.com/lordadrianbold/ai-guardrails-eval)
- **Journal Entry Classifier + LayoutLMv2**: [github.com/lordadrianbold/journal-entry-classifier](https://github.com/lordadrianbold/journal-entry-classifier)
- **Multimodal Receipt Processing**: [github.com/lordadrianbold/multimodal-receipt-processing](https://github.com/lordadrianbold/multimodal-receipt-processing)

Double-check these URLs match your actual repo names exactly.

Each project's `notes.md` is the detailed, week-by-week technical
record. Each `README.md` is the front-door summary a reviewer sees
first.






