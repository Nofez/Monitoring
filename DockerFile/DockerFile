# Dockerfile
# ---- Stage 1: получаем статический бинарник envsubst ----
FROM golang:1.22-alpine AS envsubst-builder

RUN apk add --no-cache git \
    && go install github.com/a8m/envsubst/cmd/envsubst@latest

# ---- Stage 2: финальный образ на базе prometheus ----
FROM prom/prometheus:v3.4.1

# Копируем статический бинарник envsubst из builder-стадии
COPY --from=envsubst-builder /go/bin/envsubst /bin/envsubst

# Копируем entrypoint-скрипт
COPY entrypoint.sh /entrypoint.sh

USER root
RUN chmod +x /entrypoint.sh /bin/envsubst

ENTRYPOINT ["/entrypoint.sh"]