# EigenAgents — Agentic Application Showcase
#### _The characteristic agents of a problem space._

Reference implementations of agentic AI applications built by [EigenAgents](https://eigenagents.ai).

Each project here is a small, self-contained agent: a real problem, a multi-agent or structured-reasoning pipeline behind it, and a clean separation between the engine, the model interface, and the UI. They are meant to be read as much as run — the emphasis is on architecture, evaluation discipline, and reusable patterns.

## Agents

| Agent | What it does |
|---|---|
| [novelty-asymptote](./novelty-asymptote) | Evaluates trends, technologies, and cultural or financial themes against the *Novelty Asymptote* framework — an eight-stage multi-agent pipeline returning a schema-constrained verdict on where a trend sits in its lifecycle, how much novelty it has left, and what to do about it. |
| [content-credibility](./content-credibility) | Scores social-media content, ads, and viral claims for credibility across 11 weighted dimensions — non-partisan and non-commercial — returning per-axis scores, red flags, and a verdict tier from `CREDIBLE` to `EXTREME FAILURE`. |

## Shared design principles

- **Structured output over free text.** Agents return schema-constrained JSON that the UI renders directly — no scraping of model prose.
- **Secure-by-default model access.** API keys stay server-side behind a thin local proxy; nothing sensitive reaches browser-visible code.
- **Composable agents.** Specialized roles are composed behind a single orchestration layer that reconciles their findings into one verdict.
- **Read first, run second.** Each agent ships with its own README describing its architecture and how to run it.

## Author

Built by Vincent J. Kowalski — [LinkedIn](https://www.linkedin.com/in/vincent-kowalski-0a1892329/).

## License

MIT — see [LICENSE](./LICENSE).
