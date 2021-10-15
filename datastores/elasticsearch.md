# ElasticSearch

ElasticSearch is a [search engine](https://en.wikipedia.org/wiki/Search_engine_\(computing\)) based on the [Lucene](https://en.wikipedia.org/wiki/Lucene) library. It provides a distributed, [multitenant](https://en.wikipedia.org/wiki/Multitenancy)-capable [full-text search](https://en.wikipedia.org/wiki/Full-text_search) engine with an [HTTP](https://en.wikipedia.org/wiki/HTTP) web interface and schema-free [JSON](https://en.wikipedia.org/wiki/JSON) documents. 

ElasticSearch has a nice REST API to retrieve all important settings for a running cluster. 

## Check cluster health

A good start to check on the cluster health is the "/health" endpoint. 

```
curl localhost:9200/_cluster/health?pretty
{
  "cluster_name" : "global_eu-west-1",
  "status" : "green",
  "timed_out" : false,
  "number_of_nodes" : 68,
  "number_of_data_nodes" : 65,
  "active_primary_shards" : 16200,
  "active_shards" : 32400,
  "relocating_shards" : 4,
  "initializing_shards" : 0,
  "unassigned_shards" : 0,
  "delayed_unassigned_shards" : 0,
  "number_of_pending_tasks" : 0,
  "number_of_in_flight_fetch" : 0,
  "task_max_waiting_in_queue_millis" : 0,
  "active_shards_percent_as_number" : 100.0
}
```

## Configure cluster wide settings

If you want to reboot a machine or do some maintenance it makes sense to delay "index.unassigned.node_left.delayed_timeout" to 10min. Afterwards you can change it back to 30sec. For more details see [https://www.elastic.co/guide/en/elasticsearch/reference/current/delayed-allocation.html](https://www.elastic.co/guide/en/elasticsearch/reference/current/delayed-allocation.html)

```
curl -XPUT -H "Content-Type: application/json" \ 
  localhost:9200/_all/_settings \
  -d '{ "settings": { "index.unassigned.node_left.delayed_timeout": "5m" }}'
```

## Cat Endpoint

```
curl localhost:9200/_cat
=^.^=
/_cat/allocation
/_cat/shards
/_cat/shards/{index}
/_cat/master
/_cat/nodes
/_cat/indices
/_cat/indices/{index}
/_cat/segments
/_cat/segments/{index}
/_cat/count
/_cat/count/{index}
/_cat/recovery
/_cat/recovery/{index}
/_cat/health
/_cat/pending_tasks
/_cat/aliases
/_cat/aliases/{alias}
/_cat/thread_pool
/_cat/plugins
/_cat/fielddata
/_cat/fielddata/{fields}
/_cat/nodeattrs
/_cat/repositories
/_cat/snapshots/{repository}
```
