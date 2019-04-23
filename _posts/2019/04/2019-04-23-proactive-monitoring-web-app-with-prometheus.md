---
categories: [tech, Dev]
---

### Prometheus server

Runing prometheus server, with your config file
```shell
docker run --name prom -p 9090:9090 -v $(pwd)/prom/prometheus.yaml:/etc/prometheus/prometheus.yml -d prom/prometheus
```

The simple prometheus.yaml
```yaml
scrape_configs:
  - job_name: 'workshop_app'
    metrics_path: '/metrics'
    scrape_interval: 5s
    static_configs:
      - targets: ['10.0.2.2:8080']
```

### Grafana

Runing grafana server, it will present delicate gragh and many other features
```shell
docker run --name grafana -p 3000:3000 -d grafana/grafana
```
