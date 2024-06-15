# DOCUMENTATION SAE203

## Bienvenue dans le guide d'installation et de paramétrage !

### Étape 1 : L'Architecture | Prérequis


Il te faut des serveurs (ex : serveur1.net ; serveur2.net ; serveur3.net) (1 Go de RAM suffira) :

- Ces machines n'ont pas d'interface graphique.

- Avoir un serveur Apache2 installé avec un hôte virtuel configuré.

- Ce sont ces machines qui auront les événements en flux RSS.


Il te faut aussi une machine agrégateur (ex : aggreg.net) (1 Go de RAM suffira) :

- Cette machine n'a pas d'interface graphique.

- Avoir un serveur Apache2 installé avec un hôte virtuel configuré.

Enfin un client (2 Go de RAM suffira) :

- Cette machine a une interface graphique et un accès à un navigateur web.


### Etape 2 : Serveurs


En ce qui concerne les serveurs, vérifiez que les flux des événements soient bien sous le format suivant :

    rss.xml

Ces flux doivent se retrouver sur votre serveur Apache dans l'arborescence suivante :

    X@serveurX: ~/var/www/serveurX 

C'est tout ce dont tu as vraiment besoin sur tes serveurs !


### Etape 3 : Aggrégateur


aggreg.net est un peu le centre de tout, il va exécuter un script Python pour récupérer le flux de tous tes serveurs et renvoyer une page HTML.

Sur cette machine, tu dois installer plusieurs choses :

    'aggreg.py'   # Le programme d'agrégation.

    'conf.yaml'   # Fichier qui permettra de configurer le travail de ton programme Python rapidement et facilement.

    'style.css'   # Ajout d'un peu de style quand même !!!


Pour 'aggreg.py', je t'invite à : [Cliquer ici](AggregPy.md)


Pour 'conf.yaml', Crée un fichier du même nom et inscris-y :

    sources:                                    
            - http://serveur1.net/rss.xml
            - http://serveur2.net/rss.xml
            - http://serveur3.net/rss.xml
            .... (ajoute autant de liens que tu as de serveurs)
        rss-name: rss.xml                           
        destination: /var/www/aggreg/index.html     
        tri-chrono: True     


Pour 'style.css', je t'invite à : [Cliquer ici](StyleCss.md)


#### aggreg.py

'aggreg.py' se trouve dans votre répertoire /home ou un autre répertoire.

Il est important de bien savoir où il se trouve pour l'automatisation !!

#### conf.yaml

Mettez-le dans un répertoire facile d'accès, c'est le seul fichier que vous aurez à modifier par la suite.

#### style.css

'style.css' doit se trouver au même endroit où la page HTML est générée, soit dans votre serveur Apache :

    /var/www/aggreg/style.css

Vous pouvez y ajouter dans ce répertoire une image de fond de votre choix, c'est paramétré pour ! (l'image doit s'appeler 'fond.jpg')

Je vous propose celle-ci par exemple : [Cliquer ici](https://drive.google.com/file/d/1Pug2jDliUAHsWg-INjGTAQo20B8lawGK/view?usp=drive_link)


### Etape 4 : Automatisation de l'agrégateur

Pour l'automatisation, je vous propose d'utiliser crontab.

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


Après cela, si tout s'est bien passé, le programme se lance automatiquement et génère une page HTML (toute jolie pardi !) avec les flux de tes différents serveurs.


### Etape 5 : Côté client

Pour la machine client, il faut que tu aies accès au serveur Apache de aggreg.net pour visualiser la page HTML des logs des événements, et pour cela, rien de plus simple !

Sur votre navigateur, installez l'extension LiveHosts disponible sur la plupart des navigateurs. [Github de LiveHosts](https://github.com/Aioros/livehosts)

Puis paramétrez LiveHosts avec :

    Hostname : (par exemple: aggreg.net)

    IP : (par exemple 192.168.122.20)





