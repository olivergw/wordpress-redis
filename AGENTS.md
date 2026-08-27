# Repository Guidelines

## Project Structure & Module Organization

This repository builds a small Redis image for WordPress object caching.

- `Dockerfile` pins the official Alpine Redis base, installs the healthcheck, and activates `redis.conf`.
- `redis.conf` defines the cache memory ceiling and eviction policy.
- `docker-healthcheck` performs the container health probe.
- `.github/workflows/docker-publish.yml` builds `linux/amd64` and `linux/arm64` images and publishes Alpine aliases.
- `README.md` documents behavior, tags, and usage.

Keep deployment topology and container resource limits in consuming Compose repositories.

## Build, Test, and Development Commands

```bash
docker build -t wordpress-redis:test .
docker run --rm wordpress-redis:test redis-server --version
docker run -d --rm --name wordpress-redis-test wordpress-redis:test
docker exec wordpress-redis-test redis-cli ping
```

After the runtime check, stop the test container with `docker stop wordpress-redis-test`. Pull requests run a multi-platform build without pushing.

## Coding Style & Naming Conventions

Pin a complete Alpine base tag such as `redis:8.10.1-alpine`. Keep Dockerfile instructions uppercase, shell scripts POSIX-compatible, and YAML indented by two spaces. Use lowercase Redis directive names and one setting per line. Published tags are derived from `FROM`; do not add SHA aliases manually.

## Testing Guidelines

Every change must build and return `PONG`. Configuration changes should also verify effective values with `redis-cli CONFIG GET`, especially `maxmemory` and `maxmemory-policy`. Confirm the container reaches a healthy state after healthcheck changes.

## Commit & Pull Request Guidelines

Follow the existing short, imperative style, for example `Update Redis image and align Alpine tags`. Pull requests should describe configuration or tag changes, include verification commands, and mention deployment implications. Never commit Docker Hub tokens or Redis credentials; CI reads `DOCKERHUB_USERNAME` and `DOCKERHUB_TOKEN` from GitHub Actions secrets.
