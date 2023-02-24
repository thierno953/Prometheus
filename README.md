# Prometheus - Node_Exporter - Rules - Alertmanager - Grafana

## Prometheus 

- Prometheus is a open source Linux Server Monitoring tool mainly used for metrics monitoring, event monitoring, alert management, etc...
- Prometheus has changed the way of monitoring systems and that is why it has become the Top-level project of Cloud Native Computing Foundation(CNCF).
- Prometheus uses a powerful query language i.e ."PromQL".
- In Prometheus tabs are on and handles hundreds of services and microservices.
- Prometheus use multiple modes used for graphing and dashboarding support.

## Node Exporter

- Node exporter is one of the Prometheus exporters which is used to expose servers or system OS metrics.
- With the help of Node exporter we can expose various resources of the system like RAM, CPU utilization, Memory Utilization, disk space.
- Node exporter runs as a system service which gathers the metrics of your system and that gathered metrics is displayed with the help of Grafana visualization tool.

## Grafana 

- Grafana is a free and open source visualization tool monstly used with Prometheus to which monitor metrics.
- Grafana provides various dashboards, charts, graphs, alerts for the particular data source.
- Grafana allows us to query, visualize, explore metrics and set alerts for the data source which can be system, server, nodes, cluster, etc...
- We can also create our own dynamic dashboard for visualization and monitoring.
- We can save the dashboard and can even share with our team members which is one of the main advantage of Grafana.

## Rules in Prometheus 

- Prometheus supports two types of rules which may be configured and then evaluated at regular intervals: recording rules and alerting rules.
- To include rules in Prometheus, create a file containing the necessary rule statements and have Prometheus load the file via the rule_files.

**Prerequisites**

- Ubuntu with 22.04 Version
- Root user account with sudo privilege.
- Prometheus system user and group.
- Sufficient storage on your system and good internet connectivity.
- Ports Required **9090** (Prometheus), **3000** (Grafana), **9100** (Node Exporter).

```shell                                                                                           
# my global config
global:
  scrape_interval: 15s # Set the scrape interval to every 15 seconds. Default is every 1 minute.
  evaluation_interval: 15s # Evaluate rules every 15 seconds. The default is every 1 minute.
  # scrape_timeout is set to the global default (10s).

# Alertmanager configuration
alerting:
  alertmanagers:
    - static_configs:
        - targets: 
            - 192.168.X.X:9093

# Load rules once and periodically evaluate them according to the global 'evaluation_interval'.
rule_files:
  - "rules/myrules.yml"

# A scrape configuration containing exactly one endpoint to scrape:
# Here it's Prometheus itself.
scrape_configs:
  # The job name is added as a label `job=<job_name>` to any timeseries scraped from this config.
  - job_name: "prometheus"

    # metrics_path defaults to '/metrics'
    # scheme defaults to 'http'.

    static_configs:
      - targets: ["localhost:9090"]
  - job_name: 'Node_Exporter'
    scrape_interval: 5s
    static_configs:
      - targets: ['192.168.X.X:9100']
```

**Other resources**

[More prometheus](https://prometheus.io/download/#alertmanager)
