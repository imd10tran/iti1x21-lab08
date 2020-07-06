
# Running junit Tests

Ne télécharger pas les fichiers junit.

Prérequis au fonctionnement des tests:

* Tous les fichiers doivent être présents.

* Tous les fichiers doivent être compilés.

* Tous les fichiers doivent avoir le nom exact attendu.

* Toutes les méthodes doivent être présentes avec le nom exact attendu.

* Toutes les méthodes doivent avoir la signature exacte attendue.

Même si vous ne pouviez pas terminer un exercice particulier, les fichiers
doivent être inclus, ils doivent compiler et contenir toute les définitions
de méthode requises par l'exercice.

## Références

* [Junit 4 jars](https://github.com/junit-team/junit4/wiki/Download-and-Install)


## Usage:

Pour exécuter les tests, vous avez besoin des deux packages jar: junit et hamcrest.

Vous pouvez les utiliser en utilisant deux options:

(1) ajout des fichiers \*.jar contenant les packages JUnit dans le même dossier que les tests unitaires

 OU

(2) mettre à jour votre CLASSPATH.


## Option 1

Si vous n'avez PAS mis à jour votre CLASSPATH pour pointer vers les packages junit, alors:

1. Copiez tous vos fichiers .java, les fichiers de test, junit-4.13.jar et hamcrest-core-1.3.jar dans un dossier.

2. À partir de ce dossier, compilez et exécutez TestL08.java:

### Sous Windows

```
javac -encoding UTF-8 -cp "junit-4.13.jar;hamcrest-core-1.3.jar;." TestL08.java
java -cp "junit-4.13.jar;hamcrest-core-1.3.jar;." TestL08
```

### Sous Mac OSX, Linux ou Unix :

```
javac -encoding UTF-8 -cp junit-4.13.jar:hamcrest-core-1.3.jar:. TestL08.java
java -cp junit-4.13.jar:hamcrest-core-1.3.jar:. TestL08
```

***

## Option 2

Si vous avez correctement mis à jour votre CLASSPATH pour pointer vers les packages JUnit (pour obtenir des instructions sur la façon de procéder, veuillez consulter les exigences de configuration et les instructions ci-dessous), alors:

1. Copiez tous vos fichiers .java et tous les fichiers de test dans un dossier

2. À partir de ce dossier, compilez et exécutez TestL08.java:

```
javac TestL08.java
java TestL08
```

### CLASSPATH Setup:

Instructions pour la mise à jour du CLASSPATH:

1. Téléchargez les packages junit et hamcrest.

2. Mettez à jour votre CLASSPATH.

#### Pour Windows:

Accédez Setings> Edit the system environment variables > Environment variables > System variables > CLASSPATH.

Ajoutez deux nouvelles entrées contenant les chemins qui pointent vers les fichiers jar téléchargés. Par exemple:

```
C:\javalibs\junit-4.13.jar
C:\javalibs\hamcrest-core-1.3.jar
```

### Sous Mac OSX, Linux ou Unix :

Ajoutez à .bash_profile (ou exécutez dans le terminal à chaque fois):

```
export CLASSPATH=$CLASSPATH:/Users/yourusername/lib/java/junit-4.13/junit-4.13.jar:/Users/yourusername/lib/java/junit-4.13/hamcrest-core-1.3.jar:.
```

remplacez "yourusername" par votre nom d'utilisateur.
