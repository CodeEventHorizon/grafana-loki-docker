# Grafana Loki with docker compose and local filesystem storage

## Tools used:

1. Docker
2. Grafana
3. Grafana Loki

## Images

1. [Grafana](https://hub.docker.com/r/grafana/grafana)
2. [Grafana Loki](https://hub.docker.com/r/grafana/loki)

## Containers

1. init - since Loki containers are running as `user 10001` and mounted data is owned by root, Loki would not have permissions to create the directories. Therefore the `init` container changes permissions of the mounted directory
2. loki - Grafana Loki container
3. grafana - Grafana container

## Networks

`grafana-net` is important as it gives us ability to call containers by their names within the network instead of using IP addresses. For instance we are able to call Grafana Loki container as `http://loki:3100` in Grafana container

## Volumes

3 volumes are used, 2 for Grafana Loki and 1 for Grafana

1. `./loki-config.yaml:/etc/loki/local-config.yaml` this volume loads the `loki-config.yaml` file from the host system and makes it available inside the loki container at the location `/etc/loki/local-config.yaml` **NOTE: do not change the path and file name!** 

2. `loki-data:/loki` is a bind mount, you will have to create `/loki` directory beforehand. All the grafana loki data & logs will be stored here **NOTE: it needs to be `/loki` to work properly**

3. `grafana-data:/var/lib/grafana` stores grafana data, like settings and dashboards