# Source Tree and Module Organization

## Project Structure (Actual)

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

## Key Modules and Their Purpose

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
