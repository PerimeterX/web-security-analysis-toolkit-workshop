```
GET _template
```

```
PUT _template/access_logs_data
{
  "order": 0,
  "template": "access_logs_data",
  "settings": {
    "index": {
      "refresh_interval": "5s"
    }
  },
  "mappings": {
    "_default_": {
      "_all": {
        "enabled": true,
        "norms": false
      },
      "dynamic_templates": [
        {
          "message_field": {
            "path_match": "message",
            "match_mapping_type": "string",
            "mapping": {
              "type": "text",
              "norms": false
            }
          }
        },
        {         
          "strings_as_keywords": {
            "match": "*",
            "match_mapping_type": "string",
            "mapping": {
              "type": "keyword"
            }
          }
        }
      ],
      "properties": {
        "@timestamp": {
          "type": "date",
          "copy_to": false
        },
        "@version": {
          "type": "keyword",
          "copy_to": false
        },
        "remote_ip": {
          "type": "ip"
        },
        "geoip": {
          "dynamic": true,
          "properties": {
            "ip": {
              "type": "ip"
            },
            "location": {
              "type": "geo_point"
            },
            "latitude": {
              "type": "half_float"
            },
            "longitude": {
              "type": "half_float"
            }
          }
        }
      }
    }
  },
  "aliases": {}
}
```
