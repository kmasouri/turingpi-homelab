# Pi-hole

## About

Pi-hole provides network-wide DNS ad blocking. Because it is DNS infrastructure, its Docker networking matters more than it does for most web apps: DNS traffic should reach Pi-hole without source address rewriting so query logs and client-specific filtering can identify real LAN clients.

Recommended network pattern:

```text
LAN clients
 -> router DHCP advertises Pi-hole as DNS
    -> Docker host running Pi-hole with host networking
       -> upstream DNS
```

## Deployment

| Field | Value |
| --- | --- |
| Runtime | Compose |
| Compose file | `pihole-compose.yaml` |
| Project/container | Container name `pihole` |
| Deploy | `sudo docker compose -f pihole-compose.yaml up -d` |
| Update | `sudo docker compose -f pihole-compose.yaml pull && sudo docker compose -f pihole-compose.yaml up -d` |
| Stop | `sudo docker compose -f pihole-compose.yaml down` |
| Access | `http://<node-ip>:8080/admin` |
| Persistent data | Compose volume `volume`, created as `pihole_volume`, mounted at `/etc/pihole` |

## Notes

- Set `PIHOLE_PASSWORD` in `.env`.
- Pi-hole uses `network_mode: host` so it can bind DNS directly and see real client IPs.
- Configure LAN clients or router DHCP to use only the Pi-hole host IP as DNS.
- The compose project is named `pihole`, so Docker creates the persistent volume as `pihole_volume`.
- Avoid public secondary DNS servers on clients unless you intentionally want clients to bypass Pi-hole sometimes.
- If IPv6 is enabled on your LAN, also configure IPv6 DNS to point at Pi-hole or disable IPv6 DNS advertisement from the router.
- Make sure host port `53` is available for DNS and host port `8080` is available for the Pi-hole admin UI configured in this repo.

### Docker Networking Gotchas

Avoid deploying Pi-hole behind:

- Docker Swarm ingress
- Overlay networks
- Routing mesh
- Service meshes
- NAT-heavy container networking

Those layers can rewrite client source IPs. When that happens, Pi-hole may show Docker or gateway addresses such as:

```text
10.0.0.2
10.0.2.3
127.0.0.1
```

instead of real LAN clients such as:

```text
192.168.x.x
```

Preferred approaches for DNS infrastructure:

- Host networking
- macvlan
- Dedicated LAN IPs

### Secondary DNS Bypass

Do not configure clients like this if you want all DNS traffic filtered:

```text
Primary DNS: Pi-hole
Secondary DNS: 9.9.9.9
```

Modern clients do not always treat secondary DNS as strict fallback. They may query either server, which can make filtering look inconsistent.

Use only Pi-hole as client DNS:

```text
DNS: <pi-hole-host-ip>
```

Configure upstream DNS inside Pi-hole instead of giving clients public DNS servers directly.

### systemd-resolved Port 53 Conflict

When Pi-hole uses host networking, it may fail to bind DNS if `systemd-resolved` is already listening on port 53:

```text
failed to create listening socket for port 53: Address in use
```

On the Docker host, update `/etc/systemd/resolved.conf`:

```ini
DNSStubListener=no
```

Restart `systemd-resolved`:

```bash
sudo systemctl restart systemd-resolved
```

If the host resolver needs repair afterward, point `/etc/resolv.conf` at the full systemd resolver file:

```bash
sudo ln -sf /run/systemd/resolve/resolv.conf /etc/resolv.conf
```

### Expected Result

After this setup, Pi-hole should:

- Bind directly to host port `53`.
- Show real LAN clients in query logs.
- Apply filtering consistently.
- Avoid Docker overlay addresses like `10.0.0.x` dominating the top clients list.
