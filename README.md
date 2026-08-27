# ML Engineering Portfolio — Summary

Five end-to-end projects, each covering a different core ML problem
type, all deployed and verified live on AWS (not just locally). Built to
demonstrate breadth (supervised classification, time-series forecasting,
unsupervised anomaly detection, NLP, and retrieval-augmented agents)
plus the deployment/production skills that separate an ML engineer from
someone who only builds notebooks.

The fifth project specifically closes a real, current market gap:
current ML engineering hiring data shows RAG, vector databases, and
agent orchestration frameworks (LangChain/LangGraph) as the highest-
premium skill cluster — verified via direct market research, not
assumed — and none of the first four projects touch them directly.

---

## The five projects at a glance

| | Time-Series Forecasting | Credit Risk Classification | GL Anomaly Detection | Financial NLP | Financial RAG Agent |
|---|---|---|---|---|---|
| **Problem type** | Regression / forecasting | Supervised classification (imbalanced) | Unsupervised anomaly detection | NLP: sentiment classification + real-world signal test | Retrieval-augmented generation + agentic tool use |
| **Data** | Apple's real SEC filings (35 quarters) | Real Lending Club data (1.05M rows, 425K resolved) | Synthetic GL data (20K entries, statistically verified against Benford's Law) | Financial PhraseBank (2,264 real sentences) + Apple's real 10-K filings (2015-2025) | 19 real 10-K filings across Apple, Microsoft, JPMorgan Chase |
| **Core technique** | SARIMA, Prophet, XGBoost, LSTM | Logistic regression, Random Forest, XGBoost (class-weighted + SMOTE) | Isolation Forest, autoencoder | TF-IDF+LogReg, FinBERT (zero-shot), DistilBERT (fine-tuned) | ChromaDB + sentence-transformers retrieval, LangChain `create_agent()`, Claude Haiku 4.5 |
| **Core finding** | Simpler models with explicit seasonal structure beat flexible/deep models on small data | Class weighting beats SMOTE; XGBoost wins on PR-AUC | A hypothesis was tested and refuted — models have complementary, mechanistically-explained strengths | A hypothesis was tested and completely confirmed (15/15); a "strong" real-world correlation was shown to be statistically fragile via leave-one-out analysis | 100% agent routing accuracy including 2 deliberately adversarial "trap" questions; a mechanical groundedness check found and precisely explained a real, bounded hallucination |
| **Deployment** | ECS Fargate batch task + FastAPI/Streamlit (Docker-tested) | ECS Fargate batch task, 525MB S3 data pipeline | ECS Fargate batch task, self-contained (generates own data) | ECS Fargate, 4 separate sub-pipeline task runs, cross-project S3 dependency | **A genuine live ECS Service** (not a batch task) — first project in this portfolio with real inbound networking and a real managed secret |
| **Real bugs found & fixed** | `uvicorn` import path gotcha, container filesystem isolation, S3 client timeout | Windows/Linux case-sensitivity, stale Docker CMD, IAM role mixup, missing log group permission | Same case-sensitivity/CMD lessons applied *proactively*; stale local files caught via S3 path verification | Broken/deprecated public dataset infrastructure (3 layers deep), a fiscal-year calculation bug, a slow Docker build diagnosed and fixed | 4 distinct real HTML parsing bugs, a silently-wrong extraction caught only by manual verification, and 5 distinct deployment/IAM/networking issues in a single live deployment session |

---

## What this portfolio actually demonstrates

**Breadth across ML problem types.** A genuine regression/forecasting
problem, a genuine imbalanced classification problem, a genuine
unsupervised problem with no labels at training time, a genuine NLP
problem spanning classical ML through fine-tuned transformers, and now
a genuine retrieval-augmented agentic system — five different core
techniques, five different evaluation approaches, not five variations
on the same idea.

**A portfolio that responds to real market signal, not just personal
interest.** The fifth project exists specifically because live research
into current ML hiring data showed RAG/agent skills carrying the
highest wage premium of any specific technical cluster — the project
was scoped, architected, and built directly in response to that
finding, including verifying framework choices (like LangChain's
`create_agent()`, confirmed as the current v1.0-era idiomatic approach
rather than the now-deprecated `AgentExecutor` pattern) against the
actual installed library version rather than trusting possibly-stale
training knowledge.

**Rigor that goes beyond running a model.** Every project includes a
specific, falsifiable hypothesis tested against real evidence, real
production bugs found and fixed with direct evidence, and honest
limitations stated plainly rather than glossed over. The fifth project
adds a genuinely new form of rigor: **mechanical, not eyeballed,
verification of LLM output** — a real, automated groundedness checker
that extracts every quoted claim from a generated answer and verifies
it against real source text, escalating through multiple hypotheses
when a check failed, rather than accepting a plausible-looking answer
at face value.

**Real deployment experience, including a genuinely new class of
problem.** All five projects run live on AWS, verified via actual
CloudWatch logs and direct file/S3 checks — never trusted from a clean
exit code alone. The fifth project's deployment was categorically
different from the first four: a live, continuously-running *service*
(not a batch job) needing real inbound networking and a real managed
secret for the first time in this portfolio, surfacing five genuinely
distinct infrastructure issues in a single deployment session — a
secrets-management gap, two separate IAM identities needing separate
permission grants, a permission change that silently failed to apply
(caught only by verifying via CLI rather than trusting the console), a
cross-platform tooling gap (`.sh` scripts don't run natively on
Windows), and a networking gap (the service landed on the VPC's default
security group, which has no public inbound rule at all) — each
diagnosed with direct evidence, not guesswork.

---

## Resume bullets (pick and adapt per application)

- Built and deployed five end-to-end ML pipelines (time-series
  forecasting, imbalanced classification, unsupervised anomaly
  detection, NLP sentiment analysis, and a retrieval-augmented agentic
  system) on real and realistic data, each containerized with Docker
  and deployed live on AWS (ECS Fargate batch tasks and a live ECS
  Service) with S3-backed output storage.
- Built a retrieval-augmented generation system and LangChain-based
  agent (`create_agent()`) with three distinct tools (single-company
  search, cross-company comparison, real financial data lookup) over
  real SEC 10-K filings, achieving 100% agent tool-routing accuracy on
  a held-out test set including deliberately adversarial questions
  designed to test genuine reasoning over surface keyword matching.
- Designed and implemented a mechanical groundedness verification
  system for LLM-generated financial answers — automatically extracting
  and validating every quoted claim against real source text, precisely
  diagnosing failure causes (encoding artifacts, grammatical
  conventions, real cross-year wording differences, and one genuine,
  bounded hallucination) rather than relying on manual review alone.
- Diagnosed and resolved real production deployment issues across five
  projects, including a cross-platform file-casing bug, IAM role
  misconfigurations, a silently-incomplete containerized pipeline, and
  — deploying this portfolio's first live, continuously-running web
  service — a secrets-management gap, cross-identity IAM permission
  issues, and a networking/security-group misconfiguration, each
  verified fixed using direct AWS CLI evidence rather than assuming
  success.
- Compared 5+ modeling approaches per project with rigorous, metric-
  appropriate evaluation (MAPE, PR-AUC, ROC-AUC, top-K precision, per-
  class F1, retrieval accuracy, agent routing accuracy); diagnosed and
  explained *why* each approach won or lost using direct statistical
  and mechanistic evidence rather than reporting metrics alone.

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
escalating, evidence-confirmed hypotheses (character-encoding
artifacts, a grammatical capitalization convention, a real wording
difference between two actual filing years, and cross-checking whether
a failing quote existed in a *different* retrieved source than the one
cited). Final result: 8 of 9 fully vindicated as genuine; 1 of 9 was a
real, bounded hallucination — precisely diagnosed and directly addressed
in the system prompt, rather than either ignored or overstated as a
systemic failure.

### "Tell me about a deployment that didn't go smoothly"
*The financial RAG agent's live ECS Service deployment.* Five genuinely
distinct issues in one deployment session, each diagnosed with direct
CLI evidence rather than guesswork: a secrets-management gap (the first
real credential anywhere in this portfolio, requiring AWS Systems
Manager Parameter Store rather than a plain environment variable), two
separate IAM identities each needing their own distinct permission
grant, a permission change that silently didn't apply the first time
(caught only by verifying via `aws iam list-attached-role-policies`
directly rather than trusting the console's confirmation), a
cross-platform tooling gap (`.sh` scripts don't execute natively on
Windows — worked around by running the exact same underlying `aws`
commands the scripts wrapped), and a networking gap (the service landed
on the VPC's default security group by omission, which permits no
public inbound traffic at all — identified by checking the *actual*
security group attached to the running task's network interface, not
assuming it was whichever one had been edited earlier for a different
purpose). Final result: a real HTTP request, from a local machine, over
the internet, correctly answered by a live container on AWS.

### "Tell me about relying on a public dataset or library that turned out to be broken"
*Financial PhraseBank loading (financial NLP project).* The commonly
documented way to load this benchmark is fully broken. Resolved by
bypassing the fragile library abstraction entirely — downloading and
parsing the raw source file directly — also a more durable fix
long-term, since it no longer depends on deprecated script-execution
support existing at all.

---

## Where each project's full write-up lives

- **Time-series forecasting**: [github.com/lordadrianbold/ts-forecast-project](https://github.com/lordadrianbold/ts-forecast-project)
- **Credit risk classification**: [github.com/lordadrianbold/credit-risk-project](https://github.com/lordadrianbold/credit-risk-project)
- **GL anomaly detection**: [github.com/lordadrianbold/gl-anomaly-project](https://github.com/lordadrianbold/gl-anomaly-project)
- **Financial NLP**: [github.com/lordadrianbold/financial-nlp-project](https://github.com/lordadrianbold/financial-nlp-project)
- **Financial RAG Agent**: [github.com/lordadrianbold/financial-rag-agent](https://github.com/lordadrianbold/financial-rag-agent)

*(Double-check these URLs match your actual repo names exactly.)*

Each project's `notes.md` is the detailed, week-by-week technical record;
each `README.md` is the front-door summary a reviewer sees first.




