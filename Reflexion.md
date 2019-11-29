# Reflexions

## Nouvelle architecture (finale):
- Penser comme un vrai programme à executer: comme un programme d'installation ?
- Boutons "Suivant/Précédent" pour revenir à l'étape voulu --> Ces boutons font le lien entre les différentes fonctions, on créera donc les uns après les autres des objets de nos fonctions dans la fenêtre principale, qui se passeront leur paramètres (qu'ils soient créés ou importés)
- Etapes:
    - 1: Bienvenue dans l'application [Nom] vous permettant de gérer efficacement vos stratégies de trading suivant les cours actuels. (écran de démarrage)
    - 2: Veuillez importer le fichier au format CSV contenant les informations des cours à analyser (Import)
        - Afficher le chemin du fichier importé sur la fenetre, griser le bouton suivant si le fichier n'est pas un .CSV et afficher un message d'erreur à côté du chemin récupéré
        - Permettre l'affichage du fichier importé si désiré (bouton supplémentaire)
    - 3: Selection des paramètres de la simulation (ChoixParam)
    - 4: Sélectionner le mode souhaité (radio bouton) avec une description de chaque mode juste en dessous:
        -  Mode automatique :
            -  On choisit une stratégie parmi celle existante dans une liste déroulante
        -  Mode Manuel :
            -  
        -  Mode Pas à Pas :
            -  
    - A la fin : Editeur de Texte pour prendre des notes, puis sauvegarder le fichier avec les données saisies au cours de la manipulation.

## Affichage

- Un fichier .csv contient des valeurs séparées par des virgules
--> Faire une **boucle for** lors de la lecture du fichier qui prend les valeurs une à une dans cet ordre avant de le répéter:
1ère valeur: Open
2ème valeur: High
3ème valeur: Low
4ème valeur: Close
5ème valeur: Volume
**A respecter:** 
    - Chaque ligne doit avoir le même nombre de valeur (= même nombre de virgule)
    - Pas de ligne vide
    - Pas d'espace après/avant les virgules

- Deroulement fonction Import pour remplir le tableau de CoursOHLC:
    - Entrée: Tableau d'éléments de type CoursOHLC vide
    - Cherche le chemin du .csv avec QFile
    - Boucle: (jusqu'à ce qu'il n'y ai plus de lignes dans le .csv)
        - Lit une ligne du .csv avec Lecteur
        - Remplis l'objet CoursOHLC d'indice i
        - i++
    
- Affichage en graphique de Chandelier

- Affichage du volume graphique de Barre

## ChoixParam
- bouton choix Parametre
- formulaire avec les différentes données à rentrer:
    - Date (label + textbox) avec défaut sur la date de début du fichier (utiliser la méthode cherchant la date)
    - devise de base (label + textbox) 0 par défaut dans la textbox
    - devise de contrepartie (label + textbox) avec 1000000 par défaut dans la textbox
    - intermediaire (label + listbox) choisir dans le menu déroulant la personne qui va etre rémunérée
    - pourcentage (label + textbox) avec 0.1 par défaut dans la text box
    - Achat? (label + radiobutton) 
    - valider (button) pour valider nos informations et lancer la fonction

- fonction qui réalise le calcul: 
    - si achat coché deduire sur les devises de base
    - si achat non coché (donc vente) deduire sur les devises de contrepartie 
    - actualiser dans les textboxs les valeurs et ne pas oublier le pourcentage
    - enregistrer dans le tableau
## StrategieTrading
- Créer un tableau "ListeStrat" contenant des éléments de type StrategieTrad -> On affichera les éléments de ce tableau dans ModeAuto
- Crée une nouvelle classe StrategieTrad avec comme attribut:
    - Date (de la transaction)
    - devise MontantCotation (calculer la différence avec l'objet précedent)
        -> DiffCota = strat[i].getDiffCota - strat.getDiffCota[i-1]
    - devise MontantBase (calculer la différence avec l'objet précedent)
        -> DiffBase = strat[i].DiffBase - strat.DiffBase[i-1]
    - ROI (retour sur investissement) = Ratio montantApresTransaction / montantAvantTransaction

- Transaction = achat

## ModeManuel

- L'utilisateur peut ici lancer manuellement la simulation:
    - Charger une paire de devise (au choix) -> faire une fenêtre de selection parmis un choix donné
    - Choisir dateAchat
    - Choisir dateVente
    - Possibilité d'annuler la dernière transaction

## ModeAuto

- Fenêtre de selection des stratégies de trading existantes (celles stockées dans le taleau "ListeStrat")
- (à continuer)

## EditTexte

- Voir si on peut afficher simplement un editeur de texte sur Qt sans le coder entièrement (QTextEditor ?)
- La note écrite sera liée à la stratégie en cours (mettre un attribut facultatif 'QText' aux objets du taleau "ListeStrat")
- Possibilité de sauvegarder une simulation, contenant 3 parties:
    - Le fichier .CSV des données ('enregistrer un fichier' avec QFile)
    - Le tableaux des transactions
    - Les notes prises
- Mettre un bouton "enregistrer"

## IndicTechnique

- 

## AnalyseForme

## ModePasAPas

## ATTENTION
- Classe::methode (ex: QBarSet::setLabel(car))
- Objet.methode (ex: car.setNum(i))