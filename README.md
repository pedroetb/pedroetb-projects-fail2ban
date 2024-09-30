# fail2ban

Deployment of docker-fail2ban, a dockerized Fail2Ban daemon to ban hosts that cause multiple authentication errors

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
