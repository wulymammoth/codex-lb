## ADDED Requirements

### Requirement: Default Docker image uses a hardened minimal runtime

The repository's default `Dockerfile` build MUST ship a hardened runtime image that minimizes OS package surface area compared with a full Debian slim runtime. The shipped runtime MUST run as a non-root user and MUST preserve the existing Python application startup behavior.

#### Scenario: Default Docker build produces the hardened runtime

- **WHEN** a user builds the repository's default Docker image
- **THEN** the final runtime stage uses the hardened minimal runtime path instead of a full `python:*‑slim` Debian runtime
- **AND** the runtime executes as a non-root user
- **AND** the application still starts through the repository's Python entrypoint flow

### Requirement: Compose development flow uses a dedicated development runtime

The repository MUST preserve a local Docker development workflow that keeps Python CLI tooling and reload support available without weakening the default shipped runtime.

#### Scenario: Local compose builds the development target

- **WHEN** a user runs the repository's local Docker Compose development workflow
- **THEN** the server service builds a dedicated development runtime target
- **AND** that target retains the Python CLI tooling needed for local reload and debugging
- **AND** the default image build path remains the hardened runtime used for release and production-style builds
