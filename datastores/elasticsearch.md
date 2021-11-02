# ElasticSearch

ElasticSearch is a [search engine](https://en.wikipedia.org/wiki/Search\_engine\_\(computing\)) based on the [Lucene](https://en.wikipedia.org/wiki/Lucene) library. It provides a distributed, [multitenant](https://en.wikipedia.org/wiki/Multitenancy)-capable [full-text search](https://en.wikipedia.org/wiki/Full-text\_search) engine with an [HTTP](https://en.wikipedia.org/wiki/HTTP) web interface and schema-free [JSON](https://en.wikipedia.org/wiki/JSON) documents.&#x20;

ElasticSearch has a nice REST API to retrieve all important settings for a running cluster.&#x20;

## Check cluster health

A good start to check on the cluster health is the "/health" endpoint.&#x20;

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

If you want to reboot a machine or do some maintenance it makes sense to delay "index.unassigned.node\_left.delayed\_timeout" to 10min. Afterwards you can change it back to 30sec. For more details see [https://www.elastic.co/guide/en/elasticsearch/reference/current/delayed-allocation.html](https://www.elastic.co/guide/en/elasticsearch/reference/current/delayed-allocation.html)

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

## Check pending tasks

```
curl localhost:9200/_cluster/pending_tasks?pretty
```

## Check max result size settings

```
curl -s localhost:9200/_settings | jq . | grep max_result_window | sort | uniq -c
     64         "max_result_window": "150000",
   2586         "max_result_window": "300000",
```

## Get list of nodes

```
curl localhost:9200/_cat/nodes
10.254.101.109 10.254.101.109 79 99 3.13 d - Aragorn
10.254.105.237 10.254.105.237 82 99 9.42 d - Geirrodur
10.254.127.205 10.254.127.205 65 99 3.06 d - Nezarr the Calculator
10.254.122.73  10.254.122.73  56 98 3.57 d - Psi-Lord
10.254.84.58   10.254.84.58   38 99 5.29 d - Patriot II
10.254.126.196 10.254.126.196 45 99 4.90 d - Abominatrix
10.254.95.218  10.254.95.218  54 99 2.69 d - Warstrike
...
```

## Check Cluster settings

```
curl -s localhost:9200/_cluster/settings | jq .
{
  "persistent": {
    "cluster": {
      "routing": {
        "allocation": {
          "cluster_concurrent_rebalance": "5",
          "node_concurrent_recoveries": "10",
          "disk": {
            "watermark": {
              "low": "70%",
              "high": "73%"
            }
          }
        }
      }
    },
    "indices": {
      "breaker": {
        "fielddata": {
          "limit": "65%"
        },
        "request": {
          "limit": "35%"
        }
      },
      "recovery": {
        "concurrent_streams": "5",
        "max_bytes_per_sec": "200mb"
      }
    }
  },
  "transient": {
    "cluster": {
      "routing": {
        "allocation": {
          "cluster_concurrent_rebalance": "5",
          "node_concurrent_recoveries": "10",
          "disk": {
            "threshold_enabled": "true",
            "watermark": {
              "low": "78%",
              "high": "85%"
            }
          },
          "exclude": {
            "_ip": ""
          },
          "awareness": {
            "attributes": "az",
            "force": {
              "az": {
                "values": "eu-west-1a,eu-west-1b,eu-west-1c"
              }
            }
          },
          "enable": "all"
        }
      }
    },
    "logger": {
      "_root": "INFO",
      "action": "INFO"
    }
  }
}
```

### Find problematic shards

If nodes crash and leave the cluster the status for the affected shards will change to "NODE\_LEFT".

```
curl -XGET localhost:9200/_cat/shards | grep -v STARTED
```

