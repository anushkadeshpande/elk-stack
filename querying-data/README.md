```
GET news_headlines/_search
```

This does not hit all records (hits 10000 only)

<hr>

#### To get total number of hits
```
GET news_headlines/_search
{
  "track_total_hits": true
}
```

<hr>

#### Querying data within a time range
```
GET news_headlines/_search
{
  "query": {
    "range": {
      "date": {
        "gte": "2015-06-20",
        "lte": "2015-09-22"
      }
    }
  }
}
```

<hr>

#### Aggregation
```
GET news_headlines/_search
{
  "aggs": {
    "by_category": {
      "terms": {
        "field": "category",
        "size": 100
      }
    }
  }
}
```

<hr>

#### Combination of query and aggregation

Performs a significant_text aggregation
```
GET news_headlines/_search
{
  "query": {
    "match": {
      "category": "ENTERTAINMENT"
    }
  },
  "aggregations": {
    "popular_in_entertainment": {
      "significant_text": {
        "field": "headline"
      }
    }
  }
}
```

#### Querying multiple keywords
```
GET news_headlines/_search
{
  "query": {
    "match": {
      "headline": {
        "query": "Khloe Kardashian Kendall Jenner"
      }
    }
  }
}
```

The above query returns 955 records

Internally, elasticsearch uses `or` logic to check for terms and will return any documents with any of the terms present in the query

The Match query focuses on increasing ***recall***

To increase the precision, use `and` operator

```
GET news_headlines/_search
{
  "query": {
    "match": {
      "headline": {
        "query": "Khloe Kardashian Kendall Jenner",
        "operator": "and"
      }
    }
  }
}
```

This returns just 1 record


#### minimum_should_match

This query specifies minimum how many of the given keywords should match

```
GET news_headlines/_search
{
  "query": {
    "match": {
      "headline": {
        "query": "Khloe Kardashian Kendall Jenner",
        "minimum_should_match": 3
      }
    }
  }
}
```

This returns 6 records