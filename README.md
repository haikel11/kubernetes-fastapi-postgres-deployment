# Déploiement d'une application FastAPI avec PostgreSQL sur Kubernetes

## Présentation

Ce projet consiste à déployer une application microservices dans un environnement Kubernetes.
L’architecture repose sur deux composants principaux :

* Une application backend développée avec **FastAPI**
* Une base de données **PostgreSQL**

L’objectif est de mettre en place un déploiement scalable, persistant et accessible via un Ingress.

---

## Architecture

### Application FastAPI

* Déployée via un **Deployment**
* Configurée avec **3 replicas**
* Exposée via un **Service**
* Accessible via un **Ingress**

### Base de données PostgreSQL

* Déployée via un **StatefulSet**
* Utilise un **PersistentVolumeClaim (PVC)** de 10Gi
* Configuration via **ConfigMap** et **Secret**
* Accessible uniquement en interne via un Service nommé `db`

---

## Structure du projet

```bash
.
├── config/
├── models/
├── routes/
├── schemas/
│
├── app.py
├── requirements.txt
├── Dockerfile
├── docker-compose.yml
├── README.md
├── .gitignore
│
├── YAML-STANDARD/
│   ├── fastapi-deployment.yaml
│   ├── fastapi-service.yaml
│   ├── fastapi-ingress.yaml
│   ├── postgres-statefulset.yaml
│   ├── postgres-service.yaml
│   ├── postgres-pvc.yaml
│   ├── postgres-configmap.yaml
│   ├── postgres-secret.yaml
│
├── logs/
│   ├── fastapi.log
│   └── postgres.log
│
├── backup/
│   └── etcd-snapshot.db
```

---

## Configuration

L’application FastAPI se connecte à la base de données PostgreSQL avec l’URL suivante :

```bash
postgresql://admin:password@db:5432/storedb
```

* `db` correspond au nom du Service Kubernetes exposant PostgreSQL

---

## Déploiement

### 1. Construction de l’image Docker

```bash
docker build -t fastapi-app .
```

---

### 2. Déploiement sur Kubernetes

```bash
kubectl apply -f YAML-STANDARD/
```

---

## Accès à l’application

L’application est accessible via l’Ingress configuré avec un sous-domaine.

Endpoints disponibles :

* `/docs` : interface Swagger
* `/users` : liste des utilisateurs
* `/users/count` : nombre total d’utilisateurs

---

## Persistance des données

* Volume de stockage : **10Gi**
* Gestion via **PersistentVolumeClaim**
* Compatible avec plusieurs pods (selon StorageClass)

---

## Sécurité et configuration

* **Secrets** : stockage des données sensibles (mot de passe)
* **ConfigMap** : configuration non sensible

---

## Logs et sauvegarde

* `logs/fastapi.log` : logs de l’application
* `logs/postgres.log` : logs de la base de données
* `backup/etcd-snapshot.db` : sauvegarde ETCD du cluster

---

## Remarques

* Seul le service FastAPI est exposé via Ingress
* PostgreSQL est accessible uniquement en interne
* Les fichiers Kubernetes sont fournis dans le dossier `YAML-STANDARD` 