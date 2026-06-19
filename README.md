# Ambient Agents

AI-assisted observability and incident triage platform built with Kafka, Prometheus, Docker, Ollama, and Claude.

Ambient Agents ingests infrastructure alerts, classifies and enriches incidents using a multi-stage AI pipeline, and routes high-priority events for escalation. The project demonstrates how local and cloud LLMs can be combined with deterministic rules to create a reliable and cost-efficient incident response workflow.

## Architecture

```text
Prometheus Alerts
        │
        ▼
 Alertmanager
        │
        ▼
 Kafka (alerts.raw)
        │
        ▼
 Triage Agent (Ollama)
        │
        ├── Low Severity → Archived
        │
        └── High Severity
                │
                ▼
        Escalation Agent (Claude)
                │
                ▼
        incidents.enriched
                │
                ▼
        Slack / Dashboard / Consumers
```

## Features

* Event-driven architecture using Kafka
* Multi-agent incident processing pipeline
* Local-first AI triage with Ollama
* Cloud-based escalation analysis using Claude
* Deterministic rule engine for safety and consistency
* Dead Letter Queue (DLQ) support
* Prometheus monitoring integration
* Alertmanager webhook ingestion
* Dockerized development environment
* Real-time event streaming

## Why This Project Exists

Modern monitoring systems generate large numbers of alerts, many of which are repetitive or low priority.

Ambient Agents explores an architecture where:

1. Infrastructure alerts are collected automatically.
2. Deterministic rules identify obvious incidents.
3. A local LLM performs inexpensive triage.
4. Only potentially critical incidents are escalated to a more capable cloud model.
5. Enriched incidents are routed to downstream systems.

This approach reduces alert fatigue while controlling LLM costs.

## Tech Stack

### Infrastructure

* Docker
* Docker Compose
* Apache Kafka
* Zookeeper
* Prometheus
* Alertmanager

### AI

* Ollama
* Claude (Anthropic)

### Backend

* Python
* FastAPI
* Kafka-Python

### Monitoring

* Prometheus
* Alertmanager

## Project Structure

```text
ambient-agents/
├── triage-agent/
├── escalation-agent/
├── alert-ingestor/
├── websocket-gateway/
├── monitoring/
├── docker/
├── kafka/
└── docs/
```

## How It Works

### 1. Alert Ingestion

Prometheus alerts are delivered through Alertmanager and published into Kafka topics.

### 2. Triage

The Triage Agent:

* Applies deterministic severity rules
* Uses Ollama for contextual classification
* Determines whether escalation is required

### 3. Escalation

High-priority incidents are forwarded to the Escalation Agent.

The Escalation Agent:

* Uses Claude for deeper analysis
* Generates incident summaries
* Suggests probable root causes
* Produces enriched incident records

### 4. Distribution

Enriched incidents are published back into Kafka and can be consumed by:

* Slack integrations
* Dashboards
* Incident management systems
* Analytics pipelines

## Running Locally

### Prerequisites

* Docker
* Docker Compose
* Ollama
* Anthropic API Key

### Start Services

```bash
docker compose up -d
```

### Start Ollama

```bash
ollama serve
```

Pull the required model:

```bash
ollama pull llama3
```

### Configure Environment

Create a `.env` file:

```env
ANTHROPIC_API_KEY=your_key_here
OLLAMA_HOST=http://localhost:11434
```

### Verify Services

```bash
docker ps
```

Check Kafka topics:

```bash
docker exec kafka kafka-topics \
  --bootstrap-server localhost:9092 \
  --list
```

## Current Status

This project is a production-oriented prototype.

Implemented:

* Kafka event pipeline
* Multi-agent workflow
* Alert ingestion
* AI triage
* AI escalation
* Docker deployment
* Monitoring integration

Not Yet Implemented:

* Comprehensive test suite
* Persistent incident storage
* Distributed state management
* Kubernetes deployment
* Full authentication and authorization
* OpenTelemetry tracing

## Learning Goals

This project was built to explore:

* Event-driven system design
* Agentic AI workflows
* LLM orchestration patterns
* Reliability in AI-assisted systems
* Observability and incident management
* Distributed architectures

## Future Improvements

* Redis-backed context storage
* PostgreSQL incident persistence
* OpenTelemetry tracing
* Grafana dashboards
* Kubernetes deployment
* Schema Registry integration
* Automated recovery workflows
* Multi-model routing strategies

## Disclaimer

Ambient Agents is an experimental project intended for learning and architectural exploration. It demonstrates production-inspired patterns but is not intended for direct deployment into critical production environments without additional hardening, testing, and security controls.
