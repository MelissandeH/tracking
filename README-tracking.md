# Titre du projet

Module File Upload , page Tracking Log System

## Introduction

Nous allons voir ci-dessous comment utiliser et configurer le module d'envoi de fichier sur la page Tracking.

### Fichiers 

- _edittracking.php (index de la page)
- _delete.php
- _file-upload.php
- _update.php
- _updateCategory.php

### Pré-requis

Sont inclus dans ce module :

```
Bootstrap
```

```
DropZoneJS
```

```
Jquery
```

```
Fontawesome
```

## Utiliser le module d'envoi de fichiers

Il existe deux façons d'envoyer des fichiers via le module d'envoi de fichier

A.- Envoyer un seul fichier : 

1.- Cliquez sur "Définir une catégorie"
2.- Puis sur "Parcourir" à fin d'envoyer votre fichier
3.- Son nom est donc directement indiqué dans "Votre fichier" , vous pouvez le renommer
4.- Si vous le souhaitez, vous avez la possibilité d'ajouter un commentaire lié à votre document
5.- Pour finir, choisissez la catégorie associée à votre fichier
6.- Appuyez sur "Envoyer".

B.- Envoyer plusieurs fichiers à la fois :

{En cours de développement}

### Personnalisation disponibles

- id (GET)
- Nom du fichier (POST)
- Catégorie (AJAX)
- Commentaire (AJAX)

### Actions disponibles

- Visualiser (HREF)
- Télécharger (HREF)
- Supprimer (AJAX)

## Développement

> 186 à 238 : Multiple file upload (+btn file upload pour un fichier)
    {Développement en cours}
    Le bouton "Définir une catégorie" fait apparaître une fenêtre 'pop-up' permettant de personnaliser son envoi

> 240 à 317 : Table secondaire générée local (uploads/docs)
    On génère une table grâce à une boucle répertoriant tous les fichiers se trouvant dans '/var/www/web/uploads/docs'
    Deux actions sont associées à ces fichiers : Visualiser et Enregistrer

> 321 à 480 : Table principale générée BDD (devtracing)
    On génère une table grâce à une boucle récupérant le filename, la catégorie, le commentaire qui correspondent à l'id de la session
    Le type est créé grâce au filename qui est lui-même décomposé à fin de récuperer l'extension
    Trois actions sont disponibles : visualiser et enregistrer mais également supprimer qui permet de retirer les données du document de la BDD {+ Développement en cours : suppresion du fichier en local}
    Deux champs sont ainsi personnalisables : catégorie ainsi que commentaire. Le nom du fichier lui est modifiable seulement à sa création dans file upload

> 488 à 636 : Fenêtre pop-up (déclenchée par le btn "Définir une catégorie)
    On upload un fichier que l'on peut tout de suite renommer et lui ajouter une description. 
    Si le fichier est un fichier image lisible (PNG,JPG...) un preview apparaîtra entre le titre et la description du fichier envoyé
    Il est nécessaire pour l'utilisateur de sélectionner une catégorie à associer à ce document
    Les données du documents sont résumées dans le footer de la fenêtre à fin de faciliter la démarche