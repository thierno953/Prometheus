# What is Prometheus ?

- Prometheus is a open source Linux Server Monitoring tool mainly used for metrics monitoring, event monitoring, alert management, etc...
- Prometheus has changed the way of monitoring systems and that is why it has become the Top-level project of Cloud Native Computing Foundation(CNCF).
- Prometheus uses a powerful query language i.e ."PromQL".
- In Prometheus tabs are on and handles hundreds of services and microservices.
- Prometheus use multiple modes used for graphing and dashboarding support.

# What is Node Exporter ?

- Node exporter is one of the Prometheus exporters which is used to expose servers or system OS metrics.
- With the help of Node exporter we can expose various resources of the system like RAM, CPU utilization, Memory Utilization, disk space.
- Node exporter runs as a system service which gathers the metrics of your system and that gathered metrics is displayed with the help of Grafana visualization tool.

# What is Grafana ?

- Grafana is a free and open source visualization tool monstly used with Prometheus to which monitor metrics.
- Grafana provides various dashboards, charts, graphs, alerts for the particular data source.
- Grafana allows us to query, visualize, explore metrics and set alerts for the data source which can be system, server, nodes, cluster, etc...
- We can also create our own dynamic dashboard for visualization and monitoring.
- We can save the dashboard and can even share with our team members which is one of the main advantage of Grafana.

# Rules in Prometheus

- Prometheus supports two types of rules which may be configured and then evaluated at regular intervals: recording rules and alerting rules.
- To include rules in Prometheus, create a file containing the necessary rule statements and have Prometheus load the file via the rule_files.

**Prerequisites**

- Ubuntu with 22.04 Version
- Root user account with sudo privilege.
- Prometheus system user and group.
- Sufficient storage on your system and good internet connectivity.
- Ports Required **9090** (Prometheus), **3000** (Grafana), **9100** (Node Exporter).

**Other resources**

[More prometheus](https://prometheus.io/download/#alertmanager)
