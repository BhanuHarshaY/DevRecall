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

## Project Structure
```
DevRecall/
├── src/
│   ├── agent/          # Client-side Python agent
│   ├── backend/        # Cloud services (API, fusion, search)
│   ├── frontend/       # React dashboard
│   └── docker/         # Container configurations
├── data/               # Raw and processed datasets
├── airflow/            # DAGs for data pipelines
├── cloud/              # GCP configurations
├── vector_db/          # Pinecone integration scripts
├── monitoring/         # Prometheus/Grafana configs
└── notebooks/          # Experimentation notebooks
```

## Prerequisites

- Python 3.9+
- Node.js 18+
- Docker & Docker Compose
- Google Cloud SDK
- Git

## Installation

### 1. Clone the Repository
```bash
git clone https://github.com/your-org/DevRecall.git
cd DevRecall
```

### 2. Set Up Virtual Environment
```bash
python -m venv venv
source venv/bin/activate  # On Windows: venv\Scripts\activate
pip install -r requirements.txt
```

### 3. Configure Environment Variables

Create a `.env` file in the root directory:
```bash
# Google Cloud
GCP_PROJECT_ID=your-project-id
GCP_REGION=us-central1
PUBSUB_TOPIC=devrecall-ingestion

# Pinecone
PINECONE_API_KEY=your-pinecone-api-key
PINECONE_ENVIRONMENT=us-west1-gcp
PINECONE_INDEX_NAME=devrecall-index

# LLM Configuration
VERTEX_AI_LOCATION=us-central1
MODEL_NAME=gemini-pro

# Application
SECRET_KEY=your-secret-key
DEBUG=False
```

### 4. Install the Client Agent
```bash
cd src/agent
pip install -e .
devrecall-agent install
```

This will add the necessary hooks to your shell configuration (~/.bashrc or ~/.zshrc).

### 5. Start Backend Services (Local Development)
```bash
docker-compose up -d
```

### 6. Initialize the Vector Database
```bash
python vector_db/index_manager.py --create
```

## Usage

### Starting the Agent

The agent starts automatically with your terminal after installation. To manually control it:
```bash
# Start the agent
devrecall-agent start

# Stop the agent
devrecall-agent stop

# Check status
devrecall-agent status
```

### Searching Past Sessions

Access the web dashboard at `http://localhost:3000` or use the CLI:
```bash
# Search by error message
devrecall search "ConnectionRefusedError Redis"

# Search by natural language
devrecall search "How did we fix the authentication timeout issue?"
```

### Generating Documentation
```bash
# Generate a post-mortem for a specific session
devrecall generate postmortem --session-id <SESSION_ID>

# Generate a runbook from recent debugging sessions
devrecall generate runbook --topic "Redis connection issues"
```

### Viewing Session History
```bash
# List recent sessions
devrecall sessions list --limit 10

# View details of a specific session
devrecall sessions show <SESSION_ID>
```

## Development

### Running Tests
```bash
# Run all tests
pytest

# Run with coverage
pytest --cov=src --cov-report=html

# Run specific test suite
pytest tests/unit/
pytest tests/integration/
```

### Code Quality
```bash
# Linting
flake8 src/
black src/ --check

# Type checking
mypy src/
```

### Running Airflow Locally
```bash
cd airflow
airflow db init
airflow webserver --port 8080 &
airflow scheduler &
```

## Deployment

### Deploy to Google Cloud
```bash
# Authenticate with GCP
gcloud auth login
gcloud config set project $GCP_PROJECT_ID

# Deploy backend services
./scripts/deploy_backend.sh

# Deploy frontend
./scripts/deploy_frontend.sh
```

### CI/CD Pipeline

The project uses GitHub Actions for continuous integration and deployment. Workflows are defined in `.github/workflows/`:

- `ci.yml`: Runs tests and linting on every push
- `cd.yml`: Deploys to staging/production on merge to main
- `pii_leak_test.yml`: Validates PII scrubber catches all secrets

## API Documentation

API documentation is available at:
- **Swagger UI**: `http://localhost:8000/docs`
- **ReDoc**: `http://localhost:8000/redoc`

## Configuration

### Agent Configuration

Edit `~/.devrecall/config.yaml`:
```yaml
capture:
  terminal: true
  git: true
  
filtering:
  ignore_commands:
    - cd
    - ls
    - clear
    - pwd
  
privacy:
  redact_env_vars: true
  redact_ips: true
  redact_emails: true
  
upload:
  batch_size: 100
  interval_seconds: 30
```

### Backend Configuration

Configuration is managed via environment variables. See `.env.example` for all available options.

## Contributing

We welcome contributions! Please follow these steps:

1. Fork the repository
2. Create a feature branch (`git checkout -b feature/amazing-feature`)
3. Commit your changes (`git commit -m 'Add amazing feature'`)
4. Push to the branch (`git push origin feature/amazing-feature`)
5. Open a Pull Request

Please ensure your code:
- Passes all tests
- Follows the existing code style
- Includes appropriate documentation
- Has adequate test coverage

## Team

- Bhanu Harsha Yanamadala
- Jennisha Christina Martin
- Mohammed Ahnaf Tajwar
- Raghav Jadia
- Sanjana Satish Menon
- Sri Ram Sathya Narayanan

## License

This project is licensed under the MIT License - see the [LICENSE](LICENSE) file for details.

## Acknowledgments

- IE7374 - MLOps Course, Northeastern University
- Google Cloud Platform for cloud credits
- The open-source community for the amazing tools and libraries

## Support

For questions or issues, please:
1. Check the [documentation](docs/)
2. Search existing [GitHub Issues](https://github.com/your-org/DevRecall/issues)
3. Open a new issue if needed

---

**DevRecall** - *Never lose tribal knowledge again.*
