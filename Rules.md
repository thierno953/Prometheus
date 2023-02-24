# Rules in Prometheus

## Recording rules

```bash
groups:
  - name: my-rules
    rules:
    - record: job:node_memory_MemFree_bytes_percent
      expr: 100 - (100 * node_memory_MemFree_bytes / node_memory_MemTotal_bytes)
    - record: job:node_cpu
      expr: avg without(cpu)(rate(node_cpu_seconds_total{mode="idle"}[5m]))
```

**Checking rules**

```shell
./promtool check rules rules/myrules.yml
```

## Alerting rules

```bash
groups:
  - name: my-rules
    rules:
    - record: job:node_memory_MemFree_bytes_percent
      expr: 100 - (100 * node_memory_MemFree_bytes / node_memory_MemTotal_bytes)

    - alert: NodeExporterDown
      expr: up{job="Node_Exporter"} == 0
      for: 1m
```

**Checking rules**

```shell
./promtool check rules rules/myrules.yml
```
