# DOCUMENTATION SAE203

## Bienvenue dans le guide d'installation et de paramétrage !




### Étape 1 : Prérequis Matériels et Logiciels


Serveurs (ex : serveur1.net, serveur2.net, serveur3.net)

- Configuration matérielle : 1 Go de RAM suffira.

- Interface graphique : Ces machines n'ont pas d'interface graphique.

- Logiciels requis : Serveur Apache2 installé avec un hôte virtuel configuré.

- Utilisation : Ces machines recevront les événements en flux RSS.



Machine Agrégatrice (ex : aggreg.net) :

- Configuration matérielle : 1 Go de RAM suffira.

- Interface graphique : Cette machine n'a pas d'interface graphique

- logiciels requis : Serveur Apache2 installé avec un hôte virtuel configuré.

- Utilisation : Cette machine sera utilisée pour exécuter le programme aggreg.py.



Client

- Configuration matérielle : 2 Go de RAM suffira.

- Interface graphique : Cette machine dispose d'une interface graphique.

- Accès : Un accès à un navigateur web est requis



Assurez vous que toutes les machines respectent ces configurations pour le bon fonctionnement du programme aggreg.py




### Etape 2 : Configuration des Serveurs



#### Vérification des Flux RSS

Pour chaque serveur, assurez-vous que les flux des événements sont bien au format suivant :

    rss.xml


#### Emplacement des Fichiers RSS

Ces fichiers de flux doivent se trouver sur votre serveur Apache dans l'arborescence suivante :

    X@serveurX: ~/var/www/serveurX 


C'est tout ce dont vous avez vraiment besoin sur vos serveurs. Assurez-vous que cette structure soit respectée pour le bon fonctionnement de l'agrégation des flux RSS.




### Etape 3 : Configuration de la Machine Agrégatrice



#### Rôle de la Machine Agrégatrice


La machine aggreg.net est le centre de l'agrégation. Elle exécute un script Python pour récupérer les flux de tous vos serveurs et générer une page HTML.


Sur cette machine, installez les éléments suivants :

    'aggreg.py'   # Le programme d'agrégation.

    'conf.yaml'   # Fichier de configuration pour paramétrer facilement le programme Python.

    'style.css'   # Ajout d'un peu de style quand même !!!


Pour 'aggreg.py': [Cliquez ici](AggregPy.md)


Créez un fichier nommé 'conf.yaml' et insérez-y le contenu suivant :

    sources:                                    
            - http://serveur1.net/rss.xml
            - http://serveur2.net/rss.xml
            - http://serveur3.net/rss.xml
            .... ( ajoutez autant de liens que vous avez de serveurs)
        rss-name: rss.xml                           
        destination: /var/www/aggreg/index.html     
        tri-chrono: True     


Pour 'style.css': [Cliquez ici](StyleCss.md)



#### Script d'Agrégation 'aggreg.py'

Le script 'aggreg.py' se trouve dans votre répertoire '/home' ou dans un autre répertoire.

Il est important de bien connaître son emplacement pour l'automatisation.


#### Fichier de Configuration 'conf.yaml'

Placez le fichier 'conf.yaml' dans un répertoire facile d'accès, car c'est le seul fichier que vous aurez à modifier par la suite.


#### style.css

Le fichier 'style.css' doit se trouver au même endroit où la page HTML est générée, donc dans votre serveur Apache :

    /var/www/aggreg/style.css

Dans ce répertoire, vous pouvez également ajouter une image de fond de votre choix. Assurez-vous que l'image se nomme 'fond.jpg'. 

Je vous propose celle-ci par exemple : [Cliquer ici](https://drive.google.com/file/d/1Pug2jDliUAHsWg-INjGTAQo20B8lawGK/view?usp=drive_link)




### Etape 4 : Automatisation de l'agrégateur


Pour l'automatisation, je vous propose d'utiliser 'crontab'.

Le but est que le programme se lance automatiquement toutes les minutes, heures ou jours selon vos besoins.

Pour cela :

    crontab -e

Puis :

    .---------------- minutes (0 - 59)
    |  .------------- heures (0 - 23)
    |  |  .---------- jours (1 - 31)
    |  |  |  .------- mois (1 - 12) OR jan,feb,mar,apr ...
    |  |  |  |  .---- jour de la semaine (0 - 6) (Dimanche =0 ou 7)
    |  |  |  |  |
    *  *  *  *  * python3 /chemin/vers/aggreg.py


Après cela, si tout s'est bien passé, le programme se lance automatiquement et génère une page HTML (toute jolie pardi !), avec les flux de vos différents serveurs.




### Etape 5 : Configuration du Client


Pour la machine client, vous devez avoir accès au serveur Apache de aggreg.net pour visualiser la page HTML des logs des événements. Et pour cela, rien de plus simple !


Sur votre navigateur, installez l'extension LiveHosts disponible sur la plupart des navigateurs.[Github de LiveHosts](https://github.com/Aioros/livehosts)

Puis configurez LiveHosts comme suit :

    Hostname : (par exemple: aggreg.net)

    IP : (par exemple 192.168.122.20)





