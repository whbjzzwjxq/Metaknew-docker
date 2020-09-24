## es docker
docker run -d --name es_container -p 127.0.0.1:9200:9200 -p 127.0.0.1:9300:9300 -e "discovery.type=single-node" es:v1

## kibana docker
docker run -d --link YOUR_ELASTICSEARCH_CONTAINER_NAME_OR_ID:elasticsearch -p 5601:5601 kibana:7.6.2
docker run -d --link es_container:elasticsearch -p 127.0.0.1:5601:5601 kibana:7.6.2

## neo4j docker
