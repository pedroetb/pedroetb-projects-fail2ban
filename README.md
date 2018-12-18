# fail2ban

## Usage

Jails and filters from `./config` directory are not loaded by default. Instead, they are mounted at `/config` inside of container.
On initial run or after updating this config, you must copy the new definition inside container `/data` directory:

```
// Run this in container shell
$ cp -r /config/* /data
```
