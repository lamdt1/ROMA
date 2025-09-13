# Appendix - Useful Commands and Scripts

## Frequently Used Commands

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

## Debugging and Troubleshooting

*   **Logs**: The application uses `loguru` for logging. By default, logs are sent to standard output.
*   **Debug Mode**: The backend server can be started in debug mode using `make run-debug`.
*   **Docker Logs**: When running in Docker, logs can be viewed using `make docker-logs`.
