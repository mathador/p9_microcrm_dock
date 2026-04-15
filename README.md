# MicroCRM - Déploiement Docker

Ce répertoire contient la configuration Docker Compose pour orchestrer le déploiement de l'application MicroCRM.
La stack inclut maintenant le Frontend, le Backend et l'observabilité avec ELK (Elasticsearch, Logstash, Kibana).

## Prérequis

- Docker Desktop (Windows/Mac) ou Docker Engine (Linux)
- Docker Compose

## Lancement de l'application

Pour démarrer l'ensemble des services (Frontend, Backend, Elasticsearch, Logstash et Kibana) :

```bash
docker-compose up
```
ou
```bash
docker-compose up --build
```
ou
```bash
docker-compose up --build -d
```

## Arrêt

```bash
docker compose down
```

## Accès

- Frontend : `http://localhost:80`
- Backend : `http://localhost:8080`
- Elasticsearch : `http://localhost:9200`
- Kibana : `http://localhost:5601`
- Logstash monitoring API : `http://localhost:9600`

## Vérification des logs dans Kibana

1. Ouvrir `http://localhost:5601`.
2. Aller dans **Stack Management > Data Views**.
3. Créer un Data View avec le pattern `microcrm-logs-*`.
4. Aller dans **Discover** pour visualiser les logs applicatifs.


Les logs des conteneurs `front` et `back` sont envoyés à Logstash via GELF, puis indexés dans Elasticsearch sous la forme `microcrm-logs-YYYY.MM.dd`.
**Dashboards :** La visualisation des données repose sur une **Data View** nommée `microcrm-logs`, configurée avec le pattern `microcrm-logs-*` et le champ temporel `@timestamp`. Cette Data View est le socle commun pour les dashboards "DORA Metrics" et "CI/CD Performance".