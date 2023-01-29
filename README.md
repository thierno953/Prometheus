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

### Prerequisites

- Ubuntu with 22.04 Version
- Root user account with sudo privilege.
- Prometheus system user and group.
- Sufficient storage on your system and good internet connectivity.
- Ports Required 9090 (Prometheus), 3000 (Grafana), 9100 (Node Exporter).

# Install Prometheus:

Create a system user for Prometheus using below commands:

```bash
sudo useradd --no-create-home --shell /bin/false prometheus
```

Create the directories in which we will be storing our configuration files and libraries:

```bash
sudo mkdir /etc/prometheus

sudo mkdir /var/lib/prometheus
```

- Set the ownership of the /var/lib/prometheus directory with below command:

```bash
sudo chown prometheus:prometheus /var/lib/prometheus
```

- We need to inside /tmp:

```bash
cd /tmp/
```

Prometheus downloads:

```bash
wget https://github.com/prometheus/prometheus/releases/download/v2.42.0-rc.0/prometheus-2.42.0-rc.0.linux-amd64.tar.gz
```

Extract the files using tar:

```bash
tar -xvf prometheus-2.41.0.linux-amd64.tar.gz
```

Move the configuration file and set the owner to the prometheus user:

```bash
cd prometheus-2.41.0.linux-amd64

sudo mv console* /etc/prometheus

sudo mv prometheus.yml /etc/prometheus

sudo chown -R prometheus:prometheus /etc/prometheus
```

Move the binaries and set the owner:

```bash
sudo mv prometheus /usr/local/bin/

sudo chown prometheus:prometheus /usr/local/bin/prometheus
```

- Create the service file using below command:

```bash
sudo nano /etc/systemd/system/prometheus.service
```

Add:

```bash
[Unit]
Description=Prometheus
Wants=network-online.target
After=network-online.target

[Service]
User=prometheus
Group=prometheus
Type=simple
ExecStart=/usr/local/bin/prometheus \
    --config.file /etc/prometheus/prometheus.yml \
    --storage.tsdb.path /var/lib/prometheus/ \
    --web.console.templates=/etc/prometheus/consoles \
    --web.console.libraries=/etc/prometheus/console_libraries

[Install]
WantedBy=multi-user.target
```

Reload systemd:

```bash
sudo systemctl daemon-reload
```

Start Prometheus service:

```bash
sudo systemctl start prometheus
sudo systemctl enable prometheus
sudo systemctl status prometheus
```

config file:

```bash
sudo nano /etc/prometheus/prometheus.yml
```

service file:

```bash
sudo nano /etc/systemd/system/prometheus.service
```

# Node Exporter

- Step 1 : Got to the official release page of prometheus Node Exporter and copy the link of the latest version of the Node Exporter package according to your OS type.

```bash
cd /tmp
```

```bash
wget https://github.com/prometheus/node_exporter/releases/download/v1.5.0/node_exporter-1.5.0.linux-amd64.tar.gz
```

- Step 2: Extract the file

```bash
tar xvfz node_exporter-*.*-amd64.tar.gz
```

- Step 3: Move the binary file node_exporter to /usr/local/bin location.

```bash
sudo mv node_exporter-*.*-amd64/node_exporter /usr/local/bin/
```

Create a node node_exporter user to run the node exporter service.

```bash
sudo useradd -rs /bin/false node_exporter
```

Create a Custom Node Exporter Service

- Step 4: Create a node_exporter service file in the /etc/systemd/system directory.

```bash
sudo nano /etc/systemd/system/node_exporter.service
```

```bash
[Unit]
Description=Node Exporter
After=network.target
[Service]
User=node_exporter
Group=node_exporter
Type=simple
ExecStart=/usr/local/bin/node_exporter
[Install]
WantedBy=multi-user.target
```

- Step 5: Register the node exporter as a service in systemctl by reloading the system daemon and start the node exporter service.

```bash
sudo systemctl daemon-reload
sudo systemctl start node_exporter
```

- Step 6: Check the status of node exporter service to make sure it is active, and service is running.

```bash
sudo systemctl status node_exporter
```

- Step 7: Now, after starting the service, node exporter is already to export metrics on port 9000.

```bash
curl http://<server-IP>:9100/metrics
```

- Step 8: SSH into the Prometheus machine and open the prometheus.yml file which is at location /etc/Prometheus.

```bash
sudo nano /etc/Prometheus/Prometheus.yml
```

```bash
 - job_name: 'Node_Exporter'
    scrape_interval: 5s
    static_configs:
      - targets: ['<Server_IP_of_Node_Exporter_Machine>:9100']
```

- Step 9: Restart

```bash
sudo systemctl restart prometheus.service
```

# Grafana

- Download the Grafana GPG key with wget, then pipe the output to apt-key. This will add the key to your APT installation's list of trusted keys, which will allow you to download and verify the GPG-signed Grafana package:

```bash
wget -q -O - https://packages.grafana.com/gpg.key | sudo apt-key add -
```

Next, add the Grafana repository to your APT sources:

```bash
sudo add-apt-repository "deb https://packages.grafana.com/oss/deb stable main"
```

Refresh your APT cache to update your package lists:

```bash
sudo apt update
```

You can now proceed with the installation:

```bash
sudo apt install grafana
```

Once Grafana is installed, use systemctl to start the Grafana server:

```bash
sudo systemctl start grafana
```

Next, verify that Grafana is running by checking the service's status:

```bash
sudo systemctl status grafana
sudo systemctl enable grafana
```

- Creating Grafana Dashboard to monitor Linux server, come to Grafana Home page and we can see a "+"icon. Click on that and select "import"
- Now provide the Grafana.com Dashboard ID which is 14513 and click on Load.

# Alertmanager

- Create the alertmanager system user:

```bash
sudo useradd --no-create-home --shell /bin/false prometheus
```

- Create the /etc/alertmanager directory

```bash
sudo mkdir /etc/alertmanager
```

- Downloads Alertmanager from the Prometheus downloads page:

```bash
cd /tmp/

wget https://github.com/prometheus/alertmanager/releases/download/v0.25.0/alertmanager-0.25.0.linux-amd64.tar.gz
```

- Extract the files:

```bash
tar -xvf alertmanager-0.25.0.linux-amd64.tar.gz
```

- Move the binaries:

```bash
cd alertmanager-0.25.0.linux-amd64
sudo mv alertmanager /usr/local/bin/
sudo mv amtool /usr/local/bin/
```

- Set the ownership of the binaries:

```bash
sudo chown alertmanager:alertmanager /usr/local/bin/alertmanager
sudo chown alertmanager:alertmanager /usr/local/bin/amtool
```

- Move the configuration file into the /etc/alertmanager directory:

```bash
sudo mv alertmanager.yml /etc/alertmanager/
```

- Set the ownership of the /etc/alertmanager directory:

```bash
sudo chown -R alertmanager:alertmanager /etc/alertmanager/
```

- Create the alertmanager.service file for systemd:

```bash
sudo nano /etc/systemd/system/alertmanager.service
```

- Copy and Paste content:

```bash
[Unit]
Description=Alertmanager
Wants=network-online.target
After=network-online.target

[Service]
User=alertmanager
Group=alertmanager
Type=simple
WorkingDirectory=/etc/alertmanager/
ExecStart=/usr/local/bin/alertmanager \
    --config.file=/etc/alertmanager/alertmanager.yml
[Install]
WantedBy=multi-user.target
```

- Reload systemd, and then start the alertmanager services:

```bash
sudo systemctl daemon-reload
sudo systemctl start alertmanager
sudo systemctl enable alertmanager
sudo systemctl status alertmanager
```

- Now update the Prometheus configuration file to use alertmanager:

```bash
sudo nano /etc/prometheus/prometheus.yml
```

```bash
alert:
  alertmanagers:
  - static_configs:
    - targets:
      - localhost:9093
```

- Reload systemd, and then restart the prometheus services:

```bash
sudo systemctl daemon-reload
sudo systemctl restart prometheus
```

```bash
route:
 receiver: admin

receivers:
- name: admin
  email_configs:
  - to: "show@gmail.com"
    from: "test@gmail.com"
    smarthost: smtp.gmail.com:587
    auth_username: "test@gmail.com"
    auth_identitfy: "test@gmail.com"
    auth_password: "mypassword"
```
