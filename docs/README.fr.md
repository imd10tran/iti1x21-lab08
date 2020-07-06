# ITI 1121 - Lab 08

## Submission

Lire le [junit instructions](JUNIT.fr.md) au besoin pour aider
avec les tests du lab.

Lire le [guides de soumission](SUBMISSION.fr.md) avec attention.
Les erreurs de soumission vont affecté votre évaluation.

Soumettez les réponses aux

* Queue.java
* ArrayQueue.java
* Customer.java
* Cashier.java

### Objectifs d'apprentissage

*   Utiliser des files afin de résoudre des problèmes.
*   Implémenter une file d'attente à l'aide de tableaux et des éléments chaînés.



## Introduction

Il y a plusieurs applications des files dans la vie de tous les jours. Les files d'attente en sont un bon exemple. Afin d'améliorer le service à la clientèle, les gestionnaires cherchent des techniques permettant de réduire le temps moyen d'attente ou la longueur moyenne des files. Pour les compagnies aériennes, on a fait l'introduction de files d'attente séparées pour les voyageurs adhérant à un programme de fidélité (frequent flyers), en plus des files normales. Pour les supermarchés, nous avons fait l'ajout de files express, en plus des files normales. Ce sont là des façons de réduire le temps d'attente.

Cependant, les gestionnaires ont besoin d'outils leur permettant de comparer plusieurs scénarios afin d'améliorer les services à moindres coûts. Ils doivent donc connaître le temps moyen d'attente pour chaque scénario.

Il y a deux grandes approches permettant de déterminer ces statistiques. Une branche des mathématiques (Queueing theory) étudie spécifiquement ces questions. L'approche alternative consiste à développer une **simulation informatique** de ces systèmes et de mesurer les valeurs de certains paramètres à l'aide de la simulation.

Pour ce laboratoire, nous développons une simulation des files d'attente pour un supermarché ayant des files express ainsi que des files normales. Les files d'attente seront modélisées à l'aide de files (objets réalisant l'interface **Queue**).

### Implémentation d'une file

Avant tout, assurez-vous de bien comprendre l'implémentation d'une file. D'abord, nous verrons comment implémenter une file à l'aide d'un tableau.

*   [Queue.java](https://www.site.uottawa.ca/~gvj/Courses/ITI1121/lectures/labs/lab8_template/Queue.java)

#### Les méthodes
```java
    public interface Queue<E> {

        public abstract void enqueue( E obj );
        public abstract E dequeue();
        public abstract boolean isEmpty();
        public abstract int size();

    }
```
*   **void enqueue**: ajouter un élément à l'arrière de la file
*   **E dequeue**: retire et retourne l'élément au début de la file
*   **boolean isEmpty**: retourne true si la file est vide, false autrement
*   **int size**: retourne le nombre d'éléments dans la file

#### L'implémentation avec un tableau

Une file doit toujours respecter le principe de first in **first out (FIFO)** (premier entré premier sorti). Vous remarquerez donc dans l'implémentation de ArrayQueue l'usage de **front** (devant) qui indique l’index du premier élément de la file (donc le prochain à sortir) ainsi que **rear** (arrière) indiquant l’index du dernier élément de la file (-1 si la file est vide). La variable **size** permet de conserver la taille réelle de notre file (n'oubliez pas que elems.length n'équivaut pas à size à moins que la file soit pleine!). Enfin, c'est dans un tableau **E elems[]** que les éléments de la file seront sauvegardés.


*   [ArrayQueue.java](https://www.site.uottawa.ca/~gvj/Courses/ITI1121/lectures/labs/lab8_template/ArrayQueue.java)

```java
    1 public class ArrayQueue<E> implements Queue<E> {
    2
    3     // Constant
    4
    5     private static final int MAX_QUEUE_SIZE = 10000;
    6
    7     // Instance variables
    8
    9     private E[] elems; // stores the elements of this queue
    10    private int front, rear, size;
    11
    12    @SuppressWarnings( "unchecked" )
    13
    14    public ArrayQueue() {
    15        elems = (E []) new Object[MAX_QUEUE_SIZE];
    16        front =  0;
    17        rear = -1;
    18        size = 0;
    19    }
    20
    21    // Instance methods
    22
    23    public int size() {
    24        return size;
    25    }
    26
    27    public boolean isEmpty() {
    28        return size == 0;
    29    }
    30
    31    public boolean isFull() {
    32        return size == MAX_QUEUE_SIZE;
    33    }
    34
    35    public void enqueue( E elem ) {
    36
    37        // pre-condition: ???
    38
    39        if ( rear == ( MAX_QUEUE_SIZE -1 ) ) {
    40
    41            int j=0;
    42            for ( int i=front; i<=rear; i++ ) {
    43                elems[ j++ ] = elems[ i ];
    44            }
    45
    46            front = 0;
    47            rear = size - 1;
    48
    49        }
    50
    51        elems[ ++rear ] = elem;
    52        size++;
    53    }
    54
    55    public E dequeue() {
    56
    57        // pre-condition: ???
    58
    59        E saved = elems[ front ];
    60        elems[ front ] = null; // scrubbing the memory!
    61
    62        front++;
    63        size--;
    64
    65        return saved;
    66    }
    67
    68 }
```
**Remarque:** lorsque nous retirons l'élément à la position **front** du tableau dans la méthode **dequeue**, nous assignons en fait la valeur **null** à cet élément et incrémentons **front** afin que le prochain élément dans la file devienne le premier. Ceci cause donc un décalage dans le tableau où les premiers éléments sont null jusqu'à la position **front**. Lorsque le **tableau elems[]** avec lequel nous implémentons notre file devient plein, il est fort probable qu'en réalité il y ait de l'espace dans les premières positions du tableau qui aurait pu être **dequeue**. Les **lignes 39 à 49** permettent donc de décaler ce tableau vers la gauche dans le cas où la variable **rear** aurait atteint la taille maximale du tableau!

Prenez le temps de déterminer les pré-conditions de chacune des méthodes.

#### L'implémentation avec des éléments chaînés

*   [LinkedQueue.java](https://www.site.uottawa.ca/~gvj/Courses/ITI1121/lectures/labs/lab8_template/LinkedQueue.java)
```java
    public class LinkedQueue<E> implements Queue<E> {

        private static class Elem<T> {

            private T value;
            private Elem<T> next;

            private Elem(T value, Elem<T> next) {
                this.value = value;
                this.next = next;
            }
        }

        private Elem<E> front;
        private Elem<E> rear;
      private int size;

      public LinkedQueue() {
        size = 0;
        }

        public int size() {
            return size;
        }

        public void enqueue(E value) {
            Elem<E> newElem;
            newElem = new Elem<E>(value, null );

            if (rear == null) {
                front = rear = newElem;
            } else {
                rear.next = newElem;
                rear = newElem;
            }
        size++;
        }

        public E dequeue() {
            E result = front.value;
            if (front != null && front.next == null) {
                front = rear = null;
            } else {
                front = front.next;
            }
        size--;
            return result;
        }

        public boolean isEmpty() {
            return front == null;
        }

    }
```
Cette implémentation diffère de celle vue précédemment. Nous utilisons cette fois 3 variables d'instance, soit **Elem front, Elem rear** et **int size**. Qu'est-ce que cet objet de type Elem? Il provient de la classe imbriquée statique privée **Elem** (private static class) dans laquelle sont sauvegardées la valeur réelle de l'élément de la file (un type T générique!) ainsi qu'une référence au prochain Elem de la file. Ainsi, **front** est une référence au premier **Elem** de la file, tandis que **rear** est le dernier **Elem** de la file.


**Réflexion:**

*   Dans quel cas est-ce que front == rear?
*   La file peut-elle être pleine?
*   Sans la variable **size**, comment pourrions-nous déterminer la taille de notre file?



## Application d'une file à un problème de la vie courante

![Main classes for the simulation](lab8_img1_supermarket-01.png "Les principales classes de la simulation") **Figure 1 :** Les principales classes de la simulation.

The above figure shows the UML diagrams of the two main classes for the simulation. In addition to these classes, there is an interface, called Queue, as well as a simple implementation, called ArrayQueue. The source code is provided below.

### Customer

Vous devez concevoir une classe afin de modéliser un client. Un client mémorise le moment de son arrivée (une valuer int), le nombre initial de produits (articles), ainsi que le nombre de produits qui n'ont pas encore été traités par le caissier. Le nombre maximal d'articles est le même pour tous les clients (**MAX_NUM_ITEMS**).

*   Quelles sont les **variables d'instances** ?
*   Quelles sont les **variables de classes** ?

L'unique constructeur de cette classe n'a qu'un paramètre. Ce dernier spécifie l'heure à laquelle le client s'est joint à la file. Pour le bien de cet exercice, le temps d'arrivée sera une variable de type **int** Le nombre initial d'articles est déterminé à l'aide de la formule suivante :
```java
    Random generator;
    generator = new Random();

    int numItems;
    numItems = generator.nextInt(MAX_NUM_ITEMS-1)+1;
```
Ainsi, nous nous assurons qu'il y ait au moins un (1) item.

Il y a aussi les méthodes d'instance suivantes :

*   **int getArrivalTime()** retourne l'heure d'arrivée ;
*   **int getNumberOfItems()** retourne le nombre d'articles à traiter ;
*   **int getNumberOfServedItems()** retourne le nombre total d'articles traités ;
*   **serve()** décrémente de 1 le nombre d'articles de ce client.

### Cashier

Un caissier traite une file de clients. Il sert un client à la fois. Puisque le but de cette simulation est d’estimer la valeur de certains paramètres, tel que le temps moyen d’attente, le caissier doit aussi mémoriser certaines données telles que le nombre de clients servis, le temps total passé dans la file d’attente, ainsi que le nombre total d’articles traités.

*   Quelles sont les variables d’instance ainsi que les variables de classe ?

Cette classe n’a qu’un constructeur. Ce dernier n’a aucun paramètre. Il sert à initialiser les variables d’instance de ce caissier.

La méthode **addCustomer(Customer c)** ajoute un client à l’arrière de la file. La méthode **int getQueueSize()** retourne le nombre de clients qui attendent présentement.

La méthode **serveCustomers(int currentTime)** est un élément clé de la simulation. La méthode **serveCustomers** de chaque caissier est appelée exactement une fois par itération. Le paramètre **currentTime** servira à déterminer le temps total qui s’est écoulé depuis que le client a joint la file. Voici le comportement du caissier.

*   Si le caissier n’a pas de clients courants, le prochain client en file devient le client courant, sauf si la file est vide, auquel cas, le caissier sera inactif pour cette itération. Lorsqu’un client est retiré de la file, il faut comptabiliser le temps total passé à attendre ;
*   Le caissier traite un article (appel à la méthode **serve()** du client courant) ;
*   Si tous les articles du client courant ont été traités, le caissier comptabilise le nombre total d’articles traités. Il laisse son client partir (ce caissier n’a plus de client courant).

Il y a aussi 3 méthodes d’instances qui donnent accès aux statistiques de ce caissier : temps total passé en file, nombre total d’articles traités, ainsi que le nombre total de clients servis : **int getTotalCustomerWaitTime()**, **int getTotalItemsServed()**, et **int getTotalCustomersServed()**. Finalement, la méthode String toString() retourne une chaîne de caractères qui présente les statistiques accumulées par ce caissier.

Voici les fichiers requis pour votre file de clients.

*   [Queue.java](https://www.site.uottawa.ca/~gvj/Courses/ITI1121/lectures/labs/lab8_template/Queue.java)
*   [ArrayQueue.java](https://www.site.uottawa.ca/~gvj/Courses/ITI1121/lectures/labs/lab8_template/ArrayQueue.java)

### Simulation

*   [Simulation.java](https://www.site.uottawa.ca/~gvj/Courses/ITI1121/lectures/labs/lab8_template/Simulation.java)

La classe **Simulation** orchestre la simulation. Un objet **Simulation** à deux caissiers, l’un est responsable de la file express, l’autre s’occupe d’une normale. Cet objet mémorise aussi la durée de la simulation. Le constructeur crée les deux caissiers nécessaires.

La méthode **run()** réalise la boucle principale de la simulation. Au début, l’horloge est mise à 0, pour chaque itération, l’horloge avance d’une quantité fixe (TICK).

À chaque itération, la méthode **run()** doit :

*   Déterminer si un nouveau client est arrivé, et si oui, le placer la bonne file, selon le nombre d’articles achetés ;
*   S’assurer que tous les caissiers traitent un client ;
*   Incrémenter la valeur de temps.

À la fin de la simulation, la méthode **run()** affiche les statistiques.

1.  Exécutez la simulation plusieurs fois. Discutez vos observations avec vos voisins.Voici un exemple de sortie :

    SIMULATION ::
    The duration (in seconds) of the simulation was 28800
```java
    EXPRESS LINE ::
    The total number of customers served is 376
    The average number of items per customer was 6
    The average waiting time (in seconds) was 16

    REGULAR LINE ::
    The total number of customers served is 311
    The average number of items per customer was 18
    The average waiting time (in seconds) was 2597
```
3.  Hum, comme vous pouvez le constater le temps moyen d’attente pour la file express est très court (16 secondes !). Cependant, le temps d’attente à la file normale est très long (près de 45 minutes !). Le gérant du supermarché devra ajouter de nouvelles files, mais combien ? Afin de l’aider avec sa prise de décision, vous devez modifier ces classes afin d’y ajouter des caisses normales ;
4.  Si le temps le permet, faites plusieurs expériences où vous allez varier les paramètres de la simulation, changer la probabilité de l’arrivée d’un client, le nombre maximal d’articles, ainsi que le nombre de caisses, express ou normales.

### Resources

*   [https://docs.oracle.com/javase/tutorial/getStarted/application/index.html](https://docs.oracle.com/javase/tutorial/getStarted/application/index.html)
*   [https://docs.oracle.com/javase/tutorial/getStarted/cupojava/win32.html](https://docs.oracle.com/javase/tutorial/getStarted/cupojava/win32.html)
*   [https://docs.oracle.com/javase/tutorial/getStarted/cupojava/unix.html](https://docs.oracle.com/javase/tutorial/getStarted/cupojava/unix.html)
*   [https://docs.oracle.com/javase/tutorial/getStarted/problems/index.html](https://docs.oracle.com/javase/tutorial/getStarted/problems/index.html)
