
# Running junit Tests

Do not commit the junit files.

Prerequisites to the tests working:

* All files must be present.

* All files must compile.

* All files must have the exact name expected.

* All methods must be present with the exact name expected.

* All methods must have the exact signature expected.

Even if you could not complete a particular exercise, the files
must be included, they must compile, and contain all the method
definitions required by the exercise.

## References

* [Download junit 4 jars](https://github.com/junit-team/junit4/wiki/Download-and-Install)


## Usage:

For running the tests, you need the two libraries: junit and hamcrest.

You can use them using two options:

(1) adding the jar files containing the libraries in the same folder as the unit tests

 OR

(2) updating your CLASSPATH.


## Option 1

If you have NOT updated your CLASSPATH to point at the junit libraries, then:

1. Copy all your .java files, the test files, junit-4.13.jar and hamcrest-core-1.3.jar into one folder.

2. From that folder, compile and run TestL08.java:


### Under Windows

```
javac -encoding UTF-8 -cp "junit-4.13.jar;hamcrest-core-1.3.jar;." TestL08.java
java -cp "junit-4.13.jar;hamcrest-core-1.3.jar;." TestL08
```

#### For Mac OSX/Linux/Unix:

```
javac -encoding UTF-8 -cp junit-4.13.jar:hamcrest-core-1.3.jar:. TestL08.java
java -cp junit-4.13.jar:hamcrest-core-1.3.jar:. TestL08
```



***

## Option 2

If you have correctly updated your CLASSPATH to point at the junit libraries (for instructions on how to do that, please see the setup requirements and instructions below), then:

1. Copy all your .java files and all of the test files into one folder

2. From that folder, compile and run TestL08.java:

```
javac TestL08.java
java TestL08
```


### CLASSPATH Setup:

Instructions for updating the CLASSPATH:

1. Download junit and Hamcrest-core jars.

2. Update your CLASSPATH.

#### For Windows:

Go to Setings> Edit the system environment variables > Environment variables > System variables > CLASSPATH.

Add two new entries containing the paths that point to the downloaded jar files. For example:

```
C:\javalibs\junit-4.13.jar
C:\javalibs\hamcrest-core-1.3.jar
```

#### For Mac OSX/Linux/Unix:

Add to .bash_profile (or run in terminal each time):

```
export CLASSPATH=$CLASSPATH:/Users/yourusername/lib/java/junit-4.13/junit-4.13.jar:/Users/yourusername/lib/java/junit-4.13/hamcrest-core-1.3.jar:.
```

replace "yourusername" with your username.
