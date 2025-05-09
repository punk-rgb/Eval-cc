# Eval-cc Baptiste Ramos 

## App 

Exemple de Déploiement Kubernetes : Voting App

Description générale

L'example-voting-app est une application composée de plusieurs microservices qui simule un système de vote en ligne. Le déploiement Kubernetes orchestre tous ces composants pour assurer leur fonctionnement conjoint.

Composants principaux déployés

1. Frontend (Voting-app)
Interface web pour permettre aux utilisateurs de voter.

Déployé en tant que Deployment avec 2 réplicas.

Exposé via un Service de type LoadBalancer (vote-service.yaml).

2. Backend API (Result-app)

Application web pour afficher les résultats en temps réel.
Déployée avec 1 réplicat et exposée comme le frontend.

3. Bases de données

3.1 Redis

Utilisé pour stocker temporairement les votes reçus par le frontend.

Déclaré avec un Deployment et exposé comme ClusterIP (redis-deployment.yaml).

3.2 Postgres

Stockage persistant des résultats.

Déployé avec un PersistentVolumeClaim pour le stockage des données (postgres-deployment.yaml).

Un Secret Kubernetes contient les mots de passe nécessaires.

4. Worker

Service de traitement asynchrone qui déplace les votes de Redis à Postgres.

Déployé comme un Deployment simple.

### Fichiers Kubernetes présents

vote-deployment.yaml et vote-service.yaml : Déploient et exposent le frontend.

result-deployment.yaml et result-service.yaml : Pour les résultats.

redis-deployment.yaml et redis-service.yaml

postgres-deployment.yaml, postgres-service.yaml, postgres-pvc.yaml, postgres-secret.yaml

worker-deployment.yaml

(optionnel) : deployment.yaml qui peut servir à déployer l’ensemble des composants avec un seul fichier.

#### Fonctionnement global
L’utilisateur accède au frontend via un service LoadBalancer.

Les votes sont enregistrés dans Redis par le frontend.

Le worker lit les votes de Redis et les stocke dans Postgres.

Le result-app lit les résultats de Postgres et l'affiche en temps réel sur une page web.

Points clés du déploiement

Isolation de chaque composant dans un Pod dédié.

Scalabilité : la couche frontend est répliquée (2 pods par défaut) pour gérer plus d'utilisateurs.

Stockage persistant pour la base de données via un PersistentVolumeClaim.

Sécurité : utilisation de Secrets pour les mots de passe Postgres.

Interopérabilité : les services communiquent par des noms DNS internes Kubernetes.

## Command

git clone https://github.com/dockersamples/example-voting-app.git

cd example-voting-app

sudo kubectl apply -f k8s-specifications/

sudo kubectl get pods

sudo kubectl get services

Les pods doivent être en status running

## Capture 

![Result app](result.png)

![Vote app](vote.png)

## Schéma infra

![infra](graph.png)