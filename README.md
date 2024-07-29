## Grafana-Prometheus-And-Node-Exporter
Create Simple Dashboard With Grafana Prometheus And Node Exporter
Monitoring Systems with Node Exporter, Prometheus, and Grafana
Node Exporter, Prometheus, and Grafana are widely used together to monitor
system-level application insights. These tools enable developers to analyze real-time
system metrics effectively. Prometheus Node Exporter, in particular, is used to collect
node metrics and provide comprehensive system-wide insights.
In this article, we'll explore Node Exporter, its installation prerequisites, and how to
configure it for system monitoring.
What is Prometheus Node Exporter?
Prometheus Node Exporter collects statistics from applications in their native format
(such as XML), converts these statistics into a format that Prometheus can
understand, and exposes them on a URL suitable for Prometheus. A vast library of
applications can export metrics from third-party sources and transform them into
Prometheus-compatible metrics, which can be found here.
This information is crucial for monitoring the performance of nodes or servers. Node
Exporter needs to be installed on all servers or virtual machines to gather data across
all nodes. It exposes metrics at the /metrics endpoint on port 9100.
## What is Grafana?
Grafana is an open-source web application for multi-platform analytics and
interactive visualization. It can generate charts, graphs, and alerts for the web when
connected to supported data sources. A Grafana dashboard consists of one or more
panels, organized into rows, providing a comprehensive view of related information.
These panels are created using components that query and transform raw data from
a data source into visualizations like charts and graphs.
You can do all these things through Docker Compose, whose address is given below
But I have put the items below
To install Prometheus, you can install it both through Docker and through the
command in the system. If you want to install it through the command in the system,
you can install it in this way. And if you want to install it through Docker, you can do
it this way
```
docker run \
-p 9090:9090 \
-v /path/to/prometheus.yml:/etc/prometheus/prometheus.yml \
prom/prometheus
```
If you want to install prometheus through the command
```
wget https://github.com/prometheus/prometheus/releases/download/v2.31.1/prometheus-2.31.1.linux-
amd64.tar.gz
tar xvfz prometheus-2.31.1.linux-amd64.tar.gz
sudo mv prometheus-2.31.1.linux-amd64 /etc/prometheus
sudo mv /etc/prometheus/prometheus /usr/local/bin/
sudo mv /etc/prometheus/promtool /usr/local/bin/
sudo mkdir /etc/prometheus/config
sudo mkdir /var/lib/prometheus
sudo mv /etc/prometheus/prometheus.yml /etc/prometheus/config/
sudo mv /etc/prometheus/consoles /etc/prometheus/config/
sudo mv /etc/prometheus/console_libraries /etc/prometheus/config/
sudo chown -R prometheus:prometheus /etc/prometheus
sudo chown -R prometheus:prometheus /var/lib/prometheus
sudo nano /etc/systemd/system/prometheus.service
[Unit]
Description=Prometheus
Wants=network-online.target
After=network-online.target
[Service]
User=prometheus
Group=prometheus
Type=simple
ExecStart=/usr/local/bin/prometheus \
--config.file=/etc/prometheus/config/prometheus.yml \
--storage.tsdb.path=/var/lib/prometheus/ \
--web.console.templates=/etc/prometheus/config/consoles \
--web.console.libraries=/etc/prometheus/config/console_libraries
[Install]
WantedBy=multi-user.target
sudo systemctl daemon-reload
sudo systemctl start prometheus
sudo systemctl enable prometheus
```
you can see prometheus to this address http://your-ip:9090/metrics
And if you want to install NodeExport, it can be done in two ways
via Docker
```
sudo docker run -d -p 9100:9100 --net="host" --pid="host" -v "/:/host:ro,rslave" quay.io/prometheus/node-
exporter --path.rootfs=/host
through the command
sudo useradd --no-create-home --shell /bin/false node_exporter
wget https://github.com/prometheus/node_exporter/releases/download/v1.3.1/node_exporter-1.3.1.linux-
amd64.tar.gz
tar xvfz node_exporter-1.3.1.linux-amd64.tar.gz
sudo mv node_exporter-1.3.1.linux-amd64/node_exporter /usr/local/bin/
sudo chown node_exporter:node_exporter /usr/local/bin/node_exporter
sudo nano /etc/systemd/system/node_exporter.service
[Unit]
Description=Node Exporter
Wants=network-online.target
After=network-online.target
[Service]
User=node_exporter
Group=node_exporter
Type=simple
ExecStart=/usr/local/bin/node_exporter
[Install]
WantedBy=multi-user.target
sudo systemctl daemon-reload
sudo systemctl start node_exporter
sudo systemctl enable node_exporter
sudo ufw allow 9100/tcp
```
And you can see metrics node exporter on http://your-address:9100/metrics
and you can install grafana with command
```
docker run -d -p 3000:3000 --name=grafana grafana/grafana-enterprise
```
You can connect to Grafana through the following address
```
http://your-address:3000
```
After logging in Grafana, you need to add Prometheus to Grafana
This is possible through connection
After establishing the necessary connections with Grafana, now it is necessary to
create a dashboard. To create a dashboard, it can be created in three ways.
After establishing the necessary connections with Grafana, now it is necessary to
create a dashboard. To create a dashboard, it can be created in three ways.
1- Make the dashboard yourself
2- By importing the Grapha dashboard ID taken from the site Dashboard Grafana
3- Through the Json file
You can import 1860 ID or view your dashboard through the Json file placed in the
github
and you can see your dashboard
Conclusion:
System monitoring using Node Exporter, Prometheus and Grafana is an effective way
to analyze and monitor the performance of high-level systems. These tools allow
developers to view and analyze their system metrics in real-time and with high
accuracy.
Considering the comprehensiveness and high efficiency of this set of tools, it can be
said that using Node Exporter, Prometheus and Grafana is an excellent choice for
monitoring systems and improving their performance and stability. This method not
only provides detailed information about the performance of the systems, but also
reduces the time of diagnosing and fixing problems.
