---
layout: page
title: Excepcions
permalink: /excepcions/
nav_order: 3
---

Una **excepció** és un esdeveniment que passa durant l'execució d'un programari, i que trenca el flux normal d'execució d'instruccions.

Quan hi ha una excepció, el mètode on passa crea un objecte (**excepció**) i la llença a l'entorn d'execució (**throws**). Aquest objecte indica què ha passat i quin era l'estat quan ha passat. L'objecte de l'excepció inclou la llista de mètodes que s'havia cridat quan passa l'excepció, o **call stack**.

L'entorn d'execució busca un bloc de codi que pugui **gestionar** l'excepció. Aquesta cerca es fa des del mètode on es produeix l'excepció i en ordre invers de crida, fins arribar al mètode **main**. Quan es troba el codi que gestiona l'excepció (**catch**), l'entorn d'execució passa l'excepció al codi corresponent. Si no troba cap, el programa acaba.

La gestió de l'excepció és apropiada si el tipus de l'excepció coincideix amb el de la gestió de l'excepció.

Avantatges de les excepcions:

*   Permeten separar el codi de gestió d'errors del codi normal.
*   Permeten propagar els errors, tenint un punt comú per gestionar errors d'un cert tipus.
*   Permet agrupar i diferenciar tipus d'errors, utilitzant gestors més o menys generals.

## Tipus d'excepcions

*   **Checked**: són les excepcions que cal gestionar obligatòriament. Exemple: FileNotFoundException. Hi ha dues formes de gestionar-les:
    *   una sentència "**try/catch**"que gestioni l'excepció

```java
try {
    // codi que llença una excepció
} catch (SomeException e) {
    // codi que gestiona l'excepció 
}
```

*   que el mètode especifiqui que pot llençar aquesta excepció (**throws**)

```java
public void metode() throws SomeException {
    // codi que llença una excepció
}
```

*   **Unchecked**: són les excepcions que no cal gestionar.
    *   **Errors** (tipus Error): són esdeveniments excepcionals (del tipus Error) i habitualment irrecuperables. Exemple: OutOfMemoryError.
    *   Excepcions de l'entorn d'execució (tipus **RuntimeException**): són excepcionals i associats a l'aplicació. Exemple: NullPointerException.

![Tipus d'excepcions](/images/exceptions.png)

## Gestió d'excepcions

### Throws

Es pot tractar una excepció simplement fent throws:

    public void metode() throws FileNotFoundException { ...

### Try-catch

Podem gestionar l'excepció rellançant-la:


```java
try {
catch (SomeException e} {
    throw new AnotherException("un missatge");
}
```

O bé fent accions per recuperar-nos:

```java
try {
catch (SomeException e} {
    return VALOR_PER_DEFECTE;
}
```

### Finally

Quan volem executar algun codi, passi o no passi l'excepció. Pot servir per alliberar recursos.

```java
try {
    // ...
} catch (SomeException e) {
    // ...
} finally {
    // el codi aqui sempre s'executarà 
}
```

### Try-with-resources

Permet treballar amb recursos, i tancar-los sense fer-ho explícitament al codi. Cal que el recurs implementi AutoCloseable.

    try (AutoCloseableObject o = new AutoCloseableObject()) {
      // ...
    } catch (SomeException e) {
      // ...
    }

### Multiples catch

Permet gestionar diverses excepcions. L'ordre és important: si tenim excepcions que comparteixen tipus, cal que estiguin les més específiques abans.

```java
try {
    // ...
} catch (SomeEspecificException e) {
    // ...
} catch (LessEspecificException e) {
    // ...
} catch (AnotherException e) {
    // ...
}
```

### Union

Permet gestionar diverses excepcions al mateix bloc.

```java
try {
    // ...
} catch (SomeException | SomeOtherException | AnotherException e) {
    // ...
}
```

## Excepcions pròpies

Habitualment, tenim diverses signatures per crear un objecte d'excepció. Tot i que els paràmetres són lliures (podem afegir els que ens ajudin a interpretar el nostre error particular), dos sovintegen:

*   el missatge d'error, un String.
*   una excepció, la causa. Permet enllaçar excepcions amb les seves causes (**wrapping**).

Per exemple, una excepció checked estenent `Exception`:

```java
public class SomeException extends Exception {    
    public SomeException (String message, Throwable t) {
        super(message, t);
    }
}
```

Per exemple, una excepció unchecked estenent `RuntimeException`:

```java
public class SomeException extends RuntimeException {
    public SomeException (String message, Throwable t) {
        super(message, t);
    }
}
```

## Llençant excepcions

### Checked

Es pot llençar, i cal especificar-ho.

```java
public void metode() throws SomeException {
    // ...
    throw new SomeException("some text");
}
```

### Unchecked

No cal especificar-la:

```java
public void metode() {
    // ...
    throw new SomeException("some text");
}
```

### Rethrowing

Tornem a llençar l'excepció gestionada:

```java
try {
    // ...
} catch (SomeException e) {
    throw e;
}
```

### Wrapping

Fem un embolcall:

```java
try {
    // ...
} catch (SomeException e) {
    throw new SomeOtherException("una explicació", e);
}
```

### Herència

Quan sobreescribim un mètode, podem fer que la subclasse tingui una signatura menys arriscada, o sigui, sense el throws.

Veure la [interpretació de les excepcions](/pindoles-java/#interpretació-de-les-excepcions).

## Indicacions

La tentació d'un programador podria ser utilitzar excepcions **unchecked**, ja que no cal gestionar-les ni declarar-les als mètodes.

Quan un mètode especifica una excepció a la seva signatura, està demanant a qui el crida què vol fer: si tornar a llençar o bé gestionar l'excepció. Les excepcions unchecked són el resultat d'esdeveniments no recuperables, i no té sentit que hagin de ser declarades (tot i que seria possible) per tot arreu, ja que el codi no seria clar.

**La regla seria**: si una crida pot recuperar-se d'una excepció, fes que sigui **checked**. Si no pot fer res, fes que sigui **unchecked**.

### Anti-patrons

Alguns anti-patrons típics:

*   Empassar-se excepcions
*   Fer return a un finally
*   Fer throw a un finally
*   Utilitzar throw com a goto

## Logging

El registre en logs (**logging**) permet organitzar d'una forma més óptima els missatges generats en temps d'execució per un programari, i és una alternativa millor que els `System.out.println`. Té els següents avantatges:

*   Permet assignar una prioritat als missatges, i posteriorment filtrar-los. Tant de forma global com per paquets (packages) i classes.
*   Permet reencaminar els missatges a diferents destins, com consola, fitxer, etc.
*   Permet adaptar el format dels missatges, com per exemple en text, en XML o a base de dades.

Java conté a la seva llibreria base el paquet `java.util.logging`. Aquest paquet pot ser utilitzat de forma bàsica de la següent forma:

```java
private static final Logger LOGGER = Logger.getLogger(LaMevaClasse.class.getName());
```

Aquesta sentència es pot afegir al començament d'una classe, per tal de fer-hi referència posteriorment. Per exemple:

```java
LOGGER.log(Level.INFO, "un missatge");
```

El primer paràmetre és el nivell de la notificació, i el segon el missatge. El nivell pot variar entre SEVERE (valor més alt), WARNING, INFO, CONFIG, FINE, FINER i FINEST (valor més baix).

A més, aquest mètode permet utilitzar paràmetres per posició amb el codi {0}, {1}... o passant una excepció.

```java
LOGGER.log(Level.FINE, "un missatge amb paràmetre {0}", "1");
LOGGER.log(Level.WARNING, "un missatge amb paràmetres {0} i {1}", new Object[]{"1", 2});
LOGGER.log(Level.SEVERE, "un missatge sense paràmetre i amb excepció", new RuntimeException("una excepció"));
```

Tot i que el mètode `log(String)` és el més estàndard i convenient, també hi ha mètodes de `Logger` per a cada nivell que faciliten l'escriptura:

```java
severe(String), warning(String), info(String), config(String), fine(String), finer(String), finest(String)
```

### Configuració

Si tenim un arxiu `loggingConfigFile` (tipus `File`) amb l'arxiu de configuració, hem de configurar el registre al començament de l'execució del programari mitjançant la classe [LogManager](https://docs.oracle.com/javase/8/docs/api/java/util/logging/LogManager.html):

```java
LogManager.getLogManager().readConfiguration(new FileInputStream(loggingConfigFile));
```

Aquesta podria ser una configuració mínima amb nivell màxim de FINE que utilitza un formatador, [SimpleFormatter](https://docs.oracle.com/javase/8/docs/api/java/util/logging/SimpleFormatter.html) per a la consola i un arxiu:

```
# two handlers defined:
handlers= java.util.logging.ConsoleHandler, java.util.logging.FileHandler

# default root level, overrided for package and for the LoggingTest class:
level = FINE
test.excepcions.level = INFO
test.excepcions.LoggingTest.level = FINEST

# console handler configuration (https://docs.oracle.com/javase/8/docs/api/java/util/logging/ConsoleHandler.html)
java.util.logging.ConsoleHandler.level = FINEST
java.util.logging.ConsoleHandler.formatter = java.util.logging.SimpleFormatter

# default SimpleFormatter format
java.util.logging.SimpleFormatter.format=%1$tY-%1$tm-%1$td %1$tH:%1$tM:%1$tS.%1$tL %4$-7s [%3$s] %5$s %6$s%n

# file handler configuration (https://docs.oracle.com/javase/8/docs/api/java/util/logging/FileHandler.html)
java.util.logging.FileHandler.level = ALL
java.util.logging.FileHandler.formatter=java.util.logging.SimpleFormatter
java.util.logging.FileHandler.pattern = %h/logging_test_%g.log
java.util.logging.FileHandler.limit = 1000000
java.util.logging.FileHandler.count = 10
```

El nivell FINE s'utilitza per defecte per tots els missatges. Es podria configurar per classe:
```
test.LoggingTest.level = FINEST
```

## Referències

*   [Compile and runtime errors in Java](https://introcs.cs.princeton.edu/java/11cheatsheet/errors.pdf)
*   [Java Logging (Jenkov)](http://tutorials.jenkov.com/java-logging/index.html)
*   [Java Logging Overview (Oracle)](https://docs.oracle.com/javase/8/docs/technotes/guides/logging/overview.html)