FROM ubuntu:latest
LABEL maintainer="xrsec"
LABEL mail="troy@zygd.site"
LABEL Github="https://github.com/XRSec/Docker_Zeek"
LABEL org.opencontainers.image.source="https://github.com/XRSec/Docker_Zeek"
LABEL org.opencontainers.image.title="Docker_Zeek"
COPY entrypoint.sh /usr/local/bin/entrypoint

RUN apt-get update \
  && apt-get install --no-install-recommends gnupg2 wget ca-certificates -y \
  && wget -qO - https://artifacts.elastic.co/GPG-KEY-elasticsearch | apt-key add - \
  && echo "deb https://artifacts.elastic.co/packages/7.x/apt stable main" | tee -a /etc/apt/sources.list.d/elastic-7.x.list \
  && apt-get update \
  && apt-get install --no-install-recommends kibana \
  && chmod +x /usr/local/bin/entrypoint \
  && useradd -s /sbin/nologin -d /nonexistent kibana \

ENV PATH $PATH:/usr/share/kibana/bin
COPY config/kibana/kibana.yml /usr/share/kibana/config/kibana.yml
WORKDIR /usr/share/kibana
EXPOSE 5601
ENTRYPOINT ["entrypoint"]
CMD ["kibana"]