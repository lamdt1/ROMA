# High Level Architecture

## Technical Summary

ROMA is a Python-based framework for building hierarchical multi-agent systems. It uses a recursive plan-execute loop to decompose and solve complex tasks. The backend is built with FastAPI and Flask, and the frontend is a React/TypeScript application. The framework is designed to be extensible, allowing for the creation of custom agents, tools, and execution logic.

## Actual Tech Stack (from pyproject.toml & package.json)

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

## Repository Structure Reality Check

*   **Type**: Monorepo
*   **Package Manager**: PDM for the backend, npm for the frontend.
*   **Notable**: The project is well-structured, with a clear separation of concerns between the core framework, the server, and the frontend. The `.bmad-core` directory contains a rich set of templates, tasks, and workflows for agent-based development.
