# 📈 Project Echelon: Distributed Multi-Agent AI Decision Swarm

> **An Event-Driven, Autonomous Digital Boardroom built on LangGraph and Redpanda, running entirely on Edge Hardware (Apple Silicon M4).**

## 🌐 Overview
Project Echelon is a proof-of-concept autonomous algorithmic trading engine. It eliminates LLM hallucinations by utilizing a **Multi-Agent Swarm** architecture. Instead of relying on a single AI prompt, Echelon routes high-frequency market data into a "Digital Boardroom" where multiple specialized AI agents debate risk, quantitative momentum, and real-world news context before executing a final decision.

This system demonstrates enterprise-grade **Event-Driven Architecture (EDA)**, decoupling high-speed data ingestion from complex LLM inference using a containerized message broker.

## 🏗️ System Architecture

### 1. The Nervous System (Data Ingestion)
* **Message Broker:** A containerized **Redpanda** (C++) cluster handles high-frequency data ingestion.
* **Producer:** An asynchronous Python stream simulating live market ticks (Ticker, Price, Volume, Volatility), pushed via `confluent-kafka`.
* **Threshold Trigger:** The AI does not waste compute on stable data. The LangGraph inference engine only awakens when the Kafka consumer detects a `volatility_index > 4.0`.

### 2. The Agentic Boardroom (LangGraph Swarm)
When triggered, the data payload is passed into a LangGraph state machine powered locally by `qwen2.5:7b` via **Ollama** (utilizing Apple Metal GPU acceleration).

The State Graph executes sequentially:
1. **The Researcher Node:** Pulls contextual real-world news and supply-chain data regarding the specific ticker.
2. **The Quant Node:** Analyzes volume and price momentum, proposing aggressive mathematical strategies.
3. **The Risk Manager Node:** Evaluates the volatility index against the current context, prioritizing capital preservation.
4. **The CEO Node:** Reads the outputs of all three subordinate agents, resolves any conflicting arguments, and autonomously outputs a final execution command (`EXECUTE BUY`, `HOLD POSITION`, `EXECUTE SELL`).

## 🛠️ Tech Stack
* **Orchestration:** `LangGraph`, `LangChain`
* **Message Queue:** `Redpanda`, `confluent-kafka`
* **Local Inference:** `Ollama` (`qwen2.5:7b`) optimized for Apple M4 Unified Memory
* **Containerization:** `Docker`, `Docker Compose`
* **Terminal UI:** `Rich` (for real-time boardroom output visualization)

## 🚀 Quick Start (Local Edge Deployment)

### Prerequisites
* Docker Desktop installed and running.
* Ollama installed with the `qwen2.5:7b` model pulled locally.

### 1. Spin up the Distributed Infrastructure
```bash
docker-compose up -d
This starts the Redpanda broker and exposes the external listener on localhost:19092.

2. Start the Autonomous Swarm (Consumer)
In your first terminal window, initialize the LangGraph supervisor. It will listen to the broker and wait for volatility triggers.

Bash
python supervisor.py
3. Ignite the Market Feed (Producer)
In a second terminal window, start the high-frequency market simulation:

Bash
python data_streamer.py
📊 Sample Output
When a high-volatility event occurs, the terminal visualizes the swarm's debate and final resolution:

[SWARM INGESTING] AMZN | Price: $452.06 | Volatility: 4.61
High volatility detected! Activating AI swarm with research context...
╭────────────────────────── 🎯 ECHELON SWARM DECISION - AMZN ──────────────────────────╮
│ HOLD POSITION                                                                        │
│                                                                                      │
│ Given the robust fundamentals highlighted by AWS's strong revenue growth and ongoing │
│ logistics optimizations, combined with current market volatility, it is prudent to   │
│ hold the position to manage risk while benefiting from long-term potential.          │
│                                                                                      │
│ Research Intel: AWS revenue up 14% this quarter; logistics network optimization      │
╰──────────────────────────────────────────────────────────────────────────────────────╯
