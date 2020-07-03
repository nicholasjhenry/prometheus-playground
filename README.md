# Prometheus Playground

https://prometheus.io/docs/introduction/overview/

## Docs

- Expression Language: https://prometheus.io/docs/prometheus/latest/querying/basics/

## Notes

https://prometheus.io/docs/introduction/first_steps/

- Query metrics with console at `http://localhost:9090/graph`, e.g. `promhttp_metric_handler_requests_total`
- Filter (only success responses) `promhttp_metric_handler_requests_total{code="200"}`
- Aggregation `count(promhttp_metric_handler_requests_total)`
- Graph ( per-second HTTP request rate returning status code 200 ) `rate(promhttp_metric_handler_requests_total{code="200"}[1m])`