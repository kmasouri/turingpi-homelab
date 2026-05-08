# Postgres

## About

Postgres provides a PostgreSQL database service for homelab applications that need relational storage.

## Deployment

| Field | Value |
| --- | --- |
| Runtime | Swarm |
| Compose file | `postgres-compose.yaml` |
| Stack name | `postgres` |
| Deploy | `sudo docker stack deploy -c postgres-compose.yaml postgres` |
| Update | `sudo docker stack deploy -c postgres-compose.yaml postgres` |
| Stop | `sudo docker stack rm postgres` |
| Access | `<node-ip>:5432` |
| Persistent data | Docker volume `volume`, created as `postgres_volume`, mounted at `/var/lib/postgresql/data` |

## Notes

- The current compose file publishes Postgres on host port `5432`.
- Change the default `POSTGRES_DB`, `POSTGRES_USER`, and `POSTGRES_PASSWORD` values before using this outside a trusted local environment.
- The service is constrained to a manager node.
- If the image tag already exists locally and you want to force a refresh, run `sudo docker service update --force postgres_postgres`.
