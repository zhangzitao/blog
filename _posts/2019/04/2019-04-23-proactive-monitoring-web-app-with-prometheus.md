---
categories: [tech, Dev]
---

Runing prometheus server, with your config file
```shell
docker run --name prom -p 9090:9090 -v $(pwd)/prom/prometheus.yaml:/etc/prometheus/prometheus.yml -d prom/prometheus
```

Runing grafana server, it will present delicate gragh and many other features
```shell
docker run --name grafana -p 3000:3000 -d grafana/grafana
```
