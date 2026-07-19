<div align="center">

# 🔬 Lambda Benchmark Dashboard

**Find the cheapest, fastest memory configuration for your AWS Lambda — with data, not guesswork.**

![Python](https://img.shields.io/badge/Python-0b0b0e?style=for-the-badge&logo=python&logoColor=3776AB)
![FastAPI](https://img.shields.io/badge/FastAPI-0b0b0e?style=for-the-badge&logo=fastapi&logoColor=009688)
![AWS Lambda](https://img.shields.io/badge/AWS_Lambda-0b0b0e?style=for-the-badge&logo=awslambda&logoColor=FF9900)
![Chart.js](https://img.shields.io/badge/Chart.js-0b0b0e?style=for-the-badge&logo=chartdotjs&logoColor=FF6384)

</div>

## The problem

AWS Lambda pricing is a two-variable trap: more memory costs more per millisecond, but often runs *so much faster* that total cost drops. The only way to find the sweet spot is to benchmark — this dashboard automates that comparison across **6 memory configurations (128MB → 3008MB)** and **3 endpoint profiles** (health, I/O-bound, CPU-bound transform).

## What it measures

| Metric | Why it matters |
|---|---|
| ❄️ **Cold start init (ms)** | First-invocation latency your users feel after idle |
| 🔥 **Warm billed duration (ms)** | What AWS actually charges you for |
| 💰 **Cost per million requests** | The number that decides your memory config |
| 📊 **Memory utilization %** | Detect over-provisioning at a glance |
| ⚖️ **Weighted perf score** | Latency × cost composite for one-glance ranking |

## Dashboard views (8)

1. **Overview** — KPI cards, response-time chart, cold-start comparison, cost area chart, auto-generated insights
2. **Health endpoint** — latency + billed-duration charts, sortable table
3. **Transform endpoint** — CPU-bound profile analysis
4. **Cost analysis** — area charts + cost heatmap across all configs
5. **Performance** — radar chart (weighted score) + bubble scatter
6. **Cold start analysis** — init-time comparison, cold vs warm delta
7. **Memory utilization** — per-config progress cards
8. **Raw data** — searchable/sortable table + CSV download

## Live demo mode ⚡

Hit **"🚀 Demo Benchmark"** and the dashboard replays a full benchmark run **streamed over Server-Sent Events** — you watch the terminal-style execution log live, then every chart re-renders with the incoming data. No AWS account needed to explore.

## API

| Endpoint | Method | Purpose |
|---|---|---|
| `/` | GET | Dashboard UI (Jinja2) |
| `/api/data` | GET | All benchmark results as JSON |
| `/api/upload` | POST | Upload your own benchmark CSVs |
| `/api/demo-benchmark/stream` | GET | SSE stream of simulated run |
| `/api/download/{endpoint}` | GET | Export results as CSV |
| `/api/reset` | POST | Restore bundled sample data |

## Quick start

```bash
pip install -r requirements.txt
uvicorn main:app --host 0.0.0.0 --port 8000
# open http://localhost:8000
```

## Architecture notes

- **FastAPI + Jinja2** — single service, zero-JS-build frontend with Chart.js via CDN
- **CSV-driven** — benchmark results are plain CSVs in `benchmark_data/`; swap in your own via upload or file drop
- **SSE over WebSockets** — one-directional live streaming without connection-management overhead

## Sample finding (bundled data)

> 128MB is the *most expensive* config for the CPU-bound transform endpoint — higher-memory configs finish so much faster they cost less per million requests. Memory ≠ cost.
