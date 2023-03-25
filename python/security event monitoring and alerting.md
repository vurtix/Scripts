
```python

#!/usr/bin/python

import os
import sys
import json
import requests

# Set the Elasticsearch URL
elasticsearch_url = "https://elasticsearch.example.com"

# Set the Elasticsearch username and password
username = "elastic_user"
password = "elastic_password"

# Set the search queries
search_queries = [
  {
    "name": "Failed Logins",
    "query": {
      "query": {
        "match": {
          "event_id": "4625"
        }
      },
      "aggs": {
        "failed_logins_by_user": {
          "terms": {
            "field": "user_name.keyword",
            "size": 10
          }
        },
        "failed_logins_by_source_ip": {
          "terms": {
            "field": "source_ip.keyword",
            "size": 10
          }
        }
      }
    },
    "index": "winlogbeat-*",
    "time_range": {
      "range": {
        "@timestamp": {
          "gte": "now-1h"
        }
      }
    },
    "size": 10000,
    "alert": {
      "condition": {
        "compare": {
          "ctx.payload.hits.total": {
            "gte": 1000
          }
        }
      },
      "actions": {
        "send_email": {
          "email": {
            "to": "admin@example.com",
            "subject": "Failed Login Alert",
            "body": "The number of failed logins in the last hour has exceeded 1000. Here are the top 10 users and IP addresses:\n\nUsers:\n{{ctx.payload.aggregations.failed_logins_by_user.buckets}}\n\nIP Addresses:\n{{ctx.payload.aggregations.failed_logins_by_source_ip.buckets}}"
          }
        }
      }
    }
  },
  {
    "name": "Suspicious Processes",
    "query": {
      "query": {
        "match": {
          "event_id": "4688"
        }
      },
      "aggs": {
        "suspicious_processes_by_user
```


```

#!/usr/bin/python

import os
import sys
import json
import requests
import splunklib.client as splunk

# Set the Elasticsearch URL
elasticsearch_url = "https://elasticsearch.example.com"

# Set the Elasticsearch username and password
username = "elastic_user"
password = "elastic_password"

# Set the Splunk URL, username, and password
splunk_url = "https://splunk.example.com"
splunk_username = "splunk_user"
splunk_password = "splunk_password"

# Set the search queries
search_queries = [
  {
    "name": "Failed Logins",
    "query": {
      "query": {
        "match": {
          "event_id": "4625"
        }
      },
      "aggs": {
        "failed_logins_by_user": {
          "terms": {
            "field": "user_name.keyword",
            "size": 10
          }
        },
        "failed_logins_by_source_ip": {
          "terms": {
            "field": "source_ip.keyword",
            "size": 10
          }
        }
      }
    },
    "index": "winlogbeat-*",
    "time_range": {
      "range": {
        "@timestamp": {
          "gte": "now-1h"
        }
      }
    },
    "size": 10000,
    "alert": {
      "condition": {
        "compare": {
          "ctx.payload.hits.total": {
            "gte": 1000
          }
        }
      },
      "actions": {
        "send_email": {
          "email": {
            "to": "admin@example.com",
            "subject": "Failed Login Alert",
            "body": "The number of failed logins in the last hour has exceeded 1000. Here are the top 10 users and IP addresses:\n\nUsers:\n{{ctx.payload.aggregations.failed_logins_by_user.buckets}}\n\nIP Addresses:\n{{ctx.payload.aggregations.failed_logins_by_source_ip.buckets}}"
          }
        },
        "send_to_splunk": {
          "webhook": {
            "method": "POST",
            "url": splunk_url + "/services/collector",
            "headers": {
              "Authorization": "Splunk " + splunk.token,
              "Content-Type": "application/json"
            },
            "body": "{{ctx.payload}}"
          }
        }
      }
    }
  },
  {
    "name": "Suspicious Processes",
    "query": {
      "query": {
        "match": {
          "event_id": "4688"
        }
      },
      "aggs": {
        "suspicious_processes_by_user": {
          "terms": {
            "field": "user_

```

