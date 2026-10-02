---
title: "Docker Commands"
date: 2016-05-22
categories: 
  - "technical"
tags: 
  - "docker"
---

**Get a Docker Image**

```
docker pull ubuntu
```

**Run a Docker** **Container**

```
docker run -d -ti -v /data--name=Odoo /bin/bash
```
d = Detach mode; -t = Terminal; -i = Interactive; -v = attach volume. The container runs /bin/bash when it starts.

**Enter into a Docker Container**

```
docker attach odoo
```
Attaches to the odoo container.

**Quit from a container without stopping it**

Ctrl+P+Q : Quit without stopping the running process from the container

**Attach a Volume**

```
docker run -d -ti -v /srv/data1:/data --name=Odoo1 /bin/bash
```
Creates the Odoo1 container and mounts the host's /srv/data1 at /data in the container.

**List all Containers in the host**

```
docker ps -a
```
Lists running and stopped containers.

**Start/Stop Container**

```bash
docker start odoo
docker stop odoo
```

Expose a port:

```bash
docker run -d -p 3306:3306 -ti mysql /bin/bash
```

Start the container mysql and open port 3306 and map it to localport 3306

Networking:

```bash
docker network ls
docker network inspect bridge
```

List all the ports

```
iptables -L -t nat
```

```
docker run -it -p 80:8080 -d --restart=always --name shipyard --link shipyard-rethinkdb:rethinkdb shipyard/shipyard
```
