# Elasticsearch Hands On

## Start on docker

```bash
docker compose up -d
```

```bash
docker exec elasticsearch /usr/share/elasticsearch/bin/plugin install lmenezes/elasticsearch-kopf/2.1.2
```

After that, go to `http://localhost:9200/_plugin/kopf`.

## Commands

### Create document

```bash
curl -XPOST 'http://localhost:9200/<index>/<type>' -d '{ ... }'
```

### Update or create document with specific id

```bash
curl -XPUT 'http://localhost:9200/<index>/<type>/<id>' -d '{ ... }'
```

### Update document partially

```bash
curl -XPOST 'http://localhost:9200/<index>/<type>/<id>/_update' -d '{ doc: { ... } }'
```

### Delete document

```bash
curl -XDELETE 'http://localhost:9200/<index>/<type>/<id>'
```

### Get document by id

```bash
curl -XGET 'http://localhost:9200/<index>/<type>/<id>'
```

### Get all documents

```bash
curl -XGET 'http://localhost:9200/<index>/<type>/_search'
```

### Search document by query

```bash
curl -XGET 'http://localhost:9200/<index>/<type>/_search?q=<query>'
```

### Search document by attribute

```bash
curl -XGET 'http://localhost:9200/<index>/<type>/_search?q=<attribute>:<query>'
```

### Paginate documents

```bash
curl -XGET 'http://localhost:9200/<index>/<type>?size=<size>&from=<from>'
```

### Check if document exists

```bash
curl -XHEAD 'http://localhost:9200/<index>/<type>/<id>'
```

### Get type schema

```bash
curl -XGET 'http://localhost:9200/<index>/_mapping/<type>'
```

### Get tokens by analyzers

```bash
curl -XGET 'http://localhost:9200/<index>/_analyze?analyzer=standard&text=Eu+nasci+a+10+mil+(sim,+10+mil)+anos+atrás'
```

### Create index with analyzers

```bash
curl -XPUT 'http://localhost:9200/<index>' -d '{ "settings": { "index": { "number_of_shards": 3, "number_of_replicas": 0 } }, "mappings": { "<type>": { "_all": { "type": "string", "index": "analyzed", "analyzer": "portuguese" }, "properties": { "<attribute>": { "type": "string", "index": "analyzed", "analyzer": "portuguese", "search_analyzer": "portuguese" } } } }'
```

### Create index with synonym analyzers

```bash
curl -XPUT 'http://localhost:9200/<index>' -d '{ "settings": { "analysis": { "filter": { "synonym_filter": { "type": "synonym", "synonyms": [ "term_1,term_2,term_3" ] } }, "analyzer": { "sinonimos": { "tokenizer": "standard", "filter": [ "lowercase", "synonym_filter" ] } } } } }'
```

```bash
curl -XPUT 'http://localhost:9200/<index>' -d '{ "settings": { "analysis": { "filter": { "synonym_filter": { "type": "synonym", "synonyms": [ "term_1 => term_2,term_3" ] } }, "analyzer": { "sinonimos": { "tokenizer": "standard", "filter": [ "lowercase", "synonym_filter" ] } } } } }'
```

### Create data in batch

```bash
curl -XPOST 'http://localhost:9200/<index>' -d '{"create": {}} { ... } {"create": {}} { ... }'
```
