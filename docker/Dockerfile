ARG ubuntu_release=jammy
FROM ubuntu:${ubuntu_release} as base

ARG clickhouse_version="23.12.2.59"

# Install build dependencies
RUN apt-get update && apt-get install -y --no-install-recommends  \
 && apt-get clean \
 && rm -rf /var/lib/apt/lists/*

# Install GPG and dirmngr
RUN apt-get update && apt-get install -y apt-transport-https=2.4.11 ca-certificates=20230311ubuntu0.22.04.1 dirmngr=2.2.27-3ubuntu2.1 gnupg=2.2.27-3ubuntu2.1 --no-install-recommends \
 && apt-get clean \
 && rm -rf /var/lib/apt/lists/*


# From https://clickhouse.com/docs/en/install#available-installation-options
SHELL ["/bin/bash", "-o", "pipefail", "-c"]
RUN GNUPGHOME=$(mktemp -d) \
 && GNUPGHOME="$GNUPGHOME" gpg --no-default-keyring --keyring /usr/share/keyrings/clickhouse-keyring.gpg --keyserver hkp://keyserver.ubuntu.com:80 --recv-keys 8919F6BD2B48D754 \
 && rm -rf "$GNUPGHOME" \
 && chmod +r /usr/share/keyrings/clickhouse-keyring.gpg \
 && echo "deb [signed-by=/usr/share/keyrings/clickhouse-keyring.gpg] https://packages.clickhouse.com/deb stable main" | tee /etc/apt/sources.list.d/clickhouse.list \
 && apt-get update

# prevent cli popup requesting password
ENV DEBIAN_FRONTEND=noninteractive

# Install Clickhouse server and client
RUN apt-get install -y clickhouse-server=${clickhouse_version} clickhouse-client=${clickhouse_version} --no-install-recommends \
 && apt-get clean \
 && rm -rf /var/lib/apt/lists/*


# Set configs
#COPY ./clickhouse.conf /etc/clickhouse-server/config.xml

EXPOSE 8123 9000

# Start
# CMD ["/usr/bin/clickhouse-server"]



