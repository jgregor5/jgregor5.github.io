---
layout: page
title: Col.leccions i mapes
permalink: /colleccions-mapes/
nav_order: 2
---

El framework de col.leccions de Java permet emmagatzemar, obtenir, manipular i communicar dades agregades. Conté els següents elements:

*   **Interfícies**: són tipus de dades abstractes que representen col·leccions. Les interfícies permeten manipular les col·leccions independentment dels detalls de la seva representació. En llenguatges orientats a objectes, les interfícies formen generalment una jerarquia.
*   **Implementacions**: Són les implementacions concretes de les interfícies de recollida. En essència, són estructures de dades reutilitzables.
*   **Algorismes**: Són els mètodes que realitzen càlculs útils, com cercar i ordenar, en objectes que implementen interfícies de col·lecció. Es diu que els algoritmes són polimorfs: és a dir, es pot utilitzar el mateix mètode en moltes implementacions diferents de la interfície de col·lecció adequada. En essència, els algoritmes són funcionalitats reutilitzables.

A continuació es poden veure les **interfícies** principals.

![Interfícies](/images/interfaces.gif)

*   **Collection** : l’arrel de la jerarquia de col·leccions. Una col·lecció representa un grup d'objectes coneguts com els seus elements. La interfície de col·lecció és el denominador menys comú que totes les col·leccions implementen i s'utilitza per passar col·leccions i manipular-les quan es desitgi la màxima generalitat. Alguns tipus de col·leccions permeten duplicar elements, i d’altres no. Alguns estan ordenats i d’altres no ordenats. La plataforma Java no proporciona cap implementació directa d'aquesta interfície, però proporciona implementacions de subinterfícies més específiques, com ara Set and List.
*   **Set** : una col·lecció que no pot contenir elements duplicats. Aquesta interfície modela l’abstracció de conjunts matemàtics i s’utilitza per representar conjunts, com ara les cartes que contenen una mà de pòquer, els cursos que configuren el programa d’un estudiant o els processos que s’executen en una màquina. Té una versió ordenada, **SortedSet** (per valor ascendent).
*   **List** : una col·lecció ordenada (de vegades anomenada seqüència). Les llistes poden contenir elements duplicats. L’usuari d’una llista generalment té un control precís sobre on s’insereix cada element a la llista i pot accedir a elements mitjançant l’índex d’enters (posició). Si heu utilitzat Vector, coneixeu el sabor general de la llista. Consulteu també la secció La interfície de llista.
*   **Queue** : una col·lecció usada per contenir diversos elements abans del processament. A més de les operacions bàsiques de recollida, una cua proporciona operacions addicionals d'inserció, extracció i inspecció. Les cues normalment, però no necessàriament, ordenen elements de manera FIFO (first-in, first-out).
*   **Deque** : és una cua de doble final, i per tant permet inserir, extraure i inspeccionar elements als dos punts. Deques es pot utilitzar tant com FIFO (primer ingrés, primer sortida) com LIFO (darrera entrada, primer sortida).
*   **Map** : un objecte que assigna mapes de valors. Un mapa no pot contenir claus duplicades; cada tecla pot associar com a màxim un valor. Si heu utilitzat Hashtable, ja coneixeu els fonaments bàsics de Map. Consulteu també la secció La interfície del mapa. Té una versió ordenada, **SortedMap** (per clau ascendent).

Les col·leccions utilitzen el concepte de **genèrics** de Java. Bàsicament, permet definir el tipus de l'element de les col·leccions com un paràmetre. Per exemple, per a la collecció de tipus List, la definició a la documentació de Java és:

```java
Interface List<E>
```

Això significa que List és una llista d'elements (E) de tipus parametritzable. Per tant, podem utilitzar List per qualsevol classe (no tipus primitiu).

## Genèrics

Els genèrics permeten que els tipus (classes i interfícies) siguin paràmetres a l’hora de definir classes, interfícies i mètodes. Un cop que s'instancien, els paràmetres són substituïts pels tipus reals.

Beneficis:

*   **Controls de tipus més forts a la compilació**.

Un compilador Java aplica una verificació de tipus forta al codi genèric i emet errors si el codi viola la seguretat del tipus. La correcció d’errors en temps de compilació és més fàcil que arreglar errors d’execució, que poden ser difícils de trobar.

*   **Eliminació de casts**. El següent codi requereix tipus:

```java
List list = new ArrayList();
list.add("hello");
String s = (String) list.get(0);
```

Quan es reescriu amb genèrics, no el requereix:

```java
List<String> list = new ArrayList<String>();
list.add("hello");
String s = list.get(0);   // no cast
```

*   Permet als programadors **implementar algoritmes genèrics**.

Mitjançant l’ús de genèrics, els programadors poden implementar algoritmes genèrics que treballin en col·leccions de diferents tipus, es poden personalitzar i són de tipus segur i més fàcil de llegir.

Des de Java 7 podem estalviar-nos la definició del paràmetre de tipus del constructor, ja que Java l'infereix:

```java
List<String> list = new ArrayList<>();
```

### Mètodes genèrics

*   Totes les declaracions genèriques del mètode tenen una secció de paràmetre tipus delimitada per claudàtors d'angle (<i>) que precedeix el tipus de retorn del mètode (<E> en el següent exemple).
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

Hi pot haver moments en què voldreu restringir els tipus de tipus que es permeten passar a un tipus de paràmetre. Per exemple, un mètode que opera sobre números només pot voler acceptar instàncies de Number o de les seves subclasses.

Per declarar un paràmetre de tipus delimitat, enumereu el nom del paràmetre del tipus, seguit de la paraula clau `extends`, seguit tipus delimitant.

```java
public static <T extends Comparable<T>> T maximum(T x, T y, T z) {

    T max = x;   // assume x is initially the largest

    if (y.compareTo(max) > 0) {
        max = y;   // y is the largest so far
    }

    if (z.compareTo(max) > 0) {
        max = z;   // z is the largest now                 
    }

    return max;   // returns the largest object   
}
```

Es poden tenir múltiples tipus delimitats:

```java
<T extends Type1 & Type2>
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

### Comodins

El comodí (?) permet referir-se a un tipus desconegut. Tenim diferents tipus:

*   comodins delimitats per dalt: `? extends Foo`
*   comodins no delimitats: `?`
*   comodins delimitats per baix: `? super Foo`

## Ús de col·leccions

[Tota Collection és un Iterable](https://docs.oracle.com/javase/8/docs/api/java/util/Collection.html), la qual cosa serveix per iterar qualsevol List, Set o Queue.

Per iterar tenim el mètode iterator():

```java
Iterator<Integer> iterator = list.iterator();

while(iterator.hasNext()) {
    Integer nextInt = iterator.next();
}
```

També es pot utilitzar el format for-each loop:

```java
for (Integer nextInt: list) {
    // ...
}
```

Altres mètodes **Collection\<E\>**:

*   `boolean add(E e);`
*   `void clear();`
*   `boolean contains(Object o);`
*   `boolean isEmpty();`
*   `boolean remove(Object o);`
*   `int size()`

El mètode `contains()` utilitza el mètode `equals()` per veure si existeix l'element. Per tant, depèn de la implementació de cada classe. En el cas de `Integer`, la documentació diu:

*   The result is `true` if and only if the argument is not `null` and is an `Integer` object that contains the same `int` value as this object.

Per tant, són iguals dos objectes Integer que contenen el mateix valor sencer. `Object` implementa el mètode `equals()` com la igualtat (`this == o`). Compte, perquè si es pretén sobreescriure el mètode `equals()`, sempre s'ha de sobreescriure també el mètode `hashcode()`. Veure `Objects.hash()`.

**List\<E\>** afegeix operacions per posicions:

*   `void add(int index, E element)`
*   `E get(int index)`
*   `E set(int index, E element)`
*   `E remove(int index)`

La implementació més clàsica és la de `ArrayList`. `LinkedList` podria ser interessant si s'insereixen elements al començament amb freqüència.

**Set\<E\>** és una col·lecció que no conté repeticions. O sigui, no hi ha dos elements tals que `e1.equals(e2)`. No conté mètodes addicionals respecte `Collection`.

Tenim tres implementacions:

*   `HashSet` utilitza el `hashCode()` de la clau per a optimitzar l'accés als elements.
*   `TreeSet` utilitza una estructura en arbre on l'element ha de ser comparable per tal de disposar les branques. Es basa en `TreeMap`.
*   `LinkedHashSet` permet tenir un ordre d'iteració concret: l'ordre d'inserció.

**Qeue\<E\>** és una col·lecció amb el concepte associat de "cap": lloc per on s'afegeixen i es treuen els elements:

*   `boolean add(E e)`: afegeix un element (amb excepció).
*   `boolean offer(E e)`: afegeix un element.
*   `E remove():` esborra l'últim element (amb excepció).
*   `E poll():` esborra l'últim element.
*   `E element():` examina l''últim element (amb excepció).
*   `E peek():` examina l'últim element.

Com es veu, hi ha dos mètodes per cada operació (afegir, esborrar, examinar), una amb excepció i una altra sense.

Les implementacions són:

*   `LinkedList`: la implementació més habitual.
*   `PriorityQueue`: permet que la cua estigui ordenada mitjançant un comparador, en lloc d'utilitzar l'ordre en que s'afegeixen els elements.

**Deque\<E\>** és una `Collection` i també una `Queue<E>`. Permet afegir i treure elements als dos costats de la col.lecció. Permet implementar una pila (classe deprecada Stack) amb els mètodes:

*   `void push(E e)`: afegeix un element a la pila (cap)
*   `E pop()`: treu un element de la pila (cap)
*   `E peek()`: examina l'element de la pila (cap)

## Ús de mapes

Els mapes són estructures de dades dinàmiques que contenen correspondències entre parelles de clau i valor.

**Map\<K, V\>** té aquestes operacions principals:

*   `int size()`
*   `boolean isEmpty()`
*   `boolean containsKey(Object)`
*   `V put(K, V)`
*   `V remove(Object)`
*   `void clear()`
*   `Set<K> keySet()`
*   `Collection<V> values()`
*   `Set<Entry<K, V>> entrySet()`

El tipus Entry\<K, V\> és una parella clau/valor inmutable amb els mètodes:

*   `K getKey()`
*   `V getValue()`

![](/images/usmapes.png)

Les tres principals implementacions són:

*   `HashMap` utilitza el `hashCode()` de la clau per a optimitzar l'accés als elements.
*   `TreeMap` utilitza una estructura en arbre on la clau ha de ser comparable per tal de disposar les branques.
*   `LinkedHashMap`: permet iterar els elements segons l'ordre d'inserció.

## Algorismes

La majoria d'algorismes operen en llistes (List), i alguns a Collection:

*   `static <T extends Comparable<? super T>> void sort(List<T> list)`: ordena una llista d'elements comparables.
*   `static <T> void sort(List<T> list, Comparator<? super T> c)`: ordena una llista d'elements, utilitzant un comparador.
*   `static void shuffle(List<?> list)`: desordena una llista
*   Cinc algorismes més per manipular objectes d'una llista:
    *   reverse
    *   fill
    *   copy
    *   swap
    *   addAll
*   `static <T> int binarySearch(List<? extends Comparable<? super T>> list, T key)`: cerca binària a una llista
*   Sobre collections:
    *   frequency
    *   disjoint
    *   min
    *   max

## Referències

*   [Algorithms, 4th Edition](https://algs4.cs.princeton.edu/home/)
*   [Expressions regulars (Jenkov)](http://tutorials.jenkov.com/java-regex/index.html)
