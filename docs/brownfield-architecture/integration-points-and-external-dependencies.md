# Integration Points and External Dependencies

## External Services

The project integrates with several external services, as indicated by the configuration and dependencies:

| Service | Purpose | Integration Type | Key Files |
| --- | --- | --- | --- |
| OpenAI/Anthropic/Google | LLM Providers | API | `sentient.yaml`, `src/sentientresearchagent/config/models.py` |
| Tavily/Bing | Web Search | API | `sentient.yaml`, `src/sentientresearchagent/config/models.py` |
| E2B | Code Execution | API | `sentient.yaml`, `src/sentientresearchagent/config/models.py` |
| Wikipedia | Information | API | `pyproject.toml` |
| DuckDuckGo | Web Search | Library | `pyproject.toml` |
| Exa.ai | Web Search | Library | `pyproject.toml` |

## Internal Integration Points

*   **Frontend-Backend Communication**: The React frontend communicates with the Python backend via a REST API (FastAPI/Flask) and a WebSocket connection (Socket.IO) for real-time updates.
*   **Agent-Tool Communication**: Agents use a variety of tools (defined as Python functions) to interact with the outside world. These tools are registered with the `AgentRegistry`.
*   **Cache**: The framework uses a cache (in-memory, Redis, or disk) to store the results of LLM calls and tool executions.
