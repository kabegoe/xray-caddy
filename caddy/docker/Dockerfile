# Caddy with Cloudflare plugin

# See *Adding custom Caddy modules* section from
# https://hub.docker.com/_/caddy?tab=description

ARG version

#
# Builder
#
FROM caddy:$version-builder-alpine AS builder
# Add Cloudflare module
RUN xcaddy build \
    --with github.com/caddy-dns/cloudflare

#
# Caddy image
#
FROM caddy:$version-alpine
COPY --from=builder /usr/bin/caddy /usr/bin/caddy

CMD ["caddy", "run", "--config", "/etc/caddy/Caddyfile"]
