# EDEY-Komi-Jonas
Devoir final AWS - IABD

# Projet AWS - Intégration S3, Lambda, DynamoDB et EC2

Ce projet a été réalisé dans le cadre de notre devoir, en utilisant AWS CloudFormation.  
L’objectif est de mettre en place un mini système de traitement qui simule le traitement automatique de fichiers image `.jpg` envoyés dans un bucket S3. Lorsqu’un fichier est ajouté, une fonction Lambda est déclenchée pour enregistrer ses métadonnées dans une table DynamoDB. Une instance EC2 agit comme simulateur client pour vérifier les données dans la table.

---

## Technologies et ressources utilisées

- **AWS S3** : pour stocker les fichiers (images)
- **AWS Lambda** : pour traiter les événements `ObjectCreated` de S3
- **AWS DynamoDB** : pour stocker les métadonnées des fichiers
- **AWS EC2** : simulateur client qui interroge DynamoDB
- **AWS CloudFormation** : déploiement de l’ensemble de l’infrastructure

---

## Structure du template CloudFormation

Le template déploie les ressources suivantes :

- Un bucket **S3** (S3Bucket) nommé `envname-file-metadata-bucket` (nom dépendant de l’environnement)
- Une table **DynamoDB** (DynamoDBTable) nommée `FileMetadata-envname` avec `FileName` comme clé primaire
- Un groupe de sécurité EC2 (MySecurityGroup) qui permet SSH (port 22)
- Une fonction **Lambda** (LambdaFunction) nommée `learn-function-envname` qui lit les événements S3 et enregistre les infos dans DynamoDB
- Une instance **EC2** avec un script utilisateur qui installe `aws-cli` et lit les données de DynamoDB
- Une permission Lambda permettant à S3 d’invoquer la fonction

---

## Test du système

Pour tester ce système, j’ai procédé ainsi :

1. Une fois les ressources déployées, j’ai uploadé un fichier `.jpg` dans le bucket S3.
2. Cela a déclenché automatiquement la fonction Lambda.
3. Lambda a extrait le nom du fichier et le nom du bucket, puis les a insérés dans DynamoDB.
4. L’instance EC2 a exécuté une commande `aws dynamodb scan` au démarrage pour lire les entrées dans DynamoDB.

---

## Remarques

- Le déclenchement de la Lambda depuis S3 nécessite une dépendance (`DependsOn`) à une ressource `AWS::Lambda::Permission` afin que S3 soit autorisé à invoquer la Lambda.
- J’ai utilisé l’option `ZipFile` directement dans la ressource Lambda pour inclure le code Python inline.
- La configuration EC2 inclut un `UserData` simple qui permet d’installer les outils nécessaires et de lire la table DynamoDB automatiquement.

---

## Informations supplémentaires

- **Nom du projet** : intégration AWS via CloudFormation
- **Étudiant** : Komi Jonas Edey
- **Classe** : M1 IA - BD
- **Évaluation** : Examen AWS - Projet pratique

---
