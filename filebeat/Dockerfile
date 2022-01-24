FROM ubuntu:latest
LABEL maintainer="xrsec"
LABEL mail="troy@zygd.site"
LABEL Github="https://github.com/XRSec/Docker_Zeek"
LABEL org.opencontainers.image.source="https://github.com/XRSec/Docker_Zeek"
LABEL org.opencontainers.image.title="Docker_Zeek"
COPY filebeat/entrypoint.sh /usr/local/bin/entrypoint

RUN apt-get update \
	&& apt-get install --no-install-recommends gnupg2 wget curl ca-certificates -y \
	&& curl -s https://artifacts.elastic.co/GPG-KEY-elasticsearch | apt-key add - \
	&& echo "deb https://artifacts.elastic.co/packages/7.x/apt stable main" | tee -a /etc/apt/sources.list.d/elastic-7.x.list \
	&& apt-get update \
	&& apt-get install --no-install-recommends filebeat \
	&& chmod +x /usr/local/bin/entrypoint \
	&& mkdir /usr/share/filebeat/logs /usr/share/filebeat/data

ENV PATH $PATH:/usr/share/filebeat
COPY filebeat/config/filebeat.yml /usr/share/filebeat/filebeat.yml
WORKDIR /usr/share/filebeat
ENTRYPOINT ["entrypoint"]
CMD ["-h"]