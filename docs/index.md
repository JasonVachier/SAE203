# DOCUMENTATION SAE203

## Présentation !

Bonjour chers administrateur système ! Bienvenue sur la doc du programme aggreg.py

1 - A quoi sert aggreg.py ?

aggreg.py est un programme à exécuté sur une machine aggregateur qui vas lire n flux RSS à partir de plusieurs URLs, et en génere une page HTML. aggreg.py contien plusieur fonction importante :

    1 - charge_urls = Charge les urls de flux rss et les analyse pour mettre à part les erreurs potentielles
    2 - convert_string_date = Fonction qui permet de convertir le format des dates du flux RSS en format exploitable pour la fonction Datetime
    3 - fusion_flux = La fonction permet de prendre tout les flux de tout les serveurs, les fusionne pour n'en faire plus qu'un. Une fois la fusion faite on peut trié en fonction de la criticité ou de la date des évents
    4 - genere_html = Cette fonction récupère les flux analysée ( avec les informations importante et trié ) et crée une page html à partir de ces information. intègres un autre fichier 'style.css' pour la lisibilité 
    5 - litYaml = la fonction récupère un fichier config en yaml, et le lit et renvoi sa config

2 - Pourquoi un fichier yaml ?

Un fichier conf.yaml est mis en place afin de faciliter l'utilsation de aggreg.py

conf.yaml :

    sources:                                    # Liste des URLs pour récupérer rss.xml
        - http://serveur1.net/rss.xml
        - http://serveur2.net/rss.xml
        - http://serveur3.net/rss.xml
        ....
    rss-name: rss.xml                           # Donne le nom du flux rss à téécharger
    destination: /var/www/aggreg/index.html     # Indique la destination du fichier sortie HTML
    tri-chrono: True                            # si True faire le trie chronologiquement, si False trie par criticité


Vas voir les autres pages pour voir :

- Le guide d'installation et de paramétrage

- Le guide d'utilisation

- Le Guide de résolution des problèmes
 

