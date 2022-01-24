FROM ubuntu:latest
LABEL maintainer="xrsec"
LABEL mail="troy@zygd.site"
LABEL Github="https://github.com/XRSec/Docker_Zeek"
LABEL org.opencontainers.image.source="https://github.com/XRSec/Docker_Zeek"
LABEL org.opencontainers.image.title="Docker_Zeek"
COPY kibana/entrypoint.sh /usr/local/bin/entrypoint

RUN apt-get update \
  && apt-get install --no-install-recommends gnupg2 wget curl ca-certificates -y \
  && wget -qO - https://artifacts.elastic.co/GPG-KEY-elasticsearch | apt-key add - \
  && echo "deb https://artifacts.elastic.co/packages/7.x/apt stable main" | tee -a /etc/apt/sources.list.d/elastic-7.x.list \
  && apt-get update \
  && apt-get install --no-install-recommends kibana \
  && chmod +x /usr/local/bin/entrypoint \
  # && chmod +x /usr/local/bin/entrypoint \
  # && useradd -s /sbin/nologin -d /nonexistent kibana
  # UBUNTU INSTALL SU-EXEC
  && curl -o /usr/local/bin/su-exec.c https://raw.githubusercontent.com/ncopa/su-exec/master/su-exec.c \
  && fetch_deps='gcc libc-dev' \
  && apt-get update \
  && apt-get install -y --no-install-recommends $fetch_deps \
  && gcc -Wall /usr/local/bin/su-exec.c -o /usr/local/bin/su-exec \
  && chown root:root /usr/local/bin/su-exec \
  && chmod 0755 /usr/local/bin/su-exec \
  && rm /usr/local/bin/su-exec.c \
  && apt-get purge -y --auto-remove $fetch_deps \
  && apt clean -y && apt autoclean -y \
  && rm -rf /var/lib/apt/lists/*

ENV PATH $PATH:/usr/share/kibana/bin
COPY kibana/config/kibana.yml /usr/share/kibana/config/kibana.yml
WORKDIR /usr/share/kibana
EXPOSE 5601
ENTRYPOINT ["entrypoint"]
CMD ["kibana"]