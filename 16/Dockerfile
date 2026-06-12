FROM ghcr.io/cloudnative-pg/postgresql:16.14-system-trixie@sha256:5471d3391c27f3fa2a39af38a29d44ca1a18fbf73633d9988a18718c563d3fa1

ENV TIMESCALEDB_VERSION=2.27.2

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
        "timescaledb-$(echo $TIMESCALEDB_VERSION | cut -d '.' -f1)-postgresql-16=${TIMESCALEDB_VERSION}*" \
        "timescaledb-$(echo $TIMESCALEDB_VERSION | cut -d '.' -f1)-loader-postgresql-16=${TIMESCALEDB_VERSION}*" \
        "postgresql-16-cron" \
        "postgresql-16-postgis" \
    ; \
    apt-get purge -y --auto-remove wget lsb-release; \
    rm -rf /tmp/* ; \
    rm -rf /var/lib/apt/lists/*;

# Switch back to the postgres user
USER postgres
