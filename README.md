# Wordpress-Redis

A minimal Redis image optimised for WordPress object caching.

Built from an official Alpine-based Redis image with a single config tweak: 512 MB maxmemory with LRU eviction.

## Features

- **Memory**: 512 MB limit with `allkeys-lru` eviction
- **Health Check**: Built-in `PING` via `HEALTHCHECK` instruction
- **Multi-platform**: Multi-arch images for `linux/amd64` and `linux/arm64`
- **Auto-tagged**: Every push produces Alpine aliases derived from the Dockerfile, plus `latest`

## Configuration

Only two lines in `redis.conf`:

| Setting | Value |
|---------|-------|
| `maxmemory` | `512MB` |
| `maxmemory-policy` | `allkeys-lru` |

Everything else uses Redis defaults — no persistence, no password, no command renaming.  
Edit `redis.conf` if you need more tuning.

Unless a consuming deployment mounts another Redis configuration, this
repository's `redis.conf` remains the source of truth for cache memory and
eviction policy. Container memory limits remain deployment concerns and should
be set separately in Compose or the chosen orchestrator.

## Usage

Pull the latest release:
```bash
docker pull olivergw/wordpress-redis:latest
```

Pull a specific version:
```bash
docker pull olivergw/wordpress-redis:8.10.1-alpine
```

Run locally with the port restricted to the host loopback interface:
```bash
docker run -d \
  --name wordpress-redis \
  -p 127.0.0.1:6379:6379 \
  olivergw/wordpress-redis:latest
```

## Security

This image does not configure Redis authentication or TLS. Do not publish port
`6379` on a public or otherwise untrusted network. In production, attach Redis
and its WordPress consumer to a private container network without a host port.

## Tags

For the current base image `redis:8.10.1-alpine`:

| Tag | Example | Triggered when |
|-----|---------|----------------|
| Exact version | `8.10.1-alpine` | Every push to `main` |
| Minor version | `8.10-alpine` | Every push to `main` |
| Major version | `8-alpine` | Every push to `main` |
| Distribution | `alpine` | Every push to `main` |
| `latest` | `olivergw/wordpress-redis:latest` | Push to `main` |

The Alpine aliases mirror the Docker Official Image tags. `latest` is an additional convenience alias for the newest custom image. Update the `FROM` line and push to `main` to publish a new release.

### Automated updates

Dependabot checks the official Redis base image every Monday at 09:10
Europe/London and opens a pull request when a newer compatible tag is available.
It checks GitHub Actions shortly afterwards. Pull requests build both supported
architectures without publishing; merging a successful PR publishes the new
image and aliases from `main`. This process runs entirely on GitHub.

## Development

Local testing:

```bash
docker build -t wordpress-redis:test .
```

## License

This repository's original Dockerfile, healthcheck, configuration, workflow,
and documentation are licensed under the [MIT License](LICENSE). Redis and
software included in the resulting image retain their respective upstream
licenses.
