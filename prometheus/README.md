# Prometheus Up & Running

[reference boox](https://m.media-amazon.com/images/I/81pKaQrnLPL._SL1500_.jpg)

## What is monitoring

**Alerting**: The primary goal of monitoring is to identify issues
promptly so that a human operator can intervene. This involves setting
up a monitoring system to notify humans when something goes wrong,
enabling them to address the problem immediately.

**Debugging**: After an alert has been triggered, the next step is for
the notified individual(s) to investigate the issue. This investigation
aims to uncover the root cause of the problem and find a resolution.
Debugging is crucial for maintaining system health and preventing future
occurrences of the same issue.

**Trending**: Beyond immediate alerts and debugging, monitoring systems
also provide insights into long-term trends. These trends, which unfold
over days, weeks, or even months, offer valuable information about
system usage and performance changes. Understanding these trends can
inform strategic decisions, such as capacity planning, and help in
optimizing system design and efficiency.

**Plumbing**: Lastly, it's worth noting that monitoring systems
essentially function as data processing pipelines. In some cases, it
might be practical to repurpose parts of the monitoring system for other
tasks instead of developing a new, specialized solution. Although this
use case extends beyond traditional monitoring, it's a common practice
and thus included here for completeness.

## Categories of Monitoring

Monitoring primarily focuses on capturing and analyzing **events**, which encompass a wide range of activities within a system. Examples include HTTP requests/responses, function calls, user actions (like login), disk writes/reads, network data transfers, and memory allocation requests. Each event carries **contextual information** relevant to its occurrence, such as IP addresses, URLs, cookies, user details, response times, status codes, call stacks, and more.

However, collecting comprehensive data on every event, including all contextual details, is impractical due to the sheer volume of data generated. This necessitates strategies to manage and reduce the data volume to a manageable level while retaining its usefulness for debugging and performance analysis. Four main approaches are commonly employed:

1. **Profiling**: Focuses on measuring the performance characteristics of applications, identifying bottlenecks and areas for optimization.
2. **Tracing**: Provides a detailed record of the sequence of events leading to a particular state or outcome, useful for understanding complex interactions within a system.
3. **Logging**: Involves recording significant events and errors occurring within a system, offering insights into its operational history and aiding in troubleshooting.
4. **Metrics**: Involves collecting numerical data representing various aspects of system performance over time, allowing for trend analysis and capacity planning.


### Profiling

**Profiling** is a monitoring approach that acknowledges the impossibility of continuously capturing full context for every event due to practical limitations, such as storage constraints. Instead, it focuses on gathering detailed information for selected periods or specific aspects of system operation.

- **Tools and Techniques**: Various tools exemplify profiling, including `tcpdump` for network traffic analysis and debug builds of binaries that track extensive profiling data, such as function call timings. However, these methods often come with significant drawbacks, like potential disk space exhaustion with `tcpdump` and performance degradation with debug builds, making them unsuitable for constant use in production environments.

- **Advanced Profiling with eBPF**: Enhanced Berkeley Packet Filters (eBPF) in the Linux kernel offer advanced profiling capabilities, providing deep insights into kernel events ranging from filesystem operations to network anomalies. eBPF stands out for its portability, safety, and the unprecedented level of detail it offers, as highlighted by experts like Brendan Gregg.

- **Continuous Profiling**: For longer-term monitoring needs, continuous profiling emerges as a solution. It addresses the challenge of managing large volumes of data by reducing the frequency of profiling, thereby maintaining lower overheads. An example is the eBPF-based Parca Agent, which operates at a reduced frequency (e.g., 19Hz) to collect statistically significant data over minutes rather than seconds. This approach aims to balance the need for insightful data on CPU time usage and application efficiency with the practicalities of data volume and system performance impact.

In essence, profiling serves tactical debugging purposes, offering deep dives into system behavior during critical periods or for specific investigations. Continuous profiling extends this capability over longer durations by optimizing data collection strategies to ensure sustainability and minimal impact on system performance.




### Tracing

**Tracing** is a monitoring technique that selectively captures a subset of events, focusing on those that pass through predefined points of interest within a system. This method provides insights into the execution flow and performance characteristics of applications, particularly regarding latency and resource utilization.

- **Selective Sampling**: Unlike comprehensive event logging, tracing samples a fraction of events (e.g., one in a hundred) to maintain manageable data volumes and minimize performance impacts. By tracking the execution path and timing of sampled events, tracing helps identify where programs spend most of their time and which code paths contribute significantly to latency.

- **Detailed Function Call Analysis**: Some tracing systems go beyond mere snapshots at points of interest, recording detailed timings for every function call within the sampled events. This granular approach enables analysis of how time is spent interacting with various system components, such as databases and caches, offering insights into performance variations based on factors like cache hits and misses.

- **Distributed Tracing**: Extends traditional tracing across distributed systems by assigning unique IDs to requests traversing multiple processes or services. This technique facilitates the reconstruction of end-to-end transaction flows across microservices architectures, aiding in debugging and optimization. Technologies like OpenZipkin and Jaeger exemplify tools designed for distributed tracing.

- **Sampling Strategy**: The effectiveness of tracing relies heavily on its sampling strategy, which balances the need for detailed insights against the constraints of data volume and instrumentation overhead. By carefully selecting which events to trace, organizations can gain valuable operational insights without overwhelming their monitoring infrastructure.

In essence, tracing offers a powerful means to understand complex application behavior and performance bottlenecks, especially in distributed environments, through strategic sampling and detailed analysis of selected events.


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
sudo mv prometheus-2.33.1.linux-amd64 /usr/local/prometheus
```

4. Create a new user for Prometheus using the following command:

```bash
sudo useradd --no-create-home --shell /bin/false prometheus
```

5. Set the ownership of the Prometheus files to the newly created user using the following command:

```bash
sudo chown -R prometheus:prometheus /usr/local/prometheus
```

6. Create libs directory and change the ownership to prometheus

```bash
sudo mkdir /var/lib/prometheus/
sudo chown -R prometheus:prometheus /var/lib/prometheus/
```

7. Create a systemd service file for Prometheus using the following command:

```bash
sudo nano /etc/systemd/system/prometheus.service
```

8. Paste the following configuration into the file:

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


Zabbix and Prometheus are both popular open-source monitoring and
alerting solutions used to monitor the performance and health of IT
infrastructure, applications, and services. They have distinct features
and capabilities that cater to different needs. Here's an overview of
each followed by a comparison:

**Zabbix:**
- Zabbix is a comprehensive monitoring solution that offers network
	monitoring, server monitoring, cloud monitoring, application
	monitoring, and more.
- It uses agents to collect data from monitored hosts and devices.
- Provides a user-friendly web interface for configuration, monitoring,
	and alerting.
- Supports various data collection methods, including SNMP, JMX, IPMI,
	agent-based, and more.
- Features a powerful trigger system to define alert conditions and
	actions.
- Offers visualization through graphs, maps, screens, and dashboards.
- Provides built-in reports and analysis tools.
- Supports automation and integrations with external tools and scripts.
- Suitable for medium to large-scale environments with complex
	monitoring needs.

**Prometheus:**
- Prometheus is a monitoring and alerting toolkit specifically designed
	for cloud-native environments and microservices.
- It uses a pull-based model where exporters are used to expose metrics,
	which are then collected by the Prometheus server.
- Employs a query language called PromQL for data querying and
	manipulation.
- Offers dynamic service discovery and automatic target configuration.
- Provides a basic web UI for visualization and querying.
- Designed for scalability and can handle large numbers of time series
	data.
- Integrates well with container orchestration platforms like
	Kubernetes.
- Supports alerting and notification through built-in alert manager.
- Well-suited for monitoring cloud-native applications and
	microservices.

**Comparison:**
- **Data Collection**: Zabbix uses agents for data collection, while
	Prometheus uses exporters to expose metrics.
- **Data Model**: Prometheus uses a time-series data model, whereas
	Zabbix uses a more flexible item-based approach.
- **Query Language**: Zabbix has a built-in query language, while
	Prometheus uses PromQL.
- **Alerting**: Both offer alerting and notification features, but
	Zabbix provides a more advanced trigger system.
- **Visualization**: Zabbix provides a more feature-rich and
	user-friendly visualization interface, while Prometheus offers a basic
	UI.
- **Scalability**: Prometheus is designed with scalability in mind,
	making it suitable for dynamic and large-scale environments.
- **Suitability**: Zabbix is suitable for traditional infrastructure and
	applications, while Prometheus excels in cloud-native and
	microservices environments.

Ultimately, the choice between Zabbix and Prometheus depends on your
specific monitoring needs, infrastructure, and technical preferences.
Zabbix provides a broader range of features, making it suitable for
diverse environments, while Prometheus is well-tailored for modern
cloud-native applications and microservices architectures.


Prometheus is generally considered to be better suited for monitoring
Kubernetes environments. Here's why:

1. **Native Integration**: Prometheus has native integration with
	 Kubernetes, making it easier to monitor Kubernetes components, pods,
	 nodes, and other resources. Kubernetes provides built-in support for
	 exposing metrics, which Prometheus can collect using its exporters.

2. **Dynamic Discovery**: Prometheus can dynamically discover and scrape
	 metrics from Kubernetes services and pods as they are created or
	 destroyed. This is especially useful in Kubernetes environments where
	 the number of instances may change frequently.

3. **PromQL**: Prometheus Query Language (PromQL) allows you to easily
	 query and aggregate metrics from your Kubernetes cluster, helping you
	 gain insights into performance, resource usage, and overall health.

4. **Alerting and Scaling**: Prometheus comes with a built-in alert
	 manager that can trigger alerts based on defined conditions. It's
	 also designed to handle large-scale monitoring environments, which is
	 important in Kubernetes clusters that can scale dynamically.

5. **Exporter Ecosystem**: The Prometheus ecosystem includes a wide
	 range of exporters and integrations for popular Kubernetes
	 components, such as kube-state-metrics, node-exporter, and more. This
	 allows you to monitor various aspects of your Kubernetes cluster out
	 of the box.

6. **Community and Adoption**: Prometheus has gained significant
	 adoption within the Kubernetes community and is widely used for
	 monitoring Kubernetes deployments. This means you'll find ample
	 resources, documentation, and community support.

While Zabbix can also be used to monitor Kubernetes, Prometheus is a
more natural fit due to its design, features, and native integration
with Kubernetes. However, it's important to evaluate both options based
on your specific monitoring requirements and familiarity with the tools
before making a decision.
