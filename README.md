# Avalanche

Avalanche serves a text-based [Prometheus metrics](https://prometheus.io/docs/instrumenting/exposition_formats/) endpoint for load testing [Prometheus](https://prometheus.io/) and possibly other [OpenMetrics](https://github.com/OpenObservability/OpenMetrics) consumers.

Avalanche also supports load testing for services accepting data via Prometheus remote_write API such as [Thanos](https://github.com/improbable-eng/thanos), [Cortex](https://github.com/weaveworks/cortex), [M3DB](https://m3db.github.io/m3/integrations/prometheus/), [VictoriaMetrics](https://github.com/VictoriaMetrics/VictoriaMetrics/) and other services [listed here](https://prometheus.io/docs/operating/integrations/#remote-endpoints-and-storage).

Metric names and unique series change over time to simulate series churn.

Checkout the [blog post](https://blog.freshtracks.io/load-testing-prometheus-metric-ingestion-5b878711711c).

> [!NOTE]
> The docker image mentioned in the file is invalid, please use the one
> mentioned below. 

A docker compose file is available as a self contained demonstration in the scripts/example directory.

## Get Avalanche

### Docker

```bash
docker run quay.io/prometheuscommunity/avalanche:main --help
```

### Go Binary

```bash
go install github.com/prometheus-community/avalanche/cmd/avalanche@latest
avalanche --help
```

## Configuration flags

```
$ avalanche --help

usage: avalanche [<flags>]

avalanche - metrics test server

Flags:
  --help                         Show context-sensitive help (also try
                                 --help-long and --help-man).
  --metric-count=500             Number of metrics to serve.
  --label-count=10               Number of labels per-metric.
  --series-count=10              Number of series per-metric.
  --metricname-length=5          Modify length of metric names.
  --labelname-length=5           Modify length of label names.
  --const-label=CONST-LABEL ...  Constant label to add to every metric. Format
                                 is labelName=labelValue. Flag can be specified
                                 multiple times.
  --value-interval=30            Change series values every {interval} seconds.
  --series-interval=60           Change series_id label values every {interval}
                                 seconds.
  --metric-interval=120          Change __name__ label values every {interval}
                                 seconds.
  --port=9001                    Port to serve at
  --remote-url=REMOTE-URL        URL to send samples via remote_write API.
  --remote-pprof-urls=REMOTE-PPROF-URLS ...  
                                 a list of urls to download
                                 pprofs during the remote write:
                                 --remote-pprof-urls=http://127.0.0.1:10902/debug/pprof/heap
                                 --remote-pprof-urls=http://127.0.0.1:10902/debug/pprof/profile
  --remote-pprof-interval=REMOTE-PPROF-INTERVAL  
                                 how often to download pprof profiles.When not
                                 provided it will download a profile once before
                                 the end of the test.
  --remote-batch-size=2000       how many samples to send with each remote_write
                                 API request.
  --remote-requests-count=100    how many requests to send in total to the
                                 remote_write API.
  --remote-write-interval=100ms  delay between each remote write request.
  --remote-tenant="0"            Tenant ID to include in remote_write send
  --tls-client-insecure          Skip certificate check on tls connection
  --remote-tenant-header="X-Scope-OrgID"  
                                 Tenant ID to include in remote_write send.
                                 The default, is the default tenant header
                                 expected by Cortex.
  --version                      Show application version.
```

## Endpoints

Two endpoints are available :
* `/metrics` - metrics endpoint
* `/health` - healthcheck endpoint

