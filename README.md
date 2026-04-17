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

## Observabilité & Métriques DORA

Le projet intègre aussi une pile ELK (Elasticsearch, Logstash, Kibana) pour centraliser les logs applicatifs et suivre les métriques **DORA** (Deployment Frequency, Lead Time for Changes, etc.) en interrogeant l'API GitHub Actions.

### Configuration des Métriques DORA

Pour que Logstash puisse récupérer les données de vos workflows GitHub (`p9_microcrm_front` et `p9_microcrm_back`), suivez ces étapes :

1.  **Générer un Personal Access Token (PAT) GitHub** :
    *   Allez dans vos paramètres GitHub : **Settings > Developer settings > Personal access tokens > Tokens (classic)**.
    *   Créez un nouveau jeton (`Generate new token`).
    *   Sélectionnez le scope **`repo`** (nécessaire pour lire l'historique des workflows).
    *   Copiez le jeton généré.

2.  **Configurer la variable d'environnement** :
    *   Créez un fichier `.env` à la racine du projet (s'il n'existe pas).
    *   Ajoutez votre jeton : `GITHUB_TOKEN=votre_jeton_ici`.

3.  **Lancer la stack** :
    ```bash
    docker-compose up -d
    ```

### Visualisation dans Kibana

1.  **Logs applicatifs** : Créez une Data View avec le pattern `microcrm-logs-*`.
2.  **Métriques DORA** : Créez une Data View avec le pattern `dora-metrics-*` et utilisez le champ `created_at` comme timestamp.
3.  **Indicateurs DORA suggérés** :
    *   **Deployment Frequency** : Nombre de documents avec `conclusion: success`.
    *   **Lead Time for Changes** : Moyenne du champ `duration_seconds` pour les succès.
    *   **Change Failure Rate** : Ratio des documents avec `conclusion: failure` par rapport au total.