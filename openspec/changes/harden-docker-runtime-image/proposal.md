# harden-docker-runtime-image

## Why
The default shipped Docker image still uses a full `python:3.13-slim` Debian runtime. Snyk is reporting multiple Debian 13 OS-package findings against that runtime image, including issues in `systemd`, `glibc`, `libcap2`, `tar`, and `util-linux`. The repository already carries a distroless-based Docker path, but it is not the default build path used by release, CI, or the documented production compose flow.

## What Changes
- Make the default `Dockerfile` ship a distroless Debian 12 runtime instead of a full Debian 13 slim runtime.
- Keep a separate development runtime target in the same `Dockerfile` so `docker compose watch` retains a shell-capable image for reload/debug workflows.
- Point local development compose at the development target while leaving default image builds on the hardened runtime path.
- Remove the now-redundant alternate distroless Dockerfile to avoid runtime drift.

## Impact
- `docker build .`, CI image builds, Helm smoke-image builds, and release image builds all ship the hardened runtime by default.
- `docker compose watch` continues to work for local development without forcing distroless-only tooling into the dev loop.
- The specific Debian 13 findings against the current runtime image are eliminated by removing Debian 13 from the shipped runtime path.
