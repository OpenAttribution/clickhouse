ARG ubuntu_release=jammy
FROM ubuntu:${ubuntu_release} as base

ARG clickhouse_version="23.12.2.59"

# Install build dependencies
RUN apt-get update && apt-get install -y --no-install-recommends

# Install GPG and dirmngr
RUN apt-get install -y apt-transport-https ca-certificates dirmngr gnupg

# Create the .gnupg directory
RUN mkdir -p /root/.gnupg && chmod 700 /root/.gnupg

# From https://clickhouse.com/docs/en/install#available-installation-options
RUN NUPGHOME=$(mktemp -d)
RUN GNUPGHOME="$GNUPGHOME" gpg --no-default-keyring --keyring /usr/share/keyrings/clickhouse-keyring.gpg --keyserver hkp://keyserver.ubuntu.com:80 --recv-keys 8919F6BD2B48D754
RUN rm -rf "$GNUPGHOME"
RUN chmod +r /usr/share/keyrings/clickhouse-keyring.gpg
RUN echo "deb [signed-by=/usr/share/keyrings/clickhouse-keyring.gpg] https://packages.clickhouse.com/deb stable main" | tee /etc/apt/sources.list.d/clickhouse.list 
RUN apt-get update

# prevent cli popup requesting password
ENV DEBIAN_FRONTEND=noninteractive

# Install Clickhouse server and client
RUN apt-get install -y clickhouse-server=${clickhouse_version} clickhouse-client=${clickhouse_version}

# Set configs
#COPY ./clickhouse.conf /etc/clickhouse-server/config.xml

EXPOSE 8123 9000

# Start
CMD ["/usr/bin/clickhouse-server"]



