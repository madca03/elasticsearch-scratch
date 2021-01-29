```
POST my-index-scratch-01/_doc
{
  "region": "US",
  "manager": {
    "age": 30,
    "name": {
      "first": "John",
      "last": "Smith"
    }
  }
}
```

Request:

```
GET my-index-scratch-01/_search
```

Response:

```
{
  "took" : 0,
  "timed_out" : false,
  "_shards" : {
    "total" : 1,
    "successful" : 1,
    "skipped" : 0,
    "failed" : 0
  },
  "hits" : {
    "total" : {
      "value" : 1,
      "relation" : "eq"
    },
    "max_score" : 1.0,
    "hits" : [
      {
        "_index" : "my-index-scratch-01",
        "_type" : "_doc",
        "_id" : "wd7bTHcBwZ69sV00le7D",
        "_score" : 1.0,
        "_source" : {
          "region" : "US",
          "manager" : {
            "age" : 30,
            "name" : {
              "first" : "John",
              "last" : "Smith"
            }
          }
        }
      }
    ]
  }
}
```

Request:

```
GET my-index-scratch-01/_mapping
```

Response:

```
{
  "my-index-scratch-01" : {
    "mappings" : {
      "properties" : {
        "manager" : {
          "properties" : {
            "age" : {
              "type" : "long"
            },
            "name" : {
              "properties" : {
                "first" : {
                  "type" : "text",
                  "fields" : {
                    "keyword" : {
                      "type" : "keyword",
                      "ignore_above" : 256
                    }
                  }
                },
                "last" : {
                  "type" : "text",
                  "fields" : {
                    "keyword" : {
                      "type" : "keyword",
                      "ignore_above" : 256
                    }
                  }
                }
              }
            }
          }
        },
        "region" : {
          "type" : "text",
          "fields" : {
            "keyword" : {
              "type" : "keyword",
              "ignore_above" : 256
            }
          }
        }
      }
    }
  }
}
```
