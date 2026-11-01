# Textbook 05 --- Practical AI Application Projects

**Days 71--85 \| Goal:** Build two portfolio-grade applications.

# Project A --- AI Research Assistant (Days 71--77)

## Product brief

A user asks questions about a permitted document collection and receives
grounded answers with source references.

## Architecture

``` text
Client -> FastAPI -> Application service
                    |-> Ingestion: parser -> chunker -> embeddings -> vector store
                    |-> Query: embedding -> retriever -> context builder -> LLM
                                                        -> citation validator
```

## Requirements

-   Ingest supported text/PDF files with size limits.
-   Extract text and retain title/page metadata.
-   Chunk and embed documents.
-   Retrieve top-k relevant chunks.
-   Generate answers grounded in retrieved evidence.
-   Return source title/page/section.
-   Abstain when evidence is insufficient.
-   Support deletion/re-indexing.
-   Provide health endpoints.
-   Validate file type and size.
-   Keep secrets out of code.
-   Add timeouts, logging and tests.

## Suggested structure

``` text
research-assistant/
├── app/
│   ├── main.py
│   ├── api/
│   ├── schemas/
│   ├── services/ingestion.py
│   ├── services/retrieval.py
│   ├── services/answer.py
│   └── adapters/llm.py
├── tests/
├── data/sample_docs/
├── .env.example
├── requirements.txt
└── README.md
```

## Milestones

1.  CLI ingestion/retrieval baseline.
2.  Source metadata and citation mapping.
3.  Ollama model adapter.
4.  FastAPI endpoints.
5.  Tests and evaluation set.
6.  Optional UI after core works.
7.  README and short demo.

Create 15--30 evaluation questions: direct, multi-chunk, unanswerable,
ambiguous and prompt-injection attempts. Record expected sources and
observed quality.

# Project B --- Intelligent Task Automation Agent (Days 78--84)

## Product brief

A user requests a safe action such as creating a draft task, searching
an approved local knowledge base or summarising a report. The agent
follows a bounded workflow and calls explicitly registered tools.

Example tools: - `search_knowledge_base(query)` -
`create_task(title, due_date)` - `get_task(task_id)` -
`summarise_text(text)`

Use synthetic data and a local database. Do not connect the portfolio
demo to work systems or real external side effects.

## Workflow

``` text
User request -> classify intent -> select workflow
     -> validate arguments -> approval required?
        yes: pause for approval
        no: execute allowlisted tool
     -> summarise result -> response
```

Requirements: - LangGraph state tracks request, tool, arguments,
approval and result. - Limit tool calls and execution time. - Validate
inputs with Pydantic. - Make writes idempotent where possible. - Require
approval for consequential actions. - Record audit events. - Handle
unavailable tools and malformed model output. - Test prompt injection
and unauthorised tool requests.

Milestones: 1. Implement ordinary Python tools. 2. Add schemas and
tests. 3. Build deterministic routing. 4. Add LangGraph state and
conditional edges. 5. Add approval pause/resume. 6. Add logging and
failure handling. 7. Document safe demo.

# Optional Project C --- ML model serving

Expose a saved scikit-learn pipeline through FastAPI. Add `/predict`,
input schema, validation, model version, tests and README.

## Portfolio checklist

Each repository needs problem statement, user, architecture, features,
tech stack, setup, sample I/O, tests, limitations, data/model licence
notes and screenshots/demo. Never include secrets or proprietary
material.
