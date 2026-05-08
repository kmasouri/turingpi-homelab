# Homelab Portal

## About

Homelab Portal is a lightweight static app launcher served by Nginx. It provides quick links to homelab services and bookmarks from a single landing page.

## Deployment

| Field | Value |
| --- | --- |
| Runtime | Swarm |
| Compose file | `homelab-portal-compose.yaml` |
| Stack name | `portal` |
| Deploy | `sudo docker stack deploy -c homelab-portal-compose.yaml portal` |
| Update | `sudo docker stack deploy -c homelab-portal-compose.yaml portal` |
| Stop | `sudo docker stack rm portal` |
| Access | `http://<node-ip>/` |
| Persistent data | None; config is bind-mounted from `homelab-portal-config.json` |

## Notes

- The portal is a static Nginx-hosted app for quick links to homelab services.
- After editing `homelab-portal-config.json`, refresh the running service with `sudo docker service update --force portal_homelab-portal`.
- See the [Homelab Portal configuration reference](https://github.com/kmasouri/homelab-portal?tab=readme-ov-file#%EF%B8%8F-configuration).
