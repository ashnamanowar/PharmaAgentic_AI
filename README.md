# 💊 Pharma Agentic AI

> **A Multi-Agent Intelligence Platform for Pharmaceutical Strategy**
> Built with CrewAI · Groq Llama 3 70B · Streamlit

[![Live Demo](https://img.shields.io/badge/🚀_Live_Demo-Streamlit-FF4B4B?style=for-the-badge)](https://pharma-ai-tensorz.streamlit.app/)
[![Python](https://img.shields.io/badge/Python-3.10+-3776AB?style=for-the-badge&logo=python&logoColor=white)](https://python.org)
[![CrewAI](https://img.shields.io/badge/CrewAI-Hierarchical-6C63FF?style=for-the-badge)](https://crewai.com)
[![Groq](https://img.shields.io/badge/Groq-Llama_3_70B-F54E00?style=for-the-badge)](https://groq.com)

---

## 🌐 Live Demo

**[→ Try it now: pharma-ai-tensorz.streamlit.app](https://pharma-ai-tensorz.streamlit.app/)**

---

## 🎯 Overview

Pharma Agentic AI orchestrates **8 specialized AI agents** to answer complex pharmaceutical strategy questions — from market whitespace analysis to patent freedom-to-operate checks — in seconds.

| Agent | Role | What It Does |
|-------|------|--------------|
| 📊 **IQVIA Agent** | Market Data Specialist | Market size, CAGR, competition levels, whitespace identification |
| 📜 **Patent Agent** | IP Legal Analyst | Patent expiry timelines, FTO assessment, generic entry windows |
| 🚢 **EXIM Agent** | Supply Chain Analyst | Import/export volumes, pricing benchmarks, sourcing options |
| 🔬 **Clinical Agent** | R&D Pipeline Analyst | Trial landscape mapping, repurposing opportunity detection |
| 💬 **Social Agent** | Patient Voice Analyst | Sentiment analysis, unmet needs, complaint pattern mining |
| ⚔️ **Competitor Agent** | Strategic War Gamer | Competitor prediction, threat assessment, launch simulation |
| 📁 **Internal Agent** | Corporate Strategist | RAG-powered search over internal documents |
| 🌐 **Web Agent** | External Researcher | Real-time news, regulatory approvals, market developments |

---

## 🏗️ Architecture

```
┌─────────────────────────────────────────────────────────────┐
│                    Streamlit Frontend                        │
│                  (Chat Interface + Reports)                  │
└─────────────────────────┬───────────────────────────────────┘
                          │
┌─────────────────────────▼───────────────────────────────────┐
│                    Master Orchestrator                       │
│              (CrewAI Hierarchical Process)                   │
└─────────────────────────┬───────────────────────────────────┘
                          │
    ┌─────────┬─────────┬─┴───────┬─────────┬─────────┐
    ▼         ▼         ▼         ▼         ▼         ▼
┌───────┐ ┌───────┐ ┌───────┐ ┌───────┐ ┌───────┐ ┌───────┐
│ IQVIA │ │Patent │ │Clinical│ │Social │ │Compet.│ │  ...  │
│ Agent │ │ Agent │ │ Agent │ │ Agent │ │ Agent │ │       │
└───┬───┘ └───┬───┘ └───┬───┘ └───┬───┘ └───┬───┘ └───────┘
    │         │         │         │         │
    ▼         ▼         ▼         ▼         ▼
┌───────────────────────────────────────────────────────────┐
│                     Mock Data Layer                        │
│  (JSON files simulating IQVIA, USPTO, ClinicalTrials...)   │
└───────────────────────────────────────────────────────────┘
```

---

## 🚀 Quick Start

### Prerequisites

- Python 3.10+
- A [Groq API key](https://console.groq.com/keys) (free tier available)
- A [Tavily API key](https://tavily.com/) for web search

### 1. Clone & Install

```bash
git clone <your-repo-url>
cd Pharma
pip install -r requirements.txt
```

### 2. Configure Environment

Create a `.env` file in the project root:

```env
GROQ_API_KEY=your-groq-api-key-here
GROQ_MODEL=llama3-70b-8192
TAVILY_API_KEY=your-tavily-api-key-here
```

### 3. Run Tests

```bash
python test_golden_tasks.py
```

### 4. Launch the App

```bash
streamlit run app.py
```

### 5. (Optional) Launch the REST API

```bash
uvicorn src.api.main:app --reload --port 8000
```

API docs available at: [http://localhost:8000/docs](http://localhost:8000/docs)

---

## ⚡ Minimal Run (No Queue Infrastructure)

If you just want to get up and running without Redis/Celery/Kafka:

```bash
# Set environment variables
export GROQ_API_KEY=your-key
export TAVILY_API_KEY=your-key
export USE_ASYNC_QUEUE=false

# Install and start
pip install -r requirements.txt
uvicorn src.api.main:app --host 0.0.0.0 --port 8000
streamlit run app.py
```

Queries execute inline. Enable async processing later by setting `USE_ASYNC_QUEUE=true` and bringing up the queue infrastructure.

---

## 🎯 Example Queries (Golden Tasks)

| # | Use Case | Example Query |
|---|----------|---------------|
| 1 | **Whitespace Analysis** | *"Which respiratory diseases show low competition but high patient burden in India?"* |
| 2 | **Drug Repurposing** | *"Identify potential repurposing opportunities for Pembrolizumab."* |
| 3 | **FTO Check** | *"Check patent expiry for Sitagliptin in the US."* |
| 4 | **Patient Voice** | *"What are patients complaining about regarding Diabetes injectables?"* |
| 5 | **Competitive War Game** | *"Simulate a launch of generic Rivaroxaban in 2025."* |

---

## 📁 Project Structure

```
Pharma/
├── app.py                      # Streamlit frontend
├── requirements.txt            # Python dependencies
├── .env                        # Environment configuration (git-ignored)
├── test_golden_tasks.py        # Validation test suite
│
├── src/
│   ├── agents/
│   │   ├── worker_agents.py    # 8 specialized agents
│   │   └── master_agent.py     # Orchestrator (CrewAI Hierarchical)
│   │
│   ├── tools/
│   │   ├── iqvia_tool.py       # Market data queries
│   │   ├── patent_tool.py      # Patent / FTO analysis
│   │   ├── exim_tool.py        # Trade data queries
│   │   ├── clinical_tool.py    # Clinical trials queries
│   │   ├── social_tool.py      # Social media sentiment
│   │   ├── competitor_tool.py  # War gaming tools
│   │   ├── internal_tool.py    # Internal docs RAG
│   │   └── web_tool.py         # Web search (Tavily)
│   │
│   ├── services/
│   │   ├── rag_service.py      # ChromaDB vector store
│   │   └── report_generator.py # PDF / Excel generation
│   │
│   ├── api/
│   │   └── main.py             # FastAPI REST API
│   │
│   └── config/
│       └── settings.py         # Configuration management
│
├── mock_data/                  # Simulated data sources
│   ├── iqvia_market_data.json
│   ├── clinical_trials.json
│   ├── uspto_patents.json
│   ├── exim_trade_data.json
│   ├── social_media_posts.json
│   ├── competitor_strategies.json
│   ├── internal_docs_metadata.json
│   └── web_search_results.json
│
└── reports/                    # Generated PDF / Excel outputs
```

---

## 🔧 REST API Reference

| Method | Endpoint | Description |
|--------|----------|-------------|
| `POST` | `/query` | Run a full multi-agent query |
| `POST` | `/tools/{tool_name}` | Call a specific tool directly |
| `GET` | `/agents` | List all available agents |
| `GET` | `/examples` | Browse example queries |
| `GET` | `/health` | Health check |

**Example:**

```bash
curl -X POST http://localhost:8000/query \
  -H "Content-Type: application/json" \
  -d '{
    "query": "Which respiratory diseases show low competition in India?",
    "generate_pdf": true
  }'
```

---

## 📊 Mock Data Coverage

| Data Source | Molecules Covered |
|-------------|-------------------|
| IQVIA Market | Pembrolizumab, Sitagliptin, Rivaroxaban, Pirfenidone, Roflumilast, Fluticasone, Omalizumab, Tiotropium |
| Patents (USPTO) | All above molecules with expiry dates and jurisdiction data |
| Clinical Trials | NSCLC, Diabetes, IPF, COPD, Asthma, PAH, Melanoma, TNBC |
| EXIM Trade | Import volumes and pricing for all molecules |
| Social Media | Diabetes, Respiratory, Oncology patient communities |
| Competitor Intel | Sitagliptin, Pembrolizumab, Rivaroxaban, Pirfenidone, Tiotropium |

---

## 🧪 Technology Stack

| Layer | Technology |
|-------|------------|
| **LLM** | Groq · Llama 3 70B (sub-second latency) |
| **Agent Framework** | CrewAI · Hierarchical Process |
| **Vector Database** | ChromaDB (for RAG) |
| **Backend API** | FastAPI |
| **Frontend** | Streamlit |
| **Report Generation** | fpdf2 · openpyxl |
| **Web Search** | Tavily API |

---

## 📄 License

Internal use only — Proprietary.

---

*Built with ❤️ for Pharmaceutical Innovation*