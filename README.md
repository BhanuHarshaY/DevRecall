# DevRecall

An intelligent "Flight Recorder" for software development that autonomously captures, correlates, and preserves institutional knowledge from debugging sessions.

## Overview

DevRecall is an always-on background agent that passively observes the entire development lifecycle: terminal commands used to diagnose issues, Git diffs applied to fix them, and ticket requirements driving the work. Unlike standard history tools, DevRecall distinguishes between "noise" (typos, trial-and-error) and "signal" (successful fixes), creating an auto-updating knowledge base for engineering teams.

## Features

- **Multi-Modal Context Fusion**: Automatically links terminal errors to code changes and ticket requirements
- **Automated Noise Filtering**: Local AI filtering strips out navigation commands (cd, ls) and retains high-value debugging data
- **Privacy-First PII Redaction**: Secrets, API keys, and IPs are scrubbed locally before any data leaves the developer's machine
- **Semantic Search**: Search past incidents by error message or natural language queries
- **Auto-Generated Documentation**: Generate post-mortems and runbooks from recorded sessions with a single click
- **Proactive Fix Suggestions**: Automatically surfaces relevant past solutions when similar errors are encountered

## Tech Stack

- **Client Agent**: Python (async hooks for Bash/Zsh)
- **Backend**: FastAPI, Google Cloud Run
- **Message Queue**: Google Cloud Pub/Sub
- **Vector Database**: Pinecone
- **LLM Integration**: Vertex AI, LangChain
- **Frontend**: React
- **Orchestration**: Apache Airflow
- **Monitoring**: Prometheus, Grafana
- **Containerization**: Docker, Docker Compose


## Team

- Bhanu Harsha Yanamadala
- Jennisha Christina Martin
- Mohammed Ahnaf Tajwar
- Raghav Jadia
- Sanjana Satish Menon
- Sri Ram Sathya Narayanan

## License

This project is licensed under the MIT License - see the [LICENSE](LICENSE) file for details.




---

**DevRecall** - *Never lose tribal knowledge again.*
