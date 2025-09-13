# ROMA Brownfield Enhancement PRD

## Intro Project Analysis and Context

### Existing Project Overview

*   **Analysis Source**: IDE-based fresh analysis, documented in `docs/brownfield-architecture.md`.
*   **Current Project State**: ROMA is a Python-based framework for building hierarchical multi-agent systems. It uses a recursive plan-execute loop to decompose and solve complex tasks. The backend is built with FastAPI and Flask, and the frontend is a React/TypeScript application.

### Available Documentation Analysis

*   [x] Tech Stack Documentation
*   [x] Source Tree/Architecture
*   [ ] Coding Standards (partially inferred)
*   [x] API Documentation
*   [ ] External API Documentation (partially inferred)
*   [ ] UX/UI Guidelines
*   [x] Technical Debt Documentation

### Enhancement Scope Definition

This PRD is to document the existing project and prepare for future enhancements.

### Goals and Background Context

*   **Goals**: To create a comprehensive PRD for the existing ROMA project that can be used as a baseline for future development.
*   **Background Context**: This PRD will serve as a single source of truth for the project's requirements and will be used to guide the implementation of new features.

### Change Log

| Change | Date | Version | Description | Author |
| --- | --- | --- | --- | --- |
| Initial PRD | 2025-09-13 | 1.0 | Initial Brownfield PRD | Architect |

## Requirements

### Functional Requirements

*   **FR1**: The system shall provide a framework for building and running hierarchical multi-agent systems.
*   **FR2**: The system shall support the decomposition of complex tasks into a directed acyclic graph (DAG) of subtasks.
*   **FR3**: The system shall allow for the registration and use of custom agents, tools, and LLM providers.
*   **FR4**: The system shall provide a web-based user interface for interacting with the agents and visualizing the task graph.
*   **FR5**: The system shall expose a REST API for programmatic interaction with the agent framework.
*   **FR6**: The system shall support Human-in-the-Loop (HITL) for reviewing and approving agent actions.

### Non-Functional Requirements

*   **NFR1**: The system shall be extensible, allowing developers to add new agents, tools, and capabilities.
*   **NFR2**: The system shall be modular, with a clear separation of concerns between the core framework, the server, and the frontend.
*   **NFR3**: The system shall be performant, with the ability to execute multiple tasks in parallel.
*   **NFR4**: The system shall be reliable, with robust error handling and recovery mechanisms.
*   **NFR5**: The system shall be secure, with protection against common web application vulnerabilities.

### Compatibility Requirements

*   **CR1**: All future enhancements must maintain backward compatibility with the existing REST API.
*   **CR2**: Changes to the database schema must be managed through a migration process to ensure data integrity.
*   **CR3**: New UI components must be consistent with the existing design system and user experience.
*   **CR4**: Integrations with external services (LLMs, search APIs, etc.) must be implemented in a way that allows for easy swapping of providers.

## Technical Constraints and Integration Requirements

### Existing Technology Stack

*   **Languages**: Python >=3.12, TypeScript
*   **Frameworks**: FastAPI, Flask, React 18.2.0
*   **Database**: Not explicitly defined, but the project is designed to be extensible.
*   **Infrastructure**: Docker, Docker Compose
*   **External Dependencies**: `litellm`, `pydantic`, `loguru`, `networkx`, `vite`, and others as defined in `pyproject.toml` and `frontend/package.json`.

### Integration Approach

*   **Database Integration Strategy**: To be defined when a database is added.
*   **API Integration Strategy**: New backend functionality should be exposed via the existing FastAPI server.
*   **Frontend Integration Strategy**: New UI components should be developed in React and integrated into the existing Vite application.
*   **Testing Integration Strategy**: New code should be accompanied by `pytest` unit tests.

### Code Organization and Standards

*   **File Structure Approach**: New backend code should be placed in the appropriate subdirectory of `src/sentientresearchagent/`. New frontend code should be placed in `frontend/src/`.
*   **Naming Conventions**: Follow existing naming conventions for files, classes, and functions.
*   **Coding Standards**: The project uses `ruff` for linting and formatting. All new code should adhere to the `ruff` configuration.
*   **Documentation Standards**: All new code should be documented with docstrings.

### Deployment and Operations

*   **Build Process Integration**: The existing `Makefile` and `docker-compose.yml` files should be updated to include any new services or build steps.
*   **Deployment Strategy**: The project is designed to be deployed using Docker.
*   **Monitoring and Logging**: The project uses `loguru` for logging. New code should use the existing logging configuration.
*   **Configuration Management**: All new configuration options should be added to the `sentient.yaml` file and the corresponding Pydantic models.

### Risk Assessment and Mitigation

*   **Technical Risks**: The primary technical risks are related to the ongoing refactoring of the `SystemManager` and the circular dependencies in the agent loading mechanism.
*   **Integration Risks**: The main integration risk is ensuring that new features are integrated in a way that does not break existing functionality.
*   **Deployment Risks**: The main deployment risk is ensuring that the Docker configuration is updated correctly for any new services.
*   **Mitigation Strategies**:
    *   Complete the refactoring of the `SystemManager` and the agent loading mechanism.
    *   Write comprehensive unit and integration tests for all new features.
    *   Use a staging environment to test all changes before deploying to production.

## Epic and Story Structure

### Epic Approach

*   **Epic Structure Decision**: A single epic focused on "Project Documentation and Refactoring" will allow us to address the identified technical debt and improve the overall quality of the codebase before we begin adding new features.

### Epic 1: Project Documentation and Refactoring

*   **Epic Goal**: To create a comprehensive set of documentation for the ROMA project and to address the most critical areas of technical debt.

#### Story 1.1: Finalize and Review Brownfield Documentation
*   **As a** developer,
*   **I want** to finalize and review the brownfield architecture document,
*   **so that** all team members have a shared understanding of the project's current state.
*   **Acceptance Criteria**:
    1.  The `brownfield-architecture.md` document is complete and accurate.
    2.  The document is reviewed and approved by the project stakeholders.
    3.  The document is checked into the project's version control system.
*   **Integration Verification**:
    *   **IV1**: This story does not involve any code changes, so there is no risk to existing functionality.

#### Story 1.2: Refactor SystemManager
*   **As a** developer,
*   **I want** to refactor the `SystemManager` to remove the redundant `SystemManagerV2`,
*   **so that** there is a single, consistent implementation of the system manager.
*   **Acceptance Criteria**:
    1.  The `SystemManagerV2` class is removed from the codebase.
    2.  All code that previously used `SystemManagerV2` is updated to use the refactored `SystemManager`.
    3.  All existing tests pass after the refactoring.
*   **Integration Verification**:
    *   **IV1**: All existing functionality that relies on the `SystemManager` continues to work as expected.
    *   **IV2**: The public API of the `SystemManager` remains unchanged.

#### Story 1.3: Refactor Agent Loading
*   **As a** developer,
*   **I want** to refactor the agent loading mechanism to remove circular dependencies,
*   **so that** the code is more maintainable and easier to understand.
*   **Acceptance Criteria**:
    1.  The circular dependency between `agent_blueprints.py` and `profile_loader.py` is removed.
    2.  The agent loading mechanism is simplified and easier to follow.
    3.  All existing tests pass after the refactoring.
*   **Integration Verification**:
    *   **IV1**: All agents can be loaded and used as expected.
