### Create new index with configured mapping for the nested type

Request:

```
PUT my-index-scratch-04
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

### Perform reindex / copy from the index with object type to the new index with nested type

Request:

```
POST _reindex
{
  "source": {
    "index": "my-index-scratch-02"
  },
  "dest": {
    "index": "my-index-scratch-04"
  }
}
```

### Can now run nested query on new index with nested type

Request:

```
GET /my-index-scratch-04/_search
{
  "query": {
    "nested": {
      "path": "user",
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
                "user.last": "White"
              }
            }
          ]
        }
      },
      "inner_hits": {
        "highlight": {
          "fields": {
            "user.first": {}
          }
        }
      }
    }
  }
}
```
