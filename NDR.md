# 03/05/2019
- Nommage des fonctions du projet:
    - Affichage
    - ChoixParam
    - StrategieTrading
    - ModeManuel
    - ModeAuto
    - EditTexte
    - IndicTechnique
    - AnalyseForme
    - ModePasAPas
- Creation du projet Qt et de chaque fonction (ayant chacune un fichier source .cpp et un fichier en-tête .h)
- Création des branches Git pour le développement séparé
- Apprentissage de l'utilisation de Git au sein de Qt pour pouvoir coder à plusieurs
- Creations des boutons de l'interface en layout grille
- Création de la fonction import lisant un fichier .csv (à compléter)



# 10/05/2019
- Réalisation de la fonction Affichage:
    - Réalisation des graphiques Candlestick & BarChart
    - Fonction Import (remplissage du tableau "cours")
    - Creation du Layout
    - Debugage de l'affichage à continuer (voir comment créer une fenêtre fille )
- Adapatation du code de CoursOHLC:
    - Suppression de la date
    - Ajout du Volume
- Reflexion à l'implementation de chaque fonction


# 17/05/2019
- Fenêtre fille créées avec succès sur l'interface
- Début de création de l'éditeur de texte:
    - Fenêtre générale créée
    - Début de l'implémentation des fonctions Enregistrer & Ouvrir un fichier
    - Ajout des raccourcis claviers & Onglet (menu) & Toolbar & Icones
    - Actions du menu devenues fonctionnelles
- Installation complète de Qt sur tous les postes
- Début de réalisation de l'UML de chaque classe

TO DO:
- Lire les design pattern EN URGENCE !!!
- Finir la modélisation UML
- Commencer l'implémentation des classes
- Charger le fichier ouvert dans l'éditeur de texte (QFile)

# 24/05/2019
- Etude des Design Pattern du poly
- Avancer l'UML de toutes les classes
- Affichage presque fini (Robin)
- Import fini (Anael)
- Commencement de ChoixParam (Estelle)
- Commencement de Indicateur (Théo)
- Commencement de ModeManuel (Elise)

- QUESTIONS:
    - Utiliser QML et donc le format XML pour stocker nos données ? (stratégies, textes, données coursOHLC, ...)

# 31/05/2019
- Finalisation de Affichage (+ ajout de la vue en détails / retour à la vue globale)
- Réalisation de Analyse de Forme dans Affichage
- Avancement de ChoixParam (statut : presque terminé)
- Avancement de Indicateur (statut : presque terminé)
- Commencement de Stratégie

TO DO:
- Avancer sur les fonctions manquantes (notamment Stratégie et un mode !)
- Refaire l'architecture globale une fois toutes les fonctions réalisées
- Voir la sauvegarde de fichiers depuis EditText (format XML avec QML pour stocker des strategies / données autres que texte ?)
--> Gestion de fichier sous Qt pour save des données : https://openclassrooms.com/forum/sujet/qt-gestion-et-sauvegarde-de-donnees

# 07/06/2019
- Implémentation des modes Automatique et Manuel dans l'interface principale
- Fusion de Mode Manuel et Affichage
- Finalisation de l'affichage de l'historique des transactions effectuées
- Commencement de la doc Doxygen
- Commencement de la recherche d'implémentation de QML (EditText)
- Réglage de divers problèmes

# 14/06/2019

# A FAIRE AVANT DE RENDRE LE PROJET:
- Relire le rapport
- Relire les commentaires, enlever les out-dated et les /* AMELIORATION POSSIBLE */
- Vérifer que tout marche niquel
- Prendre en vidéo le fonctionnement du programme