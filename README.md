# Node Graph with Elasticsearch Data

## Deploy Elasticsearch

### Deploy Docker Image

```bash
docker network create ntwrk1
docker run --name es --net ntwrk1 -p 9200:9200 -e "http.host=0.0.0.0" -e "transport.host=127.0.0.1" -e "xpack.security.enabled=false" -d -m 1GB docker.elastic.co/elasticsearch/elasticsearch:8.11.1
```

## Add Data to Elasticsearch

```bash
curl -X PUT "localhost:9200/graph-data/_bulk?pretty" -H 'Content-Type: application/json' -d'
{ "create": { } }
{ "@timestamp": "2023-11-28T16:34:32.000Z", "id": "e-001", "source": "001", "target": "002", "edge": "true"}
{ "create": { } }
{ "@timestamp": "2023-11-28T16:34:32.000Z", "id": "e-002", "source": "001", "target": "004", "edge": "true"}
{ "create": { } }
{ "@timestamp": "2023-11-28T16:34:32.000Z", "id": "e-003", "source": "002", "target": "003", "edge": "true"}
{ "create": { } }
{ "@timestamp": "2023-11-28T16:34:32.000Z", "id": "001", "title": "Load Balancer", "mainstat": 0.95, "node": "true"}
{ "create": { } }
{ "@timestamp": "2023-11-28T16:34:32.000Z", "id": "002", "title": "Worker 1", "mainstat": 0.90, "node": "true"}
{ "create": { } }
{ "@timestamp": "2023-11-28T16:34:32.000Z", "id": "003", "title": "DB 1", "mainstat": 0.50, "node": "true"}
{ "create": { } }
{ "@timestamp": "2023-11-28T16:34:32.000Z", "id": "004", "title": "Worker 2", "mainstat": 0.15, "node": "true"}
'
```

## Deploy Grafana

### Deploy the Grafana Image

```bash
docker run --name grafana --net ntwrk1 -p 3000:3000 -d grafana/grafana-enterprise
```

### Login to Grafana

Open `localhost:3000` in a browser.  

Login with `admin/admin`

### Add Elasticsearch Data Source

Navigate to Connections > Data sources

Find Elasticsearch

Click Add New Data Source

Name: Elasticsearch

Connection  
URL: `http://localhost:9200`

Index: `graph-data`

## Clean Up

### Clean Up Index (in case)

`curl -X DELETE "localhost:9200/graph-data"`






