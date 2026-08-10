FROM ghcr.io/cloudnative-pg/postgresql:17.10-system-trixie@sha256:74f29d3343f8db89135b33b0b4c55641c415b96d08605034a6d0c2782f929146

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
        "timescaledb-$(echo $TIMESCALEDB_VERSION | cut -d '.' -f1)-postgresql-17=${TIMESCALEDB_VERSION}*" \
        "timescaledb-$(echo $TIMESCALEDB_VERSION | cut -d '.' -f1)-loader-postgresql-17=${TIMESCALEDB_VERSION}*" \
        "postgresql-17-cron" \
        "postgresql-17-postgis" \
    ; \
    apt-get purge -y --auto-remove wget lsb-release; \
    rm -rf /tmp/* ; \
    rm -rf /var/lib/apt/lists/*;

# Switch back to the postgres user
USER postgres
