FROM ghcr.io/cloudnative-pg/postgresql:17.4-bookworm

# Switch to root user to install timescaledb
USER root

# Install timescaledb
RUN set -xe; \
    apt-get update; \
    apt-get install -y --no-install-recommends \
        "postgresql-${PG_MAJOR}-timescaledb" \
        "postgresql-${PG_MAJOR}-cron" \
        "postgresql-${PG_MAJOR}-postgis" \
    ; \
    rm -fr /tmp/* ; \
    rm -rf /var/lib/apt/lists/*;

# Switch to the postgres user back
USER postgres
