# Node Exporter

**Step 1 :** Got to the official release page of prometheus Node Exporter and copy the link of the latest version of the Node Exporter package according to your OS type.

```bash
cd /tmp
```

```bash
wget https://github.com/prometheus/node_exporter/releases/download/v1.5.0/node_exporter-1.5.0.linux-amd64.tar.gz
```

**Step 2 :** Extract the file

```bash
tar xvfz node_exporter-*.*-amd64.tar.gz
```

**Step 3** Move the binary file node_exporter to /usr/local/bin location.

```bash
sudo mv node_exporter-*.*-amd64/node_exporter /usr/local/bin/
```

**Create a node node_exporter user to run the node exporter service**

```bash
sudo useradd -rs /bin/false node_exporter
```

**Create a Custom Node Exporter Service**

- **Step 4 :** Create a node_exporter service file in the /etc/systemd/system directory.

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

**Step 5 :** Register the node exporter as a service in systemctl by reloading the system daemon and start the node exporter service.

```bash
sudo systemctl daemon-reload
sudo systemctl start node_exporter
```

**Step 6 :** Check the status of node exporter service to make sure it is active, and service is running.

```bash
sudo systemctl status node_exporter
```

**Step 7 :** Now, after starting the service, node exporter is already to export metrics on port 9000.

```bash
curl http://<server-IP>:9100/metrics
```

**Step 8 :** SSH into the Prometheus machine and open the prometheus.yml file which is at location /etc/Prometheus.

```bash
sudo nano /etc/Prometheus/Prometheus.yml
```

```bash
 - job_name: 'Node_Exporter'
     scrape_interval: 5s
     static_configs:
      - targets: ['<Server_IP_of_Node_Exporter_Machine>:9100']
```

**Step 9 :** Restart

```bash
sudo systemctl restart prometheus.service
```
