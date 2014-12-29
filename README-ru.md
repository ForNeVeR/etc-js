Сборник полезных утилит для JavaScript
==

Предистория
--
Длительное время писал код на JS и при этом собирал часто используемы фрагменты кода в одном файле чтобы было легче копипастить. В итоге этот нарор разросся до того что его целесообразней было оформить как библиотеку.

Основная концепция
--
Функции такие же значения как и все остальные, следовательно их можно вычислять.

Все функции используют `this` и следовательно могут использоваться как методы. Таким образом функции вычисленые из функций так же должны использовать `this` и использоваться в качестве методов.

Большенство использованых алгоритмов используют элементарный набор возможностей. В итоге оказалось что возможности ECMAScript5 можно не использовать. Библиотека выдержана в стиле ECMAScript3.

Почему CoffeeScript
--
Быблиотека могла быть написана на оригинальном JavaScript, однако на CoffeeScript писать проще, короче и быстрее. В общем, не зачем создавать себе сложности.

Почти все примеры кода в данной инструкции будут на CoffeeScript. Если по ним возникнут вопросы, всегда можно зайти на [coffeescript.org] и воспользоваться вебинтерфейсом для преобразования кода в JavaScript.

Декоратор `$F`
--
Чтобы не модифицировать Function.prototype импользутеся декоратор $F. Он создаёт из функции функцию делающую то же самое, но при этом имеющую дополнительное API. Это позволяет с оригинлальной функцией работать так же как и раньше в других частях кода. Тем более что эта функция может быть экземпляром другого типа данных основаного на функциях.

Прототип-модификатор `$F.prototype`
--
Чтобы работали встроенные в JavaScript возможности ООП необходимо чтобы прототип типа данных основаного на функциях так же был функцией. Можно было бы использовать для этого произвольную функцию, но мне понадобилась функция которая прикрепляет необходимый интерфейс к существующим функциям а придумать для неё подходящее название было проблематично. Таким образом в прототип поместил функцию прикрепляющую конструктор $F к заданой функции.

Данная функция широко используется внутри библиотеки, однакто рекомендуется её использовать и в обычном коде, так как это будет оптимальней по сравнению с использованием декоратора `$F` так как при этом не создаётся вспомгательная функция. Противопоказание: использование другого подобного интерфейса, оригинальный интерфейс просто заменяется данным.

`then`
--
Позвояет после выполнения текущей функции вызвать следующую с результатом работы текущей функции в качестве аргумента.

Визуально это выглядит как смена порядка применения унарных функций.
```coffeescript
do $F(->"example").then(alert)
```
работает так же как
```coffeescript
alert "example"

```

Примечание: есть предложение позволить вместе с функцией передавать дополнительно список аргументов. Целесообразность этого под вопросом, возможно это будет реализовано в дальнейшем.

`catch`
--
Исключение такой же результат работы вичислительного алгоритма как и возращенное значение. Более того это неотемлемая часть API как стандартных библиотек так и многих других. Раз это результат работы, то и обрабатываться он должен как результат работы.

Функция catch устанавливает обработчик исключений вызваных функцией, что предполагает его дальнейшее использование в вычислениях.
```coffeescript
parseJSON = $F(JSON.parse).catch ->null
```

Примечание: тут тоже можно перндавать дополнительные параметры.

`bind`
--
В стандартном наборе ECMAScript5 этот метод возращает функцию, следовательно в нашем случае эта функция должна иметь соответствующее API. В ECMAScript3 этого метода нет, однако он очень полезен, следовательно должен быть частью библиотеки.

`bindArguments`
--
Позволяет получить функцию игнорирующую агументы. По такому же принципу, по которому результат работы `bind` игнорирует `this`.

Название может вскоре поменятся, потому в продакшене его лучше пока не использовать.

Так же рассматривается необходимость метода который прикрепляет к функции только первые аргументы, название для него так же выбирается.

`catchCond`
--
Часто библиотека или просто часть приложения использует в своей работе исключения. Однако в большенстве случаев их интересуют их собственные исключения, остальные предоставляются другим частям кода.
Данный метод позволяет отфильтровать возникшие исключения по условию.

`catchVal`
--
Для возрата управлени через исключения часто используются специальные значения. Этот метод позволяет их учитывать.

`catchType`
--
Фильтрует исключения по их типу.

`loop`
--
Повторяет функцию до возникновения исключения. В некоторых случаях удобно использовать вмето циклов.

`times`
--
Повторяет функцию указанное количество раз помещая результат в результирующий массив. Удобная замена для циклов.

Так же позволяет указать `this` вторым аргументом.

`repeat`
--
Просто повторяет функцию заданное количество раз. Для удобства возращает её же.

`this` передаётся вторым аргументом.

`curry`
--
Позволяет аргументы применять частично. Превращает функцию в цепочку функций с одним аргументом возращающих функции. Количество этих вложенностей можно задавать.

`bindedCurry`
--
Применяя `curry` теряется связь с объектом для метода которого эта функция была вызвана. Чтобы устранить этот недостаток этот объект прикрепляетмя к функции при первом вызове. Таким образом результат работы каррированой функции удобно использовать в качестве метода.

`curryBreak`
--
Работает наподобие `curry`, однако список аргументов разбиваент не по одному, а диапазонами. Рекомендуется использовать с API где у функций достаточно большое количество аргументов.

`preprocessAll`
--
Функция более вспомогательная. Принимает функцию принимающую массив и возращающую массив. Используется для замены списка аргументов функции. Можно использовать для работы с различными API.

`flip`
--
Меняет местами аргументы с указаными номерами.

`preprocess`
--
Принимает фуннкии которые применяются к первым аргументам итоговой функции. Можно использовать как обёртку над различными API чтобы заменить аргументы на допустимые.

`preprocessStrict`
--
Делает то же самое что и `preprocess`, только аргументы для которых не указан колбек игнорируются.

`guard`
--
Проверяет результат на соответствие условию. В случае несоответствия бросает исключение типа `$F.Error`.

Примечания: Название функции возможно поменяется. Так же планируется доработать иерархию исключений в расчёте на упрощение отладки.

`guardType`
--
Проверяет тип результата работы функции. В случае несоответствия бросает исключение типа `$F.Error`.

`guardArguments`
--
Проверяет аргументы на соответствие условиям. В случае несоответствия бросает исключение типа `$F.Error`.

`guardArgumentsTypes`
--
Проверяет типы аргументов на соответствие условиям. В случае несоответствия бросает исключение типа `$F.Error`.

`zipper`
--
Порождает функцию принимающую массивы и выдающую массив элементы которого являются результатами применения исходной функции к элементам заданных маммивов.

Результат работы `zipper` может использоваться в качестве метода.

`zip`
--
Объеденяет массивы данной функцией.

`zipWith`
--
То же что и `zip` только первым аргумтом идёт объект который будет использоваться как `this`.

Примечание: Необходимость данной функции сомнительна, возможно она будет удалена. К тому же под сомнением её название.

`objectZipper`
--
Аналог `zipper`, но для объектов. Можно указать объект приемник. В качестве приемника рекомендуется использовать один из входных параметров для модификации его полей.

`zipObjects`
--
Применяет текущую функцию для объеденения объектов.

`zipObjectsWith`
--
Объеденяет объекты, первым аргументом задаётся `this`

`zipObjectsTo`
--
Объеденяет объекты используя первый аргумент в качестве приемника.

`cell`
--
Реализует API подобное клеткам в Clojure.

Примечания: В данную библиотеку слабо вписывается, возможно будет вынесено отдельно в качестве дополнения.

`commonKeys`
--
Выдаёт массив ключей общих для указаных объектов. Используется в `objectZipper`, однако полезна сама по себе.

`Error`
--
Планируется набор исключений для упрощения отладки внутри библиотеки. Сейчас это только заглушка.


[coffeescript.org]:http://coffeescript.org