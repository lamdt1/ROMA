# ROMA Brownfield Architecture Document

## Introduction

This document captures the CURRENT STATE of the ROMA (Recursive Open Meta-Agents) codebase, including technical debt, workarounds, and real-world patterns. It serves as a reference for AI agents working on enhancements.

### Document Scope

Comprehensive documentation of the entire system.

### Change Log

| Date | Version | Description | Author |
| --- | --- | --- | --- |
| 2025-09-13 | 1.0 | Initial brownfield analysis | Architect |

## Quick Reference - Key Files and Entry Points

### Critical Files for Understanding the System

*   **Main Entry**: `src/sentientresearchagent/__main__.py`
*   **Framework Entry**: `src/sentientresearchagent/framework_entry.py`
*   **Configuration**: `sentient.yaml`, `src/sentientresearchagent/config/`
*   **Core Business Logic**: `src/sentientresearchagent/core/`, `src/sentientresearchagent/hierarchical_agent_framework/`
*   **API Definitions**: `fastapi_server.py`, `src/sentientresearchagent/server/`
*   **Agent Definitions**: `src/sentientresearchagent/hierarchical_agent_framework/agent_blueprints.py`, `src/sentientresearchagent/hierarchical_agent_framework/agents/`

## High Level Architecture

### Technical Summary

ROMA is a Python-based framework for building hierarchical multi-agent systems. It uses a recursive plan-execute loop to decompose and solve complex tasks. The backend is built with FastAPI and Flask, and the frontend is a React/TypeScript application. The framework is designed to be extensible, allowing for the creation of custom agents, tools, and execution logic.

### Actual Tech Stack (from pyproject.toml & package.json)

| Category | Technology | Version | Notes |
| --- | --- | --- | --- |
| Language | Python | >=3.12 | |
| Backend Framework | FastAPI, Flask | | |
| Frontend Framework | React | 18.2.0 | with TypeScript |
| Frontend Build Tool | Vite | 5.4.19 | |
| Package Manager | PDM, npm | | |
| LLM Orchestration | LiteLLM | >=1.72.6 | |
| Data Handling | Pydantic, OmegaConf | | |
| API Communication | websockets, socket.io | | |

### Repository Structure Reality Check

*   **Type**: Monorepo
*   **Package Manager**: PDM for the backend, npm for the frontend.
*   **Notable**: The project is well-structured, with a clear separation of concerns between the core framework, the server, and the frontend. The `.bmad-core` directory contains a rich set of templates, tasks, and workflows for agent-based development.

## Source Tree and Module Organization

### Project Structure (Actual)

```text
ROMA/
├── src/
│   └── sentientresearchagent/
│       ├── core/                  # Core components (SystemManager, ProjectManager)
│       ├── hierarchical_agent_framework/ # Main framework logic
│       │   ├── agents/            # Agent definitions
│       │   ├── graph/             # Task graph and state management
│       │   ├── node/              # Node processor and task nodes
│       │   └── orchestration/     # Execution orchestrator
│       ├── server/                # Flask/FastAPI server
│       └── config/                # Configuration models and loader
├── frontend/                      # React/TypeScript frontend
├── docs/                          # Project documentation
├── .bmad-core/                    # BMAD agent development framework
└── ...                            # Other project files
```

### Key Modules and Their Purpose

*   **`sentientresearchagent`**: The main Python package for the agent framework.
*   **`core`**: Contains the central `SystemManager` which initializes and coordinates all other components. Also includes project management and error handling.
*   **`hierarchical_agent_framework`**: The heart of the framework, containing the logic for hierarchical planning, task decomposition, and execution.
    *   **`agents`**: Defines the different types of agents (planners, executors, etc.).
    *   **`graph`**: Manages the task graph (a DAG of tasks) and its state.
    *   **`node`**: Handles the processing of individual nodes in the task graph.
    *   **`orchestration`**: The `ExecutionOrchestrator` drives the overall plan-execute loop.
*   **`server`**: Implements the backend server using FastAPI and Flask, exposing the agent's capabilities via a REST API and WebSockets.
*   **`frontend`**: A React and TypeScript single-page application that provides a user interface for interacting with the agent.
*   **`.bmad-core`**: A framework for developing with AI agents, containing prompts, templates, and workflows.

## Data Models and APIs

### Data Models

The project uses Pydantic models for data validation and serialization, primarily for configuration and API requests/responses.

*   **Configuration Models**: Defined in `src/sentientresearchagent/config/models.py`. These models (`LLMConfig`, `CacheConfig`, `ExecutionConfig`, etc.) structure the `sentient.yaml` configuration file.
*   **API Models**: Defined in `fastapi_server.py`. These models (`ResearchRequest`, `ResearchResponse`) define the structure of data for the API endpoints.
*   **Agent Blueprints**: The `AgentBlueprint` model in `src/sentientresearchagent/hierarchical_agent_framework/agent_blueprints.py` defines the structure of an agent's capabilities.

### API Specifications

The primary API is defined in `fastapi_server.py` and includes the following endpoints:

*   **`GET /health`**: Returns the health status of the server and system information.
*   **`POST /research`**: The main endpoint for executing a research query. It takes a `ResearchRequest` and returns a `ResearchResponse`.
*   **`POST /research/async`**: Starts a research query in the background.
*   **`GET /profiles`**: Lists the available agent profiles.
*   **`GET /config`**: Returns the current system configuration.

The frontend communicates with the backend via this REST API and a WebSocket connection for real-time updates.

## Technical Debt and Known Issues

### Critical Technical Debt

Based on my analysis, I have identified the following areas of potential technical debt:

1.  **Redundant `SystemManager` and `SystemManagerV2`**: The presence of both `system_manager.py` and a `system_manager_v2.py` (which I've inferred from the V2 class name) suggests a refactoring in progress. The older system may still be in use in some places, leading to inconsistencies.
2.  **Circular Dependencies**: The `agent_blueprints.py` file contains deprecated functions that cause circular dependencies with `profile_loader.py`. This indicates a need for refactoring the agent loading mechanism.
3.  **Inconsistent Naming**: The project uses both `ExecutionEngine` and `ExecutionOrchestrator`, and `HITLCoordinator` and `HITLService`. This suggests a refactoring where the names have not been updated consistently across the codebase.
4.  **Manual Environment Variable Mapping**: The `config_loader.py` file manually maps environment variables to the configuration object. This could be brittle and hard to maintain. A more robust solution using OmegaConf's built-in environment variable interpolation would be better.

### Workarounds and Gotchas

*   **WebSocket HITL Availability**: The WebSocket Human-in-the-Loop (HITL) functionality is not available by default and requires specific environment variables to be set. The code includes fallback functions, but this could be a "gotcha" for developers who expect it to work out of the box.
*   **Jupyter Environment Safety**: The `framework_entry.py` file includes a `_run_async_safely` function to handle running async code in Jupyter environments. This is a good workaround, but it adds complexity and indicates that running the framework in a notebook environment requires special handling.

## Integration Points and External Dependencies

### External Services

The project integrates with several external services, as indicated by the configuration and dependencies:

| Service | Purpose | Integration Type | Key Files |
| --- | --- | --- | --- |
| OpenAI/Anthropic/Google | LLM Providers | API | `sentient.yaml`, `src/sentientresearchagent/config/models.py` |
| Tavily/Bing | Web Search | API | `sentient.yaml`, `src/sentientresearchagent/config/models.py` |
| E2B | Code Execution | API | `sentient.yaml`, `src/sentientresearchagent/config/models.py` |
| Wikipedia | Information | API | `pyproject.toml` |
| DuckDuckGo | Web Search | Library | `pyproject.toml` |
| Exa.ai | Web Search | Library | `pyproject.toml` |

### Internal Integration Points

*   **Frontend-Backend Communication**: The React frontend communicates with the Python backend via a REST API (FastAPI/Flask) and a WebSocket connection (Socket.IO) for real-time updates.
*   **Agent-Tool Communication**: Agents use a variety of tools (defined as Python functions) to interact with the outside world. These tools are registered with the `AgentRegistry`.
*   **Cache**: The framework uses a cache (in-memory, Redis, or disk) to store the results of LLM calls and tool executions.

## Development and Deployment

### Local Development Setup

The project provides a comprehensive setup script (`setup.sh`) that can configure a local development environment in two ways:

1.  **Docker (Recommended)**: The `setup.sh` script can be used to build and run the entire application (backend and frontend) in Docker containers. This is the recommended approach for a clean, isolated environment.
2.  **Native**: The script can also install all the necessary dependencies (Python, Node.js, etc.) directly on the host machine (macOS or Ubuntu/Debian).

The `quickstart.sh` script provides a convenient way to start both the backend and frontend servers in detached `screen` sessions.

### Build and Deployment Process

*   **Build**:
    *   The backend is a Python application and does not require a separate build step for development.
    *   The frontend is a Vite application and can be built for production using `npm run build`.
    *   Docker images for both the backend and frontend can be built using the `docker/Makefile` or `docker-compose build`.
*   **Deployment**:
    *   The project is designed to be deployed using Docker. The `docker-compose.yml` file defines the services for the backend and frontend.
    *   The `setup.sh` script can also be used to set up a production-like environment.
*   **Environments**: The project uses a `.env` file to manage environment-specific configurations, such as API keys and database connection strings.

## Testing Reality

### Current Test Coverage

The project includes a testing framework, but the extent of test coverage is not immediately clear from the file structure alone.

*   **Unit Tests**: The `Makefile` includes a `test` command that runs `pdm run pytest`, which suggests that unit tests are written using the `pytest` framework. However, without a `tests` directory visible in the initial file listing, the location and extent of these tests are unknown.
*   **Integration Tests**: There is no dedicated integration testing setup apparent from the project structure.
*   **E2E Tests**: There is no evidence of an end-to-end testing framework.
*   **Manual Testing**: Given the complexity of the application, it is likely that manual testing is a significant part of the quality assurance process.

### Running Tests

The following command can be used to run the tests:

```bash
make test
```

This command will execute the `pytest` test runner.

## Appendix - Useful Commands and Scripts

### Frequently Used Commands

The `Makefile` provides several useful commands for managing the project:

```bash
# Run the interactive setup
make setup

# Install dependencies
make install

# Run the backend server
make run

# Run the frontend development server
make frontend-dev

# Run tests
make test

# Lint the code
make lint

# Format the code
make format

# Start Docker services
make docker-up

# Stop Docker services
make docker-down
```

### Debugging and Troubleshooting

*   **Logs**: The application uses `loguru` for logging. By default, logs are sent to standard output.
*   **Debug Mode**: The backend server can be started in debug mode using `make run-debug`.
*   **Docker Logs**: When running in Docker, logs can be viewed using `make docker-logs`.
