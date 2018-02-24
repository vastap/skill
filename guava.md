# <a name="Home"></a> Guava

## Содержание:
- [Введение](#Overview)
- [Basic Utilities](#BasicUtilities)
    - [Optional](#Optional)
    - [Preconditions](#Preconditions)
    - [Ordering](#Ordering)
- [String](#String)
- [Primitives](#Primitives)
- [Math](#Math)
- [IO](#IO)
- [Collections](#Collections)
- [Links](#Links)

## [↑](#Home) <a name="Overview"></a> Введение
Язык Java имеет множество готовых иснтрументов "из коробки", т.е. доступных для использования без использования каких-либо дополнительных библиотек. Однако, иногда всё таки приходится использовать эти самые сторонние библиотеки.
Одной из таких библиотек является Guava от Google. В общем случае Guava служит для упрощения кода. Более того, многие подходы к написанию кода, реализованные в Guava были взяты на вооружение и вошли в **JDK 8**.

Библиотека **Guava** состоит из набора модулей. Например: basic utilities, collections, strings, primitives, IO, math
Подробнее можно прочитать здесь: [Guava wiki](https://github.com/google/guava/wiki)
Для эксперементов создадим gradle проект. В этом нам поможет [Gradle Init](https://docs.gradle.org/current/userguide/build_init_plugin.html). Создадим отдельный каталог "**Guava**" и выполним в нём: ```gradle init --type java-application```.
После этого импортируем проект в IDE. Откроем Build script файл **build.gradle**. В нём добавим записимость от guava, как это указано в [документации Guava](https://github.com/google/guava):
```
dependencies {
    compile 'com.google.guava:guava:24.0-jre'
    testCompile 'junit:junit:4.12'
}
```
Теперь мы готовы начинать.
А знания черпать будем из гугла и официального [Guava wiki](https://github.com/google/guava/wiki).

## [↑](#Home) <a name="BasicUtilities"></a> Basic utilities
Данный модуль старается сделать написание кода более приятным и реализует базовые вещи (т.е. как раз те самые basic utilities).

### [↑](#Home) <a name="Optional"></a> > Optional
Первым в списке того, что предоставляет нам "базовый набор", идёт "[Using and avoiding null](https://github.com/google/guava/wiki/UsingAndAvoidingNullExplained)".
Для этого служит класс **com.google.common.base.Optional**. Он похож на **java.util.Optional**, но существовал ещё до появления JDK 8. Собственно, благодаря удачной реализации в Guava мы получили Optional в JDK.


Создание Optional может выглядеть следующим образом:
```java
Optional<String> password;
// Не может содержать null, иначе NPE
password = Optional.of("admin");
// Без значений (аналог Optional.empty() в JDK)
password = Optional.absent();
// Может содержать null (аналог Optional.ofNullable в JDK)
password = Optional.fromNullable(null);
// Конвертировать из/в Google Optional
Optional.toJavaUtil(password);
```
Есть возможность получать значение из Optional'а и проверять, есть ли оно там (чтобы избежать ошибок ):
```java
optional = Optional.fromNullable("Text");
if (optional.isPresent()) {
	System.out.println(optional.get());
}
```
А так же возвращать значения по умолчанию в случае отсутствия значения
```java
Optional<String> optional = Optional.fromNullable(null);
// Возвращаем значение по умолчанию (аналог Optional.orElse в JDK)
System.out.println(optional.or("empty"));
// Возвращаем null, если значение не задано
System.out.println(optional.orNull());
// Вернёт результат из Supplier (аналог Optional.orElseGet в JDK)
optional.or(() -> "emp" + "ty");
```

### [↑](#Home) <a name="Preconditions"></a> > Preconditions
Так же в базовые утилиты входят предусловия: [Preconditions](https://github.com/google/guava/wiki/PreconditionsExplained#preconditions).
```java
String password = "admin";
// Не null. Аналог Objects.requireNonNull из JDK 7
Preconditions.checkNotNull(password);
// Проверка состояния и аргумента
Preconditions.checkState(System.currentTimeMillis() > 0);
Preconditions.checkArgument(password.length() > 1);
```
Данные методы так же принимают вторым аргументом сообщение, которое нужно отобразить в случае ошибки. Например:
```java
Preconditions.checkNotNull(args, "arguments list is empty");
```

Guava позволяет выполнять базовые операции над Objects, реализуя [Object common methods](https://github.com/google/guava/wiki/CommonObjectUtilitiesExplained).
Например:
```java
String first = "1";
String second = String.valueOf(1);
System.out.println(Objects.equal(first, second));
System.out.println(Objects.hashCode(first, second));
```

### [↑](#Home) <a name="Ordering"></a> > Ordering
Guava реализует "[Ordering](https://github.com/google/guava/wiki/OrderingExplained)" при помощи класса com.google.common.collect.Ordering.
Это так называемый "fluent" Comparator. Вот пример:
```java
List<Integer> toSort = Arrays.asList(3, 5, 4, 1, 2);
Collections.sort(toSort, Ordering.natural().reverse());
System.out.println("min: " + Ordering.natural().min(toSort));
```
Подробнее можно прочитать тут: [Guava Ordering Cookbook](http://www.baeldung.com/guava-order).
Или тут: [Ordering example](https://www.leveluplunch.com/java/examples/guava-ordering-example/).

Также Guava позволяет упростить работу с [Exceptions](https://github.com/google/guava/wiki/ThrowablesExplained) в старых версиях JDK.

## [↑](#Home) <a name="String"></a> String
Конечно же, Guava не обошла стороной и тему обработки строк.
Данный раздел описан в "[Guava String Explained](https://github.com/google/guava/wiki/StringsExplained)".

Guava String содержит Joiner, похожий на StringJoiner из JDK 8:
```java
// Guava
Joiner joiner = Joiner.on("; ").skipNulls();
String result = joiner.join("Harry", null, "Ron", "Hermione");
System.out.println(result);
// JDK8
StringJoiner stringJoiner = new StringJoiner(";");
stringJoiner.add("Harry").add(null).add("Ron").add("Hermione");
System.out.println(stringJoiner);
```

Guava Strings содержит так же и Splitter. См. [Joiner And Splitter](http://www.baeldung.com/guava-joiner-and-splitter-tutorial).
Аналога в JDK 8 нет. Того же можно достигнуть при помощи Stream API. Подробнее см. [Java 8 API by Example: Strings, Numbers, Math and Files](http://winterbe.com/posts/2015/03/25/java8-examples-string-number-math-files).

Так же Guava содержит [CharMatcher](http://www.baeldung.com/guava-string-charmatcher) и класс Strings.
Подробнее: [Guava's Strings Class](https://dzone.com/articles/guavas-strings-class) и [Guava's Strings Class](https://www.javaworld.com/article/2074448/core-java/guava-s-strings-class.html).

## [↑](#Home) <a name="Primitives"></a> Primitives
Может показаться странным, что за блок [Primitives](https://github.com/google/guava/wiki/PrimitivesExplained) у Guava.
Возьмём список из [Guava - Primitive Utilities](https://www.tutorialspoint.com/guava/guava_primitive_utilities.htm).
Например:
```java
int[] intArray = {1,2,3,4,5,6,7,8,9};
//convert array of primitives to array of objects
List<Integer> objectArrayError = Arrays.asList(intArray); // Нельзя
List<Integer> objectArrayOK = Ints.asList(intArray); // Можно
```
А так же позволяет выполнять прочие операции над массивом примитивов, такие как min, max, contains.
Так же благодаря Guava Primitives есть поддержка Unsigned (т.е. беззнаковых) примитивов. Например:
```java
UnsignedInteger integer = UnsignedInteger.fromIntBits(5);
integer = integer.minus(UnsignedInteger.fromIntBits(6));
System.out.println(integer); //4294967295
System.out.println(Integer.MAX_VALUE); //2147483647
```

Подробнее см. [Guava Primitives Explained](https://github.com/google/guava/wiki/PrimitivesExplained).

## [↑](#Home) <a name="Math"></a> Math
Guava так же старается упростить математические действия при помощи [Guava Math](https://github.com/google/guava/wiki/MathExplained).
Например:
```java
// Макс. кол-во шагов в бинарном поиске для 100 элементов
int logFloor = LongMath.log2(100, RoundingMode.CEILING);
logFloor = LongMath.log2(256, RoundingMode.CEILING);
```
Так же рассчёт [биномиального коэффициента](http://www-formula.ru/binomial-factors). Например:
```java
// В комбинаторике биномиальный коэффициент означает:
// число всех возможных вариантов выборки k элементов из множества элементов n.
int elementsCount = 6;
// выбираем все возможные комбинации из трёх элементов, k=3
int result = IntMath.binomial(elementsCount, 3);
System.out.println(result);
```

Более подробно: [Guava Math Explained](https://github.com/google/guava/wiki/MathExplained).
А так же тут: [Guide to Mathematical Utilities in Guava](http://www.baeldung.com/guava-math).

## [↑](#Home) <a name="IO"></a> IO
Guava содержит различные улучшения по работе с IO.
Подробнее см. [Guava IO Explained](https://github.com/google/guava/wiki/IOExplained).

Примеры использования Guava IO:
- [Guava – Write to File, Read from File](http://www.baeldung.com/guava-write-to-file-read-from-file)

Со временем работу с файлами улучшили и в Java 8:
- [Java 8 API by Example: Strings, Numbers, Math and Files](http://winterbe.com/posts/2015/03/25/java8-examples-string-number-math-files/)

Для сравнения:
```java
// Guava
File file = new File("C:\\Program Files\\7-Zip\\readme.txt");
try {
	List<String> lines  = com.google.common.io.Files.readLines(file, Charset.defaultCharset());
	lines.forEach(line -> System.out.println(line));
} catch (IOException e) {
    e.printStackTrace();
}
// JDK8
Path filePath = Paths.get("C:\\Program Files\\7-Zip\\readme.txt");
try {
	List<String> lines  = Files.readAllLines(filePath);
	lines.forEach(line -> System.out.println(line));
} catch (IOException e) {
    e.printStackTrace();
}
```

## [↑](#Home) <a name="Collections"></a> Collections
Библиотека не только упрощает написание кода, но и добавляет некоторые структуры данных или оптимизирует их.
Новые коллекции описаны в "[Guava New Collection Types](https://github.com/google/guava/wiki/NewCollectionTypesExplained)".

Цикл статей про коллекции: [Collection Creation and Immutability with Google Guava](https://dzone.com/articles/collection-creation-and).
Описываются:
- [MultiMaps](http://tomjefferys.blogspot.ru/2011/09/multimaps-google-guava.html)
MultiMap это карта, где значением для ключа является не просто значение, а список значений:
```java
Map<String,List<String>> jdk = new HashMap<String,List<String>>();
Multimap<String, String> multimap = HashMultimap.create();
```
- [MultiSets](http://tomjefferys.blogspot.ru/2011/09/multisets.html)
Особый Set, который позволяет одинаковые элементы хранить как подSet:
```java
Multiset<String> myMultiset = HashMultiset.create();
myMultiset.add("Registration");
myMultiset.add("Registration");
System.out.println(myMultiset.count("Registration")); // 2 элемента
System.out.println(myMultiset.elementSet().size()); // 1 уникальных
```
- [BiMap](http://tomjefferys.blogspot.ru/2011/09/bimaps.html)
Карта, с которой можно работать как по ключу, так и инвертировать карту для работы по значению как по ключу. При этом, должна соблюдаться уникальность как ключей, так и значений.
- [Table](https://github.com/google/guava/wiki/NewCollectionTypesExplained#table)
Если есть:
```java
Map<FirstName, Map<LastName, Person>>
```
То Table позволяют работать с FirstName и LastName как с колонками и строками:
- [ClassToInstanceMap](https://github.com/google/guava/wiki/NewCollectionTypesExplained#classtoinstancemap)
Карта, позволяющая "мапить" на тип экземпляры

А так же некоторые другие, такие как [RangeSet](https://github.com/google/guava/wiki/NewCollectionTypesExplained#rangeset) и [RangeMap](https://github.com/google/guava/wiki/NewCollectionTypesExplained#rangemap).

Так же предоставляются [Immutable collections](https://github.com/google/guava/wiki/ImmutableCollectionsExplained).
А так же некоторые утилиты. Подробнее см. "[Collection Utilities Explained](https://github.com/google/guava/wiki/CollectionUtilitiesExplained)". Пример использования утилит - фильтраци коллекций (см. "[Filter collection with guava](https://www.leveluplunch.com/java/tutorials/001-filtering-collection-with-guava/)")

Помимо всего выше указанного, Guava реализует работу с графами: [Graphs Explained](https://github.com/google/guava/wiki/GraphsExplained).

## [↑](#Home) <a name="Links"></a> Links
Ссылки на тему Guava:
- [Baeldung : Guava Collections Cookbook](http://www.baeldung.com/guava-collections)
- [Tutorialspoint : Guava tutorial](https://www.tutorialspoint.com/guava/guava_tutorial.pdf)
- [Guava wiki](https://github.com/google/guava/wiki)