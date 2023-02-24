# Grafana

Download the Grafana GPG key with wget, then pipe the output to apt-key. This will add the key to your APT installation's list of trusted keys, which will allow you to download and verify the GPG-signed Grafana package:

```bash
wget -q -O - https://packages.grafana.com/gpg.key | sudo apt-key add -
```

**Next, add the Grafana repository to your APT sources:**

```bash
sudo add-apt-repository "deb https://packages.grafana.com/oss/deb stable main"
```

**Refresh your APT cache to update your package lists:**

```bash
sudo apt update
```

**You can now proceed with the installation:**

```bash
sudo apt install grafana
```

**Once Grafana is installed, use systemctl to start the Grafana server:**

```bash
sudo systemctl start grafana
```

**Next, verify that Grafana is running by checking the service's status:**

```bash
sudo systemctl status grafana
```

**Now finally enable the Grafana service which will automatically start the Grafana on boot.**

```bash
sudo systemctl enable grafana
```

- To access Grafana Dashboard open favorite browser, type server IP or Name followed by grafana default port **3000**

Here you can see login page of Grafana now you will have to login with below Grafana default **UserName** and **Password**

```bash
Username - admin

password - admin
```

## Configure Prometheus as Grafana DataSource

- Once you logged into Grafana Now first Navigate to Settings **Icon ->> Configuration ->> data sources**.

Now lets click on **Add Data sources** and select **Prometheus**

- Creating Grafana Dashboard to monitor Linux server, come to Grafana Home page and we can see a **+**icon. Click on that and select **import**
- Now provide the Grafana.com Dashboard ID which is **14513** and click on Load.
