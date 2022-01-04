# Elastic_Docker

## Objectif

apprendre à manipuler le `NoSQL` avec le moteur `elasticSearch`, bien sur toujours utilisé avec Docker.
Nous allons en plus de cela utiliser `Kibana` qui va nous permet de visualiser sur web nos data d'ElasticSearch. Pour cela nous allons devoir containeuriser ces 2 outils, puis nous utiliserons le docker-compose pour les utiliser plutot que les lancer separement.

## Partie 1 (sans docker-composite)

d'abords nous allons recuperer une image docker d'ElasticSearch :

```bash
docker pull docker.elastic.co/elasticsearch/elasticsearch:7.16.2
```

On creer un reseau pour notre BDD

```bash
docker network create
```

Ensuite nous allons comme dans la doc essayer de faire un cluster avec un seul node.

```bash
docker run -p 127.0.0.1:9200:9200 -p 127.0.0.1:9300:9300 -e "discovery.type=single-node" docker.elastic.co/elasticsearch/elasticsearch:7.16.2
```

> ce qui a failli faire crash mon pc.

Cette fonction nous allons l'incorporer a Kibana pour pouvoir utiliser elasticsearch.

Il faut donc l'egerement modifier la commande en lui donnant un name:

```bash
docker run --name elas1 --net elastic -p 9200:9200 -p 9300:9300 -e "discovery.type=single-node" docker.elastic.co/elasticsearch/elasticsearch:7.16.2
```

on recup aussi l'image kibana:

```bash
docker pull docker.elastic.co/kibana/kibana:7.16.2
```

puis on run:

```bash
docker run --name kib1 --net elastic -p 5601:5601 -e "ELASTICSEARCH_HOSTS=http://elas1:9200" docker.elastic.co/kibana/kibana:7.16.2
```

Enfin on va à l'adresse `http://localhost:5601/`

## Partie 2 (avec docker-compose)

On reprend le meme principe que les ligne de commande et on configure le fichier `docker-compose.yml`

> Cette partie n'a pas encore été tester car il y a un pb avec easticsearch lors du run qui s'arrete et laisse kibana atteindre
