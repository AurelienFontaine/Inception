# Inception - Mini Infrastructure Docker

## Description
Ce projet consiste à mettre en place une mini-infrastructure de différents services en suivant des règles spécifiques. L'intégralité du projet est à réaliser dans une machine virtuelle en utilisant **Docker Compose**. Chaque service disposera de son propre container Docker, construit à partir d'Alpine ou de Debian.

## Prérequis
- Docker
- Docker Compose
- Makefile

## Services
L'infrastructure contiend les services suivants :

- **NGINX** : serveur web avec TLSv1.2 ou TLSv1.3 uniquement.
- **WordPress + PHP-FPM** : installé et configuré sans NGINX.
- **MariaDB** : gestionnaire de base de données.
- **Volumes** :
  - Un volume pour la base de données WordPress.
  - Un volume pour les fichiers du site WordPress.
- **Docker Network** : communication entre les containers.

## Contraintes et Bonnes Pratiques
- Chaque service tourne dans un container distinct avec son propre **Dockerfile**.
- Les images Docker sont construites **localement**.
  <br>
- **Interdictions :**
  - Utilisation de `network: host`, `--link`, `links:`.
  - Démarrage des containers avec une boucle infinie (`tail -f`, `sleep infinity`, `while true`, etc.).
  - Utilisation du tag `latest`.
  - Inclusion de mots de passe en dur dans les Dockerfiles.
  <br>
- Les containers redémarrent automatiquement en cas de crash.
- Utilisation des **variables d'environnement** avec un fichier `.env`.
- Utilisation de **Docker secrets** pour les informations sensibles.

## Déploiement
1. Cloner le dépôt :
   ```sh
   git clone https://github.com/mon-repo/inception.git
   cd inception
   ```
2. Construire et démarrer les containers :
   ```sh
   make up
   ```
3. Arrêter et supprimer les containers :
   ```sh
   make down
   ```

## Configuration du Nom de Domaine
- Notre domaine pointe vers notre **adresse IP locale**.
- Format : `afontain.42.fr`
- La configuration DNS est mise en place pour permettre cette redirection.

## Structure des Volumes
Les volumes sont stockés sous :
```
/home/afontain/data/
   ├── database/      # Base de données WordPress
   ├── wordpress/     # Fichiers du site WordPress
```

## Base de Données WordPress
- Deux utilisateurs doivent être créés.
- L'**Admin** ne doit **pas** avoir un username contenant "admin" (exemples interdits : `admin`, `administrator`, `admin123`).

## Point d'Entrée
- Le container **NGINX** doit être l'unique point d'entrée via le port **443**.
- Seuls **TLSv1.2 et TLSv1.3** sont autorisés.

---




