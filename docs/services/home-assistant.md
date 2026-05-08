# Home Assistant

## About

Home Assistant is a home automation platform for managing local smart home devices, automations, and integrations.

## Deployment

| Field | Value |
| --- | --- |
| Runtime | Compose |
| Compose file | `homeassistant-compose.yaml` |
| Project/container | Compose project name `homeassistant` |
| Deploy | `sudo docker compose -f homeassistant-compose.yaml up -d` |
| Update | `sudo docker compose -f homeassistant-compose.yaml pull && sudo docker compose -f homeassistant-compose.yaml up -d` |
| Stop | `sudo docker compose -f homeassistant-compose.yaml down` |
| Access | `http://<node-ip>:8123` |
| Persistent data | Compose volume `volume`, created as `homeassistant_volume`, mounted at `/config` |

## Notes

- Home Assistant uses host networking, which is common for device discovery and local integrations.
- The host `/etc/localtime` file is mounted read-only into the container.
