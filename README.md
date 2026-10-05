# Shubhankar Gupta

**AI engineer — LLM applications, retrieval (RAG), agent tooling (MCP), and production systems that stay up.**

Delhi, India (UTC+5:30) · open to remote work worldwide · **[portfolio](https://shubhankar360.github.io)** · [solquara.com](https://solquara.com) · shubhankar15august@gmail.com · [LinkedIn](https://www.linkedin.com/in/shubhankar-gupta-a73aa21a0)

I build LLM systems end to end — retrieval, generation, tool servers, evaluation,
serving — and I run a production AI studio's whole platform solo, which is where
I learned that the interesting part of engineering is what you can measure. Most
of what's below reports numbers, including the ones that came out against
expectation. Every AI repo here runs on a clean checkout with no API key, and CI
proves it on each push.

---

### [agentcheck](https://github.com/shubhankar360/agentcheck) — grade agents on what they did, not what they said

An evaluation harness for tool-using LLM agents: a deterministic back-office world with
seven tools and 36 tasks, graded on the **end state** of the world, a zero-tolerance
**policy** check over the action log, and the facts in the answer. The environment
enforces physics but not policy, so violations happen and get caught.

Two findings: a transcript-only grader agreed with ground truth **worse than chance
(κ = −0.40)** on an agent that skips checks, passing 16 runs that broke policy; and a
96% pass@1 agent gets all eight attempts right on only **75%** of tasks (pass^8). The
regression gate pairs an exact McNemar test with zero-tolerance policy checks, because
each catches what the other misses.

`Python` · `Anthropic SDK` · `pass^k` · `Cohen's κ` · `McNemar` · `22 tests` · `CI regenerates the report`

### [job-radar](https://github.com/shubhankar360/job-radar) — which remote jobs can you actually get?

Nine sources (Greenhouse, Ashby and Lever boards for 138 companies, Hacker News, five
aggregators): **12,523 postings a run**, each with an eligibility verdict and a reason,
and an itemised score whose parts provably add up. SQLite history flags reposted roles.
CV tailoring is evidence-constrained: it selects and reorders facts from a profile and
never writes new ones, and a test enforces that.

`Python` · `SQLite` · `data pipeline` · `headless Chrome` · `67 tests` · `stdlib only`

### [pg-hybrid-search](https://github.com/shubhankar360/pg-hybrid-search) — hybrid search inside Postgres, measured

Full-text, BM25 and pgvector retrieval fused with reciprocal rank fusion in one SQL
statement, benchmarked on BEIR SciFact. Hybrid with real BM25 reached **nDCG@10 0.712**
against 0.643 for vectors alone, but hybrid on Postgres's built-in `ts_rank_cd` made
results *worse* (0.597). pgvector's default `ef_search` silently returned **40 rows when
asked for 100**. The same tests run on PGlite and on a real pgvector server in CI, which
caught two portability bugs the in-process run could not.

`TypeScript` · `PostgreSQL` · `pgvector` · `BM25` · `Docker` · `14 tests on 2 engines`

### [aistudio-mcp](https://github.com/shubhankar360/aistudio-mcp) — Gemini, Veo and TTS as tools Claude can call

A zero-dependency Model Context Protocol server exposing Google AI Studio to
Claude: Gemini text and vision, Nano Banana image generation and editing, Omni
Flash and Veo 3.1 video with native audio, TTS voiceover, the Files API and live
model discovery. JSON-RPC over stdio implemented directly, long renders that
resume instead of timing out, and errors returned with a hint the model can act
on (404 → re-list models, 429 → back off).

Tested the way it runs: the suite spawns the real server, speaks JSON-RPC to it,
and points it at a **local mock of Google's API**, so it can assert on the exact
requests sent upstream — the key stays in a header, the echoed input image is
never mistaken for the output, TTS PCM gets a byte-correct WAV header.

`Node.js` · `MCP` · `Gemini API` · `Veo` · `12 end-to-end tests` · `CI on Node 18/20/22`

### [hybrid-rag-eval](https://github.com/shubhankar360/hybrid-rag-eval) — does hybrid search actually beat BM25?

A reproducible benchmark for the retrieval half of RAG. Four stacks — BM25,
dense, hybrid RRF, hybrid + reranking — over a labelled query set, scored on
Recall@k, MRR and nDCG alongside latency. Okapi BM25 and reciprocal rank fusion
implemented directly rather than imported, so a bad result is attributable.

The headline finding disagrees with the consensus: **fusion did not beat its best
leg**, BM25 was the strongest single stack at Recall@3 *and* 40× faster, and
reranking bought the best Recall@1 while losing the tail. The README explains
why, and the split-by-query-wording table shows where.

`Python` · `BM25` · `reciprocal rank fusion` · `cross-encoder reranking` · `27 tests`

### [rag-document-qa](https://github.com/shubhankar360/rag-document-qa) — cited answers, or an honest refusal

A full RAG service: recursive chunking with overlap and character offsets, vector
retrieval, context assembly, and answers that cite the exact passage they came
from. Claude for generation when a key is present, an extractive fallback when
it isn't, so the whole pipeline is verifiable with no credentials.

The part most demos skip is the guard that refuses when the documents *don't*
contain an answer. A similarity floor turned out not to work — measured on the
sample corpus, a nonsense question scored **0.907** against a relevant question's
**0.905**. Vocabulary coverage separates them cleanly (0.75–1.00 vs 0.00–0.25),
and both query sets are checked in as tests.

`Python` · `FastAPI` · `Streamlit` · `Anthropic API` · `sentence-transformers` · `65 tests`

### [promptdesk](https://github.com/shubhankar360/promptdesk) — LLM support agent with RAG and escalation

Grounds every answer in a retrieved knowledge base, classifies sentiment with a
chain-of-thought prompt, adapts tone, and uses a structured-output prompt to
decide when it can't help — opening a ticket automatically. Retrieval written
from scratch so it runs in demo mode with no API key.

`Python` · `FastAPI` · `Claude / OpenAI` · `Pydantic` · `SQLite`

---

### In production — [solquara.com](https://solquara.com)

Solquara is an AI video-ad and automation studio. I designed, built and operate
its entire platform as the only engineer:

- **Accounts, plans and entitlements** — auth, four plan tiers, usage limits, a
  one-time free sample keyed on user *and* canonical email, signed billing
  webhooks with replay protection. Covered by **125 end-to-end checks** run in a
  local WordPress sandbox (real PHP 8.3) and 112 against production, including
  IDOR, session and login-throttling cases.
- **Generative media pipeline** — keyframe-then-animate video generation that
  measured **~4× cheaper** than the platform's built-in narrated-video workflow
  for the same length of footage, and the MCP server above for driving Gemini and Veo from Claude.
- **Instant ad-concept writer** — a visitor enters their business and trade and
  reads a storyboarded 10-second ad in about 20 seconds; deterministic per input,
  so every concept is a shareable link and becomes the brief for the free sample.
- **Performance** — homepage **23 stylesheets → 2, 26 scripts → 1, HTML
  123 kB → 36 kB**. Profiling found `backdrop-filter` eating ~80% of the frame
  budget (6 fps → 55 fps once replaced). Effects gate on *measured* frame rate,
  because spec sniffing provably excluded capable hardware.
- **Regional pricing in 10 currencies** from one formula shared by build and
  browser, detected from the time zone with no IP lookup, and audited so a price
  can never exist only when a script succeeds.

---

### Machine learning

| Project | Result |
| --- | --- |
| [Heart disease classification](https://github.com/shubhankar360/Heart-disease-classification) | 88.5% test accuracy, F1 ≈ 0.87, recall ≈ 0.92 across three tuned models |
| [Wine quality prediction](https://github.com/shubhankar360/Wine-quality-prediction) | 13 models benchmarked; R² 0.52 regression, 93% accuracy / 0.95 ROC-AUC classification |
| [Movie recommendation system](https://github.com/shubhankar360/movies-recommendation-system) | Content-based recommender over TMDB 5000 with an NLP feature pipeline |

---

**Toolkit** — Python · TypeScript · FastAPI · Pydantic · PostgreSQL + pgvector · Docker · Anthropic Claude & OpenAI APIs · Gemini API ·
Model Context Protocol · sentence-transformers · BM25 / dense / hybrid retrieval ·
scikit-learn · pandas · NumPy · Node.js · JavaScript / TypeScript · PHP · WebGL · SQL ·
GitHub Actions · Git

**Currently** — building agent evaluation and retrieval tooling, and open to remote AI / LLM
engineering roles on distributed teams (UK, EU, US, UAE; contractor or EOR). [Portfolio and CV →](https://shubhankar360.github.io)
