# fail2ban

Deployment of docker-fail2ban, a dockerized Fail2Ban daemon to ban hosts that cause multiple authentication errors

## Services available

This project is designed to deploy 2 different Docker Swarm services: `fail2ban-input` and `fail2ban-docker`.

Two different services are needed because they work with different `iptables` *chains* (more info at <https://github.com/crazy-max/docker-fail2ban/blob/master/README.md#notes>) and have different deployment mode needs.

Both are responsible for blocking unauthorized access by different protocols.

### fail2ban-input

This service is deployed using global mode (a container for each Docker Swarm node), and is responsible for blocking unauthorized SSH login tries. Reads logs from system on each node at `/var/log/auth.log`.

### fail2ban-docker

This service is deployed using global mode with placement restrictions (a container for each Docker Swarm node meeting label requirements), and is responsible for blocking unauthorized HTTPS requests. Reads logs from `traefik` on each node at local volume `traefik-log-vol`.

## Configuration

You may define these environment variables (**bold** are mandatory):

| Variable name | Default value |
| - | - |
| **TZ** | `Atlantic/Canary` |
| *F2B_LOG_LEVEL* | `INFO` |
| *F2B_DB_PURGE_AGE* | `15d` |
| *IPTABLES_MODE* | `nft` |

> :bulb: There is also other environment variables used at compose file, but not propagated to deployed services. Check `deploy/compose.yaml` to inspect them.

## Common actions

```sh
# get jail status
docker exec -t <container> fail2ban-client status <jail>

# unban ip, single jail
docker exec -t <container> fail2ban-client set <jail> unbanip <ip>

# unban ip, all jails
docker exec -t <container> fail2ban-client unban <ip>

# unban all, all jails
docker exec -t <container> fail2ban-client unban --all
```

## References

- Check `docker-fail2ban` official repository at <https://github.com/crazy-max/docker-fail2ban>.
- Check `Fail2Ban` official repository at <https://github.com/fail2ban/fail2ban>.
