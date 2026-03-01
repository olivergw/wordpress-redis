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

Pull a specific Redis-backed release:
```bash
docker pull <your-username>/wordpress-redis:8.6.1
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
2. Create and push a tag that matches the Redis version in the `Dockerfile`:
   ```bash
   git tag -a v8.6.1 -m "Release Redis 8.6.1"
   git push origin v8.6.1
   ```
   This creates Docker tags such as `8.6.1`, `8.6`, `8`, and `latest`

## Security Note

Remember to change the default password in `redis.conf` before deploying to production!
