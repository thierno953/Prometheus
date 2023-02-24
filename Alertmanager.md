# Alertmanager

**Create the alertmanager system user**

```bash
sudo useradd --no-create-home --shell /bin/false prometheus
```

**Create the /etc/alertmanager directory**

```bash
sudo mkdir /etc/alertmanager
```

**Downloads Alertmanager from the Prometheus downloads page :**

```bash
cd /tmp/

wget https://github.com/prometheus/alertmanager/releases/download/v0.25.0/alertmanager-0.25.0.linux-amd64.tar.gz
```

**Extract the files:**

```bash
tar -xvf alertmanager-0.25.0.linux-amd64.tar.gz
```

**Move the binaries :**

```bash
cd alertmanager-0.25.0.linux-amd64
sudo mv alertmanager /usr/local/bin/
sudo mv amtool /usr/local/bin/
```

**Set the ownership of the binaries :**

```bash
sudo chown alertmanager:alertmanager /usr/local/bin/alertmanager
sudo chown alertmanager:alertmanager /usr/local/bin/amtool
```

**Move the configuration file into the /etc/alertmanager directory :**

```bash
sudo mv alertmanager.yml /etc/alertmanager/
```

**Set the ownership of the /etc/alertmanager directory :**

```bash
sudo chown -R alertmanager:alertmanager /etc/alertmanager/
```

**Create the alertmanager.service file for systemd :**

```bash
sudo nano /etc/systemd/system/alertmanager.service
```

**Copy and Paste content :**

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

**Reload systemd, and then start the alertmanager services :**

```bash
sudo systemctl daemon-reload
sudo systemctl start alertmanager
sudo systemctl enable alertmanager
sudo systemctl status alertmanager
```

**Now update the Prometheus configuration file to use alertmanager :**

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

**Reload systemd, and then restart the prometheus services :**

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
