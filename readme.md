## include env variable and auth
export $(cat .env | xargs)
export $(cat .env.secret | xargs)
chmod -R 777 $VOLUME

## es docker
docker run -d --name $ES_NAME \
 -p $LOCALHOST:$ES_PORT_API:9200 \
 -p $LOCALHOST:$ES_PORT_CLUSTER:9300 \
 -e "discovery.type=single-node" \
 -v $VOLUME/elasticsearch/data:/usr/share/elasticsearch/data \
 -v $VOLUME/elasticsearch/log:/usr/share/elasticsearch/log \
 metaknew_es:$RELEASE_VERSION

## kibana docker
docker run -d --name $KIBANA_NAME \
 -p $LOCALHOST:$KIBANA_PORT:5601 \
 --link $ES_NAME:elasticsearch \
 kibana:7.6.2

## neo4j docker
docker run -d --name $NEO4J_NAME \
 -p $LOCALHOST:$NEO4J_PORT_API:7474 \
 -p $LOCALHOST:$NEO4J_PORT_BOLT:7687 \
 -v $VOLUME/neo4j/data:/data \
 -v $VOLUME/neo4j/logs:/logs \
 -e NEO4J_AUTH=neo4j/$NEO4J_PASSWORD \
 neo4j:3.5

## redis docker
docker run -d --name $REDIS_NAME \
 -p $LOCALHOST:$REDIS_PORT:6379 \
 redis:5.0.9

## postgresql docker
## comfirm $VOLUME/postgres is empty
docker run -d --name $POSTGRES_NAME \
 -p $LOCALHOST:$POSTGRES_PORT:5432 \
 -e POSTGRES_USER=$POSTGRES_USER \
 -e POSTGRES_PASSWORD=$POSTGRES_PASSWORD \
 -e POSTGRES_DB=$POSTGRES_DB \
 -e PGDATA=$POSTGRES_VOLUME/pgdata \
 -v $VOLUME/postgres:$POSTGRES_VOLUME \
 --shm-size=256MB \
 postgres:11 \
 -c 'listen_addresses='*''

## metaknew
pip3 install -r requirements.txt -i https://pypi.tuna.tsinghua.edu.cn/simple
python3 manage.py runserver 127.0.0.1:9000
