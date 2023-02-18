# Sample Code from the Index Management Episode
## Daily Elastic Byte s02e13
The purpose of this repo is capture my notes and code from the *[Daily Elastic Byte S02E13 Episode on YouTube](https://www.youtube.com/watch?v=eULpbvFJfDk)*.

### Key Points
* Index templates are used to control the mappings of indices


## Code samples executed in Dev Tools

### **Create, Delete a simple index called users**
To get a list of the indices use this command (~1:00): 
```
GET /_cat/indices
```
Create an index call users without an index template (~1:04)
```
PUT users 
```
Index a sample document containing two fields (~1:18)
```
POST users/_doc/1
{
  "firstname": "John",
  "lastname": "Doe"
}
```
Get the mapping of fields for the users index (~1:24)
```
GET users/_mapping
```
Get the settings for the users index (~1:40)
```
GET users/_mapping
```
Delete the users index (~4:43)
```
DELETE users
```
Create an index template for any index with the index pattern of beginning with user* (~4:43)
```
PUT _index_template/users
{
  "template": {
    "mappings": {
      "properties": {
        "firstname": {
          "type": "keyword"
        },
        "lastname": {
          "type": "keyword"
        }
      }
    }
  },
  "index_patterns": [
    "user*"
  ]
}
```
### **Create an time-series index called my-logs**
Create an Index Lifecycle Management called my-lifecycle-policy a pre-req for the index template (in the next step) (~7:22)
```
PUT _ilm/policy/my-lifecycle-policy
{
  "policy": {
    "phases": {
      "hot": {
        "min_age": "0ms",
        "actions": {
          "rollover": {
            "max_primary_shard_size": "50gb"
          }
        }
      },
      "warm": {
        "min_age": "30d",
        "actions": {
          "shrink": {
            "number_of_shards": 1
          },
          "forcemerge": {
            "max_num_segments": 1
          }
        }
      }
    }
  }
}
```
Create an index template called my-logs for time series data for any index with the index pattern of beginning with my* (~7:22)
```
PUT _index_template/my-logs
{
  "template": {
    "settings": {
      "index": {
        "lifecycle": {
          "name": "my-lifecycle-policy"
        },
        "number_of_replicas": 2
      }
    }
  },
  "index_patterns": [
    "my*"
  ],
  "composed_of": [
    "logs-mappings",
    "logs-settings"
  ]
}

```



#### References
* This is the *[Daily Elastic Byte S02E13 Episode on YouTube](https://www.youtube.com/watch?v=eULpbvFJfDk)*.
