FROM ghcr.io/cloudnative-pg/postgresql:16.9-bookworm

ENV TIMESCALEDB_MAJOR=2
ENV TIMESCALEDB_VERSION=2.21.0

# Switch to root user to install extensions
USER root

# Add timescaledb repository, and install extensions
RUN set -xe; \
    apt-get update; \
    apt-get install -y --no-install-recommends wget lsb-release; \
    echo "deb https://packagecloud.io/timescale/timescaledb/debian/ $(lsb_release -cs) main" > /etc/apt/sources.list.d/timescaledb.list; \
    wget --quiet -O - https://packagecloud.io/timescale/timescaledb/gpgkey | gpg --dearmor -o /etc/apt/trusted.gpg.d/timescaledb.gpg; \
    apt-get update; \
    apt-get install -y --no-install-recommends \
        "timescaledb-${TIMESCALEDB_MAJOR}-postgresql-${PG_MAJOR}=${TIMESCALEDB_VERSION}*" \
        "timescaledb-${TIMESCALEDB_MAJOR}-loader-postgresql-${PG_MAJOR}=${TIMESCALEDB_VERSION}*" \
        "postgresql-${PG_MAJOR}-cron" \
        "postgresql-${PG_MAJOR}-postgis" \
    ; \
    apt-get purge -y --auto-remove wget lsb-release; \
    rm -rf /tmp/* ; \
    rm -rf /var/lib/apt/lists/*;

# Switch back to the postgres user
USER postgres
