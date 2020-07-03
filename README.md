# Prometheus Playground

https://prometheus.io/docs/introduction/overview/

## Docs

- Expression Language: https://prometheus.io/docs/prometheus/latest/querying/basics/
- Configuration: https://prometheus.io/docs/prometheus/latest/configuration/configuration/

## Notes

https://prometheus.io/docs/introduction/first_steps/

- Query metrics with console at `http://localhost:9090/graph`, e.g. `promhttp_metric_handler_requests_total`
- Filter (only success responses) `promhttp_metric_handler_requests_total{code="200"}`
- Aggregation `count(promhttp_metric_handler_requests_total)`
- Graph ( per-second HTTP request rate returning status code 200 ) `rate(promhttp_metric_handler_requests_total{code="200"}[1m])`

https://prometheus.io/docs/concepts/data_model/

- "streams of timestamped values belonging to the same metric and the same set of labeled dimensions."
- "Every time series is uniquely identified by its metric name and optional key-value pairs called labels."
- metric name, e.g. `http_requests_total`
- "Labels enable Prometheus's dimensional data model"
- labels, e.g. method `POST` to route `/api/tracks`
- "Samples form the actual time series data."
- sample `<metric name>{<label name>=<label value>, ...}`, e.g. `api_http_requests_total{method="POST", handler="/messages"}`
- same notation: http://opentsdb.net

https://prometheus.io/docs/concepts/metric_types/

- metric types: counter, gauge, histogram, summary
- counter: a cumulative metric
- gauge: "represents a single numerical value that can arbitrarily go up and down", e.g. temperatures, current memory usage, number of concurrent requests (count)
- histogram: samples observations, counts in configurable buckets, e.g. request durations, response sizes
    - cumulative counters: `<basename>_bucket{le="<upper inclusive bound>"}`
    - total sum: `<basename>_sum`
    - count: `<basename>_count`
    - `histogram_quantile()`
    - Apdex (Application Performance Index): https://en.wikipedia.org/wiki/Apdex
- summary: a summary samples observations, e.g. request durations, response sizes
    - streaming quantiles: `<basename>{quantile="<φ>"}`
    - total sum: `<basename>_sum`
    - count: `<basename>_count`

https://prometheus.io/docs/concepts/jobs_instances/

- job with multiple instances
- applied as labels: `job`, `instance`

https://prometheus.io/docs/prometheus/latest/getting_started/

- console
    - `prometheus_target_interval_length_seconds`
    - `count(prometheus_target_interval_length_seconds)`
- graph
    - `rate(prometheus_tsdb_head_chunks_created_total[1m])`
- Setup node exporter, try `node_cpu_seconds_total`
- graph `avg by (job, instance, mode) (rate(node_cpu_seconds_total[5m]))`

https://prometheus.io/docs/guides/cadvisor/

- http://localhost:8090 (not the default port of `8080`)
- http://localhost:8090/docker/redis
- http://localhost:9090/graph
  - `container_start_time_seconds`
  - `container_start_time_seconds{name="redis"}`
  - `rate(container_cpu_usage_seconds_total{name="redis"}[1m])`
  - `container_memory_usage_bytes{name="redis"}`
  - `rate(container_network_transmit_bytes_total[1m])`
  - `rate(container_network_receive_bytes_total[1m])`
- docs: https://github.com/google/cadvisor/blob/master/docs/storage/prometheus.md