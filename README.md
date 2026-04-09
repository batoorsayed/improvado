# Ad Performance Dashboard

Unified advertising analytics dashboard for Facebook, Google, and TikTok — built as a technical assignment for Improvado.

**[Live demo](https://improvado-sda-batoors.streamlit.app/)**

---

## What it does

- Pulls ad data from three platforms into a single Neon PostgreSQL view
- Displays platform efficiency (CTR, conversion rate, CPA) side by side
- Ranks campaigns with a composite performance score (conv rate + CPA + CTR + volume)
- Surfaces best CPA, most impressions, and lowest CPC per current filter state
- Chat interface powered by Claude Haiku — answers questions about the visible data

## Stack

| Layer | Tool |
|---|---|
| Database | Neon (serverless PostgreSQL) |
| App | Streamlit |
| Charts | Plotly Express |
| AI | Anthropic Claude Haiku |
| Hosting | Streamlit Community Cloud |
| Tooling | uv, ruff |

## Local setup

```bash
# Install dependencies
uv sync

# Add secrets
cp .streamlit/secrets.toml.example .streamlit/secrets.toml
# fill in DATABASE_URL and ANTHROPIC_API_KEY

# Run
uv run streamlit run main.py
```

## Secrets format

```toml
ANTHROPIC_API_KEY = "sk-ant-..."

[connections.neon]
url = "postgresql://..."
dialect = "postgresql"
driver = "psycopg2"
```

## Architecture

```
Neon (unified_ads view)
    └── main.py
        ├── Sidebar filters (date, platform, campaign)
        ├── KPIs (spend, CTR, conv rate, CPA)
        ├── Platform efficiency bars
        ├── Spend vs Conversions scatter
        ├── Top 3 campaigns (composite score)
        └── Claude Haiku chat (filtered data as context)
```

## Campaign scoring

Top 3 campaigns are ranked by a composite score:

- **35%** — Conversion rate (conversions / clicks)
- **35%** — CPA, inverted (lower cost per acquisition = better)
- **20%** — CTR (clicks / impressions)
- **10%** — Impression volume (log-scaled)

All metrics are normalized 0–1 across the current filter state before scoring.

---

main.py was developed with Claude Code.
