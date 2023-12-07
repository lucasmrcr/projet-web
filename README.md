# Lucas Mercier

# Lancement de l'application
Un fichier docker-compose est présent pour gérer le démarrage de l'application si besoin de l'intégrer dans un environnement.

`docker comppse up` pour lancer l'application.
`docker compose pull` pour mettre à jour les images.

# CI
Les actions sont présentes dans le dossier `.github/actions`, elles sont utilisées dans les workflows présents dans le dossier `.github/workflows`.
Deux workflows sont présents :
- `develop.yaml` : Déclenché à chaque push sur la branche develop, il va lancer les tests unitaires et vérifier que le build se fait correctement.
- `release.yaml` : Déclenché à chaque création de tag sur marster, il va lancer les tests unitaires et faire un build. Ce build sera compressé et envoyé sur le serveur distant.

L'image docker est push sur le github container registry à chaque release. Pour récupérer la dernière version il suffit de pull l'image `ghcr.io/lucasmrcr/projet-web:latest`.

# Description workflows
## develop.yaml
- Déclenché à chaque push sur la branche develop
- Lance les tests unitaires
- Vérifie que le build se fait correctement

## release.yaml
- Déclenché à chaque création de tag sous format `[0-9]+.[0-9]+.[0-9]+` sur master
- Clone le projet
- Télecharge les dépendances du projet
- Lance les tests unitaires
- Construit une image docker nommé `ghcr.io/lucasmrcr/projet-web:latest` ou `ghcr.io/lucasmrcr/projet-web:version` et la push sur le github container registry
- Envoie la dernière version du docker-compose sur le serveur distant
- Build le projet en version static
- Compress le build en tar.gz
- Envoie le tar.gz sur le serveur distant