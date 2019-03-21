# Wordpress-Redis
## A simple Redis container which works well for wordpress

maxmemory 256MB
maxmemory-policy allkeys-lru

There is also a healthcheck for redis within the container which issues a redis cli ping check
