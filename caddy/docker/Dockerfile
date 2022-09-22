# Caddy with Cloudflare plugin

# See *Adding custom Caddy modules* section from
# https://hub.docker.com/_/caddy?tab=description

#
# Builder
#
ARG version
FROM caddy:$version-builder-alpine AS builder
# Add Cloudflare module
RUN xcaddy build \
    --with github.com/caddy-dns/cloudflare

#
# Caddy image
#
ARG version
FROM caddy:$version-alpine
ARG timezone
COPY --from=builder /usr/bin/caddy /usr/bin/caddy
RUN apk add --no-cache tzdata ca-certificates && \
    cp /usr/share/zoneinfo/$timezone /etc/localtime && \
    apk del tzdata
CMD ["caddy", "run", "--config", "/etc/caddy/Caddyfile"]
