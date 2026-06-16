# Novelty Asymptote Agent

An **EigenAgents** multi-agent analysis engine that evaluates trends, technologies, cultural movements, and financial themes against the **Novelty Asymptote** framework — the point at which a trend's novelty value approaches zero regardless of its objective quality.

Give it a subject — a product, a technology, a genre, a sector — and it runs an eight-stage agentic pipeline and returns a structured verdict: where the trend sits in its lifecycle, how much novelty it has left, what it risks being absorbed into, and what to do about it.

*Part of the [EigenAgents showcase](../README.md).*

---

## What it produces

Each subject is classified into exactly one of eight **trend states**:

| State | Meaning |
|---|---|
| `ASCENDING` | Rising, novelty reserves intact, alpha still available |
| `PEAKING` | Maximum visibility, smart money rotating out |
| `DECLINING` | Asymptote hit, novelty depleting, imitators dominate |
| `ABSORBING` | Core identity dissolving, elements metabolized into adjacent forms |
| `HIBERNATING` | Dormant with reserves intact — resurrection possible |
| `EXTINCT` | Hard death, no revival path |
| `STILLBORN` | Never reached escape velocity |
| `COUNTERCYCLICAL` | Rising precisely as the dominant trend peaks — highest-conviction signal |

It scores each subject across eight dimensions — recurrence, substitutability, workflow embeddedness, depth, social infrastructure, dilution ratio, incumbent-absorption risk, and the "brilliant-minds trap" — and applies a set of named analytical signals (e.g. the Publication Signal for crowded trades, an imitator-to-originator dilution metric, a standards-survival axiom, and a hibernation-reserve test).

---

## Architecture

```
Browser UI (HTML / React)  ──POST──▶  Flask proxy (/api/messages)  ──▶  Anthropic Messages API
                                       │
                                       └─ API key injected server-side (from .env)
```

Three design choices worth calling out:

1. **Secure proxy.** The API key lives only on the server, loaded from `.env`, and never appears in browser-visible code. The frontend posts to a local `/api/messages` route; the Flask layer attaches the key and forwards the request to Anthropic. A `/health` route reports configuration status without exposing the key.

2. **Multi-agent pipeline.** Eight specialized roles — Domain Classifier, Novelty Decay Scorer, Dilution Ratio, Incumbent Absorption Risk, Brilliant Minds Trap, Hibernation & Resurrection, Standards Viability, and a final Verdict synthesizer — are composed behind a single system prompt that enforces the framework and reconciles their findings into one verdict.

3. **Strict structured output.** The model must return a single JSON object conforming to a fixed schema (trend state, scores, trajectory, resurrection triggers, per-agent findings, key insight, verdict, action, and the closest historical parallel). The client parses and renders that object directly — no free-text scraping.

---

## Project structure

```
novelty-asymptote/
├── server.py                          # Flask app: serves the UI + proxies Anthropic, key server-side
├── eigenagents_novelty_asymptote.html # Deployable single-page UI (calls the local proxy)
├── novelty_asymptote_agent.jsx        # React component source for the same UI
├── requirements.txt                   # Pinned Python dependencies
├── .env.example                       # Copy to .env and add your key
├── setup.sh                           # One-time: create venv, install deps
└── start.sh                           # Run the server
```

---

## Running it locally

```bash
cp .env.example .env          # then edit .env and add your ANTHROPIC_API_KEY
bash setup.sh                 # creates venv, installs dependencies
bash start.sh                 # serves at http://127.0.0.1:5432
```

Requires Python 3. Dependencies (Flask, flask-cors, requests, python-dotenv) are pinned in `requirements.txt`.

---

## A note on the two frontends

The deployable app is **`eigenagents_novelty_asymptote.html`**, which is wired to the local `/api/messages` proxy so the key stays server-side. **`novelty_asymptote_agent.jsx`** is the React component source in its development form, where the model API is called directly (as it would be inside a hosted sandbox). Run the app via the HTML plus `server.py`.

---

## License

MIT — see [LICENSE](../LICENSE).
