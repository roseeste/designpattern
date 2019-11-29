UML

*choix des paramètres*
* <=> addTransaction *
```UML
class ChoixParam{ /*singleton*/
    -fichier : QFil
    -date : QDate (=QDate)
    -devisedebase : double (=0) /* ex : EUR/USD EUR est la devise de base et USD la devise de contrepartie/cotation.
    -devisedecontrepartie : double (=1000000)
    -pourcentage : double(=0.1)

    
    +ChoixParam(QFile f, const date &d, const devise &ddb, const devise &dbc, const double &p, const bool inter, const bool &type)
    +getDate() const : Date
    +setDate() : Date
    +getDeviseB() const : Devise
    +setDeviseB()  : Devise
    +getDeviseC() const : Devise
    +setDeviseC() : Devise
    +getPourcentage() const : double
    +getTransaction() const : QString
    +calcul() : 
    
}
```



*Strategie Trading*

*Tableau de Strategie*
* design patern strategie *

```UML
class Transaction : public OHLC{
    -date : QDate
    -montantCotation : double(DiffCota)
    -montantBase : double(DiffBase)
    -ROI : double
    -montantdepart : double
    +Transaction(const CoursOHLC cours, const double &cot, const double &base, const double &roi);
    +getDate();
    +getMontantCotation();
    +getMontantBase();
    +ConversionBaseCotation() : double;
    +setROI(double montantAvantTransaction){
        return montantCotation+ConversionBaseCotation()/ montantdepart ;
    }
    
}
Notes:
- DiffCota = strat[i].getMontantCotation - strat[i-1].getMontantCotation
- DiffBase = strat[i].getMontantBase - strat[i-1].getMontantBase
-  heritage de La classe cours Ohlc pour récupérer le prix d ouverture et de fermeture 

```UML
class Mode{ // classe abstraite permettant de récupérer les données dont nous avons besoin dans editeur de texte 
- vector<Transaction> tab;
- param : ChoixParam
}
```

```UML
class Manuel : public Mode{

- achat : bool

+ isAchat(){return achat;}
+ calcul()/* depend de ce que retourne isAchat */;
+ iterator ou memento pour parcourir sequentiellement;


/*pairededevise est hérité de la classe transaction 
Et pour les dates */
}
```

```UML
class Automatique : public Mode{
    +iterator/memento pour passer chaque journée 
    +recupere les cours OHLCV d'affichage.
    +calcul se faisant par rapport aux stratégies entrées(design patern strategie à implementer)
}
```

```UML
class EditText : public Mode{
    -notes : string
    +save() 
}
```

```UML
class Indicateurs{
    - vector<CoursOHLC> cours;
    - Indicateurs(vector<CoursOHLC> c): cours(c){}
    - UpdateRsi(int nbjours) /*faire le calcul comme indiqué sur internet à partir du cours OHLCV*/
    - UpdateEMA(int nbjours)
    - UpdateMACD(int nbjours)
}
```

```UML
class Analyse{
    Bougie bougie;
    Analyse(Bougie b):bougie(b){}
    string getForme(); // utiliser les informations sur le sujet afin de determiner via des test la forme                           de la bougie.
    
}
```

```UML
class PasàPas : public Manuel{

    +iterator avec previous pour revenir plusieurs fois en arrière et a chaque fois qu'on revient on delete la case actuelle.
    
}
```