# fail2ban

Deployment of docker-fail2ban, a dockerized Fail2Ban daemon to ban hosts that cause multiple authentication errors

## Services available

This project is designed to deploy 2 different Docker Swarm services: `fail2ban-input` and `fail2ban-docker`.

Two different services are needed because they work with different `iptables` *chains* (more info at <https://github.com/crazy-max/docker-fail2ban/blob/master/README.md#notes>) and have different deployment mode needs.

Both are responsible for blocking different protocols unauthorized access and are able to notify events via email.

### fail2ban-input

This service is deployed using global mode (a container for each Docker Swarm node), and is responsible for blocking unauthorized SSH login tries. Read logs from system on each node at `/var/log/auth.log`.

### fail2ban-docker

This service is deployed using replicated mode with node label restrictions (a container for each Docker Swarm node meeting label requirements, with a configurable maximum number of replicas), and is responsible for blocking unauthorized HTTPS requests. Read logs from `traefik` on each node at local volume `traefik-log-vol`.

## Configuration

You may define these environment variables:

| Variable name | Default value |
| - | - |
| *TZ* | `Atlantic/Canary` |
| *F2B_LOG_LEVEL* | `INFO` |
| *F2B_DB_PURGE_AGE* | `15d` |
| *IPTABLES_MODE* | `auto` |
| *SSMTP_HOST* | `localhost` |
| *SSMTP_PORT* | `25` |
| *SSMTP_USER* | - |
| *SSMTP_PASSWORD* | - |
| *SSMTP_TLS* | `NO` |
| *SSMTP_STARTTLS* | `NO` |
| *EMAIL_RECIPIENT* | `user@example.org` |
| *EMAIL_SENDER* | `fail2ban@change.me` |
| *EMAIL_SENDER_NAME* | `Fail2Ban` |

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
