TP2 admins : PHP stack

Sommaire

- [TP2 admins : PHP stack](#tp2-admins--php-stack)
  - [Sommaire](#sommaire)
- [I. Good practices](#i-good-practices)
- [II. Reverse proxy buddy](#ii-reverse-proxy-buddy)
  - [A. Simple HTTP setup](#a-simple-http-setup)
  - [B. HTTPS auto-signé](#b-https-auto-signé)
  - [C. HTTPS avec une CA maison](#c-https-avec-une-ca-maison)

I. Good practices

🌞 Limiter l'accès aux ressources

```
$ docker run --memory 1g --cpus 1.0 alpine echo "ha"
ha


$ cat docker-compose.yml 
version: '3'

services:
  limitedbruh:
    image: alpine 
    deploy:
      resources:
        limits:
          cpus: '1.0'
          memory: 1G
    entrypoint: echo haha

$ docker compose up -d && docker compose logs
[+] Running 2/2
 ✔ Network m4ul_default          Created                                                        0.1s 
 ✔ Container m4ul-limitedbruh-1  Started                                                        0.0s 
m4ul-limitedbruh-1  | haha
```

🌞 No root

```
$ docker run --user 420 -it alpine whoami
whoami: unknown uid 420

$ cat docker-compose.yml 
version: '3'

services:
  ratio:
    image: alpine
    user: "420"
    entrypoint: whoami

$ docker compose up -d && docker compose logs
[+] Running 1/1
 ✔ Container m4ul-ratio-1  Started                                                              0.0s 
m4ul-ratio-1  | whoami: unknown uid 420
```

II. Reverse proxy buddy

On continue sur le sujet PHP !

On va ajouter un reverse proxy dans le mix !

A. Simple HTTP setup

🌞 Adaptez le docker-compose.yml 

[Dossier reverse proxy](http/)

B. HTTPS auto-signé

🌞 HTTPS auto-signé

[Dossier https auto signé](https_auto/)

C. HTTPS avec une CA maison

> **Vous pouvez jeter la clé et le certificat de la partie précédente :D**

On va commencer par générer la clé et le certificat de notre Autorité de Certification (CA). Une fois fait, on pourra s'en servir pour signer d'autres certificats, comme celui de notre serveur web.

Pour que la connexion soit trusted, il suffira alors d'ajouter le certificat de notre CA au magasin de certificats de votre navigateur sur votre PC.

Il vous faudra un shell bash et des commandes usuelles sous la main pour réaliser les opérations. Lancez une VM, ou ptet Git Bash, ou ptet un conteneur debian oneshot ?

🌞 Générer une clé et un certificat de CA

```bash
# mettez des infos dans le prompt, peu importe si c'est fake
# on va vous demander un mot de passe pour chiffrer la clé aussi
$ openssl genrsa -des3 -out CA.key 4096
$ openssl req -x509 -new -nodes -key CA.key -sha256 -days 1024  -out CA.pem
$ ls
# le pem c'est le certificat (clé publique)
# le key c'est la clé privée
```

Il est temps de générer une clé et un certificat que notre serveur web pourra utiliser afin de proposer une connexion HTTPS.

🌞 Générer une clé et une demande de signature de certificat pour notre serveur web

```bash
$ openssl req -new -nodes -out www.supersite.com.csr -newkey rsa:4096 -keyout www.supersite.com.key
$ ls
# www.supersite.com.csr c'est la demande de signature
# www.supersite.com.key c'est la clé qu'utilisera le serveur web
```

🌞 Faire signer notre certificat par la clé de la CA

- préparez un fichier `v3.ext` qui contient :

```ext
authorityKeyIdentifier=keyid,issuer
basicConstraints=CA:FALSE
keyUsage = digitalSignature, nonRepudiation, keyEncipherment, dataEncipherment
subjectAltName = @alt_names

[alt_names]
DNS.1 = www.supersite.com
DNS.2 = www.tp2.admins
```

- effectuer la demande de signature pour récup un certificat signé par votre CA :

```bash
$ openssl x509 -req -in www.supersite.com.csr -CA CA.pem -CAkey CA.key -CAcreateserial -out www.supersite.com.crt -days 500 -sha256 -extfile v3.ext
$ ls
# www.supersite.com.crt c'est le certificat qu'utilisera le serveur web
```

🌞 Ajustez la configuration NGINX

- le site web doit être disponible en HTTPS en utilisant votre clé et votre certificat
- une conf minimale ressemble à ça :

```nginx
server {
    [...]
    # faut changer le listen
    listen 10.7.1.103:443 ssl;

    # et ajouter ces deux lignes
    ssl_certificate /chemin/vers/le/cert/www.supersite.com.crt;
    ssl_certificate_key /chemin/vers/la/clé/www.supersite.com.key;
    [...]
}
```

🌞 Prouvez avec un `curl` que vous accédez au site web

- depuis votre PC
- avec un `curl -k` car il ne reconnaît pas le certificat là

🌞 Ajouter le certificat de la CA dans votre navigateur

- vous pourrez ensuite visitez `https://www.supersite.com` sans alerte de sécurité, et avec un cadenas vert
- il est nécessaire de joindre le site avec son nom pour que HTTPS fonctionne (fichier `hosts`)

> *En entreprise, c'est comme ça qu'on fait pour qu'un certificat de CA non-public soit trusted par tout le monde : on dépose le certificat de CA dans le navigateur (et l'OS) de tous les PCs. Evidemment, on utilise une technique de déploiement automatisé aussi dans la vraie vie, on l'ajoute pas à la main partout hehe.*
