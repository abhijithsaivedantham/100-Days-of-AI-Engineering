# Textbook 03 --- NLP, LLMs and Transformers

**Days 31--45 \| Goal:** Understand modern language-model concepts and
use pretrained models from Python.

## 1. NLP and tokenisation

NLP covers language tasks such as classification, extraction,
translation, summarisation, search and question answering.

A tokenizer converts text into tokens and token IDs. A token may be a
word, part of a word, punctuation or another unit. Tokenisation is
model-specific; one word is not necessarily one token. Token count
affects context limits, latency and cost.

## 2. Context window

The context window is the amount of tokenised input and output a model
can process in one request. Long documents require chunking, retrieval
or summarisation.

## 3. Embeddings

An embedding maps text into a numeric vector. Similar meanings often
have vectors close under a similarity measure. Uses include semantic
search, recommendations, clustering and RAG.

Know vector dimensions, model limits, language/domain behaviour and
compatibility. Use the same compatible embedding model for document and
query vectors in an index.

## 4. Language models

A language model estimates likely next tokens from previous context.
Autoregressive models generate one token at a time. Fluent output can
still be false; applications need grounding, validation and review.

## 5. Attention and transformers

Attention lets token representations incorporate information from
relevant positions. Self-attention relates tokens within the same
sequence. Conceptual transformer flow: 1. Convert tokens into vectors.
2. Add positional information. 3. Apply attention to combine contextual
information. 4. Apply feed-forward layers and normalisation. 5. Repeat
through transformer blocks.

Multi-head attention uses multiple projections to learn different
relationships. Understand query, key and value conceptually; detailed
matrix derivations are not required for this track.

## 6. Transformer families

-   Encoder-only: contextual representations; often
    classification/embeddings.
-   Decoder-only: next-token generation; common for GPT-style models.
-   Encoder-decoder: maps an input sequence to an output sequence.

## 7. Training terminology

-   Pretraining: learns broad patterns from large corpora.
-   Instruction tuning: improves instruction following.
-   Fine-tuning: further trains model parameters for a task/domain.
-   Inference: uses a trained model to generate output.

RAG supplies external context at inference; fine-tuning changes model
parameters.

## 8. Hugging Face

Model cards describe intended use, limitations, available training
information and licence. Check licence before public/commercial use.

``` python
from transformers import pipeline
classifier = pipeline("sentiment-analysis")
print(classifier("The explanation was useful."))
```

Model download, hardware and API details vary. Start with small models.

## 9. Ollama local lab

Ollama runs models locally and exposes a local API. Check installed
models with `ollama list`; use a model that fits your hardware.

``` python
import requests

response = requests.post(
    "http://localhost:11434/api/chat",
    json={
        "model": "llama3.2",
        "messages": [{"role": "user", "content": "Explain a Python generator."}],
        "stream": False,
    },
    timeout=120,
)
response.raise_for_status()
print(response.json()["message"]["content"])
```

Assumes Ollama is running and the model is installed. Check current docs
for model names and API changes.

### Mini-project 1 --- Token budget inspector

Accept text, count tokens with a tokenizer suitable for a selected model
(or clearly label an estimate), compare with a configurable context
budget, and warn when too large.

### Mini-project 2 --- Local assistant

Build a CLI assistant using Ollama. Configure model and endpoint, handle
timeouts, allow conversation reset, and document hardware/model
limitations.

## Assessment

Explain tokenisation, embeddings, context windows, next-token
generation, attention, transformer families, pretraining vs fine-tuning,
and why LLM output needs evaluation.
