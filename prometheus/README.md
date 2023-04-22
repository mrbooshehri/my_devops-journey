To install Prometheus on Ubuntu, you can follow these steps:

1. Download the latest version of Prometheus from the official website using the following command:

```bash
 wget https://github.com/prometheus/prometheus/releases/download/v2.33.1/prometheus-2.33.1.linux-amd64.tar.gz
```

2. Extract the downloaded file using the following command:

```bash
tar xvfz prometheus-2.33.1.linux-amd64.tar.gz
```

3. Move the extracted directory to /usr/local using the following command:

```bash
$ sudo mv prometheus-2.33.1.linux-amd64 /usr/local/prometheus
```

4. Create a new user for Prometheus using the following command:

```bash
sudo useradd --no-create-home --shell /bin/false prometheus
```

5. Set the ownership of the Prometheus files to the newly created user using the following command:

```bash
sudo chown -R prometheus:prometheus /usr/local/prometheus
```

6. Create a systemd service file for Prometheus using the following command:

```bash
sudo nano /etc/systemd/system/prometheus.service
```

7. Paste the following configuration into the file:

```
[Unit]
Description=Prometheus
Wants=network-online.target
After=network-online.target

[Service]
User=prometheus
Group=prometheus
Type=simple
ExecStart=/usr/local/prometheus/prometheus --config.file=/usr/local/prometheus/prometheus.yml --storage.tsdb.path=/var/lib/prometheus/
Restart=always

[Install]
WantedBy=multi-user.target
```

Save and close the file.

8. Reload the systemd daemon using the following command:

```bash
sudo systemctl daemon-reload
```

9. Start the Prometheus service using the following command:

```bash
sudo systemctl start prometheus
```

10. Verify that Prometheus is running using the following command:

```bash
sudo systemctl status prometheus
```

11. Enable Prometheus to start at boot time using the following command:

```bash
sudo systemctl enable prometheus
```

That's it! Prometheus should now be installed and running on your Ubuntu system. You can access the Prometheus web interface by opening a web browser and navigating to http://localhost:9090.
