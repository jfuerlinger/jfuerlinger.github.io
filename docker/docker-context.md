# Use Docker context command to control a remote docker daemon

My initial situation is my work computer with Docker Desktop installed and my Docker VM hosted on a Proxmox.

To be able to conveniently control the Proxmox-hosted Docker daemon from the work computer, it makes sense to create another Docker context.

```shell
docker context create docker-proxmox --docker host=tcp://docker.local:2375
docker-test
Successfully created context "docker-proxmox"
```

That's the easy part. However, the Proxmox Docker daemon must also accept remote connections.

By default, the Docker daemon accepts connections via Unix sockets. To be able to communicate remotely with the Docker daemon, however, it is now necessary to accept TCP connections.

For this reason, the following steps are necessary **on the Docker host**:

```bash
sudo systemctl edit docker.service
```

The following lines must be inserted in the editor that now opens:

```text
[Service]
ExecStart=
ExecStart=/usr/bin/dockerd -H fd:// -H tcp://0.0.0.0:2375
```

The systemd configuration must then be reloaded and the Docker daemon restarted:

```bash
sudo systemctl daemon-reload
sudo systemctl restart docker.service
```

A first test directly on the Docker host is to check whether the Docker daemon is listening on port 2375:

```bash
sudo netstat -lntp | grep dockerd
```

This is how the result should look like:

```bash
tcp6       0      0 :::2375                 :::*                    LISTEN      12082/dockerd
```

As a final test, the docker context `docker-proxmox` is selected **on the work computer** and a `docker container list` is called up:

```bash
docker context use docker-proxmox
docker container list
```

This is how the result of the final verification should look like:

```text
CONTAINER ID   IMAGE                                        COMMAND                  CREATED         STATUS                    PORTS                                                                                                                 NAMES
96179deeccde   jc21/nginx-proxy-manager:latest              "/init"                  2 months ago    Up 10 minutes             0.0.0.0:80-81->80-81/tcp, :::80-81->80-81/tcp, 0.0.0.0:443->443/tcp, :::443->443/tcp                                  nginx-proxy-manager
194763410b57   vaultwarden/server:latest                    "/start.sh"              3 months ago    Up 10 minutes (healthy)   3012/tcp, 0.0.0.0:8005->80/tcp, :::8005->80/tcp                                                                       vaultwarden
cd39dc5f486a   pihole/pihole:latest                         "/s6-init"               7 months ago    Up 10 minutes (healthy)   0.0.0.0:53->53/tcp, 0.0.0.0:53->53/udp, :::53->53/tcp, :::53->53/udp, 67/udp, 0.0.0.0:8881->80/tcp, :::8881->80/tcp   pihole
3061f52bb45e   atmoz/sftp:latest                            "/entrypoint network…"   8 months ago    Up 10 minutes             0.0.0.0:2222->22/tcp, :::2222->22/tcp                                                                                 sftp
b1ed12451430   redis:latest                                 "docker-entrypoint.s…"   11 months ago   Up 10 minutes             0.0.0.0:32770->6379/tcp, :::32770->6379/tcp                                                                           redis
5c639636abe7   mcr.microsoft.com/mssql/server:2022-latest   "/opt/mssql/bin/perm…"   11 months ago   Up 10 minutes             0.0.0.0:1433->1433/tcp, :::1433->1433/tcp                                                                             sql-server
f2af2a227aa4   nginx:latest                                 "/docker-entrypoint.…"   11 months ago   Up 10 minutes             0.0.0.0:32769->80/tcp, :::32769->80/tcp, 0.0.0.0:32768->443/tcp, :::32768->443/tcp                                    webserver
bf534ece8726   b422ad996076                                 "/portainer"             11 months ago   Up 10 minutes             8000/tcp, 9443/tcp, 0.0.0.0:9000->9000/tcp, :::9000->9000/tcp                                                         portainer
```

To switch back to the local Docker daemon, it is only necessary to select the Docker context "default":

```bash
docker context use default
```

🚀🚀🚀 **Mission accomplished** 🚀🚀🚀



## References

- [Configure remote access for Docker daemon](https://docs.docker.com/config/daemon/remote-access/)