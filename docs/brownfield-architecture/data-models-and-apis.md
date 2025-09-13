# Data Models and APIs

## Data Models

The project uses Pydantic models for data validation and serialization, primarily for configuration and API requests/responses.

*   **Configuration Models**: Defined in `src/sentientresearchagent/config/models.py`. These models (`LLMConfig`, `CacheConfig`, `ExecutionConfig`, etc.) structure the `sentient.yaml` configuration file.
*   **API Models**: Defined in `fastapi_server.py`. These models (`ResearchRequest`, `ResearchResponse`) define the structure of data for the API endpoints.
*   **Agent Blueprints**: The `AgentBlueprint` model in `src/sentientresearchagent/hierarchical_agent_framework/agent_blueprints.py` defines the structure of an agent's capabilities.

## API Specifications

The primary API is defined in `fastapi_server.py` and includes the following endpoints:

*   **`GET /health`**: Returns the health status of the server and system information.
*   **`POST /research`**: The main endpoint for executing a research query. It takes a `ResearchRequest` and returns a `ResearchResponse`.
*   **`POST /research/async`**: Starts a research query in the background.
*   **`GET /profiles`**: Lists the available agent profiles.
*   **`GET /config`**: Returns the current system configuration.

The frontend communicates with the backend via this REST API and a WebSocket connection for real-time updates.
