# Portainer

## About

Portainer provides a web UI for managing Docker and Docker Swarm resources. This stack also deploys the Portainer agent globally so Portainer can communicate with every Linux node in the Swarm.

## Deployment

| Field | Value |
| --- | --- |
| Runtime | Swarm |
| Compose file | `portainer-compose.yaml` |
| Stack name | `portainer` |
| Deploy | `sudo docker stack deploy -c portainer-compose.yaml portainer` |
| Update | `sudo docker stack deploy -c portainer-compose.yaml portainer` |
| Stop | `sudo docker stack rm portainer` |
| Access | `https://<node-ip>:9443` or `http://<node-ip>:9000` |
| Persistent data | Docker volume `volume`, created as `portainer_volume`, mounted at `/data` |

## Notes

- The Portainer agent runs globally, with one instance per Linux node.
- The Portainer UI is constrained to a manager node.
- Port `8000` is also published for Portainer edge agent features.
- If the image tag already exists locally and you want to force a refresh, run `sudo docker service update --force portainer_portainer`.
