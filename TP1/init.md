I. Init

Sommaire : 
    1. Installation de Docker
    2. Vérifier que Docker est bien là
    3. sudo c pa bo
    4. Un premier conteneur en vif
    5. Un deuxième conteneur en vif

3. sudo c pa bo

🌞 Ajouter votre utilisateur au groupe docker
```
batman@BATPC:~$ sudo usermod -aG docker batman
batman@BATPC:~$ grep docker /etc/group
docker:x:128:batman
```

4. Un premier conteneur en vif

🌞 Lancer un conteneur NGINX
```
batman@BATPC:~$ docker run -d -p 9999:80 nginx
Unable to find image 'nginx:latest' locally
latest: Pulling from library/nginx
bc0965b23a04: Pull complete 
650ee30bbe5e: Pull complete 
8cc1569e58f5: Pull complete 
362f35df001b: Pull complete 
13e320bf29cd: Pull complete 
7b50399908e1: Pull complete 
57b64962dd94: Pull complete 
Digest: sha256:fb197595ebe76b9c0c14ab68159fd3c08bd067ec62300583543f0ebda353b5be
Status: Downloaded newer image for nginx:latest
826047cac6a4111209f521bcf1d4024dd092a5a824401e7d5d54b53b5a3ead97
```

🌞 Visitons

- vérifier que le conteneur est actif avec une commande qui liste les conteneurs en cours de fonctionnement
```
CONTAINER ID   IMAGE                        COMMAND                  CREATED         STATUS         PORTS                                       NAMES
826047cac6a4   nginx                        "/docker-entrypoint.…"   2 minutes ago   Up 2 minutes   0.0.0.0:9999->80/tcp, :::9999->80/tcp       confident_ishizaka
0a1021fd8ec8   sources-generous-santa-app   "docker-entrypoint.s…"   9 days ago      Up 2 days      0.0.0.0:3000->3000/tcp, :::3000->3000/tcp   sources-generous-santa-app-1
```

- afficher les logs du conteneur
```
batman@BATPC:~$ docker logs my-nginx
/docker-entrypoint.sh: /docker-entrypoint.d/ is not empty, will attempt to perform configuration
/docker-entrypoint.sh: Looking for shell scripts in /docker-entrypoint.d/
/docker-entrypoint.sh: Launching /docker-entrypoint.d/10-listen-on-ipv6-by-default.sh
10-listen-on-ipv6-by-default.sh: info: Getting the checksum of /etc/nginx/conf.d/default.conf
10-listen-on-ipv6-by-default.sh: info: Enabled listen on IPv6 in /etc/nginx/conf.d/default.conf
/docker-entrypoint.sh: Sourcing /docker-entrypoint.d/15-local-resolvers.envsh
/docker-entrypoint.sh: Launching /docker-entrypoint.d/20-envsubst-on-templates.sh
/docker-entrypoint.sh: Launching /docker-entrypoint.d/30-tune-worker-processes.sh
/docker-entrypoint.sh: Configuration complete; ready for start up
2024/12/11 10:20:41 [notice] 1#1: using the "epoll" event method
2024/12/11 10:20:41 [notice] 1#1: nginx/1.27.3
2024/12/11 10:20:41 [notice] 1#1: built by gcc 12.2.0 (Debian 12.2.0-14) 
2024/12/11 10:20:41 [notice] 1#1: OS: Linux 6.1.0-26-amd64
2024/12/11 10:20:41 [notice] 1#1: getrlimit(RLIMIT_NOFILE): 1048576:1048576
2024/12/11 10:20:41 [notice] 1#1: start worker processes
2024/12/11 10:20:41 [notice] 1#1: start worker process 29
2024/12/11 10:20:41 [notice] 1#1: start worker process 30
2024/12/11 10:20:41 [notice] 1#1: start worker process 31
2024/12/11 10:20:41 [notice] 1#1: start worker process 32
2024/12/11 10:20:41 [notice] 1#1: start worker process 33
2024/12/11 10:20:41 [notice] 1#1: start worker process 34
2024/12/11 10:20:41 [notice] 1#1: start worker process 35
2024/12/11 10:20:41 [notice] 1#1: start worker process 36
2024/12/11 10:20:41 [notice] 1#1: start worker process 37
2024/12/11 10:20:41 [notice] 1#1: start worker process 38
2024/12/11 10:20:41 [notice] 1#1: start worker process 39
2024/12/11 10:20:41 [notice] 1#1: start worker process 40
2024/12/11 10:20:41 [notice] 1#1: start worker process 41
2024/12/11 10:20:41 [notice] 1#1: start worker process 42
2024/12/11 10:20:41 [notice] 1#1: start worker process 43
2024/12/11 10:20:41 [notice] 1#1: start worker process 44
```

- afficher toutes les informations relatives au conteneur avec une commande docker inspect
```
$ docker images
batman@BATPC:~$ docker inspect my-nginx
[
    {
        "Id": "5c999de3c4747816c8e5a0538fc2eabd17af0fb992cbc6b49c0ea3fd0fba77c1",
        "Created": "2024-12-11T10:20:40.844760958Z",
        "Path": "/docker-entrypoint.sh",
        "Args": [
            "nginx",
            "-g",
            "daemon off;"
        ],
        "State": {
            "Status": "running",
            "Running": true,
            "Paused": false,
            "Restarting": false,
            "OOMKilled": false,
            "Dead": false,
            "Pid": 148043,
            "ExitCode": 0,
            "Error": "",
            "StartedAt": "2024-12-11T10:20:41.180057561Z",
            "FinishedAt": "0001-01-01T00:00:00Z"
        },
        "Image": "sha256:66f8bdd3810c96dc5c28aec39583af731b34a2cd99471530f53c8794ed5b423e",
        "ResolvConfPath": "/var/lib/docker/containers/5c999de3c4747816c8e5a0538fc2eabd17af0fb992cbc6b49c0ea3fd0fba77c1/resolv.conf",
        "HostnamePath": "/var/lib/docker/containers/5c999de3c4747816c8e5a0538fc2eabd17af0fb992cbc6b49c0ea3fd0fba77c1/hostname",
        "HostsPath": "/var/lib/docker/containers/5c999de3c4747816c8e5a0538fc2eabd17af0fb992cbc6b49c0ea3fd0fba77c1/hosts",
        "LogPath": "/var/lib/docker/containers/5c999de3c4747816c8e5a0538fc2eabd17af0fb992cbc6b49c0ea3fd0fba77c1/5c999de3c4747816c8e5a0538fc2eabd17af0fb992cbc6b49c0ea3fd0fba77c1-json.log",
        "Name": "/my-nginx",
        "RestartCount": 0,
        "Driver": "overlay2",
        "Platform": "linux",
        "MountLabel": "",
        "ProcessLabel": "",
        "AppArmorProfile": "docker-default",
        "ExecIDs": null,
        "HostConfig": {
            "Binds": null,
            "ContainerIDFile": "",
            "LogConfig": {
                "Type": "json-file",
                "Config": {}
            },
            "NetworkMode": "default",
            "PortBindings": {
                "80/tcp": [
                    {
                        "HostIp": "",
                        "HostPort": "9999"
                    }
                ]
            },
            "RestartPolicy": {
                "Name": "no",
                "MaximumRetryCount": 0
            },
            "AutoRemove": false,
            "VolumeDriver": "",
            "VolumesFrom": null,
            "CapAdd": null,
            "CapDrop": null,
            "CgroupnsMode": "private",
            "Dns": [],
            "DnsOptions": [],
            "DnsSearch": [],
            "ExtraHosts": null,
            "GroupAdd": null,
            "IpcMode": "private",
            "Cgroup": "",
            "Links": null,
            "OomScoreAdj": 0,
            "PidMode": "",
            "Privileged": false,
            "PublishAllPorts": false,
            "ReadonlyRootfs": false,
            "SecurityOpt": null,
            "UTSMode": "",
            "UsernsMode": "",
            "ShmSize": 67108864,
            "Runtime": "runc",
            "ConsoleSize": [
                0,
                0
            ],
            "Isolation": "",
            "CpuShares": 0,
            "Memory": 0,
            "NanoCpus": 0,
            "CgroupParent": "",
            "BlkioWeight": 0,
            "BlkioWeightDevice": [],
            "BlkioDeviceReadBps": null,
            "BlkioDeviceWriteBps": null,
            "BlkioDeviceReadIOps": null,
            "BlkioDeviceWriteIOps": null,
            "CpuPeriod": 0,
            "CpuQuota": 0,
            "CpuRealtimePeriod": 0,
            "CpuRealtimeRuntime": 0,
            "CpusetCpus": "",
            "CpusetMems": "",
            "Devices": [],
            "DeviceCgroupRules": null,
            "DeviceRequests": null,
            "KernelMemory": 0,
            "KernelMemoryTCP": 0,
            "MemoryReservation": 0,
            "MemorySwap": 0,
            "MemorySwappiness": null,
            "OomKillDisable": null,
            "PidsLimit": null,
            "Ulimits": null,
            "CpuCount": 0,
            "CpuPercent": 0,
            "IOMaximumIOps": 0,
            "IOMaximumBandwidth": 0,
            "MaskedPaths": [
                "/proc/asound",
                "/proc/acpi",
                "/proc/kcore",
                "/proc/keys",
                "/proc/latency_stats",
                "/proc/timer_list",
                "/proc/timer_stats",
                "/proc/sched_debug",
                "/proc/scsi",
                "/sys/firmware"
            ],
            "ReadonlyPaths": [
                "/proc/bus",
                "/proc/fs",
                "/proc/irq",
                "/proc/sys",
                "/proc/sysrq-trigger"
            ]
        },
        "GraphDriver": {
            "Data": {
                "LowerDir": "/var/lib/docker/overlay2/903e784dc0f7b6cf36e1cab41b39f798c1cab5d1972068e7c60c28c46d6d5df0-init/diff:/var/lib/docker/overlay2/24257019038cc01751780bd3f57dd882170b351f6a1a2c9cd39c82f79c81b771/diff:/var/lib/docker/overlay2/f154082df13cff955b64d3b607592e43834e27e0c47c6a72b47e0b92e4a8d59f/diff:/var/lib/docker/overlay2/4b3b5e1b98136884ce93fa123255b4003a004bdd085279bf957e7e9cf4cbe4ad/diff:/var/lib/docker/overlay2/51b0aca07220b88492a071b789dea3f3b674002f292eab0d491afd683e7bf8bd/diff:/var/lib/docker/overlay2/827eb0bca2b1f58eb31a634ec6288badea43b1a26ca275f459f6541e6377016e/diff:/var/lib/docker/overlay2/eba18c2d320b199a36b4c7d70f6969e59848265d7f5eb74a362f33ce96691199/diff:/var/lib/docker/overlay2/6304afc4eb2a4e5af0bcd0786cc7bd91476d69a9efc2667385d9b3c664f53284/diff",
                "MergedDir": "/var/lib/docker/overlay2/903e784dc0f7b6cf36e1cab41b39f798c1cab5d1972068e7c60c28c46d6d5df0/merged",
                "UpperDir": "/var/lib/docker/overlay2/903e784dc0f7b6cf36e1cab41b39f798c1cab5d1972068e7c60c28c46d6d5df0/diff",
                "WorkDir": "/var/lib/docker/overlay2/903e784dc0f7b6cf36e1cab41b39f798c1cab5d1972068e7c60c28c46d6d5df0/work"
            },
            "Name": "overlay2"
        },
        "Mounts": [],
        "Config": {
            "Hostname": "5c999de3c474",
            "Domainname": "",
            "User": "",
            "AttachStdin": false,
            "AttachStdout": false,
            "AttachStderr": false,
            "ExposedPorts": {
                "80/tcp": {}
            },
            "Tty": false,
            "OpenStdin": false,
            "StdinOnce": false,
            "Env": [
                "PATH=/usr/local/sbin:/usr/local/bin:/usr/sbin:/usr/bin:/sbin:/bin",
                "NGINX_VERSION=1.27.3",
                "NJS_VERSION=0.8.7",
                "NJS_RELEASE=1~bookworm",
                "PKG_RELEASE=1~bookworm",
                "DYNPKG_RELEASE=1~bookworm"
            ],
            "Cmd": [
                "nginx",
                "-g",
                "daemon off;"
            ],
            "Image": "nginx",
            "Volumes": null,
            "WorkingDir": "",
            "Entrypoint": [
                "/docker-entrypoint.sh"
            ],
            "OnBuild": null,
            "Labels": {
                "maintainer": "NGINX Docker Maintainers <docker-maint@nginx.com>"
            },
            "StopSignal": "SIGQUIT"
        },
        "NetworkSettings": {
            "Bridge": "",
            "SandboxID": "419096a61eb3e4756115d220a716eb3698fce0091803b765534d6eec255c7def",
            "HairpinMode": false,
            "LinkLocalIPv6Address": "",
            "LinkLocalIPv6PrefixLen": 0,
            "Ports": {
                "80/tcp": [
                    {
                        "HostIp": "0.0.0.0",
                        "HostPort": "9999"
                    },
                    {
                        "HostIp": "::",
                        "HostPort": "9999"
                    }
                ]
            },
            "SandboxKey": "/var/run/docker/netns/419096a61eb3",
            "SecondaryIPAddresses": null,
            "SecondaryIPv6Addresses": null,
            "EndpointID": "36b3187190e776f9d69f5fa35d50046d744f393b9f79dfe6e43432010f472275",
            "Gateway": "172.17.0.1",
            "GlobalIPv6Address": "",
            "GlobalIPv6PrefixLen": 0,
            "IPAddress": "172.17.0.2",
            "IPPrefixLen": 16,
            "IPv6Gateway": "",
            "MacAddress": "02:42:ac:11:00:02",
            "Networks": {
                "bridge": {
                    "IPAMConfig": null,
                    "Links": null,
                    "Aliases": null,
                    "NetworkID": "f1aef0b5a47bb187a1091078594534784315cf2891c83bd3478b3a745afae759",
                    "EndpointID": "36b3187190e776f9d69f5fa35d50046d744f393b9f79dfe6e43432010f472275",
                    "Gateway": "172.17.0.1",
                    "IPAddress": "172.17.0.2",
                    "IPPrefixLen": 16,
                    "IPv6Gateway": "",
                    "GlobalIPv6Address": "",
                    "GlobalIPv6PrefixLen": 0,
                    "MacAddress": "02:42:ac:11:00:02",
                    "DriverOpts": null
                }
            }
        }
    }
]
```

- afficher le port en écoute sur la VM avec un sudo ss -lnpt
```
batman@BATPC:~$ sudo ss -ltnp | grep docker
LISTEN 0      4096         0.0.0.0:3000       0.0.0.0:*    users:(("docker-proxy",pid=2708,fd=4))                                                                                                                                       
LISTEN 0      4096         0.0.0.0:9999       0.0.0.0:*    users:(("docker-proxy",pid=148005,fd=4))                                                                                                                                     
LISTEN 0      4096            [::]:3000          [::]:*    users:(("docker-proxy",pid=2714,fd=4))                                                                                                                                       
LISTEN 0      4096            [::]:9999          [::]:*    users:(("docker-proxy",pid=148011,fd=4))          
```

- ouvrir le port 9999/tcp (vu dans le ss au dessus normalement) dans le firewall de la VM
```
batman@BATPC:~$ sudo ufw allow 9999/tcp
Rules updated
Rules updated (v6)
```

- depuis le navigateur de votre PC, visiter le site web sur http://IP_VM:9999
```
batman@BATPC:~$ curl localhost:9999
<!DOCTYPE html>
<html>
<head>
<title>Welcome to nginx!</title>
<style>
html { color-scheme: light dark; }
body { width: 35em; margin: 0 auto;
font-family: Tahoma, Verdana, Arial, sans-serif; }
</style>
</head>
<body>
<h1>Welcome to nginx!</h1>
<p>If you see this page, the nginx web server is successfully installed and
working. Further configuration is required.</p>

<p>For online documentation and support please refer to
<a href="http://nginx.org/">nginx.org</a>.<br/>
Commercial support is available at
<a href="http://nginx.com/">nginx.com</a>.</p>

<p><em>Thank you for using nginx.</em></p>
</body>
</html>
```
🌞 On va ajouter un site Web au conteneur NGINX
```
batman@BATPC:~$ echo '<h1>MEOOOW</h1>' | sudo tee ~/nginx/index.html
<h1>MEOOOW</h1>
batman@BATPC:~$ sudo tee ~/nginx/site_nul.conf > /dev/null <<EOF
server {
    listen        8080;

    location / {
        root /var/www/html;
    }
}
EOF
batman@BATPC:~$ sudo chown -R $(whoami):$(whoami) ~/nginx
batman@BATPC:~$ ls -l ~/nginx
total 8
-rw-r--r-- 1 batman batman 16 11 déc.  11:30 index.html
-rw-r--r-- 1 batman batman 87 11 déc.  11:30 site_nul.conf
batman@BATPC:~$ docker run -d -p 9999:8080 \
  -v ~/nginx/index.html:/var/www/html/index.html \
  -v ~/nginx/site_nul.conf:/etc/nginx/conf.d/site_nul.conf \
  nginx
d6c1d3cf869a50111de08a6111fb44d059e82e6b5972067c677178c9a8d6a746
```

🌞 Visitons

- vérifier que le conteneur est actif
```
batman@BATPC:~$ docker ps
CONTAINER ID   IMAGE                        COMMAND                  CREATED          STATUS          PORTS                                               NAMES
d6c1d3cf869a   nginx                        "/docker-entrypoint.…"   33 seconds ago   Up 32 seconds   80/tcp, 0.0.0.0:9999->8080/tcp, :::9999->8080/tcp   stoic_dirac
0a1021fd8ec8   sources-generous-santa-app   "docker-entrypoint.s…"   9 days ago       Up 2 days       0.0.0.0:3000->3000/tcp, :::3000->3000/tcp           sources-generous-santa-app-1
```

- aucun port firewall à ouvrir : on écoute toujours port 9999 sur la machine hôte (la VM)
```
batman@BATPC:~$ sudo lsof -i :9999
COMMAND      PID USER   FD   TYPE  DEVICE SIZE/OFF NODE NAME
docker-pr 160266 root    4u  IPv4 2583223      0t0  TCP *:9999 (LISTEN)
docker-pr 160272 root    4u  IPv6 2583228      0t0  TCP *:9999 (LISTEN)
```

- visiter le site web depuis votre PC
```
batman@BATPC:~$ curl http://localhost:9999
<h1>MEOOOW</h1>
```

5. Un deuxième conteneur en vif

🌞 Lance un conteneur Python, avec un shell
```
batman@BATPC:~$ docker run -it python bash
Unable to find image 'python:latest' locally
latest: Pulling from library/python
fdf894e782a2: Pull complete 
5bd71677db44: Pull complete 
551df7f94f9c: Pull complete 
ce82e98d553d: Downloading   84.6MB/211.3MB
ce82e98d553d: Downloading [====================>                              ]  88.37MB/211.3MBce82e98d553d: Pull complete 
5f0e19c475d6: Pull complete 
abab87fa45d0: Pull complete 
2ac2596c631f: Pull complete 
Digest: sha256:220d07595f288567bbf07883576f6591dad77d824dce74f0c73850e129fa1f46
Status: Downloaded newer image for python:latest
root@7c7592d01c2d:/# 
```

🌞 Installe des libs Python
```
root@7c7592d01c2d:/# pip install aiohttp aioconsole
Collecting aiohttp
  Downloading aiohttp-3.11.10-cp313-cp313-manylinux_2_17_x86_64.manylinux2014_x86_64.whl.metadata (7.7 kB)
Collecting aioconsole
  Downloading aioconsole-0.8.1-py3-none-any.whl.metadata (46 kB)
Collecting aiohappyeyeballs>=2.3.0 (from aiohttp)
  Downloading aiohappyeyeballs-2.4.4-py3-none-any.whl.metadata (6.1 kB)
Collecting aiosignal>=1.1.2 (from aiohttp)
  Downloading aiosignal-1.3.1-py3-none-any.whl.metadata (4.0 kB)
Collecting attrs>=17.3.0 (from aiohttp)
  Downloading attrs-24.2.0-py3-none-any.whl.metadata (11 kB)
Collecting frozenlist>=1.1.1 (from aiohttp)
  Downloading frozenlist-1.5.0-cp313-cp313-manylinux_2_5_x86_64.manylinux1_x86_64.manylinux_2_17_x86_64.manylinux2014_x86_64.whl.metadata (13 kB)
Collecting multidict<7.0,>=4.5 (from aiohttp)
  Downloading multidict-6.1.0-cp313-cp313-manylinux_2_17_x86_64.manylinux2014_x86_64.whl.metadata (5.0 kB)
Collecting propcache>=0.2.0 (from aiohttp)
  Downloading propcache-0.2.1-cp313-cp313-manylinux_2_17_x86_64.manylinux2014_x86_64.whl.metadata (9.2 kB)
Collecting yarl<2.0,>=1.17.0 (from aiohttp)
  Downloading yarl-1.18.3-cp313-cp313-manylinux_2_17_x86_64.manylinux2014_x86_64.whl.metadata (69 kB)
Collecting idna>=2.0 (from yarl<2.0,>=1.17.0->aiohttp)
  Downloading idna-3.10-py3-none-any.whl.metadata (10 kB)
Downloading aiohttp-3.11.10-cp313-cp313-manylinux_2_17_x86_64.manylinux2014_x86_64.whl (1.7 MB)
   ━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━ 1.7/1.7 MB 2.8 MB/s eta 0:00:00
Downloading aioconsole-0.8.1-py3-none-any.whl (43 kB)
Downloading aiohappyeyeballs-2.4.4-py3-none-any.whl (14 kB)
Downloading aiosignal-1.3.1-py3-none-any.whl (7.6 kB)
Downloading attrs-24.2.0-py3-none-any.whl (63 kB)
Downloading frozenlist-1.5.0-cp313-cp313-manylinux_2_5_x86_64.manylinux1_x86_64.manylinux_2_17_x86_64.manylinux2014_x86_64.whl (267 kB)
Downloading multidict-6.1.0-cp313-cp313-manylinux_2_17_x86_64.manylinux2014_x86_64.whl (131 kB)
Downloading propcache-0.2.1-cp313-cp313-manylinux_2_17_x86_64.manylinux2014_x86_64.whl (227 kB)
Downloading yarl-1.18.3-cp313-cp313-manylinux_2_17_x86_64.manylinux2014_x86_64.whl (339 kB)
Downloading idna-3.10-py3-none-any.whl (70 kB)
Installing collected packages: propcache, multidict, idna, frozenlist, attrs, aiohappyeyeballs, aioconsole, yarl, aiosignal, aiohttp
Successfully installed aioconsole-0.8.1 aiohappyeyeballs-2.4.4 aiohttp-3.11.10 aiosignal-1.3.1 attrs-24.2.0 frozenlist-1.5.0 idna-3.10 multidict-6.1.0 propcache-0.2.1 yarl-1.18.3
WARNING: Running pip as the 'root' user can result in broken permissions and conflicting behaviour with the system package manager, possibly rendering your system unusable.It is recommended to use a virtual environment instead: https://pip.pypa.io/warnings/venv. Use the --root-user-action option if you know what you are doing and want to suppress this warning.
root@7c7592d01c2d:/# python
Python 3.13.1 (main, Dec  4 2024, 20:40:27) [GCC 12.2.0] on linux
Type "help", "copyright", "credits" or "license" for more information.
>>> import aiohttp
>>> import aioconsole
>>> quit
root@7c7592d01c2d:/# ls /
bin  boot  dev	etc  home  lib	lib64  media  mnt  opt	proc  root  run  sbin  srv  sys  tmp  usr  var
```