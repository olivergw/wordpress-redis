# Wordpress-Redis

A minimal Redis image optimised for WordPress object caching.

Built from an official Alpine-based Redis image with a single config tweak: 512 MB maxmemory with LRU eviction.

## Features

- **Memory**: 512 MB limit with `allkeys-lru` eviction
- **Health Check**: Built-in `PING` via `HEALTHCHECK` instruction
- **Multi-platform**: Multi-arch images for `linux/amd64` and `linux/arm64`
- **Auto-tagged**: Docker tags track every Redis minor/major release (e.g. `8.6.4`, `8-alpine`)

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

## Development Workflow

### Auto-build on push

Pushing to `master` automatically builds and pushes the `latest` tag to Docker Hub.
No extra steps needed.

### Versioned releases

1. Update the Redis version in the `Dockerfile` (e.g. `FROM redis:8.6.4-alpine`).
2. Create and push an annotated tag:
   ```bash
   git tag -a v8.6.4 -m "Release Redis 8.6.4"
   git push origin v8.6.4
   ```
   This triggers the workflow to build multi-arch images tagged as:
   `8.6.4`, `8.6`, `8`, `8.6.4-alpine`, `8-alpine`, and `latest`.

### Local testing (optional)

```bash
docker build -t wordpress-redis:test .
```
