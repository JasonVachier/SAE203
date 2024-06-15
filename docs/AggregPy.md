# DOCUMENTATION SAE203

## C'est ici que ce trouve le code aggreg.py


```python

    # Importation des modules

    import feedparser
    import sys
    import urllib.request
    import yaml
    import os
    from datetime import datetime


    def charge_urls(liste_url):

        """Charge les urls de flux rss et les analyse pour mettre à part les erreurs potentielles"""

        # Création d'une liste initial
        urls_analysees = []
        
        # Création d'une boucle qui parcours la liste d'url pour chaque élement "url"
        for url in liste_url:
            try:               
                flux_brut = urllib.request.urlopen(url)           
                flux_brut = feedparser.parse(url)  

                if flux_brut['bozo'] != False:
                    urls_analysees.append(None)
                else:
                    urls_analysees.append(flux_brut)

            # Initialisation d'exeptions pour certains cas
            except urllib.error.HTTPError:        
                urls_analysees.append(None)      
            except urllib.error.URLError:         
                urls_analysees.append(None)       
            except ValueError:        
                urls_analysees.append(None)
        
        return urls_analysees     # Retourne les urls analysées


    def convert_string_date(publishedDate: str):
        """Fonction qui permet de convertir le format des dates du flux RSS en format exploitable pour
        la fonction Datetime"""

        date_format = "%a, %d %b %Y %H:%M"                              # Prend chaque élément de la date et les met dans des variables (fonction Datetime)
        
        date_object = datetime.strptime(publishedDate, date_format)     # Met la date dans le bon format

        return date_object                                              # Retourne la date exploitable pour le trie

    def fusion_flux(urls_analysees, liste_flux, tri_chrono):
        """
        La fonction permet de prendre tout les flux de tout les serveurs, les fusionne pour n'en faire plus 
        qu'un. Une fois la fusion faite on peut trié en fonction de la criticité ou de la date des évents
        """
        
        # Création d'une liste initial
        evenements = []
        
        # parcours les URL (lien des serveur) et du contenues analysees
        for flux, url in zip(liste_flux, urls_analysees):
            if flux is not None:
                # Dans le flux analysé on extré les informations importantes
                for item in flux.entries:
                    evenement = {
                        'titre': item.title,
                        'categorie': item.get('category', 'MINOR'),             # Si la catégorie n'est pas spécifié dans le flux on met automatiquement en "Minor"
                        'serveur': url.split('/')[2],                           # Récupère la provenance de l'erreur (ex: Serveur2)
                        'date_publi': item.published,                           # Récupère la date de l'erreur
                        'date_program': convert_string_date(item.published),    # convertie la date dans le bon format
                        'lien': item.link,                                      # prend le lien entier de l'erreur
                        'description': item.get('description', '')              # création d'une description, si il n'y en a pas, la chaine est vide
                    }
                    # Ajout du dictionnaire dans la liste précédemment crée
                    evenements.append(evenement)

        
        # Trie en fonction des informations ci-dessus
        if tri_chrono == True: 
            # Si l'utilisateur veut trier par chronologie du plus récent au plus vieux
            # - On éffectue un trie 'sorted', en prenant dabors la date, puis sa catégorie. 
            # - "Reverse" pour trié du plus récent au plus vieux 
            evenements = sorted(evenements, key=lambda x: (x['date_program'], x['categorie']), reverse=True)
        else:
            # Si l'utilisateur veut trier par criticité de Critical à Minor
            # - On éffectue un trie 'sorted', en prenant dabors sa criticité, puis sa date. 
            evenements = sorted(evenements, key=lambda x: (x['categorie'], x['date_program']))

        return evenements     # Retourne mes évenement trié par ordre chrono ou criticité

    def genere_html(liste_evenements, chemin_html,date_heure_actuelle):
        """
        Cette fonction récupère les flux analysée ( avec les informations importante et trié ) et crée
        une page html à partir de ces information. intègres un autre fichier 'style.css' pour la lisibilité 
        """
        # Ouvre le fichier HTML en mode 'w' pour l'écriture (il faut donc les droit sur aggreg)
        with open(chemin_html, 'w', encoding='utf-8') as fichier_html:
            # On crée l'en tête du la page html et on y intègre notre style.css
            fichier_html.write(f"""<!DOCTYPE html>
                        <html lang="fr">
                        <head>
                            <meta charset="utf-8">
                            <meta name="viewport" content="width=device-width, initial-scale=1">
                            <title>Logs des Évenement</title>                                       
                            <link rel="stylesheet" href="style.css" type="text/css"/>             
                        </head>
                        <body>
                            <h1>Logs des Évenement - {date_heure_actuelle}</h1>\n\n""")                                 

            # Implémentation des éléments souhaité dans la page html
            for evenement in liste_evenements:
                # Boucle afin de déterminé la coucleur en fonction de la criticité, et crée un bloc pour chaque évenement
                couleur = ""
                if evenement['categorie'].upper() == "MINOR":      # si c'est MINOR la couleur est verte
                    couleur = "green"
                elif evenement['categorie'].upper() == "MAJOR":    # si c'est MAJOR la couleur est orange
                    couleur = "orange"
                elif evenement['categorie'].upper() == "CRITICAL": # si c'est CRITICAL la couleur est rouge
                    couleur = "red"

                fichier_html.write(f"""<article>                                       
                        <h2>{evenement['titre']}</h2>                          
                        <p>from: {evenement['serveur']}</p>                             
                        <p>{evenement['date_publi']}</p>                         
                        <p style="color: {couleur};">{evenement['categorie']}</p>       
                        <p><a href="{evenement['lien']}">{evenement['lien']}</a></p>    
                        <p>{evenement['description']}</p>                              
                        </article>\n\n""")                                              
            # écriture de la fin du fichier html
            fichier_html.write("""
                        </body>
                    </html>""")

    def litYaml(chemin_config):
        """la fonction récupère un fichier config en yaml, et le lit et renvoi sa config"""
        # ouvre le fichier à l'endroit indiqué, avec les droit 'r' de lecture
        with open(chemin_config, 'r') as fichier:
            config = yaml.safe_load(fichier)
        return config # retourne la config

    def main():
        
        vardoc = os.path.dirname(os.path.realpath(__file__))

        chemin_config = vardoc+'/conf.yaml'                                 # Indication de la localisation du fichier config yaml

        config = litYaml(chemin_config)                                     # lectire de la config yaml en fonction de 'chemin_config'

        liste_urls = config['sources']                                      # prend les source du fichier conf.yaml sans la var(liste_url)
        destination = config['destination']                                 # prend dans la config la destination de la page HTML
        tri_chrono = config['tri-chrono']                                   # voit si la config décide de trié en orde chrono ou pas

        # Charger les documents RSS à partir des URLs
        flux_rss = charge_urls(liste_urls)                                  # Charge les fichier rss à partir des sources ("liste_urls")

        tri = fusion_flux(liste_urls, flux_rss, tri_chrono)                 # fusionne et trie tout les flux RSS des 3 serveur

        date_heure_actuelle = datetime.now().strftime("%Y-%m-%d %H:%M:%S")  # Récupère la date actuel pour l'afficher sur le site des logs

        print('Page Actualisée')                                            # affiche un message pour signalé que le processuse a bien fonctionné
        genere_html(tri, destination,date_heure_actuelle)                   # génére la page html à partir du flux fusionné et trié vers la destination de la config ("destination"), avec la date actuel

    if __name__ == "__main__":
        main()

```