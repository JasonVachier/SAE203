# DOCUMENTATION SAE203
 
 
 
## Présentation
 
 
 
 
Bonjour chers administrateurs système,

Bienvenue dans la documentation du programme aggreg.py.
 
 
 
### A quoi sert 'aggreg.py' ?
 
 
 
'aggreg.py' est un programme conçu pour être exécuté sur une machine agrégatrice. Il lit des flux RSS à partir de plusieurs URLs et génère une page HTML. Le programme 'aggreg.py' contient plusieurs fonctions importantes :
 
- charge_urls : Charge les URLs des flux RSS et les analyse pour identifier d'éventuelles erreurs.
 
- convert_string_date : Cette fonction permet de convertir le format des dates du flux RSS en un format exploitable par le module Datetime.
 
- fusion_flux : Cette fonction prend tous les flux de tous les serveurs, les fusionne en un seul. Une fois la fusion effectuée, il est possible de les trier en fonction de la criticité ou de la date des événements.
 
- genere_html : Cette fonction récupère les flux analysés (avec les informations importantes et triées) et crée une page HTML à partir de ces informations. Elle intègre également un autre fichier 'style.css' pour une meilleure lisibilité.
 
- litYaml : Cette fonction récupère un fichier de configuration au format YAML, le lit et renvoie sa configuration.
 
 
### Configuration à partir d'un fichier Yaml
 

Un fichier 'conf.yaml' est mis en place afin de faciliter l'utilisation de aggreg.py.
 
 
conf.yaml :
 
    sources:                                    # Liste des URLs pour récupérer rss.xml
        - http://serveur1.net/rss.xml
        - http://serveur2.net/rss.xml
        - http://serveur3.net/rss.xml
        ....
    rss-name: rss.xml                           # Donne le nom du flux rss à télécharger
    destination: /var/www/aggreg/index.html     # Indique la destination du fichier de sortie HTML
    tri-chrono: True                            # Si True, effectue le tri chronologiquement ; si False, trie par criticité
 
 
 
### Consulter les autres pages pour
 
- [Le guide d'installation et de paramétrage](Etape1.md)

- [Le guide d'utilisation](Etape2.md)

- [FAQ](faq.md)

- [Télécharger les fichiers sources](dl.md)
