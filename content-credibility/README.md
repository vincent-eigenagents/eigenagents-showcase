# Content Credibility Analyzer

An **EigenAgents** agent that scores social-media content, advertisements, and viral claims for credibility across **11 weighted dimensions**, returning a schema-constrained verdict with per-axis scores, specific red flags, and a plain-language summary.

It is explicitly **non-partisan and non-commercial**: a single system prompt applies identical analytical standards to content from every political, commercial, and ideological source.

*Part of the [EigenAgents showcase](../README.md).*

---

## What it produces

For any pasted claim, ad, headline, or post, the agent returns a single JSON object containing:

- a 0–100 score for each of the 11 dimensions, plus a one-sentence note per axis
- a list of specific **red flags** found in the content
- a **verdict tier** — `CREDIBLE` · `QUESTIONABLE` · `HIGH RISK` · `EXTREME FAILURE`
- a weighted **final score** and a short plain-language summary

## Scoring dimensions

**Factual**

| Dimension | Weight |
|---|---|
| Factual plausibility | 12% |
| Scientific consensus | 12% |
| Source integrity | 10% |
| Logical coherence | 10% |
| Financial realism | 6% |
| Evidence vs. claims | 6% |

**Manipulation**

| Dimension | Weight |
|---|---|
| Emotional-manipulation-free | 6% |
| Vulnerable-population targeting | 11% |
| Hidden-agenda-free | 11% |
| Advertorial integrity | 13% |
| Exploitation sophistication (inverse) | 13% |

Two of the manipulation axes do unusually specific work. **Advertorial integrity** catches sales funnels disguised as journalism — the pattern where content manufactures a problem, validates it with selective science, dismisses the natural fix as impractical, and arrives at a product as the only conclusion. **Exploitation sophistication (inverse)** scores how deliberately the manipulation is engineered, on the principle that polished misinformation aimed at careful readers is more dangerous than crude misinformation.

## Architecture

```
Browser UI (index.html)  ──POST /analyze──▶  Flask backend  ──▶  Anthropic API (official SDK)
                                              │
                                              └─ API key read server-side from .env
```

- **Secure by default.** The key is read from `.env` on the server and used through the official `anthropic` SDK; it never reaches browser-visible code. The frontend only ever posts to the local `/analyze` route. A `/health` route reports status.
- **Structured output.** The model must return one JSON object matching a fixed schema. The backend strips any markdown fences, parses it, and returns explicit errors on malformed output rather than passing raw text through.
- **Symmetric standards.** The rubric and its weights live in a single system prompt, applied identically regardless of the content's source or viewpoint.

---

## Project structure

```
content-credibility/
├── app.py            # Flask backend: /analyze + /health, Anthropic SDK, 11-dimension rubric
├── index.html        # Single-page UI served by Flask
├── requirements.txt  # Python dependencies
└── .env.example      # Copy to .env and add your key
```

## Running it locally

```bash
cp .env.example .env          # then edit .env and add your ANTHROPIC_API_KEY
pip install -r requirements.txt
python3 app.py                # serves at http://localhost:5123
```

Requires Python 3. Dependencies: the official `anthropic` SDK, Flask, flask-cors, python-dotenv.

---

## License

MIT — see [LICENSE](../LICENSE).
