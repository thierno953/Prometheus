# Install Prometheus:

**Create a system user for Prometheus using below commands :**

```bash
sudo useradd --no-create-home --shell /bin/false prometheus
```

**Create the directories in which we will be storing our configuration files and libraries :**

```bash
sudo mkdir /etc/prometheus

sudo mkdir /var/lib/prometheus
```

**Set the ownership of the /var/lib/prometheus directory with below command :**

```bash
sudo chown prometheus:prometheus /var/lib/prometheus
```

**We need to inside /tmp :**

```bash
cd /tmp/
```

**Prometheus downloads :**

```bash
wget https://github.com/prometheus/prometheus/releases/download/v2.42.0-rc.0/prometheus-2.42.0-rc.0.linux-amd64.tar.gz
```

**Extract the files using tar :**

```bash
tar -xvf prometheus-2.42.0.linux-amd64.tar.gz
```

**Move the configuration file and set the owner to the prometheus user :**

```bash
cd prometheus-2.42.0.linux-amd64

sudo mv console* /etc/prometheus

sudo mv prometheus.yml /etc/prometheus

sudo chown -R prometheus:prometheus /etc/prometheus
```

**Move the binaries and set the owner :**

```bash
sudo mv prometheus /usr/local/bin/

sudo chown prometheus:prometheus /usr/local/bin/prometheus
```

**Create the service file using below command :**

```bash
sudo nano /etc/systemd/system/prometheus.service
```

**Add**

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

**Reload systemd**

```bash
sudo systemctl daemon-reload
```

**Start Prometheus service**

```bash
sudo systemctl start prometheus

sudo systemctl enable prometheus

sudo systemctl status prometheus
```

**config file**

```bash
sudo nano /etc/prometheus/prometheus.yml
```

**service file**

```bash
sudo nano /etc/systemd/system/prometheus.service
```
