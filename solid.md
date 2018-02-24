# <a name="Home"></a> SOLID

## Содержание:
- [Введение](#Overview)
- [The **S**ingle Responsibility Principle](#SingleResponsibility)
- [The **O**pen Closed Principle](#OpenClosed)
- [The **L**iskov Substitution Principle](#LiskovSubstitution)
- [The **I**nterface Segregation Principle](#InterfaceSegregation)
- [The **D**ependency Inversion Principle](#DependencyInjection)
- [Links](#Links)

## [↑](#Home) <a name="Overview"></a> Введение
В рамках разработки в объектно-ориентированном стиле существует всем известная аббревиатура SOLID.
Вот эти принципы:
- [S] - The **S**ingle Responsibility Principle
- [O] - The **O**pen Closed Principle
- [L] - The **L**iskov Substitution Principle
- [I] - The **I**nterface Segregation Principle
- [D] - The **D**ependency Inversion Principle

Данный материал хорошо описан на русскоязычном ресурсе:
Блог Александра Бындю - [Принципы проектирования классов](http://blog.byndyu.ru/2009/10/solid.html).

## [↑](#Home) <a name="SingleResponsibility"></a> The **S**ingle Responsibility Principle
Данный принцип выражает единственную ответственность класса. Подробнее можно прочитать тут: "[Принцип единственности ответственности](http://blog.byndyu.ru/2009/10/blog-post.html)".
> Формулировка: не должно быть больше одной причины для изменения класса

По ссылке выше хорошо описано. Если просуммировать, то данный принцип просто говорит, что для создания класса должна быть чёткая цель, причём одна. Если он представлят из себя описание файла, хорошо. Если он для записи файла куда-то - это уже другая задача. И смешивать их нельзя. Каждое изменение из-за изменения требований к одной из задач может негативно сказаться на других.

## [↑](#Home) <a name="OpenClosed"></a> The **O**pen Closed Principle
Про данный принцип можно подробнее прочитать тут: "[Принцип открытости/закрытости](http://blog.byndyu.ru/2009/10/blog-post_14.html)".
Если кратко, то:
> Формулировка: программные сущности (классы, модули, функции и т.д.) должны быть открыты для расширения, но закрыты для изменения

Возможно, этот принцип понятнее раскрыт здесь: "[Open Close Principle](http://www.oodesign.com/open-close-principle.html)".
А наглядным примером мне показались фрэймворки, одной из главных особенностей работы которых является реализация Open Close Principal. Как было сказано:
> The open-closed principle is still present: you don’t need to change the internals of the framework to tweak the behaviour. You make and inject your own interface implementations, hook into listeners and events, extend classes and more.

## [↑](#Home) <a name="LiskovSubstitution"></a> The **L**iskov Substitution Principle
Барбара Лисков, американский учёный в области информатики, сформулировала данный принцип.
Данный принцип кратко можно охарактиризовать как:
> Derived types must be completely substitutable for their base types.

То есть "Производные типы должны быть полностью заменяемы для их базовых классов".
В более полной версии это звучало как:
> **Формулировка №1**: eсли для каждого объекта o1 типа S существует объект o2 типа T, который для всех программ P определен в терминах T, то поведение P не изменится, если o2 заменить на o1 при условии, что S является подтипом T.
**Формулировка №2**: Функции, которые используют ссылки на базовые классы, должны иметь возможность использовать объекты производных классов, не зная об этом.

Если подумать, то это логично. Опять же, это обосновывает определение интерфейса как контракта классов, его реализующих. Смысл в том, что реализующий контакт должен понимать, что его реализация не должна нурушать общие ожидания от поведения в рамках контакта.

Подробнее в статье "[Принцип замещения Лисков](http://blog.byndyu.ru/2009/10/blog-post_29.html)".

## [↑](#Home) <a name="InterfaceSegregation"></a> The **I**nterface Segregation Principle
Принцип разделения интерфейсов кратко можно сформулировать, как:
> клиенты не должны зависеть от методов, которые они не используют

Подробнее про данный принцип можно прочитать здесь: "[Принцип разделения интерфейса
](http://blog.byndyu.ru/2009/11/blog-post_19.html)".

## [↑](#Home) <a name="DependencyInjection"></a> The **D**ependency Inversion Principle
Принцип инверсии зависимостей можно сформулировать следующим образом:
> Формулировка:
Модули верхнего уровня не должны зависеть от модулей нижнего уровня. Оба должны зависеть от абстракции.
Абстракции не должны зависеть от деталей. Детали должны зависеть от абстракций.

Подробнее можно прочитать здесь: [Принцип инверсии зависимости](http://blog.byndyu.ru/2009/12/blog-post.html)

## [↑](#Home) <a name="Links"></a> Ссылки
По теме SOLID так же можно ознакомиться со следующими ссылками:
- [SOLID принципы. Рефакторинг](https://pro-prof.com/archives/1914)
- [Object-oriented design principles and the 5 ways of creating SOLID applications](https://zeroturnaround.com/rebellabs/object-oriented-design-principles-and-the-5-ways-of-creating-solid-applications/)