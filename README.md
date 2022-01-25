## elasticsearch

### Set The Password

```
# Start elasticsearch

mv $(pwd)/elastic/config $(pwd)/elastic/config.bak

docker run -it --rm \
--name test \
elastic/elasticsearch:7.16.3 sleep 20 &

# Copy Config

docker cp test:/usr/share/elasticsearch/config $(pwd)/elastic/

cp $(pwd)/elastic/config.bak/elasticsearch.yml $(pwd)/elastic/config/elasticsearch.yml

# Set The Password

<!-- docker run -it --rm \
-e XPACK_SECURITY_ENABLED=true \
-v $(pwd)/elastic/config:/usr/share/elasticsearch/config \
elastic/elasticsearch:7.16.3 \
bin/elasticsearch-keystore create -p -->

docker run -it --rm \
-e XPACK_SECURITY_ENABLED=true \
-v $(pwd)/elastic/config:/usr/share/elasticsearch/config \
elastic/elasticsearch:7.16.3 \
bin/elasticsearch-keystore add bootstrap.password

## Set The Password
docker-compose up elasticsearch -d

docker-compose exec elasticsearch elasticsearch-setup-passwords interactive
```

### Generate a certificate

```bash
please google
docker-compose exec elasticsearch elasticsearch-certutil ca
docker-compose exec elasticsearch elasticsearch-certutil cert --ca elastic-stack-ca.p12
docker-compose exec elasticsearch mkdir data/cert
docker-compose exec elasticsearch mv elastic-certificates.p12 data/cert/
docker-compose exec elasticsearch mv elastic-stack-ca.p12 data/cert/
```

## kibana

### Set The Password

```
# Start kibana

mv $(pwd)/kibana/config $(pwd)/kibana/config.bak

docker run -it --rm \
--name test \
elastic/kibana:7.16.3 sleep 20 &

# Copy Config

docker cp test:/usr/share/kibana/config $(pwd)/kibana/

cp $(pwd)/kibana/config.bak/kibana.yml $(pwd)/kibana/config/kibana.yml
```

## filebeat

```bash
chmod go-w $(pwd)/filebeat/config/filebeat.yml
```

## zeek

```bash
docker cp zeek:/usr/local/zeek/share/zeek/site zeek
```

## Debug

```bash
docker run -it --rm \
-e "discovery.type=single-node" \
-e "XPACK_SECURITY=true" \
-e "kibana_PASSWORD=123456" \
-v $(pwd)/kibana/config/kibana.yml:/etc/kibana/kibana.yml:rw \
-v $(pwd)/kibana/entrypoint.sh:/usr/local/bin/entrypoint:ro  \
-p "9200:9200" \
blacktop/kibana:x-pack-7.7.1 bash


entrypoint kibana
```


```bash
curl --silent "${kibana_HOSTS}/_cat/health" | awk '{print $4}'
```