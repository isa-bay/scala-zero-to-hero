## Содержание

<details>
<summary>

### 1. Фундамент и философия языка
</summary>

- **Введение в Scala**
  - История создания (Мартин Одерски, EPFL, 2004)
  - Что такое Scala? **Scal**able **La**nguage
  - Взаимодействие с Java (JVM-язык, байт-код, совместимость)
  - Парадигмы: гибрид ООП и ФП (чистый ООП? чистое ФП?)
  - Кому подходит Scala? (Data Engineering, Backend (Fintech), ML)
- **Базовый синтаксис и первые шаги**
  - Установка и настройка (sbt, coursier, Metals IntelliJ)
  - Типы данных: `Int`, `Double`, `Boolean`, `Char`, `String`
  - `val` (неизменяемые ссылки) vs `var` (изменяемые) — фундаментальный выбор
  - Ленивые значения (`lazy val`)
  - Блоки выражений и тип `Unit` (`println()`)
  - Строковая интерполяция (`s`, `f`, `raw`)
- **Scala vs Java для опытных Java-разработчиков**
  - Всё — выражение (if, for, match возвращают значение)
  - Вывод типов (Type Inference) — как это работает
  - Отсутствие checked exceptions
  - Синглтон-объекты (object) вместо статики
- **Инструменты сборки: sbt (Scala Build Tool)**
  - Структура проекта (`build.sbt`, `project/`)
  - Основные задачи: `compile`, `run`, `test`, `assembly`
  - Управление зависимостями (libraryDependencies, конфликты версий)
  - Мультимодульные проекты
  - **sbt-плагины**: `sbt-assembly`, `sbt-native-packager`, `sbt-dependency-graph`
</details>

<details>
<summary>

### 2. Объектно-ориентированное программирование в Scala
</summary>

- **Классы и объекты (Classes & Objects)**
  - Определение класса, конструктор (первичный и вспомогательные)
  - Параметры класса (`class User(name: String, age: Int)`) — поля или параметры?
  - Переопределение методов (`override def`)
  - **Объекты-компаньоны (Companion Objects)**
    - Фабричные методы (`apply`), метод `unapply` (для экстракторов)
    - Хранение "статических" методов
- **Наследование и иерархия типов**
  - Абстрактные классы (abstract class)
  - Кейс-классы (case class) — мощь одной строки
    - Автоматический `equals`, `hashCode`, `toString`, `copy`
    - Сериализация
    - Применимость в паттерн-матчинге
  - Герметичные (sealed) иерархии — основа алгебраических типов данных (ADT)
  - Классы-значения (Value Classes) для избежания аллокаций (`extends AnyVal`)
- **Трейты (Traits)**
  - Что такое trait? (интерфейс + реализация)
  - Линеаризация (Linearization) — как решается проблема ромбовидного наследования
  - Стекируемые изменения (Stackable Modifications) через `super`
  - Трейты с параметрами (в Scala 3)
</details>

<details>
<summary>

### 3. Функциональное программирование (Core FP)
</summary>

- **Функции — объекты первого класса**
  - Анонимные функции (лямбды): `(x: Int) => x + 1`
  - Сахар для функций: `_ + _` (подчеркивание как заполнитель)
  - Функции как значения типов `FunctionN` (`Function1`, `Function2`)
  - Чистые функции (Pure Functions) и референциальная прозрачность (Referential Transparency)
  - Побочные эффекты (Side Effects) — где их размещать (край программы)
- **Высший порядок (Higher-Order Functions)**
  - Функции, принимающие функции: `map`, `flatMap`, `filter`
  - Функции, возвращающие функции (каррирование)
  - **Коллекции Scala: неизменяемость по умолчанию**
    - Иерархия: `Seq`, `List`, `Vector`, `Set`, `Map`
    - Производительность коллекций (головой об стену): когда `List`, когда `Vector`
    - Параллельные коллекции (`.par`)
    - Строгость (Eager) vs Ленивость (Lazy): `View`
- **Рекурсия и хвостовая рекурсия**
  - Проблема стека при обычной рекурсии
  - **Хвостовая рекурсия (Tail Recursion)** и аннотация `@tailrec`
  - Оптимизация хвостовой рекурсии компилятором (превращение в цикл)
  - Взаимная рекурсия (Trampolining)
- **Pattern Matching (Сопоставление с образцом)**
  - Базовая конструкция: `match { case ... => ... }`
  - Стражи (Guards): `case x if x > 0 =>`
  - Сопоставление с типами
  - Сопоставление с case class (глубокая декомпозиция)
  - Сопоставление с последовательностями (`case List(a, b, _*)`)
  - Запечатанные (sealed) иерархии — компилятор проверяет исчерпываемость (exhaustiveness)
  - **Экстракторы (Extractors)** и метод `unapply`
- **Частичные функции (Partial Functions)**
  - `PartialFunction[A, B]` и метод `isDefinedAt`
  - Синтаксис: `{ case ... => ... }`
  - Комбинация partial functions: `orElse`
- **Коллекции и монады (для перехода к ZIO/Cats)**
  - `Option` — контейнер для наличия/отсутствия значения
  - `Either` — вычисление с ошибкой (классический и право-ориентированный)
  - `Try` — работа с исключениями как со значениями (Success/Failure)
  - **For-comprehension** — синтаксический сахар для `flatMap`, `map`, `filter`
  - Понимание монад через for-comprehension
</details>

<details>
<summary>

### 4. Продвинутая система типов (Type System)
</summary>

- **Параметрический полиморфизм (Дженерики)**
  - Классы и методы с параметрами типа: `class Box[A]`
  - Вариантность (Variance) — ключ к безопасным дженерикам
    - **Ковариантность (Covariance):** `+A` (Producer)
    - **Контравариантность (Contravariance):** `-A` (Consumer)
    - **Инвариантность:** по умолчанию
    - Принцип PECS (Producer-Extends, Consumer-Super) в терминах Scala
  - Ограничения типов (Type Bounds)
    - Верхняя граница (Upper Bound): `A <: Animal`
    - Нижняя граница (Lower Bound): `A >: Cat`
    - Контекстные границы (Context Bounds): `A : Ordering` (связь с Type Classes)
- **Типы высшего порядка (Higher-Kinded Types)**
  - Что такое `* -> *`? Типы, принимающие типы.
  - Зачем нужно? Абстракция над контейнерами (например, `F[_]` в Cats/ZIO)
- **Неявные параметры и преобразования (Implicits)**
  - **Implicit Parameters:** автоматическая передача "контекста"
  - **Implicit Conversions:** опасная, но мощная вещь (лучше избегать, используя extension methods)
  - **Implicit Classes:** расширение существующих типов методами (pimp-my-library)
  - **Правила разрешения неявных значений** (где ищет компилятор)
  - **Где implicits в современном Scala:** Cats, ZIO, JSON-кодеки
- **Type Classes (Классы типов)**
  - Паттерн для ad-hoc полиморфизма
  - Компоненты: Type Class (trait), Instances (implicit val), Interface (методы)
  - Пример: `Show`, `Eq`, `Ordering`, `Functor`, `Monad`
  - Синтаксис (Interface Syntax) через extension-методы
- **Зависимые типы (Path-Dependent Types) и Singleton Types**
  - Внутренние классы и зависимость от внешнего экземпляра
  - Литеральные типы (Literal-based singleton types): `42.n`
- **Type Lambdas и полиморфизм**
  - Исправление несоответствия видов (kind mismatch) через лямбды
- **Материализация неявных значений (Implicit Derivation)**
  - Автоматическая генерация type class instances для case classes (shapeless, magnolia)
</details>

<details>

<summary>

### 5. Библиотеки эффектов и конкурентность (Effects & Concurrency)
</summary>

- **Проблемы "сырой" многопоточности**
  - `Thread`, `Runnable`, `synchronized`, `wait/notify` — сложно и ошибкоопасно
  - `Future` из стандартной библиотеки (scala.concurrent)
    - Проблема: строгое вычисление, запускается сразу
    - Проблема: нет контроля над эффектами (ссылочная прозрачность)
    - ExecutionContext — неявный глобальный пул потоков
- **Библиотеки эффектов (Referential Transparency)**
  - **Cats Effect**
    - `IO[A]` — описание программы с эффектами
    - Асинхронность, конкурентность, отмена (cancelation)
    - `Resource` — безопасное управление ресурсами
    - Fibers (легковесные потоки)
  - **ZIO**
    - `ZIO[R, E, A]` — эффект с окружением, ошибкой и значением
    - Службы (Services) и модульное тестирование через слой окружения (ZLayer)
    - Конкурентные структуры: Queue, Ref, Semaphore, Promise
    - Стриминг: **ZIO Streams**
  - **Сравнение:** ZIO vs Cats Effect vs Monix
- **Акторы и Akka (классика)**
  - Модель акторов
  - Akka Actors (typed vs classic)
  - Akka Cluster, Cluster Sharding, Distributed Data
  - Akka Streams (Reactive Streams)
- **Обзор: Pekko** — форк Akka после смены лицензии
</details>

<details>
<summary>

### 6. Метапрограммирование и Scala 3
</summary>

- **Макросы (Scala 2)**
  - Что такое макросы? (экспериментально, для библиотек)
- **Scala 3 (Dotty) — новая эра**
  - **Ключевые изменения:**
    - Упрощенный синтаксис (optional braces)
    - Переработанные неявные (implicits) → **Given/Using** (контекстные параметры)
    - Extension методы
    - Export clauses
    - Enumeration (теперь настоящие алгебраические типы данных)
    - Union Types (`A | B`) и Intersection Types (`A & B`)
    - Opaque Types (сокрытие реализации)
    - Мультиверсионность (Multiversal Equality)
  - **Контекстные абстракции (Contextual Abstractions)**
    - `given` instances
    - `using` clauses
    - Глобальная замена implicit'ов
  - **Макросы в Scala 3** (более безопасные и стабильные)
- **Миграция со Scala 2 на Scala 3** (совместимость, кросс-билды)
</details>

<details>
<summary>

### 7. Работа с данными и стеки для Data Engineering
</summary>

- **Apache Spark на Scala**
  - Почему Scala — "родной" язык для Spark?
  - Dataset API и типизированные трансформации
  - Написание UDF (простые и сложные)
  - Структурированные типы: работа с `ArrayType`, `MapType`, `StructType`
  - Под капотом: Catalyst и Tungsten (как генерируется код)
- **Frameless** — типизированная обертка над Spark Dataset
- **Scala и базы данных**
  - **Slick** (Functional Relational Mapping) — компилируемые запросы
  - **Doobie** (чистая функциональная работа с JDBC)
  - **Quill** — compile-time query generation
- **JSON (де)сериализация**
  - **Circe** (библиотека от авторов Cats)
  - **Play-JSON**
  - **uPickle**
  - Автоматическая генерация кодеков через полуавтоматическую и автоматическую деривацию
</details>

<details>
<summary>

### 8. Тестирование и качество кода
</summary>

- **Библиотеки тестирования**
  - **ScalaTest:** `WordSpec`, `FlatSpec`, `FunSuite`
  - **Specs2**
  - **MUnit** (легковесный, быстрый)
- **Свойства и Property-based testing**
  - **ScalaCheck** — генерация случайных данных и проверка свойств
  - Интеграция с ScalaTest (GeneratorDrivenPropertyChecks)
- **Тестирование эффектов**
  - `IO` (Cats Effect) — `IOAssertion`
  - `ZIO Test` — встроенный мощный test framework
  - Тестирование времени, контекста и отмены
- **Mock-и и стабы**
  - Mockito (с интеграцией для Scala)
  - ScalaMock
- **Инструменты статического анализа**
  - **Scapegoat**
  - **WartRemover**
  - **Scalafix** (рефакторинг и линтинг)
  - **Scalafmt** (форматирование кода)
</details>

<details>
<summary>

### 9. Паттерны проектирования и Архитектура
</summary>

- **Функциональная архитектура**
  - **Onion Architecture** (чистая архитектура на Scala)
  - **Tagless Final** — абстракция над эффектами
    - Определение алгебры (trait Algebra[F[_]])
    - Интерпретаторы для разных эффектов (IO, Task, Id)
  - **Free Monads** (альтернативный подход)
  - **ZIO Environment (ZLayer)** — модульное построение графа зависимостей
- **Стандартные паттерны GoF в Scala**
  - Строитель (Builder) через copy-метод case class
  - Одиночка (Singleton) через object
  - Фабрика (Factory) через apply в компаньоне
- **Реализация Domain-Driven Design (DDD)**
  - Моделирование домена через case classes и sealed traits
  - Value Objects и Entities
  - Алгебраические типы данных для домена
- **Обработка ошибок**
  - Не падай, возвращай! (No Exceptions for business logic)
  - `Either` vs `ZIO` vs `IO`
  - Бисквитная фабрика: композиция Either
</details>

<details>
<summary>

### 10. Сетевое программирование и Web-фреймворки
</summary>

- **HTTP-серверы**
  - **Akka HTTP** (high-level, low-level API)
  - **http4s** (чисто функциональный, на базе Cats Effect)
  - **Play Framework** (полноценный MVC)
  - **Finatra** (от Twitter)
  - **ZIO HTTP**
- **Клиенты**
  - **Sttp** — функциональный HTTP-клиент
- **Работа с gRPC / Protobuf**
  - ScalaPB
- **Брокеры сообщений**
  - Akka Streams + Kafka (Alpakka Kafka Connector)
  - FS2 (Functional Streams for Scala) + Kafka
</details>

<details>
<summary>

### 11. Производительность и оптимизация (Performance Tuning)
</summary>

- **Избегайте аллокаций**
  - Value Classes
  - `@specialized` для примитивов
  - Переиспользование объектов
- **Сборка мусора (JVM GC)**
  - Настройка GC для низких задержек (G1, Shenandoah, ZGC)
  - Понимание влияния аллокаций на паузы GC
- **Профилирование**
  - Java Flight Recorder (JFR)
  - Async Profiler
  - YourKit / VisualVM
- **Бенчмаркинг**
  - **JMH (Java Microbenchmark Harness)** с sbt-jmh
- **Параллелизм и конкурентность**
  - Понимание работы `Future` (Execution context, thread pools)
  - `IO` vs `Future`: накладные расходы на планировщик
- **Dead code elimination и оптимизации компилятора**
</details>

<details>
<summary>

### 12. Интеграция с Java-экосистемой
</summary>

- **Вызов Java из Scala** (прозрачно)
- **Scala из Java** (сложности с неявными параметрами, трейтами)
- **Использование Java-библиотек**
  - Логгирование: **Logback + SLF4J**
  - Работа с БД: HikariCP (пул соединений)
- **Сборка и упаковка**
  - Создание "толстых" (fat/uber) JAR для Spark
  - Контейнеризация (Docker) Scala-приложений (JVM-оптимизации для контейнеров)
- **Интероп с GraalVM Native Image**
  - Компиляция Scala в нативный код (ограничения, reflection)
</details>

<details>
<summary>

### 13. Управление сложными проектами
</summary>

- **Миграции кода**
  - Совместимость между минорными версиями Scala (binary compatibility)
  - Сложности переезда с 2.12 на 2.13, на 3
- **Управление транзитивными зависимостями**
  - Evicted-зависимости и как с ними бороться
- **Документирование**
  - Scaladoc — написание понятной документации
- **Code Review для Scala**
  - Что искать: неправильное использование var, мутабельные коллекции, необработанные Future, блокирующие вызовы
- **Open Source и контрибьютинг**
  - Как читать код Cats / ZIO / Spark
</details>

<details>
<summary>

### 14. Scala для Senior: Собеседование и кругозор
</summary>

- **Теоретические вопросы**
  - Ковариантность/контравариантность в коробке с фруктами
  - Что такое монада? (For-comprehension desugaring)
  - Sealed trait vs abstract class
  - lazy val, val, def — разница в инициализации
  - Как работает линеаризация трейтов?
- **Практические задачи**
  - Написание type class (например, `JsonWriter`)
  - Работа с Future.sequence и распараллеливание
  - Трансформеры (OptionT, EitherT) — зачем они?
- **Архитектурные решения**
  - Tagless Final vs ZLayer
  - Когда использовать Akka, а когда ZIO?
  - Как строить приложение, устойчивое к ошибкам?
- **Что почитать/посмотреть**
  - "Functional Programming in Scala" (Книга, она же "Красная книга")
  - "Scala with Cats"
  - Блоги: Li Haoyi, Daniel Ciocîrlan (Rock the JVM)
</details>
