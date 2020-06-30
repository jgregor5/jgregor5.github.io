---
layout: page_toc
title: Píndoles Java
permalink: /pindoles-java/
nav_order: 3
---
## Noms a Java

**CamelCase**: pràctica d’escriure frases o paraules compostes eliminant espais i posant en majúscula la primera lletra de cada paraula.

Pot ser **UpperCamelCase** o **lowerCamelCase**.

Es recomanable evitar caràcters que no siguin lletres o números.

*   **Classes** (i Interfaces): **noms** en UpperCamelCase
*   **Mètodes**: **verbs** en lowerCamelCase
*   **Variables**: en lowerCamelCase. Han de ser mnemònics i evitar variables d’una lletra, excepte per temporals: i,j,k,m,n (sencers) i c,d,e (caràcters)
*   **Constants**: paraules en majúscules separades per subratllat (underscore)
*   **Paquets** (packages): paraules en minúscules separades per punts. Solen tenir format de domini invertit (com.domain.subdomain). S’ha d’evitar el default package (sense nom)

## Valors inicials de les variables

*   Each **class** variable, **instance** variable, or **array** component is initialized with a default value when it is created:
    *   For type byte, the default value is zero, that is, the value of (byte)0.
    *   For type short, the default value is zero, that is, the value of (short)0.
    *   For type int, the default value is zero, that is, 0.
    *   For type long, the default value is zero, that is, 0L.
    *   For type float, the default value is positive zero, that is, 0.0f.
    *   For type double, the default value is positive zero, that is, 0.0d.
    *   For type char, the default value is the null character, that is, '\\u0000'.
    *   For type boolean, the default value is false.
    *   For all reference types, the default value is null.
*   Each **method** parameter is initialized to the corresponding argument value provided by the invoker of the method.
*   Each **constructor** parameter is initialized to the corresponding argument value provided by a class instance creation expression or explicit constructor invocation.
*   An **exception** parameter is initialized to the thrown object representing the exception.
*   A **local variable** must be explicitly given a value before it is used, by either initialization or assignment, in a way that can be verified using the rules for definite assignment.

## Expressions i conversió de tipus

### Ordre de les expressions

A Java, les expressions s'avaluen d'esquerra a dreta.

Java no executarà parts de l'expressió quan no sigui necessari. En particular:

*   Si tenim expressions AND i el resultat d'una a l'esquerra és `false`, ja no es continua avaluant a la dreta.
*   Si tenim expressions OR i el resultat d'una a l'esquerra és `true`, ja no es continua avaluant a la dreta.

### Tipus primitius

Quan diversos tipus intervenen en una expressió, tots es converteixen al mateix tipus mitjançant unes normes de promoció. Per començar, tots els char / byte / short es promouen a int. Si hi ha algun long, a long. Si hi ha algun float, a float. I si hi ha algun double, a double.

La conversió només es pot fer amb tipus numèrics (exclou el boolean). Hi ha de dos tipus:

*   **Widening**: cap a un tipus més ample, no cal utilitzar cap notació.
*   **Narrowing**: cap a un tipus més estret, cal utilitzar el cast: ( tipus ). La conversió pot perdre informació.

També tenim el boxing / unboxing, que permet convertir automàticament entre els tipus primitius i les classes embolcall.

### Objectes

Tenim dos tipus:

*   **Upcasting**: quan volem passar el tipus d'un objecte des de la subclasse a una superclasse.
*   **Downcasting**: quan volem passar el tipus d'un objecte des d'una superclasse a una subclasse.

L'operació de downcasting sol venir precedida de l'ús de l'operador `instanceof`. Aquest operador retorna true si la classe és del tipus que es pregunta, si és una subclasse o si implementa la interfície.

## Emmagatzematge de variables

![](/images/variables.png)

Java passa els paràmetres d'un mètode **per valor**. Però el valor d'un objecte és una referència. Això també inclou qualsevol array (de primitius o objectes).

Per tant, mai podem modificar el valor d'una variable primitiva, ni el d'un objecte (la referència). El que es pot fer és, si l'objecte és mutable, modificar-lo.

## Precedència d'operadors

![](/images/precedence.png)

## Local vs Instance vs Class variables

characteristic | Local variable | Instance variable | Class variable
-------------- | -------------- | ----------------- | --------------
Where declared | In a method, constructor, or block. | In a class, but outside a method. Typically private. | In a class, but outside a method. Must be declared static. Typically also final. 
Use | Local variables hold values used in computations in a method. | Instance variables hold values that must be referenced by more than one method (for example, components that hold values like text fields, variables that control drawing, etc), or that are essential parts of an object's state that must exist from one method invocation to another. | Class variables are mostly used for constants, variables that never change from their initial value.
Lifetime | Created when method or constructor is entered. Destroyed on exit. | Created when instance of class is created with new. Destroyed when there are no more references to enclosing object (made available for garbage collection). | Created when the program starts. Destroyed when the program stops. 
Scope/Visibility | Local variables (including formal parameters) are visible only in the method, constructor, or block in which they are declared. Access modifiers (private, public, ...) can not be used with local variables. All local variables are effectively private to the block in which they are declared. No part of the program outside of the method / block can see them. A special case is that local variables declared in the initializer part of a for statement have a scope of the for statement. | Instance (field) variables can been seen by all methods in the class. Which other classes can see them is determined by their declared access: private should be your default choice in declaring them. No other class can see private instance variables. This is regarded as the best choice. Define getter and setter methods if the value has to be gotten or set from outside so that data consistency can be enforced, and to preserve internal representation flexibility. Default (also called package visibility) allows a variable to be seen by any class in the same package. private is preferable. public. Can be seen from any class. Generally a bad idea. protected variables are only visible from any descendant classes. Uncommon, and probably a bad choice. | Same as instance variable, but are often declared public to make constants available to users of the class.
Declaration | Declare before use anywhere in a method or block. | Declare anywhere at class level (before or after use). | Declare anywhere at class level with static.
Initial value | None. Must be assigned a value before the first use. | Zero for numbers, false for booleans, or null for object references. May be assigned value at declaration or in constructor. | Same as instance variable, and it addition can be assigned value in special static initializer block.
Access from outside | Impossible. Local variable names are known only within the method. | Instance variables should be declared private to promote information hiding, so should not be accessed from outside a class. However, in the few cases where there are accessed from outside the class, they must be qualified by an object (eg, myPoint.x). | Class variables are qualified with the class name (eg, Color.BLUE). They can also be qualified with an object, but this is a deceptive style.
Name syntax | Standard rules. | Standard rules, but are often prefixed to clarify difference from local variables, eg with my, m, or m_ (for member) as in myLength, or this as in this.length. | static public final variables (constants) are all uppercase, otherwise normal naming conventions. Alternatively prefix the variable with "c_" (for class) or something similar.

## Checked versus unchecked exceptions

### Unchecked exceptions:

*   represent defects in the program (bugs) - often invalid arguments passed to a non-private method. To quote from _The Java Programming Language_, by Gosling, Arnold, and Holmes: "Unchecked runtime exceptions represent conditions that, generally speaking, reflect errors in your program's logic and cannot be reasonably recovered from at run time."
*   are subclasses of [`RuntimeException`](https://docs.oracle.com/javase/8/docs/api/java/lang/RuntimeException.html), and are usually implemented using [`IllegalArgumentException`](https://docs.oracle.com/javase/8/docs/api/java/lang/IllegalArgumentException.html), [`NullPointerException`](https://docs.oracle.com/javase/8/docs/api/java/lang/NullPointerException.html), or [`IllegalStateException`](https://docs.oracle.com/javase/8/docs/api/java/lang/IllegalStateException.html)
*   a method is _not_ obliged to establish a policy for the unchecked exceptions thrown by its implementation (and they almost always do not do so)

### Checked exceptions:

*   represent invalid conditions in areas outside the immediate control of the program (invalid user input, database problems, network outages, absent files)
*   are subclasses of [`Exception`](https://docs.oracle.com/javase/8/docs/api/java/lang/Exception.html)
*   a method is _obliged_ to establish a policy for all checked exceptions thrown by its implementation (either pass the checked exception further up the stack, or handle it somehow)

## Interpretació de les excepcions

Hem vist que podem tenir excepcions Checked (subclasses de `Exception`) i Unchecked (subclasses de `RuntimeException`).

Si mirem la jerarquia de classes, totes extenen `Throwable`, la classe pare de totes:

*   [java.lang.Throwable](https://docs.oracle.com/javase/8/docs/api/java/lang/Throwable.html)
    *   [java.lang.Error](https://docs.oracle.com/javase/8/docs/api/java/lang/Error.html)
    *   [java.lang.Exception](https://docs.oracle.com/javase/8/docs/api/java/lang/Exception.html)
        *   [java.lang.RuntimeException](https://docs.oracle.com/javase/8/docs/api/java/lang/RuntimeException.html)

Els tres mètodes més importants de `Throwable` són:

*   `public String getMessage()`: conté el missatge d'excepció.
*   `public StackTraceElement[] getStackTrace()`: conté la traça de la pila de l'excepció.
*   `public Throwable getCause()`: opcionalment, conté una referència a l'excepció que ha causat aquesta. Pot ser una cadena de diverses excepcions.

### Stacktrace

Un stacktrace no ha de ser necessàriament un error. Es pot obtenir el valor actual mitjançant aquest codi:

```java
public class TestStackTrace {
    public static void one() {
        two();
    }

    public static void two() {
        three();
    }

    public static void three() {
        StackTraceElement[] trace = Thread.currentThread().getStackTrace();
        for (StackTraceElement elem : trace) {
            System.out.println(elem);
        }
    }

    public static void main(String[] args) {
        one();
    }
}
```

### Cause

Si pots, guarda la causa quan facis throw d'una excepció més específica.

```java
    try {
      doSomething();
    } catch (NumberFormatException e) {
      throw new MyBusinessException(e, ErrorCode.CONFIGURATION_ERROR);
    } catch (IllegalArgumentException e) {
      throw new MyBusinessException(e, ErrorCode.UNEXPECTED);
    }
```

### Estructura típica

```java
paquet.NomDeLaException: missatge que explica la excepció al getMessage()
at paquet.Classe.metode(Classe.java:XXX)
at paquet.Classe.metode(Classe.java:XXX)
...
Caused by: paquet.NomDeLaException: missatge que explica la excepció al getMessage()
at paquet.Classe.metode(Classe.java:XXX)
at paquet.Classe.metode(Classe.java:XXX)
...
```

### Exemple

A continuació veiem una excepció amb les causes encadenades (Caused by). Els punts suspensius expressen la repetició de les línies respecte de l'excepció pare.

```java
org.hibernate.service.spi.ServiceException: Unable to create requested service [org.hibernate.engine.jdbc.env.spi.JdbcEnvironment]
 at org.hibernate.service.internal.AbstractServiceRegistryImpl.createService(AbstractServiceRegistryImpl.java:275)
 at org.hibernate.service.internal.AbstractServiceRegistryImpl.initializeService(AbstractServiceRegistryImpl.java:237)
 at org.hibernate.service.internal.AbstractServiceRegistryImpl.getService(AbstractServiceRegistryImpl.java:214)
 at org.hibernate.id.factory.internal.DefaultIdentifierGeneratorFactory.injectServices(DefaultIdentifierGeneratorFactory.java:152)
 at org.hibernate.service.internal.AbstractServiceRegistryImpl.injectDependencies(AbstractServiceRegistryImpl.java:286)
 at org.hibernate.service.internal.AbstractServiceRegistryImpl.initializeService(AbstractServiceRegistryImpl.java:243)
 at org.hibernate.service.internal.AbstractServiceRegistryImpl.getService(AbstractServiceRegistryImpl.java:214)
 at org.hibernate.boot.internal.InFlightMetadataCollectorImpl.<init>(InFlightMetadataCollectorImpl.java:179)
 at org.hibernate.boot.model.process.spi.MetadataBuildingProcess.complete(MetadataBuildingProcess.java:119)
 at org.hibernate.jpa.boot.internal.EntityManagerFactoryBuilderImpl.metadata(EntityManagerFactoryBuilderImpl.java:904)
 at org.hibernate.jpa.boot.internal.EntityManagerFactoryBuilderImpl.build(EntityManagerFactoryBuilderImpl.java:935)
 at org.hibernate.jpa.HibernatePersistenceProvider.createEntityManagerFactory(HibernatePersistenceProvider.java:56)
 at javax.persistence.Persistence.createEntityManagerFactory(Persistence.java:79)
 at javax.persistence.Persistence.createEntityManagerFactory(Persistence.java:54)
 at cotxes.CotxesDAO.getEMF(CotxesDAO.java:205)
 at cotxes.CotxesDAO.access$400(CotxesDAO.java:15)
 at cotxes.CotxesDAO$JPATransaction.result(CotxesDAO.java:223)
 at cotxes.CotxesDAO.findMarques(CotxesDAO.java:81)
 at cotxes.ProvaCotxes.processCommand(ProvaCotxes.java:69)
 at cotxes.ProvaCotxes.prova(ProvaCotxes.java:49)
 at cotxes.ProvaCotxes.main(ProvaCotxes.java:18)

Caused by: org.hibernate.exception.JDBCConnectionException: Error calling DriverManager#getConnection
 at org.hibernate.exception.internal.SQLStateConversionDelegate.convert(SQLStateConversionDelegate.java:115)
 at org.hibernate.exception.internal.StandardSQLExceptionConverter.convert(StandardSQLExceptionConverter.java:42)
 at org.hibernate.engine.jdbc.spi.SqlExceptionHelper.convert(SqlExceptionHelper.java:113)
 at org.hibernate.engine.jdbc.connections.internal.BasicConnectionCreator.convertSqlException(BasicConnectionCreator.java:118)
 at org.hibernate.engine.jdbc.connections.internal.DriverManagerConnectionCreator.makeConnection(DriverManagerConnectionCreator.java:37)
 at org.hibernate.engine.jdbc.connections.internal.BasicConnectionCreator.createConnection(BasicConnectionCreator.java:58)
 at org.hibernate.engine.jdbc.connections.internal.DriverManagerConnectionProviderImpl$PooledConnections.addConnections(DriverManagerConnectionProviderImpl.java:363)
 at org.hibernate.engine.jdbc.connections.internal.DriverManagerConnectionProviderImpl$PooledConnections.<init>(DriverManagerConnectionProviderImpl.java:282)
 at org.hibernate.engine.jdbc.connections.internal.DriverManagerConnectionProviderImpl$PooledConnections.<init>(DriverManagerConnectionProviderImpl.java:260)
 at org.hibernate.engine.jdbc.connections.internal.DriverManagerConnectionProviderImpl$PooledConnections$Builder.build(DriverManagerConnectionProviderImpl.java:401)
 at org.hibernate.engine.jdbc.connections.internal.DriverManagerConnectionProviderImpl.buildPool(DriverManagerConnectionProviderImpl.java:112)
 at org.hibernate.engine.jdbc.connections.internal.DriverManagerConnectionProviderImpl.configure(DriverManagerConnectionProviderImpl.java:75)
 at org.hibernate.boot.registry.internal.StandardServiceRegistryImpl.configureService(StandardServiceRegistryImpl.java:100)
 at org.hibernate.service.internal.AbstractServiceRegistryImpl.initializeService(AbstractServiceRegistryImpl.java:246)
 at org.hibernate.service.internal.AbstractServiceRegistryImpl.getService(AbstractServiceRegistryImpl.java:214)
 at org.hibernate.engine.jdbc.env.internal.JdbcEnvironmentInitiator.buildJdbcConnectionAccess(JdbcEnvironmentInitiator.java:145)
 at org.hibernate.engine.jdbc.env.internal.JdbcEnvironmentInitiator.initiateService(JdbcEnvironmentInitiator.java:66)
 at org.hibernate.engine.jdbc.env.internal.JdbcEnvironmentInitiator.initiateService(JdbcEnvironmentInitiator.java:35)
 at org.hibernate.boot.registry.internal.StandardServiceRegistryImpl.initiateService(StandardServiceRegistryImpl.java:94)
 at org.hibernate.service.internal.AbstractServiceRegistryImpl.createService(AbstractServiceRegistryImpl.java:263)
 ... 20 more

Caused by: com.mysql.jdbc.exceptions.jdbc4.CommunicationsException: Communications link failure
The last packet sent successfully to the server was 0 milliseconds ago. The driver has not received any packets from the server.
 at sun.reflect.NativeConstructorAccessorImpl.newInstance0(Native Method)
 at sun.reflect.NativeConstructorAccessorImpl.newInstance(NativeConstructorAccessorImpl.java:62)
 at sun.reflect.DelegatingConstructorAccessorImpl.newInstance(DelegatingConstructorAccessorImpl.java:45)
 at java.lang.reflect.Constructor.newInstance(Constructor.java:408)
 at com.mysql.jdbc.Util.handleNewInstance(Util.java:425)
 at com.mysql.jdbc.SQLError.createCommunicationsException(SQLError.java:990)
 at com.mysql.jdbc.MysqlIO.<init>(MysqlIO.java:342)
 at com.mysql.jdbc.ConnectionImpl.coreConnect(ConnectionImpl.java:2197)
 at com.mysql.jdbc.ConnectionImpl.connectOneTryOnly(ConnectionImpl.java:2230)
 at com.mysql.jdbc.ConnectionImpl.createNewIO(ConnectionImpl.java:2025)
 at com.mysql.jdbc.ConnectionImpl.<init>(ConnectionImpl.java:778)
 at com.mysql.jdbc.JDBC4Connection.<init>(JDBC4Connection.java:47)
 at sun.reflect.NativeConstructorAccessorImpl.newInstance0(Native Method)
 at sun.reflect.NativeConstructorAccessorImpl.newInstance(NativeConstructorAccessorImpl.java:62)
 at sun.reflect.DelegatingConstructorAccessorImpl.newInstance(DelegatingConstructorAccessorImpl.java:45)
 at java.lang.reflect.Constructor.newInstance(Constructor.java:408)
 at com.mysql.jdbc.Util.handleNewInstance(Util.java:425)
 at com.mysql.jdbc.ConnectionImpl.getInstance(ConnectionImpl.java:386)
 at com.mysql.jdbc.NonRegisteringDriver.connect(NonRegisteringDriver.java:330)
 at java.sql.DriverManager.getConnection(DriverManager.java:664)
 at java.sql.DriverManager.getConnection(DriverManager.java:208)
 at org.hibernate.engine.jdbc.connections.internal.DriverManagerConnectionCreator.makeConnection(DriverManagerConnectionCreator.java:34)
 ... 35 more

Caused by: java.net.ConnectException: Connection refused
 at java.net.PlainSocketImpl.socketConnect(Native Method)
 at java.net.AbstractPlainSocketImpl.doConnect(AbstractPlainSocketImpl.java:345)
 at java.net.AbstractPlainSocketImpl.connectToAddress(AbstractPlainSocketImpl.java:206)
 at java.net.AbstractPlainSocketImpl.connect(AbstractPlainSocketImpl.java:188)
 at java.net.SocksSocketImpl.connect(SocksSocketImpl.java:392)
 at java.net.Socket.connect(Socket.java:589)
 at com.mysql.jdbc.StandardSocketFactory.connect(StandardSocketFactory.java:211)
 at com.mysql.jdbc.MysqlIO.<init>(MysqlIO.java:301)
 ... 50 more
```

## Genèrics

### Mètodes genèrics

*   Totes les declaracions genèriques del mètode tenen una secció de paràmetre tipus delimitada per claudàtors d'angle (\<i\>) que precedeix el tipus de retorn del mètode (\<E\> en el següent exemple).
*   Cada secció de paràmetres de tipus conté un o més paràmetres de tipus separats per comes. Un paràmetre tipus, també conegut com a variable de tipus, és un identificador que especifica un nom de tipus genèric.
*   Els paràmetres de tipus es poden utilitzar per declarar el tipus de devolució i actuar com a marcadors per als tipus d’arguments passats al mètode genèric, que es coneixen com a arguments de tipus reals.
*   El cos d'un mètode genèric es declara com el de qualsevol altre mètode. Tingueu en compte que els paràmetres de tipus només poden representar tipus de referència, no tipus primitius (com int, double i char).

```java
public static <E> void printArray( E[] inputArray ) {
    // Display array elements
    for(E element : inputArray) {
        System.out.printf("%s ", element);
    }
    System.out.println();
}
```

### Paràmetres de tipus delimitat

Hi pot haver moments en què voldreu restringir els tipus de tipus que es permeten passar a un tipus de paràmetre. Per exemple, un mètode que opera sobre números només pot voler acceptar instàncies de Number o de les seves subclasses. Per a què serveixen els paràmetres del tipus de límit.

Per declarar un paràmetre de tipus delimitat, enumereu el nom del paràmetre del tipus, seguit de la paraula clau `extends`, seguit tipus delimitant.

```java
public static <T extends Comparable<T>> T maximum(T x, T y, T z) {
    T max = x;   // assume x is initially the largest
    if(y.compareTo(max) > 0) {
        max = y;   // y is the largest so far
    }
    if(z.compareTo(max) > 0) {
        max = z;   // z is the largest now                 
    }
    return max;   // returns the largest object   
}
```

### Classes genèriques

Una declaració de classe genèrica sembla una declaració de classe no genèrica, tret que el nom de classe sigui seguit per una secció de paràmetre tipus.

Com en els mètodes genèrics, la secció de paràmetres de tipus d'una classe genèrica pot tenir un o més paràmetres de tipus separats per comes. Aquestes classes es coneixen com a classes parametrizades o tipus parametrizats perquè accepten un o més paràmetres.

```java
public class Box<T> {
    private T t;

    public void add(T t) {
        this.t = t;
    }

    public T get() {
        return t;
    }
}
```

## Classes dins de classes

Java permet definir classes dins de classes. Això permet agrupar i encapsular, fent més llegible i gestionable el codi.

### Classes estàtiques imbricades

És una classe relacionada amb la classe exterior, però que no pot fer referència a les variables d'instància d'aquesta. De fet, és com una classe normal que s'ha imbricat dins d'una altra, per qüestions de conveniència.

```java
class OuterClass {
    ...
    static class StaticNestedClass {
        ...
    }
}
```

### Classes imbricades

Les classes imbricades (no estàtiques) estan associades amb una instància de la classe que les conté.

```java
class OuterClass {
    ...
    class InnerClass {
        ...
    }
}
```

Si una declaració utilitza una variable o paràmetre amb el mateix nom que una altra a un àmbit que el conté, llavors la declaració encobreix aquesta, i podem accedir-hi amb la sintaxi `OuterClass.this.variable`.

### Classes locals

Una classe es pot definir localment, en qualsevol bloc de codi. Aquestes classes només poden accedir variables locals que siguin **finals** o **efectivament finals**: el seu valor no canvia després d'una declaració amb inicialització.

```java
public class LocalClassExample {
    ...
    public static void metode() {
        ...
        class LocalClass {
            ...
        }
    }
}
```

### Classes anònimes

Les classes anònimes permeten declarar i inicialitzar una classe al mateix temps. Són com classes locals, però sense nom. S'utilitzen quan només cal utilitzar una classe un cop. Es consideren expressions.

Es componen de:

*   L'operador new.
*   El nom d'una interfície a implementar o una classe a estendre.
*   Els parèntesis amb els arguments d'un constructor.
*   Un bloc de codi de la classe.

```java
...
Runnable tasca = new Runnable() {
    @Override
    public void run() {
        ...
    }
};    
...
```

Les classes anònimes no poden contenir declaracions de constructors, ni poden accedir a variables locals que no siguin finals o efectivament finals.

### Expressions Lambda

Les expressions lambda són molt habituals al disseny d'interfícies gràfiques.

Exemple de gestió d'un esdeveniment d'un botó (control de tipus `Button`):

```java
button.setOnAction(new EventHandler<ActionEvent>() {
    @Override
    public void handle(ActionEvent event) {
        System.out.println("Botó clicat!");
    } 
});
```

Aquest codi utilitza classes anònimes.

També podem utilitzar expressions Lambda, ja que els gestors d'esdeveniments són **interfícies funcionals** (un sol mètode abstracte):

```java
button.setOnAction(
    event -> System.out.println("Botó clicat!")
);
```

Veure [https://docs.oracle.com/javase/tutorial/java/javaOO/nested.html](https://docs.oracle.com/javase/tutorial/java/javaOO/nested.html)

## Utilitats

### Llegir propietats

```java
    Properties props = new Properties();
    props.load(inputStream);
    String value = props.getProperty("nom.propietat"); // null, si no existeix

    InputStream inputStream;
    // possibles valors d'inputStream:
    inputStream = new FileInputStream("/path/nom.properties");
    inputStream = NomClasse.class.getResourceAsStream("/package1/package2/nom.properties");
    inputStream = objecte.getClass().getResourceAsStream("/package1/package2/nom.properties");
```

Quan s'utilitza getResourceAsStream(), podem també utilitzar path relatiu a la classe (NomClasse o classe de l'objecte), però cal que l'arxiu es copiï a la carpeta del classpath (bin o com es digui).

## Referències

*   [Java™ Platform, Standard Edition 8 API Specification](https://docs.oracle.com/javase/8/docs/api/)
*   Java. The Complete Reference (Ninth Edition). Herbert Schildt
    *   Capítol 20: Input/Output: Exploring java.io.
*   [Oracle Java SE Support Roadmap](https://www.oracle.com/java/technologies/java-se-support-roadmap.html)
*   [Infografia de conceptes POO](https://raygun.com/blog/images/oop-concepts-java/oops-concepts-infographic.png)
*   [Composition vs. Inheritance: How to Choose?](https://www.thoughtworks.com/insights/blog/composition-vs-inheritance-how-choose)
*   [Difference between Association, Aggregation and Composition in UML, Java and Object Oriented Programming](http://opensourceforgeeks.blogspot.com/2014/11/difference-between-association.html)
*   [Practical Test Pyramid (Martin Fowler)](https://martinfowler.com/articles/practical-test-pyramid.html)