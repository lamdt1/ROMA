# Technical Debt and Known Issues

## Critical Technical Debt

Based on my analysis, I have identified the following areas of potential technical debt:

1.  **Redundant `SystemManager` and `SystemManagerV2`**: The presence of both `system_manager.py` and a `system_manager_v2.py` (which I've inferred from the V2 class name) suggests a refactoring in progress. The older system may still be in use in some places, leading to inconsistencies.
2.  **Circular Dependencies**: The `agent_blueprints.py` file contains deprecated functions that cause circular dependencies with `profile_loader.py`. This indicates a need for refactoring the agent loading mechanism.
3.  **Inconsistent Naming**: The project uses both `ExecutionEngine` and `ExecutionOrchestrator`, and `HITLCoordinator` and `HITLService`. This suggests a refactoring where the names have not been updated consistently across the codebase.
4.  **Manual Environment Variable Mapping**: The `config_loader.py` file manually maps environment variables to the configuration object. This could be brittle and hard to maintain. A more robust solution using OmegaConf's built-in environment variable interpolation would be better.

## Workarounds and Gotchas

*   **WebSocket HITL Availability**: The WebSocket Human-in-the-Loop (HITL) functionality is not available by default and requires specific environment variables to be set. The code includes fallback functions, but this could be a "gotcha" for developers who expect it to work out of the box.
*   **Jupyter Environment Safety**: The `framework_entry.py` file includes a `_run_async_safely` function to handle running async code in Jupyter environments. This is a good workaround, but it adds complexity and indicates that running the framework in a notebook environment requires special handling.
