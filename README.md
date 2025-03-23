# Mini-Projet-Docker : Conteneurisation d'une Application avec Docker

## Auteurs
- Aboutofil Salma
- Dellahi Hiba
- Ibourk Khadija

## Description du Projet
Ce projet consiste à conteneuriser une application Python avec Flask en utilisant Docker. L'application est composée de deux modules :
1. Une API REST en Python qui fournit la liste des étudiants.
2. Une interface web en PHP pour afficher la liste des étudiants.


## I. Construire et tester l'API
### 1.	Création du Dockerfile :
- 	Nous avons créé un fichier Dockerfile pour construire l'image de l'API en utilisant l'image de base python:3.8-buster.
-	Nous avons ajouté les informations du mainteneur (nom, prénom et email) dans le Dockerfile.
-	Le code source de l'API (student_age.py) a été copié dans le conteneur à la racine.
-	Nous avons installé les dépendances nécessaires en utilisant le fichier requirements.txt avec la commande pip3 install -r /requirements.txt.
-	Un volume a été créé pour stocker les données persistantes (le fichier student_age.json).
-	Le port 5000 a été exposé pour permettre à l'API de communiquer avec l'extérieur.
- La commande de démarrage CMD ["python3", "./student_age.py"] a été définie pour exécuter l'API au lancement du conteneur.

**Capture d'écran :**

![alt text](screenshots/image.png)
### 2.	Construction et test de l'image :
-	Nous avons construit l'image avec la commande docker build -t api-image 

![Image](screenshots/image%20copy.png)


![Image](screenshots/image%20copy%202.png)

- Ensuite, nous avons lancé le conteneur en montant le fichier student_age.json dans le dossier /data du conteneur.

![Image](screenshots/image%20copy%203.png)

-	Pour vérifier que l'API fonctionne correctement, nous avons utilisé la commande curl -u root:root -X GET http://<host IP>:<API exposed port>/SUPMIT/api/v1.0/get_student_ages.
 La réponse de l'API a confirmé que tout fonctionnait correctement.
 ![Image](screenshots/image%20copy%204.png)

## II. Infrastructure as Code (docker-compose)
### 1.	Création du fichier docker-compose.yml :

- 	Nous avons créé un fichier docker-compose.yml pour déployer l'application en utilisant deux services : website (interface utilisateur en PHP) et api (l'API en Python).
-	Pour le service website, nous avons utilisé l'image php:apache et configuré les variables d'environnement pour l'authentification. Nous avons également monté le dossier ./website dans /var/www/html pour servir le site web.
-	Pour le service api, nous avons utilisé l'image construite précédemment et monté le fichier student_age.json dans /data/student_age.json.
-	Nous avons configuré les ports et ajouté un réseau spécifique pour permettre la communication entre les services.

 ![Image](screenshots/image%20copy%205.png)
### 2.	Lancement de l'application :
-	Nous avons lancé l'application avec la commande docker-compose up -d
![Image](screenshots/image%20copy%206.png)

-	Après le démarrage des conteneurs, nous avons accédé à l'interface web et cliqué sur le bouton "List Student" pour afficher la liste des étudiants.
![Image](screenshots/image%20copy%207.png)

- La liste a été récupérée avec succès depuis l'API, confirmant que l'application fonctionne correctement.
![Image](screenshots/image%20copy%208.png)
## III. Docker Registry
### 1.	Déploiement d'un registre Docker privé :
-	Nous avons déployé un registre Docker privé en utilisant l'image registry et une interface web pour visualiser les images avec l'image joxit/docker-registry-ui.
-	Nous avons configuré le fichier docker-compose-registry.yml pour lancer le registre et l'interface web.

![Image](screenshots/image%20copy%209.png)

![Image](screenshots/image%20copy%2010.png)
-	 Après avoir lancé le registre, nous avons poussé l'image de l'API sur le registre privé en utilisant la commande docker tag et docker push.
![Image](screenshots/image%20copy%2011.png)

### 2.	Vérification dans l'interface web :
-	Nous avons accédé à l'interface web du registre pour vérifier que l'image de l'API a bien été poussée et est disponible dans le registre privé.
![Image](screenshots/image%20copy%2012.png)


## Fichiers de Configuration
- `Dockerfile` : Fichier de configuration pour construire l'image de l'API.
- `docker-compose.yml` : Fichier pour déployer l'application avec Docker Compose.
- `docker-compose-registry.yml` : Fichier pour déployer le registre Docker privé.

## Comment exécuter le projet
1. Clonez ce dépôt :
   ```bash
   git clone https://gitlab.com/tp19342208/mini-projet-docker.git