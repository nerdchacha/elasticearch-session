## Settings
```
docker container exec -it docker-elk_elasticsearch_1 /bin/bash
cd config
cat elasticsearch.yml
```

## Cluster Health
```
GET /_cluster/health
```

## Cat indices
```
GET /_cat/indices?v
```

## Cat shards
```
GET /_cat/shards?v
```

## Create index
```
PUT /qualitia

GET /qualitia/_settings

DELETE /qualitia

PUT /qualitia
{
  "settings": {
    "number_of_shards": 1,
    "number_of_replicas": 2,
  }
}

POST /qualitia/_doc
{
  "employee_name": "Yatin",
  "employee_id": 114,
  "manager": "Sanjuktha",
  "skills": ["elasticsearch", "javascript"]
}

GET /qualitia/_search
{
  "query": {
    "match_all": {}
  }
}

GET /qualitia/_doc/{id}

GET /qualitia/_mapping

- Simple
  - text
  - keyword
  - date
  - long
  - double
  - boolean
  - ip
- hierarchical
  - object
  - nested
- specialised
  - geo_point
  - geo_shape
  - completion


PUT /qualitia
{
  "mappings": {
    "properties": {
      "employee_name": {
        "type": "text"
      },
      "employee_id": {
        "type": "integer"
      },
      "manager": {
        "type": "keyword"
      },
      "skills": {
        "type": "text"
      }
    }
  }
}

PUT /throwaway/_doc/1
{
  "id": 21,
  "name": "Yatin"
}

PUT /throwaway/_doc/2
{
  "id": "22",
  "name": "Pankaj"
}

GET /throwaway/_mapping

GET /throwaway/_search
{
  "query": {
    "match_all": {}
  }
}

- analyser
- coerce
- copy_to
- fields
- ignore_above
- null_value
```

<img src="https://www.clipartmax.com/png/full/47-477493_blue-dot-clip-art-at-clker-blue-dot-icon-png.png" width="50" height="50" />

## Analyser
```
GET /_analyze
{
  "analyzer" : "standard",
  "text" : ["elasticsearch", "javascript"]
}

GET /_analyze
{
  "analyzer" : "standard",
  "text" : "elasticsearch javascript"
}

GET /_analyze
{
  "analyzer" : "standard",
  "text" : ["elasticsearch", "javascript"],
  "explain": true
}

GET /_analyze
{
  "analyzer" : "standard",
  "text" : "elasticsearch javascript",
  "explain": true
}
```

## Standard Analyser
```
GET /_analyze
{
  "analyzer" : "standard",
  "text" : "This is john's dog and he is quite cute-looking",
  "explain": true
}
```

## Simple Analyser
```
GET /_analyze
{
  "analyzer" : "simple",
  "text" : "This is john's dog and is cute-looking",
  "explain": true
}
```

## Whitespace Analyser
```
GET /_analyze
{
  "analyzer" : "whitespace",
  "text" : "This is john's dog and is cute-looking",
  "explain": true
}
```

## Keyword analyser
```
GET /_analyze
{
  "analyzer" : "keyword",
  "text" : "This is john's dog and is cute-looking",
  "explain": true
}
```

## English Analyser
```
GET /_analyze
{
  "analyzer" : "english",
  "text" : "This is john's dog and is cute-looking",
  "explain": true
}
```

## Configuring analysers example
```
GET /_analyze
{
  "char_filter": ["html_strip"],
  "tokenizer": "standard",
  "filter": ["lowercase"], 
  "text" : "<p>This is a really cute <strong>dog</strong></p>",
  "explain": true
}
```

<img src="https://www.clipartmax.com/png/full/47-477493_blue-dot-clip-art-at-clker-blue-dot-icon-png.png" width="50" height="50" />

## Math all query
```
GET /products/_search
{
  "query": {
    "match_all": {}
  }
}
```

<img src="https://www.clipartmax.com/png/full/47-477493_blue-dot-clip-art-at-clker-blue-dot-icon-png.png" width="50" height="50" />

## Relevance score
```
GET /products/_search
{
  "query": {
    "term": {
      "name": {
        "value": "wine"
      }
    }
  },
  "explain": true
}

"N, total number of documents with field"
```

<img src="https://www.clipartmax.com/png/full/47-477493_blue-dot-clip-art-at-clker-blue-dot-icon-png.png" width="50" height="50" />

## Term level queries V/S Full text queries
```
GET /products/_search
{
  "query": {
    "term": {
      "name": "wine"
    }
  }
}

GET /products/_search
{
  "query": {
    "term": {
      "name": "Wine"
    }
  }
}

GET /products/_search
{
  "query": {
    "match": {
      "name": "Wine"
    }
  }
}
```

## Match query with custom analyser
```
GET /products/_search
{
  "query": {
    "match": {
      "name": {
        "query": "wine's",
        "analyzer": "english"
      }
    }
  }
}
```

## Match query with operator
```
GET /products/_search
{
  "query": {
    "match": {
      "name": {
        "query": "ice cream",
        , "operator": "and"
      }
    }
  }
}
```

## Match Phrase query
```
GET /products/_search
{
  "query": {
    "match": {
      "name": {
        "query": "cheese feta", 
        "operator": "and"
      }
    }
  }
}

GET /products/_search
{
  "query": {
    "match": {
      "name": {
        "query": "feta cheese", 
        "operator": "and"
      }
    }
  }
}

GET /products/_search
{
  "query": {
    "match_phrase": {
      "name": "cheese feta"
    }
  }
}

GET /products/_search
{
  "query": {
    "match_phrase": {
      "name": "feta cheese"
    }
  }
}
```

## Range query
```
GET /products/_search
{
  "query": {
    "range": {
      "in_stock": {
        "gte": 1,
        "lte": 5
      }
    }
  }
}
```

## Wildcard query
```
GET /products/_search
{
  "query": {
    "wildcard": {
      "tags.keyword": "Ve?e*"
    }
  }
}
```

## Regex query
```
GET /products/_search
{
  "query": {
    "regexp": {
      "tags.keyword": "Ve.*"
    }
  }
}

```

## Bool query
```
GET /products/_search
{
  "query": {
    "bool": {
      "must": [
        {
          "match": {
            "name": "tea"
          }
        },
        {
          "range": {
            "in_stock": {
              "lte": 11
            }
          }
        }
      ]
    }
  }
}

GET /products/_search
{
  "query": {
    "bool": {
      "must": [
        {
          "match": {
            "name": "tea"
          }
        }
      ],
      "filter": [
        {
          "range": {
            "in_stock": {
              "lte": 11
            }
          }
        }
      ]
    }
  }
}

- Scores are 1 lower because elasicsearch adds 1 as a score to range and other non relevant queries

- SHOULD  - boost score if match but are not required to match
```

<img src="https://www.clipartmax.com/png/full/47-477493_blue-dot-clip-art-at-clker-blue-dot-icon-png.png" width="50" height="50" />

## Metric Aggregation
```
GET /order/_search
{
  "size": 0,
  "aggs": {
    "total": {
      "sum": {
        "field": "total_amount"
      }
    }
  }
}

GET /order/_search
{
  "size": 0,
  "aggs": {
    "total": {
      "sum": {
        "field": "total_amount"
      }
    },
    "average": {
      "avg": {
        "field": "total_amount"
      }
    },
    "minimum": {
      "min": {
        "field": "total_amount"
      }
    },
    "maximum": {
      "max": {
        "field": "total_amount"
      }
    }
  }
}

GET /order/_search
{
  "size": 0,
  "aggs": {
    "salesman": {
      "cardinality": {
        "field": "salesman.id"
      }
    }
  }
}

- Cannot have sub aggregation
```

## Bucket Aggregation
```
GET /order/_search
{
  "size": 0,
  "aggs": {
    "status": {
      "terms": {
        "field": "status"
      }
    }
  }
}
```

## Nested Aggregation
```
GET /order/_search
{
  "size": 0,
  "aggs": {
    "status": {
      "terms": {
        "field": "status"
      },
      "aggs": {
        "sales_channel": {
          "terms": {
            "field": "sales_channel"
          }
        }
      }
    }
  }
}


GET /order/_search
{
  "size": 0,
  "query": {
    "range": {
      "total_amount": {
        "lt": 100
      }
    }
  },
  "aggs": {
    "stats": {
      "stats": {
        "field": "total_amount"
      }
    }
  }
}
```

## Range aggregation
```
GET /order/_search
{
  "size": 0,
  "aggs": {
    "distribution": {
      "range": {
        "field": "total_amount",
        "ranges": [
          {
            "to": 50
          },
          {
            "from": 50, 
            "to": 100
          },
          {
            "from": 100
          }
        ]
      }
    }
  }
}
```

<img src="https://www.clipartmax.com/png/full/47-477493_blue-dot-clip-art-at-clker-blue-dot-icon-png.png" width="50" height="50" />

## Proximity query
```
GET /proximity/_search
{
  "query": {
    "match_phrase": {
      "title": {
        "query": "spicy sauce"
      }
    }
  }
}

GET /proximity/_search
{
  "query": {
    "match_phrase": {
      "title": {
        "query": "spicy sauce",
        "slop": 1
      }
    }
  }
}

GET /proximity/_search
{
  "query": {
    "match_phrase": {
      "title": {
        "query": "spicy sauce",
        "slop": 2
      }
    }
  }
}
```

## Fuzzy query
```
GET /products/_search
{
  "query": {
    "match": {
      "name": {
        "query": "lobster",
        "fuzziness": "auto"
      }
    }
  }
}

auto
terms 1-2, 0 fuzziness
terms 3-5, 1 fuzziness
terms >5, 2 fuzziness
```

## Highlight
```
GET /products/_search
{
  "query": {
    "match": {
      "name": "wine"
    }
  },
  "highlight": {
    "fields": {
      "name": {}
    }
  }
}
```