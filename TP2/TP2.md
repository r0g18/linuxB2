TP2 : Utilisation courante de Docker

Sommaire :
    1. Commun à tous : App Web
    2. Dév Python
    3. Admin Maitrise de la stack web
    4. Secu Big Brain


I. Commun à tous : App Web

🌞 docker-compose.yml
```
batman@BATPC:~/ab$ docker compose up
WARN[0000] /home/batman/ab/docker-compose.yml: the attribute `version` is obsolete, it will be ignored, please remove it to avoid potential confusion 
[+] Running 11/11
 ✔ mysql Pulled                                                                                   46.0s 
   ✔ 2c0a233485c3 Already exists                                                                   0.0s 
   ✔ cb5a6a8519b2 Pull complete                                                                    0.7s 
   ✔ 570d30cf82c5 Pull complete                                                                    1.1s 
   ✔ a841bff36f3c Pull complete                                                                    2.9s 
   ✔ 80ba30c57782 Pull complete                                                                    2.9s 
   ✔ 5e49e1f26961 Pull complete                                                                    2.9s 
   ✔ ced670fc7f1c Pull complete                                                                   41.6s 
   ✔ 0b9dc7ad7f03 Pull complete                                                                   41.6s 
   ✔ cd0d5df9937b Pull complete                                                                   44.3s 
   ✔ 1f87d67b89c6 Pull complete                                                                   44.4s 
[+] Building 58.9s (7/7) FINISHED                                                        docker:default
 => [php_apache internal] load build definition from Dockerfile                                    0.0s
 => => transferring dockerfile: 138B                                                               0.0s
 => [php_apache internal] load .dockerignore                                                       0.0s
 => => transferring context: 2B                                                                    0.0s
 => [php_apache internal] load metadata for docker.io/library/php:apache                           1.5s
 => [php_apache 1/3] FROM docker.io/library/php:apache@sha256:204de2d31416e176774b98217beb8e078a  45.4s
 => => resolve docker.io/library/php:apache@sha256:204de2d31416e176774b98217beb8e078a9f3b55306b37  0.0s
 => => sha256:f71a1520fc745a27bf88d5c4814cdeea12b5c54f1e8301f575c9e5b876ee435a 11.29kB / 11.29kB   0.0s
 => => sha256:66683b96da061ad6997de6ec89d6787aac067e47886baac132cf55116e24fd2d 226B / 226B         0.2s
 => => sha256:204de2d31416e176774b98217beb8e078a9f3b55306b370d6a9f1f470af5f306 10.39kB / 10.39kB   0.0s
 => => sha256:ef88b1fd967c1fc0dfaa07e95f9b198bd26e94c479faf4a792fa18170e9ad1ec 225B / 225B         0.2s
 => => sha256:a000f6841443a8d64b4c28e1e59e0515c2cd308a5501f4f6b76af3815e289eee 3.64kB / 3.64kB     0.0s
 => => sha256:8a64a27210ceb93ea27ca865c5cf246274ba43f2d36bc96e7b2271b0e68cd3 104.15MB / 104.15MB  41.1s
 => => extracting sha256:ef88b1fd967c1fc0dfaa07e95f9b198bd26e94c479faf4a792fa18170e9ad1ec          0.0s
 => => sha256:32504ad84c84b9d8a285eedae60221bad3636904e89d355d80e6b9a29b5641b1 428B / 428B         0.5s
 => => sha256:e8ccdf33c0e695a7e87b0949151b6cd62dd1cb0fe91412cb830903b883d90bfd 20.12MB / 20.12MB  20.0s
 => => sha256:24161a143162edea1e1b312408e3e20008a5d852b66a08f592e92ffd90ed2430 481B / 481B         0.9s
 => => sha256:ce8af0c154015aa991a0d644f31f886970622b282e5d1b25c5e8c0d4ba354b6a 13.68MB / 13.68MB  11.9s
 => => sha256:e98f4ad93eaafd9096aab0c60906c74056357330c3385269ddd37eef7f7ce246 486B / 486B        12.1s
 => => sha256:eccc14a9d6abd4cf0ec580c259d8d6ab4a057372ba4a3cf10226b39d72693e41 14.13MB / 14.13MB  20.3s
 => => sha256:0b620e0f550374c5f95e4aa77b11438799d92a64f46fa76aea1c42df2de098c9 2.46kB / 2.46kB    20.7s
 => => sha256:f59b0be7e67b9f92c4b41ac5f3553248f219fa604c0f662082eadd7bb85a747d 244B / 244B        20.8s
 => => sha256:282d0b5a1397e02d10cb1676b4a00deb79f2e06df9e9b03af23983e81adf068d 891B / 891B        20.8s
 => => sha256:4f4fb700ef54461cfa02571ae0db9a0dc1e0cdb5577484a6d75e68dc38e8acc1 32B / 32B          20.9s
 => => extracting sha256:8a64a27210ceb93ea27ca865c5cf246274ba43f2d36bc96e7b2271b0e68cd3e3          2.7s
 => => extracting sha256:66683b96da061ad6997de6ec89d6787aac067e47886baac132cf55116e24fd2d          0.0s
 => => extracting sha256:e8ccdf33c0e695a7e87b0949151b6cd62dd1cb0fe91412cb830903b883d90bfd          0.5s
 => => extracting sha256:32504ad84c84b9d8a285eedae60221bad3636904e89d355d80e6b9a29b5641b1          0.0s
 => => extracting sha256:24161a143162edea1e1b312408e3e20008a5d852b66a08f592e92ffd90ed2430          0.0s
 => => extracting sha256:ce8af0c154015aa991a0d644f31f886970622b282e5d1b25c5e8c0d4ba354b6a          0.1s
 => => extracting sha256:e98f4ad93eaafd9096aab0c60906c74056357330c3385269ddd37eef7f7ce246          0.0s
 => => extracting sha256:eccc14a9d6abd4cf0ec580c259d8d6ab4a057372ba4a3cf10226b39d72693e41          0.4s
 => => extracting sha256:0b620e0f550374c5f95e4aa77b11438799d92a64f46fa76aea1c42df2de098c9          0.0s
 => => extracting sha256:f59b0be7e67b9f92c4b41ac5f3553248f219fa604c0f662082eadd7bb85a747d          0.0s
 => => extracting sha256:282d0b5a1397e02d10cb1676b4a00deb79f2e06df9e9b03af23983e81adf068d          0.0s
 => => extracting sha256:4f4fb700ef54461cfa02571ae0db9a0dc1e0cdb5577484a6d75e68dc38e8acc1          0.0s
 => [php_apache 2/3] RUN apt-get update && apt-get upgrade -y                                      4.4s
 => [php_apache 3/3] RUN docker-php-ext-install mysqli                                             7.4s 
 => [php_apache] exporting to image                                                                0.1s 
 => => exporting layers                                                                            0.1s 
 => => writing image sha256:c61b388ab62ea76a083edd846f1ea18856b7f0d4f9486bb2e538363323d8a79c       0.0s 
 => => naming to docker.io/library/ab-php_apache                                                   0.0s 
[+] Running 4/1                                                                                         
 ✔ Network ab_default         Created                                                              0.2s 
 ✔ Container ab-phpmyadmin-1  Created                                                              0.0s 
 ✔ Container ab-mysql-1       Created                                                              0.0s 
 ✔ Container ab-php_apache-1  Created                                                              0.0s 
Attaching to mysql-1, php_apache-1, phpmyadmin-1
mysql-1       | 2024-12-13 11:51:16+00:00 [Note] [Entrypoint]: Entrypoint script for MySQL Server 9.1.0-1.el9 started.
phpmyadmin-1  | AH00558: apache2: Could not reliably determine the server's fully qualified domain name, using 172.24.0.3. Set the 'ServerName' directive globally to suppress this message
phpmyadmin-1  | AH00558: apache2: Could not reliably determine the server's fully qualified domain name, using 172.24.0.3. Set the 'ServerName' directive globally to suppress this message
phpmyadmin-1  | [Fri Dec 13 11:51:16.552942 2024] [mpm_prefork:notice] [pid 1:tid 1] AH00163: Apache/2.4.62 (Debian) PHP/8.2.26 configured -- resuming normal operations
phpmyadmin-1  | [Fri Dec 13 11:51:16.552963 2024] [core:notice] [pid 1:tid 1] AH00094: Command line: 'apache2 -D FOREGROUND'
mysql-1       | 2024-12-13 11:51:16+00:00 [Note] [Entrypoint]: Switching to dedicated user 'mysql'
mysql-1       | 2024-12-13 11:51:16+00:00 [Note] [Entrypoint]: Entrypoint script for MySQL Server 9.1.0-1.el9 started.
mysql-1       | 2024-12-13 11:51:16+00:00 [Note] [Entrypoint]: Initializing database files
mysql-1       | 2024-12-13T11:51:16.776001Z 0 [System] [MY-015017] [Server] MySQL Server Initialization - start.
mysql-1       | 2024-12-13T11:51:16.777142Z 0 [System] [MY-013169] [Server] /usr/sbin/mysqld (mysqld 9.1.0) initializing of server in progress as process 80
Gracefully stopping... (press Ctrl+C again to force)
Error response from daemon: driver failed programming external connectivity on endpoint ab-php_apache-1 (b7a47feb4a5a5c8942a49409afc5434933664ac1c6ce196d981c2cebb40853ce): Error starting userland proxy: listen tcp4 0.0.0.0:80: bind: address already in use

```
II Dév. Python


II Admin. Maîtrise de la stack Web


A. Good practices

🌞 Limiter l'accès aux ressources
```

```
🌞 No root
```

```

🌞 Gestion des droits du volume qui contient le code
```

```

🌞 Gestion des capabilities sur le conteneur NGINX
```

```

🌞 Mode read-only
```

```

⭐ BONUS :
```

```

B. Reverse proxy buddy

1. Simple HTTP setup

🌞 Adaptez le docker-compose.yml de la partie précédente
```

```

2. HTTPS auto-signé

🌞 HTTPS auto-signé
```

```

3. HTTPS avec une CA maison

🌞 Générer une clé et un certificat de CA
```

```

🌞 Générer une clé et une demande de signature de certificat pour notre serveur web
```

```

🌞 Faire signer notre certificat par la clé de la CA
```

```

🌞 Ajustez la configuration NGINX
```

```

🌞 Prouvez avec un curl que vous accédez au site web
```

```

🌞 Ajouter le certificat de la CA dans votre navigateur
```

```
II Secu. Big brain

