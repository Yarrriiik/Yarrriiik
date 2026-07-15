# Ark Polybot — Prediction-Market Research & Validation System

**Role:** Research & Validation Engineer  
**Project type:** Collaborative research and engineering project  
**Status:** Historical research and paper-shadow validation; not production or real-money validated

## 1. Overview

Ark Polybot was a collaborative system for researching algorithmic strategies on recurring cryptocurrency prediction markets. It combined historical-data processing, strategy simulation, live-market infrastructure, and a later web trading platform.

My work focused on the research and validation layer: turning strategy ideas into reproducible experiments, testing whether results were causal and executable, and requiring explicit evidence before a candidate could advance toward live use. The goal was not to advertise a profitable trading bot, but to build a disciplined process for rejecting unreliable results.

## 2. My Role

As the **Research & Validation Engineer**, I was responsible for:

- strict historical-data loading and bad-row auditing;
- backtesting and market-level strategy simulation;
- chronological and walk-forward validation;
- execution-cost, slippage, delay, and fill-realism modeling;
- rolling risk, drawdown, loss-streak analysis, and promotion gates;
- ML and policy-model experiments;
- causal audits for look-ahead and hindsight-selection;
- Python/Rust parity checks;
- Rust implementation and paper-shadow validation of a final candidate.

## 3. Research Pipeline

```mermaid
flowchart LR
    A["Historical market data"] --> B["Strict loader and data-quality audit"]
    B --> C["Reproducible strategy experiments"]
    C --> D["Chronological and walk-forward validation"]
    D --> E["Causal audit and look-ahead checks"]
    E --> F["Execution-cost and fill-realism stress tests"]
    F --> G["Rust strategy candidate"]
    G --> H["Paper-shadow run"]
    H --> I["Explicit promotion gate"]
```

The main research snapshot contained 15,840 market files. The strict loader produced 15,781 valid game records while separately rejecting 1,980 malformed or inconsistent rows. The 1,980 rejected items were individual data rows, not entire markets.

The audit also exposed a coverage limitation: much of the dataset did not contain the true opening seconds of a market. Strategies that depended on unavailable opening data were reclassified or rejected instead of being evaluated under an unsupported assumption.

Each strategy family followed the same validation discipline:

1. Define a causal signal using only information available at decision time.
2. Use later observable ticks for simulated entry and exit where required.
3. Apply explicit fees, slippage, delay, and fill assumptions.
4. Aggregate at market level to avoid tick-level overcounting.
5. Evaluate chronological splits, sequential folds, rolling risk, and drawdown.
6. Reject candidates that depended on outliers, optimistic fills, or unstable parameter cells.

Most hypotheses were rejected or parked. That was an intended outcome: the system was designed to falsify weak ideas before they reached live consideration.

## 4. Causal Audit: Invalidating a False Positive

The most important episode involved a strategy that initially appeared highly profitable in historical validation. A later audit showed that its selection process used hindsight when choosing among candidate signals.

The experiment was rebuilt so that the strategy had to act on the first eligible signal using only information available at that moment.

> A seemingly profitable backtest was invalidated after a causal audit revealed hindsight-selection. Matching Python and Rust implementations reproduced the failure, and the strategy was rejected from live consideration.

Python and Rust agreed to machine precision, with matching trade counts. The corrected result was negative, so the earlier return was not promoted.

## 5. Paper-Shadow Candidate

A separate filtered candidate was later implemented in Rust. It retained the existing live-causal trading logic while adding skip-only controls derived from trade-level audits.

The candidate showed positive historical behavior across the chronological split and all five sequential folds under baseline research assumptions. However, it did not survive the harshest combined execution-cost scenario and was classified only as a **research / paper-shadow candidate**.

A paper-shadow run produced only eight simulated trades, far below the predefined 100-trade promotion gate. The sample was insufficient for any profitability conclusion, and no real-money validation was performed. Confirmed resolver outcomes were also unavailable during that run, reinforcing the decision not to promote it.

## 6. Testing and Reproducibility

The final research branch passes 75 Rust tests. Six Rust test cases were added in the final candidate commit, covering filtered-strategy configuration and runtime regression behavior alongside the existing suite.

The project also retained reproducible research scripts, decision logs, audit tables, and paper-shadow runbooks while keeping large generated outputs outside version control.

No automated Python tests are discoverable on the final branch. The Python pipeline was exercised through full experiment runs and parity checks, but focused tests for strict loading, time ordering, cost accounting, and causal feature construction would be required before treating it as production-grade.

## 7. Team Boundaries

This was a collaborative project, not a solo build. I was responsible for the research workflow, data-quality controls, strategy experiments, chronological validation, execution-realism analysis, causal audit, Python/Rust parity, and the final paper-shadow gate.

A teammate later developed a web trading platform around an earlier version of the shared core, including a Svelte/SvelteKit frontend, Rust/Axum backend, PostgreSQL persistence, authentication, charts, backtest and live launchers, and realtime market feeds.

Part of my earlier contribution was already present in that shared core. My final filtered paper-shadow candidate remained on a separate branch and was not integrated into the platform. I do not claim authorship of the frontend, authentication, platform backend, or UI.

## 8. Limitations

- Historical results do not demonstrate real-world profitability.
- The final candidate failed the harshest execution-cost profile.
- The paper-shadow sample was far below its predefined promotion gate.
- No production deployment or real-money validation was completed.
- Automated Python test coverage is absent on the final branch.
- The final candidate and the later web platform remained on separate branches.
- This project is not presented as financial advice or as a source of stable returns.

## 9. Technology

**My research and engineering work:** Python, Rust, CatBoost and neural-policy experiments, CSV/SQLite research datasets, backtesting, chronological validation, simulation, regression testing, and Git-based experiment history.

**Broader team platform:** Rust/Axum, Svelte/SvelteKit, PostgreSQL, SQLite indexing, WebSockets, containerized services, market-data ingestion, and realtime visualization.

This public narrative intentionally excludes source code, raw market data, operational logs, credentials, account identifiers, private endpoints, exact strategy parameters, and sensitive infrastructure details.
