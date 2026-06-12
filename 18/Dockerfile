FROM ghcr.io/cloudnative-pg/postgresql:18.4-standard-trixie@sha256:184219ecec559d15fa03932b0d3005e0372f7027746bb682aca478bc4918f776

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
        "timescaledb-$(echo $TIMESCALEDB_VERSION | cut -d '.' -f1)-postgresql-18=${TIMESCALEDB_VERSION}*" \
        "timescaledb-$(echo $TIMESCALEDB_VERSION | cut -d '.' -f1)-loader-postgresql-18=${TIMESCALEDB_VERSION}*" \
        "postgresql-18-cron" \
        "postgresql-18-postgis" \
    ; \
    apt-get purge -y --auto-remove wget lsb-release; \
    rm -rf /tmp/* ; \
    rm -rf /var/lib/apt/lists/*;

# Switch back to the postgres user
USER postgres
