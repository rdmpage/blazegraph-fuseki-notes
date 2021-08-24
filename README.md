# Triple store notes: Blazegraph, Fuseki, and Oxigraph notes

<img src="https://raw.githubusercontent.com/rdmpage/blazegraph-fuseki-notes/master/blazegraph_by_systap_favicon.png" height="100">
<img src="https://raw.githubusercontent.com/rdmpage/blazegraph-fuseki-notes/master/quS6q6Yu.png" height="100">
<img src="https://raw.githubusercontent.com/rdmpage/blazegraph-fuseki-notes/master/64649343" height="100">

Notes on working with Blazegraph, Fuseki, Oxigraph.

Blazegraph and Fuseki can be run in a Docker container locally on Kitematic, or in [sloppy.io](https://sloppy.io).

## Character encoding

Need to be careful with character encoding in Blazegraph. For example, N-Triples are treated as US-ASCII, which means UTF-8 characters get managed. This follows the spec https://www.w3.org/TR/rdf-testcases/#ntrip_strings but caused me much grief. Mercifully N-Triples is a subset of n3, so setting ```Content-Type: text/rdf+n3``` enables triples to be uploaded with correct encoding. See also https://wiki.blazegraph.com/wiki/index.php/REST_API#RDF_data .

For example, 
```
curl http://localhost:32779/blazegraph/sparql -H 'Content-Type: text/rdf+n3' --data-binary '@$triples_filename'
```


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

## DigitalOcean

If you don’t have docker-machine installed see https://docs.docker.com/machine/install-machine/

### 8 Gb droplet (no longer works)

Get an DigitalOcean access token at https://cloud.digitalocean.com/account/api/tokens

Create a droplet

```
docker-machine create --digitalocean-size "s-4vcpu-8gb" --driver digitalocean --digitalocean-access-token <your-token> <name-for-your-droplet>
```
eval $(docker-machine env name-for-your-droplet)

#### Blazegraph

```
docker run -d -p 9999:9999 openkbs/blazegraph
```

#### Oxigraph

Create a DigitalOcean droplet manually, open console, and then:

```
apt install docker.io

docker run -d --init --rm -v $PWD/data:/data -p 7878:7878 oxigraph/oxigraph -b 0.0.0.0:7878 -f /data
```

## Sparql

### Clean up

```
DELETE WHERE { ?s ?p ?o }
```


