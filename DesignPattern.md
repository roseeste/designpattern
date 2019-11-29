# Design Pattern

Lien Wikipedia répertoriant tous les Design Pattern ainsi que leur description / page wikipedia respective: https://fr.wikipedia.org/wiki/Patron_de_conception

![UMLGeneral](https://i.imgur.com/ytqdisg.png)

## Modèle Vue-Controlleur

https://fr.wikipedia.org/wiki/Mod%C3%A8le-vue-contr%C3%B4leur

--> Conseillé par le prof, à étudier !

![Schéma](https://i.imgur.com/EsxxcbS.png)

## Decorator (Décorateur)
- **type:** créationnel
- **Cas d'usage:** Ajout/Suppression des responsabilités d'un objet dynamiquement pendant l'exécution. Alternative aux sous-classes permettant une surcharge flexibles des fonctionnalités.
- **but:**
    - permet d'attacher dynamiquement de nouvelles responsabilités à un objet
    - alternative assez souple à l'héritage pour composer de nouvelles fonctionnalités

**Ex Code:**

```c++
// Classe parent de toutes les marques de voitures
class IVoiture
{
public:
  virtual double prix() = 0;
};

// Exemple de Sous-Classe de IVoiture
class Citroen : public IVoiture
{
public:
  double prix() override { return 999.99l; }
};


// Classe **décorator** permettant l'ajout d'option par la suite
class Option_voiture : public IVoiture
{
public:
  Option_voiture(std::unique_ptr IVoiture voiture, double prix_option)
    : voiture_(std::move(voiture))
    , prix_option_(prix_option)
  {}
  double prix() override { return voiture_->prix() + prix_option_; }
protected:
  std::unique_ptr IVoiture voiture_;
  double prix_option_;
};

// Une option rajoutée grâce à la classe Decorator
class Option_clim : public Option_voiture
{
public:
  Option_clim(std::unique_ptr IVoiture voiture) : Option_voiture(std::move(voiture), 1.0) {}
};

// Jeu de données pour créer une voiture et lui rajouter l'option clim
int main()
{
  auto voiture = std::unique_ptr IVoiture (std::make_unique<Aston_martin>());
  voiture = std::make_unique<Option_clim>(std::move(voiture));
  std::cout << voiture->prix() << "\n"; // affiche 1110.99
  return 0;
}
```

**Ex littéral:** Si on prend un objet Voiture et une classe Voiture disposant des attributs/méthodes de bases d'une voiture (rouler(),frein()... etc) on peut alors y ajouter des **méthodes optionnelles** en plein programme, et donc des nouvelles reponsabilités pour cet objet Voiture: option_clim(), option_ABS(), option_toitOuvrant()... etc

![UML](https://i.imgur.com/RM4FPAq.png)


## factory (Fabrique)
- **type:** Créationnel
- **Cas d'usage:** Quand une classe ne peut pas anticiper à priori les classes d'objets qu'elle doit créer
- **Exemples d'usages:** On a une classe abstraite Animaux, dont on veut créer des objets Chiens, Chats... OR impossible d'implémenter tous les types d'animaux au monde, on gère donc leur ajout dans notre programme par une Factory.
- **but:**
    - Endroit du code où sont créés des objets.
    - instancier/créer des objets dont le type est dérivé d'un type abstrait.
    - Pouvoir déléguer une partie du travail d'un objet à d'autres objets qu'il créera lui-même en fonction de ses besoins **-->** rend les hiérarchies de classes plus évolutives.
- **Implémentation:** Une fabrique étant **unique** par programme, on utilise le Design Pattern **Singleton** pour l'implémenter. On adopte alors la structure suivante:
    - Une classe **ProduitGénérique** définissant l'interface des objets que la méthode de fabrication de la fabrique doit créer.
    - Une classe **ProduitConcret** utilisant l'interface de *ProduitGénérique*
    - Une classe **FabriqueGénérique** déclarant la méthode de fabrication retournant un objet de la classe *Produit*
    - Une classe **FabriqueConcrète** redéfinissant la méthode de *FabriqueGénérique* pour renvoyer un objet de la classe *ProduitConcret*.

**Ex Code:**
```c++
class ProduitGenerique {
    /* ... */
};

class ProduitConcret1 : public ProduitGenerique {
    /* ... */
};

class FabriqueGenerique {
public:
    virtual ProduitGenerique* methodeFabrication() =0; // Méthode virtuelle pure, définie dans les fabriques concrètes
};

class FabriqueConcrete1 : public FabriquerGenerique {
public:
    ProduitConcret1* methodeFabrication();
};
```

![UML](https://i.imgur.com/RfuvIL9.png)


## abstract factory (Fabrique Abstraite)
- **type:** Créationnel
- **Cas d'usage:**
    - Quand un système doit être indépendant de comment sont créés/composés/représenté les objets.
    - Quand on doit créer une famille de produits conçus pour être utilisés ensemble.
- **Exemples d'usages:** Quand des objets peuvent être amenés à respecter plusieurs standards, redéfinissant leur apparence/comportement.
- **but:**
    - Définir une interface pour créer des familles d'objets dépendant les uns les autres, **sans avoir à préciser la classe concrète à utiliser** pour leur création.
    - Plusieurs fabriques peuvent être regroupées en une **fabrique abstraite** pour instancier des objets dérivant de plusieurs types abstraits différents.

**Ex code**

```c++
namespace AbstractFactoryDesignPatternInCSharp  
{  
    /// <summary>  
    /// The 'AbstractFactory' interface.  
    /// </summary>  
    interface IMobilePhone  
    {  
        ISmartPhone GetSmartPhone();  
        INormalPhone GetNormalPhone();  
    }  
}

namespace AbstractFactoryDesignPatternInCSharp  
{  
    /// <summary>  
    /// The 'ConcreteFactory1' class.  
    /// </summary>  
    class Nokia : IMobilePhone  
    {  
        public ISmartPhone GetSmartPhone()  
        {  
            return new NokiaPixel();  
        }  
  
        public INormalPhone GetNormalPhone()  
        {  
            return new Nokia1600();  
        }  
    }  
}  

namespace AbstractFactoryDesignPatternInCSharp  
{  
    /// <summary>  
    /// The 'ConcreteFactory2' class.  
    /// </summary>  
    class Samsung : IMobilePhone  
    {  
        public ISmartPhone GetSmartPhone()  
        {  
            return new SamsungGalaxy();  
        }  
  
        public INormalPhone GetNormalPhone()  
        {  
            return new SamsungGuru();  
        }  
    }  
}  

namespace AbstractFactoryDesignPatternInCSharp  
{  
    /// <summary>  
    /// The 'AbstractProductA' interface  
    /// </summary>  
    interface ISmartPhone  
    {  
        string GetModelDetails();  
    }  
}  

namespace AbstractFactoryDesignPatternInCSharp  
{  
    /// <summary>  
    /// The 'AbstractProductB' interface  
    /// </summary>  
    interface INormalPhone  
    {  
        string GetModelDetails();  
    }  
}  
    
namespace AbstractFactoryDesignPatternInCSharp  
{  
    /// <summary>  
    /// The 'ProductA1' class  
    /// </summary>  
    class NokiaPixel : ISmartPhone  
    {  
        public string GetModelDetails()  
        {  
            return "Model: Nokia Pixel\nRAM: 3GB\nCamera: 8MP\n";  
        }  
    }  
}  

namespace AbstractFactoryDesignPatternInCSharp  
{  
    /// <summary>  
    /// The 'ProductA2' class  
    /// </summary>  
    class SamsungGalaxy : ISmartPhone  
    {  
        public string GetModelDetails()  
        {  
            return "Model: Samsung Galaxy\nRAM: 2GB\nCamera: 13MP\n";  
        }  
    }  
} 

namespace AbstractFactoryDesignPatternInCSharp  
{  
    /// <summary>  
    /// The 'ProductB1' class  
    /// </summary>  
    class Nokia1600 : INormalPhone  
    {  
        public string GetModelDetails()  
        {  
            return "Model: Nokia 1600\nRAM: NA\nCamera: NA\n";  
        }  
    }  
}

namespace AbstractFactoryDesignPatternInCSharp  
{  
    /// <summary>  
    /// The 'ProductB2' class  
    /// </summary>  
    class SamsungGuru : INormalPhone  
    {  
        public string GetModelDetails()  
        {  
            return "Model: Samsung Guru\nRAM: NA\nCamera: NA\n";  
        }  
    }  
} namespace AbstractFactoryDesignPatternInCSharp  
{  
    /// <summary>  
    /// The 'Client' class  
    /// </summary>  
    class MobileClient  
    {  
        ISmartPhone smartPhone;  
        INormalPhone normalPhone;  
  
        public Client(IMobilePhone factory)  
        {  
            smartPhone = factory.GetSmartPhone();  
            normalPhone = factory.GetNormalPhone();  
        }  
  
        public string GetSmartPhoneModelDetails()  
        {  
            return smartPhone.GetModelDetails();  
        }  
  
        public string GetNormalPhoneModelDetails()  
        {  
            return normalPhone.GetModelDetails();  
        }  
    }  
}

```



![UML](https://i.imgur.com/Pkenbe5.png)


## builder (Monteur)
- **type:** créationnel
- **Cas d'usage:**
    - L'objet à créer est imposant (création complexe), demandant une création par petits bouts.
    - Beaucoup d'arguments doivent être passés dans le constructeur de l'objet, dont certains peuvent être optionnels.
- **but:**
    - Dissocier la construction d’un objet de sa représentation
    - Créer une variété d'objets dérivant d'un objet source.
    - Créer objets complexes dont les différentes parties doivent être créées suivant un certain ordre et à partir d'autres objets. Une classe externe contrôle l’algorithme de construction.
**-->** On assemble (= monte) plusieurs objets sources ensembles afin de n'en faire qu'un seul à la fin.
**Structure:**
- Monteur (Builder)
    - classe abstraite qui contient le produit
- MonteurConcret (ConcreteBuilder)
    - fournit une implémentation de Monteur
    - construit et assemble les différentes parties des produits
- Directeur (Director)
    - construit un objet utilisant la méthode de conception Monteur
- Produit (Product)
    - l'objet complexe en cours de construction

**Ex code (Java):**

```java
class Pizza {
  private String pate = "";
  private String sauce = "";
  private String garniture = "";

  public void setPate(String pate)          { this.pate = pate; }
  public void setSauce(String sauce)         { this.sauce = sauce; }
  public void setGarniture(String garniture) { this.garniture = garniture; }
}


/* Monteur */
abstract class MonteurPizza {
  protected Pizza pizza;

  public Pizza getPizza() { return pizza; }
  public void creerNouvellePizza() { pizza = new Pizza(); }

  public abstract void monterPate();
  public abstract void monterSauce();
  public abstract void monterGarniture();
}

/* MonteurConcret */
class MonteurPizzaHawaii extends MonteurPizza {
  public void monterPate()      { pizza.setPate("croisée"); }
  public void monterSauce()     { pizza.setSauce("douce"); }
  public void monterGarniture() { pizza.setGarniture("jambon+ananas"); }
}

/* MonteurConcret */
class MonteurPizzaPiquante extends MonteurPizza {
  public void monterPate()      { pizza.setPate("feuilletée"); }
  public void monterSauce()     { pizza.setSauce("piquante"); }
  public void monterGarniture() { pizza.setGarniture("pepperoni+salami"); }
}

/* Directeur */
class Serveur {
  private MonteurPizza monteurPizza;

  public void setMonteurPizza(MonteurPizza mp) { monteurPizza = mp; }
  public Pizza getPizza() { return monteurPizza.getPizza(); }

  public void construirePizza() {
    monteurPizza.creerNouvellePizza();
    monteurPizza.monterPate();
    monteurPizza.monterSauce();
    monteurPizza.monterGarniture();
  }
}
```

**Ex littéral:** Un objet "Menu" peut se composer de plusieurs autres objets sources (sandwich, frites, boisson, ... etc), ou encore une "Pizza" avec les objets

![UML](https://i.imgur.com/dlemSeu.png)


## bridge
- **type:** Structurel
- **Cas d'usage:** pour découpler l'interface d'une classe et son implémentation. La partie concrète peut alors varier indépendamment de celle abstraite tant qu'elle respecte le contrat de réécriture associé qui les lie. Creation de deux couches ( bas et haut niveaux)Ce patron se base sur la séparation de l'abstraction d'un concept à l'implémentation de ce concept. 
- **but:**
    - utilisé pour séparer la définition de l'interface de l'implémentation. IL est donc possible de faire évoluer l'implémentation concrète d'une classe sans devoir modifier l'appel effectué par la partie cliente squi se refère alors aux signatures de fonctions/methodes de cette interface

**Ex Code**
```c++
// implementator's part
interface Key{
    public void open();
    public void close();
}
class CarDoorKey implements Key{
    @Override
    public void open(){
        System.out.println("> I'm pushing down the button of remote control.");
    }
    @Override
    public void close(){
        System.out.println("> I'm pushing up the button of remote control.");
    }
}
class HouseDoorKey implements Key{
    @Override
    public void open(){
        System.out.println("> I'm turning the key in the right side.");
    }
    @Override
    public void close(){
        System.out.println("> I'm turning the key in the left side.");
    }
}


// abstraction's part
abstract class Door{
    private final Key key;
    public Door(final Key key){
        this.key=key;
    }
    public void openTheDoor(){
        this.key.open();
    }
    public void closeTheDoor(){
        this.key.close();
    }
    protected void preventOwner(){
        System.out.println("> Hi Owner ! You have a geust.");
    }
}
class HouseOneDoor extends Door{
    public HouseOneDoor(final Key key){
        super(key);
    }
    public boolean enter(){
        preventOwner();
        openTheDoor();
        return true;
    }
    public boolean leave(){
        closeTheDoor();
        return true;
    }
}
class CarOneDoor extends Door{
    public CarOneDoor(final Key key){
        super(key);
    }
    public boolean enter(){
        openTheDoor();
        return true;
    }
    public boolean leave(){
        closeTheDoor();
        return true;
    }
}
```

**Ex litteral**
Notre problème d'ouverture d'une porte est décomposé en deux parties : l'implémentation (clé) et l'abstraction (porte). L'implémentation est une simple interface qu'utilisent deux classes. La première est la clé pour la porte d'une maison. La deuxième correspond à la clé de la porte d'une voiture. Des choses un peu plus intéressantes se passent du côté de l'abstraction.
Dans la partie de l'abstraction, on a les illustrations des portes : celle d'une maison et celle d'une voiture. Cependant, les deux ne se comportent pas exactement de la même manière. Dans le cas d'une porte maison, on prévient le propriétaire des lieux qu'une nouvelle personne est entrée (méthode preventOwner()). On ne le fait pas pour la porte de la voiture. Ceci illustre bien le fait que l'implémentation et l'abstraction vivent chacune leurs vies. Rajout du méthode preventOwner() dans HouseOneDoor n'a pas provoqué l'ajout de la même méthode dans les classes implémentant l'interface Key.

![UML](https://i.imgur.com/F2er55W.png)


## composite
- **type:** Structurel
- **Cas d'usage:** Manipulation d'une hiérarchie de classes tout en ignorant la différence entre **composition d'objets** et **objets individuels**.
- **Exemples d'usages:** Traiter des expressions mathématiques (= opérations communes entre elles).
- **but:**
    - Manipuler un groupe d'objets comme un seul et unique objet. Ces objets doivent partager des opérations communes.
    - concevoir une structure d'arbre, par exemple un arbre binaire.

- **Implémentation:**
    - **Composant**
        - déclare l'interface pour la composition d'objet et l'accès aux composants enfants
        - Met en oeuvre le comportement par défaut
    - **Feuille**
        - Composant n'ayant pas d'enfant (= feuille)
        - implémente le comportement par défaut, c'est à dire le plus primitif vu qu'il est en bas de l'échelle.
    - **Composite**
        - Composant pouvant avoir des enfants
        - Définit un comportement pour les composants ayant des enfants (= utilisant les enfants)
        - stocke ses composants enfants et permet d'y accéder
        - Utilise la gestion des enfants de l'interface *Composant*
    - **Client**
        - manipule les objets de la composition à travers l'interface de la classe Composant
    - **Avantages:**
        - Fournit à l'utilisateur une interface unique pour un objet ou une collection d'objets similaires.
        - L'utilisateur n'a plus besoin de se soucier si les objets sont simples ou composés, car il les manipule de la même façon
        - Il peut y avoir des compositions de compositions, pas de problème.
        - Simple de définir d'autres composants = Définir une nouvelle classe dérivant de la classe *Composant*

**Ex Code**

```c++
/*virtual permet de suplanter une methode, fonction d'une classe parent. C'est à dire les methodes seront définies par défaut dans a classe parent pour être ensuite explicité dans les classes heritées*/
class Expression{
public:
    virtual int evaluer()const=0;
    virtual~Expression(){}
};

class Entier: public Expression{
int e ;
public: 
    Entier(int x):e(x){
    int evaluer()const;
    }
};

//dans l'unité de compilation//
int Entier::evaluer()const{return e;}

class Addition:public Expression{
Expression* ex1;
Expression*ex2;
public : 
    Addition(Expression* x1, Expression*x2):ex1(x1),ex2(x2){}
    int evaluer()const;
};

int Addition::evaluer()const {return ex1->evaluer()+ex2->evaluer();}
```

![UML](https://i.imgur.com/tPYLQNu.png)


## iterator
- **type:** Comportemental
- **cas d'usage:** tableau,conteneur, liste, arbre ...  --> Tout ce qui peut être itéré !
- **but:**
    - parcourir sequentiellement tous les éléments contenus dans un objet agregateur,multiples "traversées" possibles 
    - parcour se fait sans exposr la structures des données du conteneur
- **implementation:** un iterateur possede une valeur qui désigne un élément donné d'un conteneur, on dit qu'il pointe sur l'élément .un itérateur dispose de deux méthodes acceder( currentItem())à l'élément pointé en cours. se deplacer(next())pour pointer vers l'élément suivant. IL faut pouvoir creer un iterateur sur le premier element(first())et il faut pouvoir déterminer à tous les moments si l'itérateur a épuise la totalité des élements du conteneur (isDone().

**Ex code:**
```c++
class Agenda{
    unsigned int nbElemt;
    unsigned int nbMaxElemt;
    Type** tab; // Tableau dynamique de 50 pointeurs d'élement de type Elemt

public:
    // Design Pattern : Iterator
        class iterator{
            Type** cur;
        public:
            iterator(Type** pt=0): cur(pt){}
            iterator& operator++() { cur++; return *this; }
            iterator& operator--() { cur--; return *this; }
            const Evt& operator*() const { return **cur; }
            bool operator==(iterator it) const { return cur == it.cur; }
            bool operator!=(iterator it) const { return cur != it.cur; }
        };
        iterator begin() const { return iterator(tab); }
        iterator end() const { return iterator(tab+nbElemt); }
        /* Suite déclaration de la classe */
```

![UML](https://i.imgur.com/2iFFRJy.png)


## template method
- **type:** Comportemental
- **Cas d'usage:** Classes abstraites, Héritage
- **but:**
    - Définir le squelette d'un algorithme à l'aide d'opérations abstraites
    - fixer clairement des comportements standards qui devraient être partagés par toutes les sous-classes, même lorsque le détail des sous-opérations diffère **-->** fixe un cadre pour toutes les sous-classes d'une classe parent contenant ce design pattern
    - factoriser du code qui serait redondant s'il se trouvait répété dans chaque sous-classe.
- **Utilisation:** c'est la méthode de la classe parent qui appelle des opérations n'existant que dans les sous-classes, pratique courante dans les classes abstraites
- L'utilisateur de la classe abstraite doit bien distinguer parmi les opérations suivantes
    - opération hook : celles qui peuvent être redefinies dans les classes dérivées pour étendre un comportement.
    - les opérations abstraites : celles qui doivent être obligatoirement définies dans les classes dérivées.
    - fournir une interface uniforme pour traverser un ensemble d'objet agrégateurs qui n'ont pas la même structures de données 

**Ex code:** 

```c++
--- .h ---
void ClasseDeBase ::operation(){ 
    // comportement de base indispensable,
    HookOperation();
};
--- .cpp ---
void ClasseDeBase::HookOperation(){
    //peut être vide et ne rien faire
}
void ClassDerivee :: HookOpération(){
//extension
}
```

**Ex littéral:** Définir une classe parent "JeuDeSociété" avec une méthode, qui sera une **template method**, qui s'appellera "JouerUnePartie()". Cette méthode est donc une méthode de base (template), qui a forcément un code de base définissant le comportement général d'une partie commune aux différents jeux, que l'on retrouvera donc dans toutes les classes filles de "JeuDeSociété". Elle a aussi des méthodes "hook" qui elles vont changer selon les jeux, permettant de diversifier le fonctionnement allant au delà du code de base. Par exemple les classes **Echecs** ou encore **Monopoly** auront des méthodes "JouerUnePartie()" ayant un code de base identique MAIS des fonctions Hook() différentes.

![UML](https://i.imgur.com/wL3xRkg.png)


## adaptateur
- **type:** Structurel
- **Cas d'usage:** Adaptation de codes issu d'autres contexte pour une utilisation dans un nouveau contexte.
- **but:**
    -  sert de liaison entre les objets manipulés et un programme les utilisant pour les adapter à ce dernier et permettant la communication entre classes.
    - permet de convertir l'interface d'une classe en une autre interface que le client attend, convenant à l'application qu'il utilise

**Ex code:**

```c++
class InterfaceObjectif{
public: 
        virtual void requete(); //eventuellement pure
};
class Client{
InterfaceObjectif* x;
    public:
        void execute(){x->requete();}
};

class InterfaceAAdapter
{
public:
        void requeteSpecifique();
}

**Heritage multiple (adaptateur de classe)**
    class InterfaceAdaptatrice:
        public InterfaceObjectif, private InterfaceAAdapter{
        public:
            void requete(){InterfaceAAdapter::requeteSpecifique();}
        };
**Heritage simple et composition (adaptateur objet)**

class InterfaceAdaptatrice:
    public InterfaceObjectif{
    InterfaceAAdapte* adaptée;
    public:
        void requete(){adaptee->requeteSpecifique();}
    };
```

**Ex littéral:** 
- Un adaptateur de classe permet de redefinir certains comportements de InterfaceAAdapter puisque INterfaceAdaptatrice en hérite, il introduit seulement un object et aucune indirection supplementaire n'est necessaire pour obtenir INterfaceAAdapter. Cette adaptateur ne peut peut être utilisé pour adapter une classe et ses sous classes.
- Un adaptateur object permet à une interface adaptatrice d'adapter la classe à adapter ainsi que n'importe laquelle de ses sous classes en utilisant un adressage indirect(pointeur ou reference) Il est difficile de redefinir le comportement de interfaceaadapter dans interfaceadaptatrice. LA classe a adapter doit d'abord être sous classée en redefinissant ses comportements. Cette sous classe est alors utilisée à la place de l'ancienne

![UML](https://i.imgur.com/CCFY0At.png)


## visitor
- **type:** Comportemental
- **Cas d'usage:** Lorsqu'on a un ensemble de classes fermées peu permissives.
- **but:**
    - permet d'obtenir le même effet que d'ajouter une **nouvelle méthode virtuelle** à un ensemble de classes qui ne le permet pas.
- **Implémentation:** Chaque classe pouvant être « visitée » doit mettre à disposition une méthode publique « accepter() » prenant comme argument un objet du type « visiteur ». La méthode « accepter() » appellera la méthode « visite() » de l'objet du type « visiteur » avec pour argument l'objet visité. De cette manière, un objet visiteur pourra connaître la référence de l'objet visité et appeler ses méthodes publiques pour obtenir les données nécessaires au traitement à effectuer (calcul, génération de rapport, etc.).

**Exemple**
Soit une classe **Objet**, de laquelle hériteront *ObjetA, ObjetB et ObjetC,* elles posséderont la méthode *accept(Visitor &v)*. On crée ensuite la classe **Visitor**, dont hériteront *Visiteur1* et *Visiteur2*. Dans chacun de ces objets, on retrouvera une méthode visiterObjetA(ObjetA &a), visiterObjetB(ObjetB &b) et visiterObjetB(ObjetB &c).

**Ex code:**
```c++
void ObjetDeTypeA::accept( Visitor& v ) {
   v.visitObjetDeTypeA( *this ) ;
}
void Visitor::visitObjetDeTypeA( ObjetDeTypeA& objet ) {
    // Traitement d'un objet de type A
}

void Visitor::visitObjetDeTypeB( ObjetDeTypeB& objet ) {
    // Traitement d'un objet de type B
}

void Visitor::visitObjetDeTypeC( ObjetDeTypeC& objet ) {
    // Traitement d'un objet de type C
}
```

![UML](https://i.imgur.com/bwtKJxG.png)


## strategy
- **type:** Comportemental
- **Cas d'usage:** Dès lors qu'un objet peut effectuer plusieurs traitements différents, dépendant d'une variable ou d'un état, nécéssitant de permuter dynamiquement les algos utilisés dans l'application; Ces différentes algos mettent en oeuvre une même fonctionnalité finale.
- **but:**
    - Séparer les tâches différentes nécéssitant d'être accessibles / exécutées au sein d'une même applciation.
    - définir une famille d'algorithmes, encapsuler chacun d'eux en tant qu'objet, et les rendre interchangeables
- **Avantages:** intégrer un nouvel algorithme est très simple il suffit de définir une nouvelle classe concrète qui dérive de stratégie.
- **Inconvénients:** L'execution d'un algorithme donne nécéssité à la création d'un nouvel objet Strategie ce qui peut affaiblir les performances s'il faut changer souvent de stratégies

**Ex code:**

```c++
class Tri{
public: 
        virtual void trier(TabInt& tab) const=0;
};

class TriRapide : public Tri{
public:
    void trier(TabInt& tab)const {/*implémentation*/}
};
class TabInt{
public:
        void trier(const Tri& strategie){ strategie.trier(*this);}
};
```

**Ex littéral:** Une stratégie c'est quand un objet peut avoir différents comportement (= méthodes()) possibles, toutes réalisées dans le même but (= cela représente donc plusieurs moyens alternatifs d'y parvenir) dont les choix de l'utilisateur où variables décideront l'exécution de l'une ou l'autre qui marche dans le cas précis, comme une stratégie. Prennons un objet SeigneurDeGuerre, faisant appel à un objet de type Strategie. On définit alors des sous classes de la classe Strategie, disposant tous de la même fonction hérité "MettreEnOeuvre()", et dont le code changera pour chaque sous classes de Stratégie, qui sont:
- DéfoncerLePontLevisDeFace
- PasserParLaFaceNord
- AttendreQueLaVilleSeRende
- SeMarierAvecLaCousineDuDuc

(chacune a donc une méthode "MettreEnOeuvre()", dont le code sera différent entre chaque sous classe)
Cependant le SeigneurDeGuerre adapte sa strategie selon la Météo (nouvelle variable), on peut donc faire un **switch** (CASE) de cette forme:
        switch (météo) {
            **case Météo.Beau:** 
                seigneur.Strategie = new DéfoncerLePontLevisDeFace(); break;
            **case Météo.Brouillard:**
                seigneur.Strategie = new PasserParLaFaceNord(); break;
            **case Météo.Chaud:**
                seigneur.Strategie = new AttendreQueLaVilleSeRende(); break;
            **case Météo.Pleut:**
                seigneur.Strategie = new SeMarierAvecLaCousineDuDuc(); break;
            **default:**
                throw new Exception("Ne Rien faire et agiter le drapeau blanc");

![UML](https://i.imgur.com/vcrjQ2K.png)

## facade
- **type:**
- **Cas d'usage:**
- **Exemples d'usage:**
    - rendre une bibliothèque plus lisible ou/et plus facile à utiliser/comprendre/tester
    - réduire les dépendances entre les clients de la bibliothèque et le fonctionnement interne de celle-ci, ainsi on gagne en flexibilité pour les évolutions futures du système
    - assainir une API que l'on ne peut pas modifier si celle-ci est mal conçue, ou mieux découper ses fonctionnalités si celle-ci n'est pas assez claire.
- **but:**
    - cacher une conception et une interface complexe difficile à comprendre
    - permet de simplifier cette complexité en fournissant une interface simple du sous-système, notamment en réduisant les fonctionnalités avancées du système initial mais en fournissant toutes les fonctions nécéssaires à la plupart des utilisateurs.

- **Structure:**
    - Façade
        - La façade fait abstraction des packages 1, 2 et 3 du reste de l'application.
    - Clients
        - Les objets utilisant le patron de conception Façade pour accéder aux ressources abstraites.
**Ex code**

``` c++
// Subsystem 1
class SubSystemOne
{
public:
	void MethodOne(){ std::cout << "SubSystem 1" << std::endl; };
};

// Subsystem 2
class SubSystemTwo
{
public:
	void MethodTwo(){ std::cout << "SubSystem 2" << std::endl; };
};

// Subsystem 3 
class SubSystemThree
{
public:
	void MethodThree(){ std::cout << "SubSystem 3" << std::endl; }
};


// Facade
class Facade
{
public:
    Facade()
    {
	pOne = new SubSystemOne();
	pTwo = new SubSystemTwo();
	pThree = new SubSystemThree();
    }

    void MethodA()
    {
	std::cout << "Facade::MethodA" << std::endl;
	pOne->MethodOne();
	pTwo->MethodTwo();
    }

    void MethodB()
    {
	std::cout << "Facade::MethodB" << std::endl;
	pTwo->MethodTwo();
	pThree->MethodThree();
    }

private:
    SubSystemOne *pOne;
    SubSystemTwo *pTwo;
    SubSystemThree *pThree;
};

int main()
{
    Facade *pFacade = new Facade();

    pFacade->MethodA();
    pFacade->MethodB();

    return 0;
}

```

![UML](https://i.imgur.com/WpcI0v5.png)

## memento
- **type:** Comportement
- **Cas d'usage:** Lorsqu'un objet doit avoir une fonction lui permettant de retourner à son état précédent.
- **Exemple d'usage:** Générateur de nombres pseudo-aléatoires, machine à états, fonction "annuler" dans un programme.
- **but:** Restaurer à un état précédent un objet sans violer le principe d'encapsulation
- **Implémentation:** Nécéssite 2 objets qui vont l'utiliser:
    - un **Créateur** ayant un état interne (celui à sauvegarder) conservant ainsi la possibilité de revenir en arrière.
    - et un **Gardien** pouvant demander au créateur de retourner en arrière en lui demandant à chaque nouvelle action de créer un objet **Memento** qui est une **sauvegarde de l'objet Créateur** avant la fin de cette action.

**Ex code**
``` c++
#include <iostream.h>
class Number;

class Memento
{
  public:
    Memento(int val)
    {
        _state = val;
    }
  private:
    friend class Number; // not essential, but p287 suggests this
    int _state;
};

class Number
{
  public:
    Number(int value)
    {
        _value = value;
    }
    void dubble()
    {
        _value = 2 * _value;
    }
    void half()
    {
        _value = _value / 2;
    }
    int getValue()
    {
        return _value;
    }
    Memento *createMemento()
    {
        return new Memento(_value);
    }
    void reinstateMemento(Memento *mem)
    {
        _value = mem->_state;
    }
  private:
    int _value;
};

class Command
{
  public:
    typedef void(Number:: *Action)();
    Command(Number *receiver, Action action)
    {
        _receiver = receiver;
        _action = action;
    }
    virtual void execute()
    {
        _mementoList[_numCommands] = _receiver->createMemento();
        _commandList[_numCommands] = this;
        if (_numCommands > _highWater)
          _highWater = _numCommands;
        _numCommands++;
        (_receiver-> *_action)();
    }
    static void undo()
    {
        if (_numCommands == 0)
        {
            cout << "*** Attempt to run off the end!! ***" << endl;
            return ;
        }
        _commandList[_numCommands - 1]->_receiver->reinstateMemento
          (_mementoList[_numCommands - 1]);
        _numCommands--;
    }
    void static redo()
    {
        if (_numCommands > _highWater)
        {
            cout << "*** Attempt to run off the end!! ***" << endl;
            return ;
        }
        (_commandList[_numCommands]->_receiver->*(_commandList[_numCommands]
          ->_action))();
        _numCommands++;
    }
  protected:
    Number *_receiver;
    Action _action;
    static Command *_commandList[20];
    static Memento *_mementoList[20];
    static int _numCommands;
    static int _highWater;
};

Command *Command::_commandList[];
Memento *Command::_mementoList[];
int Command::_numCommands = 0;
int Command::_highWater = 0;

int main()
{
  int i;
  cout << "Integer: ";
  cin >> i;
  Number *object = new Number(i);

  Command *commands[3];
  commands[1] = new Command(object, &Number::dubble);
  commands[2] = new Command(object, &Number::half);

  cout << "Exit[0], Double[1], Half[2], Undo[3], Redo[4]: ";
  cin >> i;

  while (i)
  {
    if (i == 3)
      Command::undo();
    else if (i == 4)
      Command::redo();
    else
      commands[i]->execute();
    cout << "   " << object->getValue() << endl;
    cout << "Exit[0], Double[1], Half[2], Undo[3], Redo[4]: ";
    cin >> i;
  }
}
```

![UML](https://i.imgur.com/vKVpvgz.png)

