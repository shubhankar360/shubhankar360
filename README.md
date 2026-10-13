# Shubhankar Gupta

I'm an AI engineer in Delhi. I build LLM systems end to end (retrieval, agents, the tools they call, and the tests that tell you whether any of it works), and I run the production platform behind [solquara.com](https://solquara.com) on my own.

I came to software late. I spent eight years running operations for my family's handicraft business before teaching myself to code, and the habit I brought with me is checking the numbers before believing the pitch. So most of what's here starts with a question and ends with a measurement, including the ones that went against what I expected.

Every AI repo below runs on a clean checkout without an API key, and CI checks that on every push.

## What I've been working on

| project | the question | what I found |
| --- | --- | --- |
| [handoff-graph](https://github.com/shubhankar360/handoff-graph) | Where should an agent's rules live: in the prompt, in the graph, or in a guard before each tool call? | With a careful model it makes no difference. With an eager one, a guard node kept it at 40/40; without the guard and the verify gate it broke policy in 12 of 40 conversations. |
| [agentcheck](https://github.com/shubhankar360/agentcheck) | How do you grade a tool-using agent? | A grader that only reads the transcript agreed with ground truth worse than chance (κ −0.40) and passed 16 runs that broke policy. |
| [voice-latency](https://github.com/shubhankar360/voice-latency) | Where does the time go in one turn of a voice agent? | Mostly in deciding the caller has finished. A 300 or 500 ms silence threshold cut off every caller who paused mid-sentence; only 1200 ms caught them all. Speech recognition added about 25 ms. |
| [pg-hybrid-search](https://github.com/shubhankar360/pg-hybrid-search) | Does hybrid search help inside Postgres? | Yes with real BM25 (nDCG@10 0.712 vs 0.643 for vectors alone), but Postgres's built-in ranking made it worse, and pgvector quietly returned 40 rows when asked for 100. |
| [rag-document-qa](https://github.com/shubhankar360/rag-document-qa) | Can a RAG service say "I don't know" honestly? | A similarity threshold couldn't tell an off-topic question from a real one (0.907 vs 0.905). Deploying it to Cloud Run also turned up a file-read hole in its own API, now closed. |
| [aistudio-mcp](https://github.com/shubhankar360/aistudio-mcp) | Can Claude drive Gemini, Veo video and TTS as tools? | An MCP server with no dependencies, tested end to end against a local mock of Google's API. |
| [hybrid-rag-eval](https://github.com/shubhankar360/hybrid-rag-eval) | Does hybrid retrieval beat BM25? | Not on a small, consistent corpus: BM25 won Recall@3 and was 40× faster. |
| [job-radar](https://github.com/shubhankar360/job-radar) | Which remote jobs can someone in India actually get? | About 12,500 postings a run from nine sources, each with an eligibility verdict and a reason. |

## Solquara

An AI video-ad and automation studio where I'm the only engineer. I built the generative-media pipeline (keyframes, then image-to-video, then voiceover, about 4× cheaper than the all-in-one option), the accounts and billing platform with 125 end-to-end checks, and a 40-route site that went from 123 kB to 36 kB of HTML on the homepage.

## Tools I use most

Python, TypeScript, LangGraph, the Claude and Gemini APIs, MCP, FastAPI, PostgreSQL with pgvector, Vosk speech-to-text, Docker, Terraform, Google Cloud Run, GitHub Actions.

## Get in touch

I'm open to remote AI engineering roles and to relocating. [Portfolio and CV](https://shubhankar360.github.io) · [LinkedIn](https://www.linkedin.com/in/shubhankar-gupta-a73aa21a0) · shubhankar15august@gmail.com
