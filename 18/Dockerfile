FROM ghcr.io/cloudnative-pg/postgresql:18.3-standard-trixie@sha256:81bad466fe006454482678c6c67a24c1e30454b519011e0196cba2e4c83e1e1a

ENV TIMESCALEDB_VERSION=2.26.1

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
