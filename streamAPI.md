# <a name="Home"></a> StreamAPI

## Содержание:
- [Введение](#Overview)
- [Создание](#Creation)
- [Использование Map и FlatMap](#MapFlatMap)
- [Collect и Collectors](#Collect)
- [Reduce](#Reduce)
- [Other](#Other)

## [↑](#Home) <a name="Overview"></a> Введение
В **Java 8** одним из новшевств стало появление **Stream API**.
Самое лучшее из официальных объяснений тут: "[Package java.util.stream Description](https://docs.oracle.com/javase/8/docs/api/java/util/stream/package-summary.html#package.description)".
Главная идея данного средства - предоставление функционального стиля работы с данными.
Данные обрабатываются в виде потока, над которым совершаются некоторые операции.
Операции бывают промежуточные и конечные (терминальные). Можно представить в виде конвеера. У потока есть источник (например, коллекции), за которым следует набор выполняемых дейстий. Конечное операция возвращает результат или выполняет какое либо действией.
Стоит заметить, что все промежуточные операции над потоками — ленивые(Lazy). Они не будут исполнены, пока не будет вызвана терминальная (конечная) операция.

Отличный обзор на тему потоков: "[Полное руководство по Java 8 Stream API в картинках и примерах](https://annimon.com/article/2778)".

Про особенности работы (например, про Порядок обработки) можно прочитать в обзоре: "[Полное руководство по Java 8 Stream](https://javadevblog.com/polnoe-rukovodstvo-po-java-8-stream.html)".

## [↑](#Home) <a name="Creation"></a> Создание Stream
Во-первых, у самого Stream есть статические методы для его создания. См. [JavaDoc](https://docs.oracle.com/javase/8/docs/api/?java/util/stream/Stream.html).
Есть возможность:
- Stream.empty() - создание пустого стрима
- Stream.of - создание стрима с указанными элементами или с одиночным элементом
- Stream.generate - стрим, источником которого является бесконечный генератор
- Stream.iterator - стрим, источником которого является бесконечный итератор

Последние варианты требуют указания лимита, иначе мы получим бесконечный цикл.

Так же, если посмотреть на интерфейс **java.util.Collection**, то мы увидим, что у него появился метод **stream**, позволяющий из любой коллекции получить стрим.
Так же у **java.util.Arrays** появился статический метод, который позволяет получать стрим из массива.
Интерфейс **java.util.Map** не имеет метода **stream**.

Пример создания стрима из массива:
```java
public static void main(String[] args) {
        String[] dateParts = new Date().toString().split(" ");
        Arrays.stream(dateParts).forEach(part -> System.out.println(part));
}
```
Помимо коллекций есть и другие способы получить стрим.
Подробнее можно прочитать здесь: "[Полное руководство по Java 8 Stream](https://javadevblog.com/polnoe-rukovodstvo-po-java-8-stream.html)".
Например, одним из способов создания стрима является использование **supplier**. Это может понадобится для того, чтобы объявлять стрим один раз, а использовать его несколько. Это нужно по той причине, что стрим, для которого выполнена терминальная операция, больше не пригоден к работе с ним. Например:
```java
Supplier<Stream<String>> streamSupplier =
                () -> Stream.of("dd2", "aa2", "bb1", "bb3", "cc")
                        .filter(s -> s.startsWith("a"));
        streamSupplier.get().anyMatch(s -> true);   // операция пройдет успешно
        streamSupplier.get().noneMatch(s -> true);  // здесь также все будет ok
```
Можно так же использовать генератор. Способ получения 10 случайных чисел:
```java
Random rnd = new Random();
Stream.generate(() -> rnd.nextInt(10)).limit(10).forEach(num -> System.out.println(num));
```

Можно использовать и не просто Stream, а специализированный. Например:
```java
IntStream.range(1, 4).forEach(i -> System.out.print(i)); // 123
```

## [↑](#Home) <a name="MapFlatMap"></a> Использование Map и FlatMap
Одними из самых полезных действий являются **map** и **flatMap**.
Для этого нам понадобятся простенькие внутренние классы:
```java
static class Student {
        public String name;
        public Student(String name) {
            this.name = name;
        }
}
static class Group {
        public List<Student> students;
        public Group(List<Student> students) {
            this.students = students;
        }
}
```
Всё public для простоты и быстроты написания.

Посмотрим на пример:
```java
List<Student> g1students = Arrays.asList(new Student("S1"), new Student("S2"));
Group g1 = new Group(g1students);
List<Student> g2students = Arrays.asList(new Student("S3"), new Student("S4"));
Group g2 = new Group(g2students);
Stream.of(g1, g2).flatMap(group -> group.students.stream()).forEach(student -> System.out.println(student.name));
```
Другие примеры можно увидеть здесь: "[Полное руководство по Java 8 Stream API в картинках и примерах](https://annimon.com/article/2778#intermediate-operators)".

Название flatMap помогает запомнить смысл. То есть "плоский" map.
Чтобы это понять, посмотрим на пример:
```java
List<String> exampleList = Collections.singletonList("value");
Stream<Integer> mapResult = exampleList.stream().map(string -> string.length());
```
То есть у нас есть некий поток, а **map** из него делает тоже поток, но другой.
Поэтому map возвращает тоже **Stream**, но из других элементов. Можно сказать, что это "маппинг" из одного объекта в другой, 1 к 1.
В примере выше был стрим из элементов String, а стал стрим из элементов Integer. Т.е. String был смаплен на его длинну.

А пример выше про flatMap указывает, что берётся стрим групп. Группы содержат списки студентов. Если сделать просто **map(group.students)**, то что мы получим?
А вот что:
```java
List<Student> g1students = Arrays.asList(new Student("S1"), new Student("S2"));
Group g1 = new Group(g1students);
Stream<List<Student>> listStream = Stream.of(g1).map(group -> group.students);
```
То есть каждая group мапится на students. students у нас это List. Поэтому, получается, что мы получаем стрим листов студентов. Не очень удобно, да? Поэтому, flatMap позволяет получить один единый стрим из всех полученных элементов, которые будут содержаться в полученных стримах.

Так же есть методы как у map, так и у flatMap для получения особых стримов. Например:
mapToInt возвращает IntStream, mapToLong - LongStream, а mapToDouble - DoubleStream.
Например:
```java
List<Student> g1students = Arrays.asList(new Student("S1"), new Student("S2"));
Group g1 = new Group(g1students);
IntStream intStream = Stream.of(g1).mapToInt(group -> group.students.size());
System.out.println("Самая большая группа: " + intStream.max().getAsInt());
```

## [↑](#Home) <a name="Collect"></a> Collect и Collectors
Часто нам может потребоваться после обработки стрима собрать его в новую коллекцию.
С примерами можно ознакомиться здесь: [Java 8 Stream API, часть вторая: map/reduce](https://easyjava.ru/java/language/java-8-stream-api-chast-vtoraya-map-reduce/).

Отличные примеры можно увидеть здесь: "[Полное руководство по Java 8 Stream](https://javadevblog.com/polnoe-rukovodstvo-po-java-8-stream.html)".

Про коллекторы так же описано здесь: [Java 8 Collectors](http://www.baeldung.com/java-8-collectors)

Самыми простыми являются коллекторы в простые коллекции. Например:
```java
Supplier<Stream<Integer>> stream;
stream = () -> Stream.generate(() -> rnd.nextInt(10)).limit(10);
List<Integer> list = stream.get().collect(Collectors.toList());
Set<Integer> set = stream.get().collect(Collectors.toSet());
```

Если же нужно как-то сгруппировать записи, то самым простым способом является **groupingBy**. Например:
```java
List<Student> group = new ArrayList<>();
group.add(new Student("Alex"));
group.add(new Student("Arnold"));
group.add(new Student("John"));
Map<Character, List<Student>> collect;
collect = group.stream().collect(Collectors.groupingBy(student -> student.name.charAt(0)));
collect.get('A').forEach(student -> System.out.println(student.name));
```

Но иногда этого недостаточно. Например, неудобно выводить буква и имено студентов:
```java
collect.forEach((key, value) -> {
	System.out.format("with %c: %s\n", key, 
    value.stream().map(st -> st.name).collect(Collectors.joining(", ")));
});
```
Поэтому, можно сделать при помощи **Collectors.toMap**:
```java
collect = group.stream()
	.collect(Collectors.toMap
            (s -> s.name.charAt(0), s -> s.name, (s1, s2) -> s1 + "," + s2));
collect.forEach((key, value) -> System.out.format("with %c: %s\n", key, value));
```

Интересно, что можно создавать свой коллектор. Это позволяет добиться любого результа, который только захочется. Шикарный пример приведён тут: [Полное руководство по Java 8 Stream](https://javadevblog.com/polnoe-rukovodstvo-po-java-8-stream.html):
```java
List<Student> group = new ArrayList<>();
group.add(new Student("Alex"));
group.add(new Student("Arnold"));
group.add(new Student("John"));

Collector<Student, StringJoiner, String> personNameCollector =
	Collector.of(
		() -> new StringJoiner(" | "), // supplier
		(j, s) -> j.add(s.name.toUpperCase()),  // accumulator
		(j1, j2) -> j1.merge(j2),               // combiner
		StringJoiner::toString);                // finisher
String names = group.stream().collect(personNameCollector);
System.out.println(names);
```

## [↑](#Home) <a name="Reduce"></a> Reduce
Операция Reduce сочетает в себе все элементы потока в единый результат.
Например:
```java
List<Student> students = new ArrayList<>();
students.add(new Student("Max",5));
students.add(new Student("Mike",15));
students.add(new Student("John",10));
Group group = new Group(students);
group.students.stream().reduce((s1, s2) -> s1.score > s2.score ? s1 : s2)
     .ifPresent(student -> System.out.println(student.name));
```
Данный пример выведет Mike, т.к. у него самый большой score (15).

Другие примеры хорошо описано всё там же: [Полное руководство по Java 8 Stream](https://javadevblog.com/polnoe-rukovodstvo-po-java-8-stream.html).

## [↑](#Home) <a name="Other"></a> Other
- Получить 10 случайных чисел?
```java
ThreadLocalRandom.current().ints(1, 10).limit(10).forEach(e -> System.out.println(e));
```
```java
Random rnd = new Random();
Stream.generate(() -> rnd.nextInt(10)).limit(10).forEach(num -> System.out.println(num));
```
```java
new Random().ints(1,10).limit(10).forEach(e -> System.out.println(e));
```