# Development and Deployment

## Local Development Setup

The project provides a comprehensive setup script (`setup.sh`) that can configure a local development environment in two ways:

1.  **Docker (Recommended)**: The `setup.sh` script can be used to build and run the entire application (backend and frontend) in Docker containers. This is the recommended approach for a clean, isolated environment.
2.  **Native**: The script can also install all the necessary dependencies (Python, Node.js, etc.) directly on the host machine (macOS or Ubuntu/Debian).

The `quickstart.sh` script provides a convenient way to start both the backend and frontend servers in detached `screen` sessions.

## Build and Deployment Process

*   **Build**:
    *   The backend is a Python application and does not require a separate build step for development.
    *   The frontend is a Vite application and can be built for production using `npm run build`.
    *   Docker images for both the backend and frontend can be built using the `docker/Makefile` or `docker-compose build`.
*   **Deployment**:
    *   The project is designed to be deployed using Docker. The `docker-compose.yml` file defines the services for the backend and frontend.
    *   The `setup.sh` script can also be used to set up a production-like environment.
*   **Environments**: The project uses a `.env` file to manage environment-specific configurations, such as API keys and database connection strings.
