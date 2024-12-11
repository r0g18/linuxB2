III. Docker compose

🌞 Créez un fichier docker-compose.yml
```
batman@BATPC:~$ sudo mkdir ~/compose_test
batman@BATPC:~$ cd compose_test/
batman@BATPC:~/compose_test$ sudo nano docker-compose.yml
```

🌞 Lancez les deux conteneurs avec docker compose
```
batman@BATPC:~/compose_test$ docker compose up -d
WARN[0000] /home/batman/compose_test/docker-compose.yml: the attribute `version` is obsolete, it will be ignored, please remove it to avoid potential confusion 
[+] Running 3/3
 ✔ Network compose_test_default                  Created                                           0.2s 
 ✔ Container compose_test-conteneur_nul-1        Start...                                          0.5s 
 ✔ Container compose_test-conteneur_flopesque-1  Started                                           0.4s 

```

🌞 Vérifier que les deux conteneurs tournent
```
batman@BATPC:~/compose_test$ docker ps
CONTAINER ID   IMAGE                        COMMAND                  CREATED          STATUS          PORTS                                               NAMES
87f62f5cc275   debian                       "sleep 9999"             5 seconds ago    Up 4 seconds                                                        compose_test-conteneur_flopesque-1
6cc4e2c590ef   debian                       "sleep 9999"             5 seconds ago    Up 4 seconds                                                        compose_test-conteneur_nul-1
d6c1d3cf869a   nginx                        "/docker-entrypoint.…"   26 minutes ago   Up 26 minutes   80/tcp, 0.0.0.0:9999->8080/tcp, :::9999->8080/tcp   stoic_dirac
0a1021fd8ec8   sources-generous-santa-app   "docker-entrypoint.s…"   9 days ago       Up 2 days       0.0.0.0:3000->3000/tcp, :::3000->3000/tcp           sources-generous-santa-app-1
batman@BATPC:~/compose_test$ docker compose ps
WARN[0000] /home/batman/compose_test/docker-compose.yml: the attribute `version` is obsolete, it will be ignored, please remove it to avoid potential confusion 
NAME                                 IMAGE     COMMAND        SERVICE               CREATED          STATUS         PORTS
compose_test-conteneur_flopesque-1   debian    "sleep 9999"   conteneur_flopesque   10 seconds ago   Up 9 seconds   
compose_test-conteneur_nul-1         debian    "sleep 9999"   conteneur_nul         10 seconds ago   Up 9 seconds   
```

🌞 Pop un shell dans le conteneur conteneur_nul
```
batman@BATPC:~/compose_test$ docker exec -it compose_test-conteneur_nul-1 bash
root@6cc4e2c590ef:/# apt update && apt install iputils-ping
root@6cc4e2c590ef:/# ping -c 1 compose_test-conteneur_flopesque-1
PING compose_test-conteneur_flopesque-1 (172.18.0.2) 56(84) bytes of data.
64 bytes from compose_test-conteneur_flopesque-1.compose_test_default (172.18.0.2): icmp_seq=1 ttl=64 time=0.075 ms

--- compose_test-conteneur_flopesque-1 ping statistics ---
1 packets transmitted, 1 received, 0% packet loss, time 0ms
rtt min/avg/max/mdev = 0.075/0.075/0.075/0.000 ms
```