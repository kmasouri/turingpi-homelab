# Monitoring

## About

The monitoring stack provides Prometheus for metrics collection, Grafana for dashboards, Node Exporter for host metrics, and Blackbox Exporter for endpoint probing.

## Deployment

| Field | Value |
| --- | --- |
| Runtime | Swarm |
| Compose file | `monitoring-compose.yaml` |
| Stack name | `monitoring` |
| Deploy | `sudo docker stack deploy -c monitoring-compose.yaml monitoring` |
| Update | `sudo docker stack deploy -c monitoring-compose.yaml monitoring` |
| Stop | `sudo docker stack rm monitoring` |
| Access | Prometheus: `http://<node-ip>:9090`; Grafana: `http://<node-ip>:3000`; Blackbox Exporter: `http://<node-ip>:9115`; Node Exporter: `http://<node-ip>:9100/metrics` |
| Persistent data | Docker volumes `prometheus_volume` and `grafana_volume`, created as `monitoring_prometheus_volume` and `monitoring_grafana_volume` |

## Notes

- Includes Prometheus, Grafana, Node Exporter, and Blackbox Exporter.
- Node Exporter runs globally, with one instance per node.
- Prometheus uses explicit static targets in `prometheus.yaml`.
- You can switch back to Swarm DNS discovery, such as `tasks.monitoring_prometheus-node-exporter`, if you prefer dynamic service-based discovery.
- If image tags already exist locally and you want to force refresh the running tasks, use `sudo docker service update --force <service-name>`.
