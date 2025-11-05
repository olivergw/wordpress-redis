# Wordpress-Redis
## A simple Redis container optimized for WordPress

A Redis container configured specifically for WordPress object caching with MariaDB backend.

## Features

- **Memory Management**: 256MB limit with LRU eviction policy
- **Security**: Localhost binding, password protection, dangerous commands disabled
- **Performance**: Optimized for cache use with disabled persistence
- **Health Check**: Built-in Redis ping check
- **Multi-platform**: Supports linux/amd64 and linux/arm64

## Configuration

- maxmemory: 256MB
- maxmemory-policy: allkeys-lru
- Persistence: Disabled (save "", appendonly no)
- Client timeout: 300 seconds
- Max clients: 1000
- Slowlog enabled for troubleshooting

## Usage

Pull from Docker Hub:
```bash
docker pull <your-username>/wordpress-redis:latest
```

Run the container:
```bash
docker run -d \
  --name wordpress-redis \
  -p 6379:6379 \
  <your-username>/wordpress-redis:latest
```

## Development Workflow

### Making Changes

1. Make your changes to files (redis.conf, Dockerfile, etc.)
2. Test locally (optional):
   ```bash
   docker build -t wordpress-redis:test .
   ```
3. Commit and push to master:
   ```bash
   git add .
   git commit -m "Your descriptive message"
   git push origin master
   ```
   This automatically builds and pushes the `latest` tag to Docker Hub.

### Creating Versioned Releases

1. Make and commit your changes
2. Create and push a version tag:
   ```bash
   git tag -a v1.0.0 -m "Release version 1.0.0"
   git push origin v1.0.0
   ```
   This creates multiple Docker tags: `1.0.0`, `1.0`, `1`, and `latest`

### Versioning Guidelines

- **Patch update** (bug fixes): `v1.0.1`
- **Minor update** (new features): `v1.1.0`
- **Major update** (breaking changes): `v2.0.0`

## Security Note

Remember to change the default password in `redis.conf` before deploying to production!