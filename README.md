# whoknows_monitor: Architecture Overview and Service Descriptions

The `whoknows_monitor` system is a distributed monitoring solution designed for resilience, granularity, and extensibility. By separating the monitoring components onto a dedicated VM, this architecture ensures uninterrupted observability of the main application and infrastructure. Metrics and logs are collected from the main system using exporters and processed on the monitoring system, providing actionable insights and timely alerts.

## Architecture Description

### Main System Components
 
- `node_exporter`
  
  Collects metrics about the host system (CPU, memory, disk, and network usage). This provides insights into the health and performance of the application VM.

- `cAdvisor`

  Monitors containerized applications, collecting information about CPU, memory, network, and filesystem usage for each running container. This is essential for identifying performance issues in containerized workloads.

- `nginx_exporter`

  Gathers metrics from the Nginx server, such as request counts, response statuses, and processing times. This helps analyze traffic patterns and server health.

- `promtail`
  
  A log collector for Loki. It scrapes logs from applications and systems running on the application VM and ships them to Loki for centralized storage and querying.

- `postgres_exporter`
  
  Monitors PostgreSQL database performance by collecting metrics such as query execution times, connection counts, and table statistics. This ensures the database is functioning optimally.

### Monitoring System Components
- `Prometheus`
  
  A time-series database and monitoring system. It collects, stores, and queries metrics from exporters in the main system. Prometheus is the backbone of metric collection and alerting.
  
- `Loki`
  
  A log aggregation system. It collects logs from Promtail, allowing centralized querying and visualization of logs alongside metrics for a comprehensive view of system performance.
  
- `Grafana`
  
  A visualization tool that integrates with Prometheus and Loki to display metrics and logs on customizable dashboards. Grafana provides an intuitive interface for monitoring system health and identifying anomalies.
  
- `Alertmanager`

  Handles alerts sent by Prometheus. It deduplicates, groups, and routes notifications (e.g., email, Slack) based on user-defined rules to ensure timely response to critical issues.
  
- `Blackbox Exporter`
  
  Monitors external endpoints (e.g., HTTP, TCP, DNS) by performing synthetic checks like pinging or sending HTTP requests. It ensures that services are available and accessible from an external perspective.
  

## Key Benefits of this Architecture
- **Resilience**
  
  The monitoring system operates independently of the main application VM, ensuring that metrics and logs can still be accessed even if the application system is down.

- **Granularity**
  
  By using dedicated exporters and monitoring systems, granular data collection and detailed insights into system performance are achieved without overloading the main application.

- **Extensibility**
  
  New monitoring agents, such as Blackbox Exporter or additional log sources, can be integrated into the monitoring system without affecting the main application VM.
