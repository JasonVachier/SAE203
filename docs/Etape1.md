# DOCUMENTATION SAE203

## Bienvenue dans le guide d'installation et de paramétrage !

### Etape 1 : L'Architecture | Prérequis


Il te faut des serveurs (ex: serveur1.net ; serveur2.net ; serveur3.net) (1G RAM suffira) :

- Ces machines n'ont pas d'interface graphique

- Avoir un serveur http Apache2 installé avec un hôte virtuel configuré

- c'est ces machines qui aurons les évement en flux rss


Il te faut aussi une machine aggragteur (ex: aggreg.net) (1G RAM suffira) :

- Cette machine n'a pas d'interface graphique

- Avoir un serveur Apache2 installé avec un hôte virtuel configuré


Enfin un client (2G RAM suffira) :

- Cette machine à une interface graphique et un acces à un navigateur web


### Etape 2 : Serveurs


En ce qui concernes les serveurs vérifié que les flux des évenement soit bien sous le format suivant :

    rss.xml

Ces flux doivent se retrouvé sur votre serveur apache dans l'arborésence suivante :

    X@serveur1: ~/var/www/serveur1 

C'est tout ce dont tu à vraiment besoin sur tes serveur !


### Etape 3 : Aggrégateur


aggreg.net est un peut le centre de tout il va exécuté un script python pour récupérer le flux de tout tes serveur et renvoyer une page html

Sur cette machines tu doit installer plusieur choses : 

    - aggreg.py         # Le programme d'aggrégation

    - conf.yaml         # Fichier qui permettra de configuré le travail de ton programme python rapidement et facilement

    - style.css         # Ajout d'un peut de style sur nos logs.html tout de même !!!

Pour aggreg.py je t'invite à : [Cliquer ici](AggregPy.md)

Pour conf.yaml c'est super simple ! créer un fichier du même nom et inscris-y :

    sources:                                    
            - http://serveur1.net/rss.xml
            - http://serveur2.net/rss.xml
            - http://serveur3.net/rss.xml
            .... (ajoute autant de lien que tu as de serveur)
        rss-name: rss.xml                           
        destination: /var/www/aggreg/index.html     
        tri-chrono: True     

Pour style.css je t'invite à : [Cliquer ici](StyleCss.md)




