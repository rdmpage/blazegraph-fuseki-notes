# Blazegraph and Fuseki notes

<img src="https://raw.githubusercontent.com/rdmpage/blazegraph-fuseki-notes/master/blazegraph_by_systap_favicon.png" height="100">
<img src="https://raw.githubusercontent.com/rdmpage/blazegraph-fuseki-notes/master/quS6q6Yu.png" height="100">

Notes on working with Blazegraph and Fuseki.

Blazegraph and Fuseki can be run in a Docker container locally on Kitematic, or in [sloppy.io](https://sloppy.io).

## sloppy.io

### Blazegraph

The JSON file below has the settings to run Blazegraph, which is quite resource hungry. This JSON comes from the **Versions** tab on the project page.

```
{
  "id": "app",
  "image": "openkbs/blazegraph",
  "domain": {
    "uri": "kg-blazegraph.sloppy.zone"
  },
  "instances": 0,
  "mem": 6656,
  "port_mappings": [
    {
      "container_port": 9999,
      "protocol": "tcp"
    }
  ],
  "volumes": [
    {
      "container_path": "/usr/blazegraph/config",
      "size": "8GB"
    },
    {
      "container_path": "/data",
      "size": "8GB"
    }
  ],
  "env": {},
  "health_checks": [
    {
      "type": "HTTP",
      "grace_period_seconds": 300,
      "interval_seconds": 60,
      "timeout_seconds": 20,
      "max_consecutive_failures": 3,
      "path": "/",
      "port_index": 0
    }
  ],
  "dependencies": []
}
```

### Fuseki

```
{
  "id": "app",
  "image": "stain/jena-fuseki",
  "domain": {
    "uri": "kg-fuseki.sloppy.zone"
  },
  "instances": 1,
  "mem": 1600,
  "port_mappings": [
    {
      "container_port": 3030,
      "protocol": "tcp"
    }
  ],
  "volumes": [
    {
      "container_path": "/fuseki",
      "size": "8GB"
    }
  ],
  "env": {},
  "health_checks": [],
  "dependencies": []
}
```

## Sparql

### Clean up

```
DELETE WHERE { ?s ?p ?o }
```


