# MicroCRM - Déploiement Docker

Ce répertoire contient la configuration Docker Compose pour orchestrer le déploiement de l'application MicroCRM.

## Prérequis

- Docker Desktop (Windows/Mac) ou Docker Engine (Linux)
- Docker Compose

## Lancement de l'application

Pour démarrer l'ensemble des services (Frontend et Backend) :

```bash
docker-compose up --build -d
```

## Accès

- Frontend : `http://localhost:80`
- Backend : `http://localhost:8080`

## Arrêt

```bash
docker-compose down
```
