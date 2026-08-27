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

## Usage

Pull the latest release:
```bash
docker pull olivergw/wordpress-redis:latest
```

Pull a specific version:
```bash
docker pull olivergw/wordpress-redis:8.10.1-alpine
```

Run:
```bash
docker run -d \
  --name wordpress-redis \
  -p 6379:6379 \
  olivergw/wordpress-redis:latest
```

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

## Development

Local testing:

```bash
docker build -t wordpress-redis:test .
```
