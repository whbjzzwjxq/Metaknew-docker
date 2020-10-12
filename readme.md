## network
docker network create --subnet=17.5.1.0/16 --gateway=17.5.1.1 metaknew_network

sh env.sh
## es docker
docker run -d --name $ES_NAME --7net=metaknew_network -e "discovery.type=single-node" es:v1

## kibana docker
docker run -d --link $ES_NAME:elasticsearch -p 127.0.0.1:5601:5601 kibana:7.6.2

## neo4j docker
docker run -d --name $NEO4J_NAME -

## nginx docker
docker run -d --net=host nginx:v1