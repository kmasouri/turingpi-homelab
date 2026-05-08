<!-- markdownlint-disable MD024 -->

# 🐋 turingpi-homelab

Docker Compose and Docker Swarm configuration for running homelab services on a Turing Pi cluster.

## Available Services

| Service                                                      | Purpose                                                   | Runtime | Compose file                  | Access                                              | Service doc                             |
| ------------------------------------------------------------ | --------------------------------------------------------- | ------- | ----------------------------- | --------------------------------------------------- | --------------------------------------- |
| [Portainer](https://www.portainer.io)                        | Docker management UI                                      | Swarm   | `portainer-compose.yaml`      | `https://<node-ip>:9443` or `http://<node-ip>:9000` | [Docs](docs/services/portainer.md)      |
| Monitoring                                                   | Prometheus, Grafana, Node Exporter, and Blackbox Exporter | Swarm   | `monitoring-compose.yaml`     | Grafana: `http://<node-ip>:3000`                    | [Docs](docs/services/monitoring.md)     |
| [Homelab Portal](https://github.com/kmasouri/homelab-portal) | Static app launcher and bookmark portal                   | Swarm   | `homelab-portal-compose.yaml` | `http://<node-ip>/`                                 | [Docs](docs/services/homelab-portal.md) |
| [Postgres](https://www.postgresql.org)                       | PostgreSQL database                                       | Swarm   | `postgres-compose.yaml`       | `<node-ip>:5432`                                    | [Docs](docs/services/postgres.md)       |
| [Pi-hole](https://pi-hole.net)                               | Network-wide DNS ad blocking                              | Compose | `pihole-compose.yaml`         | `http://<node-ip>:8080/admin`                       | [Docs](docs/services/pihole.md)         |
| [Home Assistant](https://www.home-assistant.io)              | Home automation platform                                  | Compose | `homeassistant-compose.yaml`  | `http://<node-ip>:8123`                             | [Docs](docs/services/home-assistant.md) |

## Docs

- [Service Docs](docs/services/) — service information, deployment, and notes for each service
