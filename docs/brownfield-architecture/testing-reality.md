# Testing Reality

## Current Test Coverage

The project includes a testing framework, but the extent of test coverage is not immediately clear from the file structure alone.

*   **Unit Tests**: The `Makefile` includes a `test` command that runs `pdm run pytest`, which suggests that unit tests are written using the `pytest` framework. However, without a `tests` directory visible in the initial file listing, the location and extent of these tests are unknown.
*   **Integration Tests**: There is no dedicated integration testing setup apparent from the project structure.
*   **E2E Tests**: There is no evidence of an end-to-end testing framework.
*   **Manual Testing**: Given the complexity of the application, it is likely that manual testing is a significant part of the quality assurance process.

## Running Tests

The following command can be used to run the tests:

```bash
make test
```

This command will execute the `pytest` test runner.
