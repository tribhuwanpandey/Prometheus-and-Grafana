# Prometheus and Grafana: 

##  What Is Prometheus?
**Prometheus** is an open-source monitoring and alerting toolkit built for reliability and scalability. It collects metrics, stores them as time-series data, and allows you to query and alert on them.

### Key Concepts
- **Metrics-based monitoring**: Collect numerical metrics like CPU usage, memory consumption, etc.
- **Pull model**: Prometheus scrapes metrics via HTTP endpoints.
- **Time-series database**: Stores data with timestamps for historical analysis.
- **PromQL**: Query language for metrics.
- **Alertmanager**: Handles alerts and routes notifications.

---

##  What Is Grafana?
**Grafana** is a visualization and analytics platform for time-series data. It connects to Prometheus and other data sources to create interactive dashboards.

### Key Concepts
- Supports multiple data sources.
- Builds dashboards with graphs, gauges, and tables.
- Enables alerting and team collaboration.

---

## Use Cases
| Use Case | Description |
|-----------|-------------|
| Infrastructure Monitoring | Monitor servers, containers, and system metrics. |
| Application Monitoring | Track latency, error rates, throughput. |
| Kubernetes Monitoring | Observe cluster health, pods, and nodes. |
| Business KPIs | Monitor custom business metrics. |

---

## Architecture Overview

```
                   ┌────────────────────────┐
                   │      Applications       │
                   │ expose /metrics endpoint│
                   └─────────────┬───────────┘
                                 │
                                 ▼
                     ┌──────────────────────┐
                     │     Prometheus       │
                     │  (Scrapes metrics)   │
                     └────────────┬─────────┘
                                  │
                                  ▼
                     ┌──────────────────────┐
                     │      Grafana         │
                     │ (Visualize metrics)  │
                     └────────────┬─────────┘
                                  │
                                  ▼
                     ┌──────────────────────┐
                     │    Alertmanager      │
                     │ (Send notifications) │
                     └──────────────────────┘
```

### Data Flow
1. Apps expose metrics at `/metrics`.
2. Prometheus scrapes and stores them.
3. Grafana queries Prometheus.
4. Alerts are sent via Alertmanager.

---

##  Benefits
| Benefit | Explanation |
|----------|-------------|
| Scalability | Handles large metric volumes. |
| Real-time Insight | Frequent metric scraping. |
| Custom Alerts | Define conditions for notifications. |
| Integrations | Works with Docker, Kubernetes, etc. |
| Visualization Power | Grafana provides interactive dashboards. |

---

##  Example Setup (Docker Compose)

```yaml
version: '3'
services:
  prometheus:
    image: prom/prometheus
    container_name: prometheus
    volumes:
      - ./prometheus.yml:/etc/prometheus/prometheus.yml
    ports:
      - "9090:9090"

  grafana:
    image: grafana/grafana
    container_name: grafana
    ports:
      - "3000:3000"
    environment:
      - GF_SECURITY_ADMIN_USER=admin
      - GF_SECURITY_ADMIN_PASSWORD=admin
```

### Example `prometheus.yml`
```yaml
global:
  scrape_interval: 5s

scrape_configs:
  - job_name: 'node_exporter'
    static_configs:
      - targets: ['node_exporter:9100']
```

---

##  Conclusion
| Tool | Purpose | Port |
|-------|----------|------|
| Prometheus | Collect and store metrics | 9090 |
| Grafana | Visualize metrics | 3000 |
| Alertmanager | Manage alerts | 9093 |

> Prometheus + Grafana form a robust observability stack for system monitoring, performance analysis, and alerting.
