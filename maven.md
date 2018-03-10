# <a name="Home"></a> Maven

## Содержание:
- [Введение](#Overview)
- [Реактор](#Reactor)
- [Жизненный цикл](#Lifecycle)
    - [Site](#Site)
    - [Clean](#Clean)
    - [Default](#Default)
- [POM file](#POM)
- [Дополнительно (ссылки)](#Additional)

## [↑](#Home) <a name="Overview"></a> Введение
Maven - это система сборки проектов, как об этом можно прочитать здесь: "[apache-maven.ru](http://www.apache-maven.ru/)".
При разработке возникает большое количество рутинных процессов, которые необходимо выполнять. Компиляция, подготовка артефактов, всё это в различных конфигурациях (тест, прод и т.д.). К этому добавляется подготовка каких-то дополнительных артефактов. Кроме того появляется проблема использования различных операционных систем. И это только вершина айзберга.
Дабы облегчить жизнь разработчика и позволить ему писать код, а не заниматься рутинными задачами и были придуманы система сборки.

Самыми популярными на сегодняшний день являются Gradle и Maven.

## [↑](#Home) <a name="Reactor"></a> Реактор
Самый простой способ создать maven проект - создать его из архетипа (что-то вроде шаблона). Данный способ описан в документации: [Maven Archetype](https://maven.apache.org/archetype/index.html).
Используется для этого команда **mvn archetype:generate**
Если выполнить для созданного проекта maven команду в режиме дебага (например, **mvn clean -X**), то мы увидим такую запись:
```
REACTOR BUILD PLAN
```
То есть у Maven есть какой-то реактор. Что это?
Это механизм, при помощи которого Maven понимает, какие есть проекты, как они зависят друг от друга. Именно благодаря реактору становится понятн, в каком порядке нужно обрабатывать проекты.

## [↑](#Home) <a name="Lifecycle"></a> Жизненный цикл
Жизненный цикл описан в документации: [Introduction to the Build Lifecycle](https://maven.apache.org/guides/introduction/introduction-to-the-lifecycle.html).
Существует следующие life cycle:
- clean
- default
- site

В свою очередь, каждый lifecycle состоиз из фаз (**phases**).
А для каждой phases выполняются определённые **goals**.

Подробнее про maven lifecycle можно прочитать и здесь: [Build Lifecycle](https://proselyte.net/tutorials/maven/build-life-cycle/).

Так же Maven позволяет создавать собственные lifecycle: [Custom Maven Lifecycle](https://nextmetaphor.io/2016/10/10/custom-maven-lifecycle/).

## [↑](#Home) <a name="Site"></a> Site
Lifecycle Site описывает генерацию документации для проекта.
Более подробно см. [Создание сайта средствами мавена](https://habrahabr.ru/post/115657/)).

Описание того, как с этим работать: [Building a Project Site with Maven](http://books.sonatype.com/mvnref-book/reference/site-generation-sect-building.html).

А так же пример здесь: [Get the most out of Maven 2 site generation](https://www.javaworld.com/article/2071733/java-app-dev/get-the-most-out-of-maven-2-site-generation.html).

Согласно документации [Lifecycle Reference](https://maven.apache.org/guides/introduction/introduction-to-the-lifecycle.html#Lifecycle_Reference) состоит из следующих фаз:
- pre-site для выполнения предварительных действий (перед генерацией)
- site для генерации документации
- post-site для выполнение завершающих действий, выполняемых до деплоя
- site-deploy для разворачивания документации на веб-сервере

## [↑](#Home) <a name="Clean"></a> Clean
Данный lifecycle описывает очистку каталогов, в которые maven складывает собранные артефакты (по умолчанию каталог target).

Данный lifecycle состоит из следующих фаз:
- pre-clean	для выполнения предварительных действий
- clean	для удаления файлов, сгенерированных при прошлом build'е проектов
- post-clean для выполнения действий по завершению очистки

## [↑](#Home) <a name="Default"></a> Default
Default - это основной жизненный цикл сборки maven проекта.
Описание фаз, из которых он состоит - [Maven Build lifecycle reference](https://maven.apache.org/guides/introduction/introduction-to-the-lifecycle.html#Lifecycle_Reference).

Важно, что если выполняется определённая фаза (например, **mvn install**), то выполняются не только указанная фаза, но и все предыдущие.

На сборку так же влияет значение packaging (см. [Maven Coordinates](https://maven.apache.org/pom.html#Maven_Coordinates)). В зависимости от указанного типа будет разный набор фаз (см. [Build-in Lifecycle Binding](https://maven.apache.org/guides/introduction/introduction-to-the-lifecycle.html#Built-in_Lifecycle_Bindings)).

## [↑](#Home) <a name="POM"></a> POM file
Главный файл, описывающий как maven должен работать с вашим проектом называется POM файлом (Project Object Model).
Более подробное описание см. здесь: [Maven: The Complete Reference](http://books.sonatype.com/mvnref-book/pdf/mvnref-pdf.pdf).

POM файлы собираются в итоге из **Super POM** (дефолтное поведение Maven. Даётся при установке Maven) и из POM проектов. После того, как данные из разных POM файлов будут объединены, будет составлен **Effective POM**, который и будет определять конечные инструкции для Maven'а.

Более подробно см. раздел документации Maven: [POM Reference](https://maven.apache.org/pom.html).

## [↑](#Home) <a name="Additional"></a> Дополнительно
Maven предоставляет большой арсенал возможностей. Чтобы более подробно про них узнать стоит посмотреть следующие источники:
- [Maven Settings Reference](https://maven.apache.org/settings.html)
- [Maven: The Complete Reference](http://books.sonatype.com/mvnref-book/pdf/mvnref-pdf.pdf)
- [Best Maven Books List](http://www.fromdev.com/2017/02/best-maven-books.html)
- Пример вопросов и ответов на собеседовании: [Часть 1](https://jsehelper.blogspot.ru/2016/05/maven-1.html) и [Часть 2](https://jsehelper.blogspot.ru/2016/05/maven-2.html)
- [Maven Udemy course](https://www.udemy.com/maven-quick-start/)