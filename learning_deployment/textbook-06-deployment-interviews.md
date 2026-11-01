# Textbook 06 --- Deployment, Reliability and Interviews

**Days 86--100 \| Goal:** Package, test, document and explain AI
applications.

## 1. Configuration

Keep configuration outside source code. Use environment variables for
model name, database URL and endpoints. Commit `.env.example`; ignore
`.env`. Never commit keys, passwords or tokens.

## 2. Docker

A container packages an application and dependencies. A Dockerfile
describes image creation.

``` dockerfile
FROM python:3.11-slim
WORKDIR /app
COPY requirements.txt .
RUN pip install --no-cache-dir -r requirements.txt
COPY app ./app
EXPOSE 8000
CMD ["uvicorn", "app.main:app", "--host", "0.0.0.0", "--port", "8000"]
```

This is a learning baseline. Production images need pinned dependencies,
non-root execution, health checks, minimal packages and security
scanning.

Know image vs container, build vs run, port mapping, volumes,
environment variables and Compose.

## 3. Deployment

Understand dev/staging/prod, secrets, health/readiness checks, TLS,
persistent storage, migrations, scaling, resource limits and rollback.
Do not expose local Ollama or a development API publicly without
authentication and network controls.

## 4. Testing

-   Unit: one function/component.
-   Integration: components together.
-   API: endpoints, validation and status codes.
-   End-to-end: user journey.
-   AI evaluation: retrieval quality, answer grounding, citations, tool
    selection and latency.

Exact text matching is often brittle for AI outputs. Use curated
evaluation cases and meaningful criteria.

## 5. Logging and observability

Capture request ID, route/status, latency, model version where safe,
token usage/cost, retrieval IDs/scores, tool names and failures. Avoid
logging secrets or private documents. Use timeouts and bounded retries;
retries can duplicate side effects.

## 6. AI security

Treat user input, documents and tool output as untrusted. Validate
files, restrict tools, use least privilege, require human confirmation
for consequential actions, protect endpoints, avoid arbitrary code
execution, test prompt injection, separate tenant data and keep
dependencies updated.

## 7. GitHub portfolio

Each repository needs a clear name/description, problem, features,
architecture, tech stack, setup, example I/O, tests, limitations and
licence/data notes. Pin projects that show backend, RAG, agent and
practical ML integration.

Commit prefixes: `feat:`, `fix:`, `test:`, `docs:`, `refactor:`,
`chore:`. Make small genuine commits; never manufacture activity.

## 8. Python interview topics

Mutability, collections, shallow/deep copy, `*args`/`**kwargs`,
decorators, generators, iterators, context managers, exceptions, OOP,
composition, venvs, packages and async basics.

## 9. SQL interview topics

Joins, aggregation, CTEs, window functions, WHERE vs HAVING, keys,
indexes, transactions/ACID and SQL injection/parameterised queries.

## 10. AI application interview topics

AI vs ML vs deep learning vs GenAI; supervised/unsupervised; leakage and
overfitting; precision/recall/F1; tokens/context; embeddings;
attention/transformers; RAG/chunking; vector stores; RAG vs fine-tuning;
agents vs deterministic workflows; LangGraph state; tool validation;
hallucinations/evaluation; prompt injection; latency/cost/retries.

## 11. Project walkthrough

Explain: problem, constraints, architecture, your personal contribution,
design decisions, failure modes, evaluation and next improvements.
Distinguish prototype from production deployment and team results from
your own work.

## 12. Job strategy

Maintain two truthful resumes: Python/backend and AI application. Apply
throughout the 100 days. Disclose your 90-day notice accurately and
discuss early release only if employer policy permits.

## Day 100 assessment

-   [ ] Build/test a Python API.
-   [ ] Query a relational database.
-   [ ] Explain common ML models and evaluation.
-   [ ] Explain tokens, embeddings and transformers.
-   [ ] Build RAG with citations.
-   [ ] Build a bounded tool workflow.
-   [ ] Explain LangGraph state/routing.
-   [ ] Containerise an app.
-   [ ] Test, log and document an AI application.
-   [ ] Walk through two projects honestly.
-   [ ] Maintain clean GitHub portfolio and job pipeline.

Day 100 means you can build, explain, test and improve a practical AI
application---not that you have mastered every framework.
