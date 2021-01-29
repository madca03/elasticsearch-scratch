# Nested type
## https://www.elastic.co/guide/en/elasticsearch/reference/current/nested.html

### Difference in mapping between object and nested

#### Create new document with default object type

```
POST my-index-scratch-02/_doc
{
  "group": "fans",
  "user": [
    {
      "first": "John",
      "last": "Smith"
    },
    {
      "first": "Alice",
      "last": "White"
    }
  ]
}
```

#### Mapping:

Request:

```
GET my-index-scratch-02/_mapping
```

Response:

```
{
  "my-index-scratch-02" : {
    "mappings" : {
      "properties" : {
        "group" : {
          "type" : "text",
          "fields" : {
            "keyword" : {
              "type" : "keyword",
              "ignore_above" : 256
            }
          }
        },
        "user" : {
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
    }
  }
}
```

#### Association between `user.first` and `user.last` are lost

Request:

```
GET /my-index-scratch-02/_search
{
  "query": {
    "bool": {
      "must": [
        {
          "match": {
            "user.first": "Alice"
          }
        },
        {
          "match": {
            "user.last": "Smith"
          }
        }
      ]
    }
  }
}
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
    "max_score" : 0.5753642,
    "hits" : [
      {
        "_index" : "my-index-scratch-02",
        "_type" : "_doc",
        "_id" : "Ct7oTHcBwZ69sV00nfC-",
        "_score" : 0.5753642,
        "_source" : {
          "group" : "fans",
          "user" : [
            {
              "first" : "John",
              "last" : "Smith"
            },
            {
              "first" : "Alice",
              "last" : "White"
            }
          ]
        }
      }
    ]
  }
}
```

#### Create mapping with nested type

Request:

```
PUT my-index-scratch-03
{
  "mappings": {
    "properties": {
      "user": {
        "type": "nested"
      }
    }
  }
}
```

#### Add new document

Request:

```
POST my-index-scratch-03/_doc
{
  "group": "fans",
  "user": [
    {
      "first": "John",
      "last": "Smith"
    },
    {
      "first": "Alice",
      "last": "White"
    }
  ]
}
```

#### Get mapping of index with nested type

Request:

```
GET my-index-scratch-03/_mapping
```

Response:

```
{
  "my-index-scratch-03" : {
    "mappings" : {
      "properties" : {
        "group" : {
          "type" : "text",
          "fields" : {
            "keyword" : {
              "type" : "keyword",
              "ignore_above" : 256
            }
          }
        },
        "user" : {
          "type" : "nested",
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
    }
  }
}
```
