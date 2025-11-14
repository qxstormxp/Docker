# Projet Docker - Architecture Réseau (TP-Networks)

Ce projet met en place une infrastructure web multi-conteneurs en utilisant Docker et Docker Compose. L'objectif principal est de maîtriser les concepts de réseaux Docker en isolant les services.

L'infrastructure est composée de trois services :
* **`proxy`**: Un serveur Nginx agissant comme reverse proxy. C'est le seul point d'entrée de l'application.
* **`app`**: Une application web "Hello World" en Python (Flask) qui se connecte à la base de données.
* **`db`**: Une base de données MariaDB pour stocker les données de l'application.
  
---

## Architecture et Réseaux

Le cœur de ce projet est la séparation des services sur deux réseaux distincts pour des raisons de sécurité et d'isolation :

* **`frontend_net`**: Un réseau "public". Seul le proxy y est connecté pour recevoir le trafic entrant de la machine hôte.
* **`backend_net`**: Un réseau "privé" et isolé. Les services `app` et `db` communiquent exclusivement sur ce réseau. Il n'est pas accessible depuis l'extérieur.

### Schéma de l'architecture

```
             Utilisateur (via 10.5.0.40:8080)
                     |
[HÔTE]               |
---------------------|----------------------
                     |
            [frontend_net]
                     |
             +-------------+
             |   proxy     |  (Service Nginx)
             +-------------+
             |      |
             +------+-----------------------
                    |
            [backend_net] (Réseau isolé)
                    |
      +-------------+-------------+
      |                           |
+-------------+             +-------------+
|    app      |  <------->  |     db      | (Service Flask)           (Service MariaDB)
+-------------+             +-------------+
```
---
## Projet

### Commande & Prompts

1.  Clonez ce dépôt sur votre machine :
    ```CMD
    git clone [https://github.com/qxstormxp/Docker.git](https://github.com/qxstormxp/Docker.git)
    cd Docker
    ```
2.  Construisez et lancez les conteneurs en mode détaché :
    ```CMD
    docker-compose up -d
    ```
    * `-d` : Lance les conteneurs en arrière-plan (detached).

3.  Vérifiez que tout fonctionne :
    Ouvrez votre navigateur et visitez **`http://10.5.0.40:8080`**.
    Vous devriez voir un message de l'application Flask confirmant la connexion à la base de données.

4.   Pour la partie connexion à github: j'ai eu quelques soucis car je pensais que l'on pouvait mettre directement son nom et son mot de passe sauf        qu'il faut mettre son nom mais à la place du mot de passe, il faut mettre un token générer dans github mais il ne faut pas oublié de mettre les       droits sur le token car sinon ça ne marchera pas.

5.   Pour la partie connexion à DockerHUB: j'ai eu le soucis d'essayé de push alors que le repo n'avais pas le meme nom, bien vérifier que les noms        soit similaire et ne pas oublié la commande docker login est bien se connecter.

6.   Pour la partie Connexion à la Base de données: j'ai eu un soucis car je n'avais pas mis les memes identifiants, dans le fichier docker-               compose.yml
    
##Structure du Projet
```
/
├── app/
│   ├── Dockerfile         # Instructions pour construire l'image Flask
│   ├── requirements.txt   # Dépendances Python (Flask, pymysql)
│   └── app.py             # Le code source de l'application
│
├── proxy/
│   ├── Dockerfile         # Instructions pour construire l'image Nginx
│   └── nginx.conf         # Fichier de configuration du reverse proxy
│
└── docker-compose.yml     # Fichier d'orchestration de tous les services
```

## Image Docker Hub

L'image de l'application Flask construite par ce projet est disponible publiquement sur Docker Hub :

* **Lien :** (https://hub.docker.com/repository/docker/gauvin/tp-networks-app/general)
