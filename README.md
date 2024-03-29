# Cloud Code Server (Public)


Content in this repo syncs with the master self-hosted code server: [code.angelulz.cloud](https://code.angelulz.cloud)

**Contact: admin@angelulz.email**

---

### Quick Links:

*Requires access to the Angelulz Cloud Org*

##### Status

[Main](https://status.angelulz.cloud/)

##### Dashboards

[Cloud](https://angelulz.cloud) | [App](https://angelulz.app) | [Xyz](https://angelulz.xyz)

##### Reverse Proxy

[Cloud](https://traefik.angelulz.cloud) | [App](https://traefik.angelulz.app) | [Xyz](https://traefik.angelulz.xyz)

##### Netdata Analytics

[Cloud](https://netdata.angelulz.cloud) | [App](https://netdata.angelulz.app)

##### Container Logs

[Cloud](https://dozzle.angelulz.cloud) | [App](https://dozzle.angelulz.app)

##### Sync

[Cloud](https://sync.angelulz.cloud) | [App](https://sync.angelulz.app) | [Xyz](https://sync.angelulz.xyz)

---

# Docker cmd reference

## Core

> Run compose file
```
sudo docker compose -f ~/docker/docker-compose.yml up -d
```

>See containers
```
sudo docker ps -a
```

#### Docker Cleanup

> Prune containers, networks, volumes
```
sudo docker container prune -f
sudo docker image prune -f
sudo docker network prune -f
sudo docker volume prune -f
```

> Remove all containers
```
for id in $(sudo docker ps -aq); do sudo docker container rm -f $id; done
```

> Remove all images
```
for img in $(sudo docker images -q); do sudo docker rmi $img; done
```

#### Stopping / Restarting Containers using Docker Compose

```
#stop a running container
sudo docker-compose -f ~/docker/docker-compose.yml stop CONTAINER-NAME #replace stop with restart for restart
```

>To completely stop and remove containers, images, volumes, and networks (go back to how it was before running docker compose file), use the following command:
```
sudo docker compose -f ~/docker/docker-compose.yml down
```

> If, for example, you comment out services from the compose YML before `down`, then try to bring them down by referencing the compose file, this won't work as expected. 
> Instead, you should review containers/images/networks/volumes to ensure there's no unwanted/lingering data from the Compose containers.
> Alternatively, to make changes to the compose file, first `down` , update file, then `up`.

#### Container Logs

```
#all
sudo docker logs
#by container
sudo docker compose -f ~/docker/docker-compose.yml logs transmission-vpn
#compose apps
sudo docker compose -f ~/docker/ds-docker-compose.yml logs
#view in real time
sudo docker-compose -f ~/docker/docker-compose.yml logs transmission-vpn logs -tf --tail="50"
```

---
## Networks

> List Docker networks
```
sudo docker network ls
```

> Get Docker internal IP of service
```
sudo docker inspect -f '{{range .NetworkSettings.Networks}}{{.IPAddress}}{{end}}' SERVICE
```

> Get IP address of all Docker containers for network NETWORKid
```
sudo docker network inspect -f \
'{{json .Containers}}' NETWORKid | \
jq '.[] | .Name + ":" + .IPv4Address'
```

> Assign a list of network IDs to var $nids
```
nids=$(sudo docker network ls | grep -v 'NETWORK' | cut -d ' ' -f1)
```

> Get IP address of all Docker containers across all networks
```
nids=$(sudo docker network ls | grep -v 'NETWORK' | cut -d ' ' -f1); for id in "$nids"; do echo -n "network: $id - " && sudo docker network inspect -f '{{json .Containers}}' $id | jq '.[] | .Name + ":" + .IPv4Address'; done
```

---
## Sync

#### Server to Sync
> Docker compose YML/Traefik rules
```
cd ~/docker/appdata/syncthing/Sync/docker/
cp -R ~/docker/appdata/traefik2/rules/* rules/
cp -R ~/docker/compose/* compose/
```

#### Sync to Server
> Single YML
```
cp ~/docker/appdata/syncthing/Sync/docker/compose/cs/FILE ~/docker/compose/ds/FILE
```

---
## SnR

> Grep Dir
```
for file in $(ls); do echo $file && grep -in 'SOMETHING' $file; done
```

> SnR File
```
sed -i 's/SEARCH/REPLACE/g' FILE
```

> SnR Files in Dir
```
for file in $(ls); do echo $file && sed -i 's/SEARCH/REPLACE/g' $file; done
```
