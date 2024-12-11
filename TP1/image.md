II. Images

Sommaire : 
    1. Images publiques
    2. Construire une image

1. Images publiques

🌞 Récupérez des images
```
batman@BATPC:~$ docker pull python:3.11
3.11: Pulling from library/python
fdf894e782a2: Already exists 
5bd71677db44: Already exists 
551df7f94f9c: Already exists 
ce82e98d553d: Already exists 
2371bf9a39a3: Pull complete 
0b3239f18dfa: Pull complete 
de07a735a679: Pull complete 
Digest: sha256:2c80c66d876952e04fa74113864903198b7cfb36b839acb7a8fef82e94ed067c
Status: Downloaded newer image for python:3.11
docker.io/library/python:3.11
batman@BATPC:~$ docker images
REPOSITORY                   TAG          IMAGE ID       CREATED       SIZE
day4-chall                   latest       678c761a73a9   5 days ago    1.65GB
python                       latest       3ca4060004b1   7 days ago    1.02GB
python                       3.11         342f2c43d207   7 days ago    1.01GB
sources-generous-santa-app   latest       da09e158cc12   9 days ago    172MB
nginx                        latest       66f8bdd3810c   2 weeks ago   192MB
node                         18-alpine    870e987bd793   3 weeks ago   127MB
nwodtuhs/exegol              full         403a41426b11   7 weeks ago   60.2GB
nwodtuhs/exegol              full-3.1.5   403a41426b11   7 weeks ago   60.2GB
```

🌞 Lancez un conteneur à partir de l'image Python
```
batman@BATPC:~$ docker run -it python:3.11 bash
root@04890bf40fc3:/# 
```


2. Construire une image

🌞 Ecrire un Dockerfile pour une image qui héberge une application Python
```
batman@BATPC:~$ sudo mkdir ~/python_app_build
batman@BATPC:~$ echo 'import emoji\nprint(emoji.emojize("Cet exemple d\'application est vraiment naze :thumbs_down:"))' > ~/python_app_build/app.py
batman@BATPC:~$ sudo nano ~/python_app_build/Dockerfile
batman@BATPC:~$ cd ~/python_app_build
batman@BATPC:~/python_app_build$
```

🌞 Build l'image
```
batman@BATPC:~/python_app_build$ docker build . -t python_app:version_de_ouf
Sending build context to Docker daemon  3.072kB
Step 1/6 : FROM debian:latest
 ---> ff869c3288a4
Step 2/6 : RUN apt update && apt install -y python3 python3-pip python3-venv
 ---> Using cache
 ---> f24d3711a648
Step 3/6 : RUN python3 -m venv /venv
 ---> Using cache
 ---> 5dcac55f29bc
Step 4/6 : RUN /venv/bin/pip install emoji
 ---> Using cache
 ---> 76a686b74955
Step 5/6 : COPY app.py /app.py
 ---> 9cfae5aa6575
Step 6/6 : ENTRYPOINT ["/venv/bin/python", "/app.py"]
 ---> Running in 902c1c379298
Removing intermediate container 902c1c379298
 ---> aea68a4680b5
Successfully built aea68a4680b5
Successfully tagged python_app:version_de_ouf
```

🌞 Lancer l'image
```
batman@BATPC:~/python_app_build$ docker run python_app:version_de_ouf
Cet exemple d'application est vraiment naze 👎
```
