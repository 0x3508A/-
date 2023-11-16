# Docker

## Docker Training with Play-With-Docker

Labs: <https://labs.play-with-docker.com/>

Training: <https://training.play-with-docker.com/>

## Docker Compose for Let’s Encrypt + Nginix + Web-App

<https://devsidestory.com/lets-encrypt-with-docker/>

<http://www.automationlogic.com/using-lets-encrypt-and-docker-for-automatic-ssl/>

## Add `sudo` inside a Container

```dockerfile
FROM ubuntu:18.04
...
RUN apt-get update \
 && apt-get install -y sudo

RUN adduser --disabled-password --gecos '' docker
RUN adduser docker sudo
RUN echo '%sudo ALL=(ALL) NOPASSWD:ALL' >> /etc/sudoers

USER docker
# this is where I was running into problems with the other approaches
RUN sudo apt-get update
```

Alternate:

```dockerfile
FROM ubuntu:18.04
...
RUN apt-get update \
 && apt-get install -y sudo

RUN \
    groupadd -g 999 foo && useradd -u 999 -g foo -G sudo -m -s /bin/bash foo && \
    sed -i /etc/sudoers -re 's/^%sudo.*/%sudo ALL=(ALL:ALL) NOPASSWD: ALL/g' && \
    sed -i /etc/sudoers -re 's/^root.*/root ALL=(ALL:ALL) NOPASSWD: ALL/g' && \
    sed -i /etc/sudoers -re 's/^#includedir.*/## **Removed the include directive** ##"/g' && \
    echo "foo ALL=(ALL) NOPASSWD: ALL" >> /etc/sudoers && \
    echo "Customized the sudoers file for passwordless access to the foo user!" && \
    echo "foo user:";  su - foo -c id
USER foo
```

## Set Time Zone data in `dockerfile`

```dockerfile
FROM ubuntu:18.04
ENV TZ=Asia/Kolkata
RUN ln -snf /usr/share/zoneinfo/$TZ /etc/localtime && echo $TZ > /etc/timezone
...
```

This would help remove the container asking for input again.
Need to do this before `apt update` or `apt install` stages.

## Docker daemon command to Clean all Running Containers

```sh
docker ps -a -q | xargs docker rm -f
```

ref : <https://www.totallymoney.com/blog/cleaning-up-docker-images-on-jenkins-build-machines/>

## Docker daemon command to Remove all dangling Images after build

```sh
docker images -q -f dangling=true | xargs docker rmi
```

ref : <https://www.totallymoney.com/blog/cleaning-up-docker-images-on-jenkins-build-machines/>

## Docker Compose Documentation

<https://docs.docker.com/compose/overview/>

## Docker Volumes Documentation

<https://docs.docker.com/storage/volumes/>

<https://docs.docker.com/engine/reference/commandline/volume/>

## Moving the Data from Docker Containers

ref : <http://blog.diovani.com/technology/2017/06/24/moving-docker-containers-data.html>

```sh
# Create an Alpine Linux based container
docker run -it --name rsync alpine sh

# Now, in the container, install rsync
apk add --no-cache rsync
exit

# At last, make it an image
docker commit rsync rsync:alpine
```

Features of the `rsync` Image for Copying:

- It is incredibly fast
- It’s more reliable to copy large files or directories and it allows stop and resume if needed.
- It provides proper progress information (with `--info=progress2`)

Backing Up Data:

```sh
docker run --volumes-from=some_mysql \
      -v $(pwd)/db/data:/backup \
      rsync:alpine \
      rsync -a --info=progress2 /var/lib/mysql/ /backup
```

Here =
- `some_mysql` is the running Container
- `/db/data` is a local directory where the backup would be stored and mounted in the `rsync` container at `/backup`

More Links:

- <https://docs.docker.com/engine/reference/commandline/export/>
- <https://docs.docker.com/engine/reference/commandline/import/>
- <https://docs.oracle.com/cd/E37670_01/E75728/html/section_x54_32w_gp.html>
- <https://blog.giantswarm.io/moving-docker-container-images-around/>
- <https://www.digitalocean.com/community/tutorials/how-to-work-with-docker-data-volumes-on-ubuntu-14-04>

## Docker Container Data Manipulation `nsenter`

<http://www.tricksofthetrades.net/2016/03/14/docker-data-volumes/>

## Docker Plugin API

<https://docs.docker.com/engine/extend/plugin_api/>

## Insight into `chroot` as alternative to Docker

<https://www.ibm.com/developerworks/library/l-mount-namespaces/index.html>

## Newer Docker that’s better than the Existing one

<https://github.com/moby/buildkit>

## ThingsBoard - IOT Dashboard - Trial

Docker Install:

<https://thingsboard.io/docs/user-guide/install/docker/>

Start Command:
```sh
docker run -it -p 9090:9090 -p 1883:1883 -p 5683:5683/udp \
    -v ~/.mytb-data:/data -v ~/.mytb-logs:/var/log/thingsboard \
    --name mytb --restart always thingsboard/tb-postgres
```

Where:

```sh
docker run - run this container
-it - attach a terminal session with current ThingsBoard process output
-p 9090:9090 - connect local port 9090 to exposed internal HTTP port 9090
-p 1883:1883 - connect local port 1883 to exposed internal MQTT port 1883
-p 5683:5683 - connect local port 5683 to exposed internal COAP port 5683
-v ~/.mytb-data:/data - mounts the host’s dir ~/.mytb-data to ThingsBoard DataBase data directory
-v ~/.mytb-logs:/var/log/thingsboard - mounts the host’s dir ~/.mytb-logs to ThingsBoard logs directory
--name mytb - friendly local name of this machine
--restart always - automatically start ThingsBoard in case of system reboot and restart in case of failure.
thingsboard/tb-postgres - docker image, can be also thingsboard/tb-cassandra or thingsboard/tb
```

After executing this command you can open `http://{your-host-ip}:9090` in you browser (for ex. `http://localhost:9090`). You should see **ThingsBoard login** page. Use the following default credentials:

```
Systen Administrator: sysadmin@thingsboard.org / sysadmin
Tenant Administrator: tenant@thingsboard.org / tenant
Customer User: customer@thingsboard.org / customer
```

You can always change passwords for each account in account profile page.

### Detaching, stop and start commands

You can detach from session terminal with `Ctrl-p Ctrl-q` - the container will keep running in the background.

To reattach to the terminal (to see ThingsBoard logs) run:

```sh
docker attach mytb
```

To stop the container:

```sh
docker stop mytb
```

To start the container:

```sh
docker start mytb
```

Token Post:

```sh
curl -v -X POST -d "{\"temperature\":25, \"humidity\":30 }" \
    http://localhost:9090/api/v1/DHT11_DEMO_TOKEN/telemetry \
    --header "Content-Type:application/json"
```

----
<!-- Footer Begins Here -->
## Links

- [Back to Linux Hub](./README.md)
- [Back to Root Document](../README.md)

