# Wordpress-Redis

A minimal Redis image optimised for WordPress object caching.

Built from an official Alpine-based Redis image with a single config tweak: 512 MB maxmemory with LRU eviction.

## Features

- **Memory**: 512 MB limit with `allkeys-lru` eviction
- **Health Check**: Built-in `PING` via `HEALTHCHECK` instruction
- **Multi-platform**: Multi-arch images for `linux/amd64` and `linux/arm64`
- **Auto-tagged**: Every push produces a version tag from the Dockerfile, plus `latest` and commit SHA

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
docker pull <your-username>/wordpress-redis:latest
```

Pull a specific version:
```bash
docker pull <your-username>/wordpress-redis:8.6.4
```

Run:
```bash
docker run -d \
  --name wordpress-redis \
  -p 6379:6379 \
  <your-username>/wordpress-redis:latest
```

## Tags

For the current base image `redis:8.6.4-alpine`:

| Tag | Example | Triggered when |
|-----|---------|----------------|
| Version from Dockerfile | `8.6.4` (from `redis:8.6.4-alpine`) | Every push to `main` |
| `latest` | `olivergw/wordpress-redis:latest` | Push to `main` |
| Commit SHA | Short + long SHA | Every push |

Just update the `FROM` line in the Dockerfile and push to `main`.

## Development

Local testing:

```bash
docker build -t wordpress-redis:test .
```
