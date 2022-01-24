FROM ubuntu:latest
LABEL maintainer="xrsec"
LABEL mail="troy@zygd.site"
LABEL Github="https://github.com/XRSec/Docker_Zeek"
LABEL org.opencontainers.image.source="https://github.com/XRSec/Docker_Zeek"
LABEL org.opencontainers.image.title="Docker_Zeek"
COPY elastic/entrypoint.sh /usr/local/bin/entrypoint

RUN apt-get update \
	&& apt-get install --no-install-recommends gnupg2 wget ca-certificates -y \
	&& wget -qO - https://artifacts.elastic.co/GPG-KEY-elasticsearch | apt-key add - \
	&& echo "deb https://artifacts.elastic.co/packages/7.x/apt stable main" | tee -a /etc/apt/sources.list.d/elastic-7.x.list \
	&& apt-get update \
	&& apt-get install --no-install-recommends elasticsearch \
	&& chmod +x /usr/local/bin/entrypoint

ENV PATH $PATH:/usr/share/elasticsearch/bin
WORKDIR /usr/share/elasticsearch

RUN set -ex \
    && for path in \
    ./data \
    ./logs \
    ./config \
    ./config/scripts \
    ./config/ingest-geoip \
    ; do \
    mkdir -p "$path"; \
    chown -R elasticsearch:elasticsearch "$path"; \
    done

COPY elastic/config/elasticsearch.yml /etc/elasticsearch/elasticsearch.yml
COPY elastic/config/log4j2.properties /etc/elasticsearch/og4j2.properties
COPY elastic/config/logrotate /etc/logrotate.d/elasticsearch
COPY elastic/docker-healthcheck /usr/local/bin/
VOLUME ["/usr/share/elasticsearch/data"]
EXPOSE 9200 9300
ENTRYPOINT ["entrypoint"]
CMD ["elasticsearch"]

# HEALTHCHECK CMD ["docker-healthcheck"]