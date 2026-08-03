# Shubhankar Gupta

**AI / LLM engineer — retrieval-augmented generation, Python, and shipping things that stay up.**

Delhi, India (UTC+5:30) · open to remote work worldwide · [solquara.com](https://solquara.com) · shubhankar15august@gmail.com

I build RAG systems end to end — chunking, embeddings, hybrid retrieval,
reranking, evaluation, serving — and I run a production website solo, which is
where I learned that the interesting part of engineering is what you can
measure. Most of what's below reports numbers, including the ones that came out
against expectation.

---

### 🔍 [hybrid-rag-eval](https://github.com/shubhankar360/hybrid-rag-eval) — does hybrid search actually beat BM25?

A reproducible benchmark for the retrieval half of RAG. Four stacks — BM25,
dense, hybrid RRF, hybrid + reranking — over a labelled query set, scored on
Recall@k, MRR and nDCG alongside latency. Okapi BM25 and reciprocal rank fusion
implemented directly rather than imported, so a bad result is attributable.

The headline finding disagrees with the consensus: **fusion did not beat its best
leg**, BM25 was the strongest single stack at Recall@3 *and* 40× faster, and
reranking bought the best Recall@1 while losing the tail. The README explains
why, and the split-by-query-wording table shows where. Runs offline, no API keys.

`Python` · `BM25` · `reciprocal rank fusion` · `cross-encoder reranking` · `27 tests`

### 📄 [rag-document-qa](https://github.com/shubhankar360/rag-document-qa) — cited answers, or an honest refusal

A full RAG service: recursive chunking with overlap and character offsets, vector
retrieval, context assembly, and answers that cite the exact passage they came
from. Claude for generation when a key is present, an extractive fallback when
it isn't, so the whole pipeline is verifiable with no credentials.

The part most demos skip is the guard that refuses when the documents *don't*
contain an answer. A similarity floor turned out not to work — measured on the
sample corpus, a nonsense question scored **0.907** against a relevant question's
**0.905**. Vocabulary coverage separates them cleanly (0.75–1.00 vs 0.00–0.25),
and both query sets are checked in as tests.

`Python` · `FastAPI` · `Streamlit` · `Anthropic API` · `65 tests`

### 🤖 [promptdesk](https://github.com/shubhankar360/promptdesk) — LLM support agent with RAG and escalation

Grounds every answer in a retrieved knowledge base, classifies sentiment with a
chain-of-thought prompt, adapts tone, and uses a structured-output prompt to
decide when it can't help — opening a ticket automatically. Retrieval written
from scratch so it runs in demo mode with no API key.

`Python` · `FastAPI` · `Claude / OpenAI` · `SQLite`

---

### Machine learning

| Project | Result |
| --- | --- |
| [Heart disease classification](https://github.com/shubhankar360/Heart-disease-classification) | 88.5% test accuracy, F1 ≈ 0.87, recall ≈ 0.92 across three tuned models |
| [Wine quality prediction](https://github.com/shubhankar360/Wine-quality-prediction) | 13 models benchmarked; R² 0.52 regression, 93% accuracy / 0.95 ROC-AUC classification |
| [Movie recommendation system](https://github.com/shubhankar360/movies-recommendation-system) | Content-based recommender over TMDB 5000 with an NLP feature pipeline |

### Beyond the repos — [solquara.com](https://solquara.com)

I designed, built and operate the site as its only engineer. Measured on the
homepage: **23 stylesheets → 2, 26 scripts → 1, HTML 123 kB → 36 kB**. Profiling a
scroll-driven glass UI found `backdrop-filter` eating ~80% of the frame budget
(22px blur → 6fps, none → 55fps); replacing it with a 2 kB build-time plate kept
the look at full frame rate. The perf guard samples real frame rate for two
seconds rather than sniffing device specs — because spec sniffing provably
excluded capable hardware.

---

**Toolkit** — Python · LangChain · LlamaIndex · FastAPI · Anthropic & OpenAI APIs ·
ChromaDB / FAISS / Pinecone · sentence-transformers · RAGAS · scikit-learn ·
pandas · NumPy · JavaScript / TypeScript · WebGL · Git

**Currently** — deepening RAG evaluation and agent tooling, and open to remote
LLM / RAG engineering roles on distributed teams.
