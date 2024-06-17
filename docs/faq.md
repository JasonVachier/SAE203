# DOCUMENTATION SAE203

## FAQ

**Bien que la documentation traite de tous les points nécessaires pour le bon fonctionnement de la solution, vous pourrez tout de même rencontrer quelques soucis 'type'.**

### Pourquoi ma page 'aggreg.net' n'est-elle pas stylisée ?

Pour commencer, un problème souvent remarqué lors du déploiement de cette solution informatique, c'est le manque de la configuration style dans notre page web.

Voici les quelques étapes à suivre :

- Bien entendu, vérifier que les droits des fichiers concernés soient bien réglés.

- La position dans l'arborescence est primordiale ! Vérifiez donc que :

        style.css ---- fichier se trouve sur le serveur Apache soit :

        /var/www/aggreg/ 

- Bien vérifier que, sur le programme Python, style.css soit appelé au bon endroit.

Si style.css se trouve dans le même répertoire que index.html, alors :

```python

    <html lang="fr">
<head>
    <meta charset="utf-8">
    <meta name="viewport" content="width=device-width, initial-scale=1">
    <title>Logs des Évenement</title>                                       
    <link rel="stylesheet" href="style.css" type="text/css"/>   #<<<<<<<<<< Mettre le bon lien, dans ce cas, simplement le nom de la fiche de style          
</head>
<body>
    <h1>Logs des Évenement - {date_heure_actuelle}</h1>\n\n""")  

```

### Comment installer le service automatiquement ?

Pour le moment, aggreg.py ne propose pas d'installation entièrement automatique, mais les équipes travaillent pour faire une solution d'installation plus simple pour l'expérience utilisateur.

Restez informé sur notre [Gitlab](https://github.com/JasonVachier/SAE203/tree/main) pour d'éventuelles avancées !

### J'ai d'autres problèmes non notifiés ?

Si vous avez bien suivi [le guide d'installation et de paramétrage](Etape1.md) et [le guide d'utilisation](Etape2.md) et qu'il reste encore des soucis pas résolus dans la FAQ.

Veuillez nous les notifier via l'adresse mail suivante :

    jason.vachier@etu.univ-amu.fr

Nous essayerons de tenir cette FAQ à jour avec les potentielles futures problématiques rencontrées par les utilisateurs.

