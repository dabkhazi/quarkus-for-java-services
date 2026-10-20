[English](quarkus-for-java-services.en.md) · **Русский**

# Quarkus для Java-сервисов: преимущества, ограничения и сравнение со Spring Boot

**Актуальность: 20 октября 2026 года.** Сравнение относится к последним стабильным версиям **Quarkus 3.40.1.1** (ветка 3.40 LTS) и **Spring Boot 4.1.1**. Предварительные выпуски не учитываются. Версии указаны на [странице выпуска Quarkus 3.40.1.1](https://github.com/quarkusio/quarkus/releases/tag/3.40.1.1) и [странице Spring Boot](https://spring.io/projects/spring-boot/).

## Почему Quarkus

Quarkus объединяет широкий набор Jakarta EE и MicroProfile API с подготовкой приложения на этапе сборки. В него входят веб-сервисы, внедрение зависимостей, доступ к данным, транзакции, валидация, JSON, конфигурация, безопасность, наблюдаемость и обмен сообщениями. Для команды с опытом Jakarta EE и MicroProfile это сохраняет привычную модель программирования, а перенос обработки метаданных на сборку сокращает работу при запуске. [Quarkus CDI 3.40](https://quarkus.io/version/3.40/guides/cdi-reference/), [Quarkus Persistence 3.40](https://quarkus.io/version/3.40/guides/hibernate-orm/), [модель расширений 3.40](https://quarkus.io/version/3.40/guides/writing-extensions/)

Quarkus подготавливает приложение на этапе сборки для обоих режимов запуска: JVM и Native Image. Подходящий режим выбирают по требованиям сервиса и результатам измерений под нагрузкой. [Quarkus: JVM и Native Image](https://quarkus.io/guides/building-native-image/)

## Развитие Quarkus и его экосистемы

Quarkus публично представили как открытый проект в марте 2019 года. Уже в ноябре того же года у проекта было 185 контрибьюторов; к выпуску Quarkus 3.40 LTS в сентябре 2026 года их совокупное число достигло 1250. Это показывает, как выросло сообщество разработчиков проекта. О распространении Quarkus говорит и опрос Eclipse Foundation 2024 года: его использовали 32% респондентов, работавших с облачными Java-приложениями. Доля относится к участникам опроса, а не ко всему рынку Java. [История проекта](https://quarkus.io/blog/quarkus-in-a-foundation/), [контрибьюторы в 2019 году](https://quarkus.io/blog/quarkus-1-0-0-CR2-released/), [Quarkus 3.40 LTS](https://quarkus.io/blog/quarkus-3-40-released/), [опрос Eclipse Foundation](https://jakarta.ee/blogs/2024-cloud-native-java-survey-findings/)

Quarkus развивается вместе с платформой Java: поддерживает Java 25 и виртуальные потоки, использует Vert.x для неблокирующего ввода-вывода, сочетает императивную и реактивную модели и позволяет собирать приложения для JVM или в Native Image. Подготовка компонентов при сборке связывает эти возможности с моделью расширений Quarkus. [Java 25](https://quarkus.io/blog/quarkus-3-31-released/), [виртуальные потоки](https://quarkus.io/version/3.40/guides/rest-virtual-threads/), [реактивная архитектура](https://quarkus.io/version/3.40/guides/quarkus-reactive-architecture/), [модель расширений](https://quarkus.io/version/3.40/guides/writing-extensions/)

Платформенные расширения и проекты Quarkiverse охватывают основные задачи современных сервисов: доступ к данным, безопасность, обмен сообщениями, наблюдаемость, интеграцию с облачной инфраструктурой и другие направления. Каталог продолжает пополняться; при выборе расширения следует учитывать его статус и совместимость с используемой версией Quarkus. [Каталог расширений](https://quarkus.io/extensions/), [реестр расширений](https://quarkus.io/guides/extension-registry-user/)

## Знакомая модель Jakarta EE и MicroProfile

Quarkus поддерживает широкий набор API Jakarta EE через расширения. Разработчики могут использовать знакомые аннотации и интерфейсы, подключая к сервису нужные технологии. Основные возможности перечислены в таблице ниже.

| Область Jakarta EE | Реализация в Quarkus 3.40.1.1 |
| --- | --- |
| Веб-ресурсы и внедрение зависимостей | Jakarta REST через Quarkus REST; CDI 4.1 Lite с дополнениями через ArC. [REST](https://quarkus.io/guides/rest/), [CDI](https://quarkus.io/version/3.40/guides/cdi-reference/) |
| Servlet и WebSocket | Servlet API через `quarkus-undertow`; Jakarta WebSocket через `quarkus-websockets`. Расширение WebSockets Next предлагает отдельный API и не реализует Jakarta WebSocket. [Servlet](https://quarkus.io/version/3.40/guides/http-reference/), [Jakarta WebSocket](https://quarkus.io/version/3.40/guides/websockets/) |
| Хранение данных и транзакции | Jakarta Persistence через Hibernate ORM и Jakarta Transactions через Narayana JTA. [Persistence](https://quarkus.io/version/3.40/guides/hibernate-orm/), [Transactions](https://quarkus.io/guides/transaction/) |
| Проверка данных | Jakarta Validation через Hibernate Validator. [Validation](https://quarkus.io/guides/validation/) |
| JSON | Jakarta JSON Binding через JSON-B и Jakarta JSON Processing через JSON-P доступны с соответствующими расширениями. [JSON REST](https://quarkus.io/guides/rest-json/) |
| Репозитории данных | Jakarta Data поддерживается расширением Quarkus Data Hibernate; само расширение находится в экспериментальном статусе, поэтому его зрелость следует учитывать при выборе. [Quarkus Data Hibernate](https://quarkus.io/version/3.40/guides/quarkus-data-hibernate/) |

### Panache для доступа к данным

Расширение `quarkus-hibernate-orm-panache` упрощает работу с сущностями и запросами поверх Hibernate ORM и Jakarta Persistence. Panache поддерживает два стиля: Active Record, при котором операции размещаются в классе сущности, и репозитории через `PanacheRepository`, при которых сущности остаются обычными Jakarta Persistence-сущностями. Это сокращает повторяющийся код для типовых операций с данными. [Hibernate ORM with Panache](https://quarkus.io/version/3.40/guides/hibernate-orm-panache/)

Panache — API Quarkus, а не часть Jakarta EE или MicroProfile. Его удобно использовать в новых сервисах, если команда принимает зависимость слоя доступа к данным от Quarkus; при требовании переносимости этого слоя можно работать напрямую с Jakarta Persistence. Panache также не следует смешивать с Jakarta Data и расширением Quarkus Data Hibernate из таблицы выше. [Hibernate ORM with Panache](https://quarkus.io/version/3.40/guides/hibernate-orm-panache/), [Quarkus Data Hibernate](https://quarkus.io/version/3.40/guides/quarkus-data-hibernate/)

### MicroProfile API

MicroProfile Config лежит в основе конфигурации Quarkus. Остальные API реализуются соответствующими расширениями платформы и подключаются по потребностям приложения:

| MicroProfile API | Реализация в Quarkus |
| --- | --- |
| MicroProfile Config | Конфигурация Quarkus основана на реализации SmallRye Config. [Руководство по конфигурации](https://quarkus.io/guides/config-reference/) |
| MicroProfile Health | Расширение `quarkus-smallrye-health` реализует проверки состояния приложения. [Руководство по Health](https://quarkus.io/guides/smallrye-health/) |
| MicroProfile REST Client | Расширение `quarkus-rest-client`, включая MicroProfile REST Client API. [Руководство по REST Client](https://quarkus.io/guides/rest-client/) |
| MicroProfile Fault Tolerance | Расширение `quarkus-smallrye-fault-tolerance` предоставляет средства повторных попыток, тайм-аутов и резервных действий. По умолчанию есть небольшие отличия от спецификации: для методов с `CompletionStage` или `Uni` асинхронная обработка применяется без `@Asynchronous`, а `@Retry`, `@Fallback` и `@CircuitBreaker` анализируют цепочку причин исключения. Для строгой совместимости включают `quarkus.fault-tolerance.mp-compatibility=true`. [Руководство по Fault Tolerance: Additional resources](https://quarkus.io/version/3.40/guides/smallrye-fault-tolerance/#additional-resources) |
| MicroProfile OpenAPI | Расширение `quarkus-smallrye-openapi` обрабатывает метаданные MicroProfile OpenAPI и публикует описание API. [OpenAPI](https://quarkus.io/version/3.40/guides/openapi-swaggerui/) |
| MicroProfile JWT RBAC | Расширение `quarkus-smallrye-jwt` реализует MicroProfile JWT для проверки токенов и работы с ролями. [JWT](https://quarkus.io/version/3.40/guides/security-jwt/) |
| MicroProfile Context Propagation | Расширение `quarkus-smallrye-context-propagation` переносит контексты между асинхронными этапами. [Context Propagation](https://quarkus.io/guides/context-propagation/) |
| MicroProfile Reactive Messaging | Расширения Quarkus Messaging используют спецификацию MicroProfile Reactive Messaging для каналов и обработчиков сообщений. [Messaging](https://quarkus.io/guides/messaging/) |
| MicroProfile GraphQL | Расширение `quarkus-smallrye-graphql` реализует MicroProfile GraphQL. [GraphQL](https://quarkus.io/guides/smallrye-graphql/) |

Команда может развивать сервис в терминах Jakarta EE и MicroProfile без перехода на Spring-компоненты. При переносе существующего приложения нужно учесть отличия в настройке, обнаружении бинов и поддержке CDI Full. Например, CDI Portable Extensions в Quarkus требуют иной реализации через механизмы расширений. [Ограничения ArC](https://quarkus.io/version/3.40/guides/cdi-reference/)

## Примечание: какая работа выполняется при сборке

При сборке Quarkus после компиляции приложения выполняется фаза **augmentation**. Она переносит часть работы фреймворка со старта приложения в сборочный конвейер. Объем работы зависит от подключенных расширений, классов приложения и конфигурации; точного универсального времени для этой фазы нет. [Quarkus: фазы запуска и сборки](https://quarkus.io/version/3.40/guides/writing-extensions/)

| Работа во время augmentation | Что уже подготовлено к запуску |
| --- | --- |
| Индексация классов и чтение метаданных аннотаций, конфигурации и дескрипторов подключенными расширениями | Метаданные не приходится заново разбирать в полном объеме при каждом запуске. [Quarkus extensions](https://quarkus.io/version/3.40/guides/writing-extensions/) |
| Обнаружение, проверка и связывание CDI-бинов; генерация необходимых классов и исключение неиспользуемых бинов | ArC получает готовую структуру для инициализации контейнера. [Quarkus CDI](https://quarkus.io/version/3.40/guides/cdi-reference/) |
| Анализ Jakarta REST-ресурсов и маршрутов, включая обнаружение конфликтующих endpoint-адресов | Ошибка дублирующего маршрута выявляется при сборке. [Quarkus REST](https://quarkus.io/guides/rest/) |
| Подготовка интеграций расширений: генерация байткода для инициализации runtime-сервисов и, где требуется, метаданных для Native Image | При запуске выполняется подготовленный код, а не повторяется весь анализ библиотек. [Quarkus extensions](https://quarkus.io/version/3.40/guides/writing-extensions/), [native tips](https://quarkus.io/guides/writing-native-applications-tips/) |
| При использовании Hibernate ORM — анализ сущностей, улучшение их байткода и разбор XML-маппингов | Модель ORM частично подготовлена заранее; `SessionFactory` при этом запускается вместе с приложением. [Hibernate ORM](https://quarkus.io/version/3.40/guides/hibernate-orm/) |

**Граница build time:** на JVM Quarkus все равно выполняет стадии инициализации при старте; подключение к БД, открытие порта и чтение runtime-конфигурации не исчезают. При сборке Native Image часть статической инициализации может дополнительно попасть в бинарный образ. Таким образом, Quarkus перемещает в сборку работу с метаданными и генерацию кода, но не весь запуск приложения. [Фазы Quarkus](https://quarkus.io/version/3.40/guides/writing-extensions/), [Hibernate ORM](https://quarkus.io/version/3.40/guides/hibernate-orm/)

## Работа с рефлексией

Рефлексия остается частью Java и нужна там, где состав типов определяется динамически. Quarkus стремится обходиться без нее в работе фреймворка: обнаруживает и связывает CDI-бины при сборке, заранее готовит необходимые классы и метаданные. Это особенно заметно при использовании расширений Quarkus, которые подготавливают интеграции к запуску и Native Image. Рефлексия при этом сохраняется в сценариях, где она нужна библиотекам приложения, например при некоторых вариантах JSON-сериализации. [Quarkus CDI](https://quarkus.io/version/3.40/guides/cdi-reference/), [Quarkus JSON](https://quarkus.io/guides/rest-json/), [Quarkus extensions](https://quarkus.io/version/3.40/guides/writing-extensions/)

Поэтому рефлексию точнее считать **затратным инструментом для отдельных сценариев**, а не устаревшим API. Перенос обнаружения классов и анализа аннотаций на сборку сокращает работу при старте. В Native Image динамические обращения дополнительно требуют явных метаданных, если компилятор не может вывести их из кода. [Quarkus: native reflection](https://quarkus.io/guides/writing-native-applications-tips/)

| Режим | Spring Boot 4.1.1 | Quarkus 3.40.1.1 |
| --- | --- | --- |
| Обычная JVM | При стандартном запуске Spring обнаруживает компоненты и разбирает конфигурацию во время старта. Для вызова `@Bean`-методов и работы с отдельными членами классов он использует рефлексию; для AOP создает прокси. [Spring Boot AOT и JVM](https://docs.spring.io/spring-boot/reference/packaging/native-image/introducing-graalvm-native-images.html), [Spring AOP proxies](https://docs.spring.io/spring-framework/reference/core/aop/proxying.html) | ArC обнаруживает, проверяет и связывает CDI-бины при сборке; часть прокси и вспомогательного кода готовится заранее. Библиотеки приложения могут использовать рефлексию во время работы. [Quarkus CDI](https://quarkus.io/version/3.40/guides/cdi-reference/), [Quarkus JSON](https://quarkus.io/guides/rest-json/) |
| JVM с AOT-подготовкой | Spring Boot может запускать на JVM JAR с заранее сгенерированным кодом инициализации. Для этого приложение собирают с AOT-подготовкой и запускают с `spring.aot.enabled=true`. [Spring Boot: AOT на JVM](https://docs.spring.io/spring-boot/reference/packaging/aot.html) | Quarkus использует результаты augmentation при обычной сборке и запуске на JVM; отдельный режим AOT для этого не требуется. [Фазы Quarkus](https://quarkus.io/version/3.40/guides/writing-extensions/) |
| Native Image | Spring AOT заранее генерирует определения бинов, прокси и подсказки для GraalVM. Рефлексивный доступ к зарегистрированным типам сохраняется. [Spring Boot Native Image](https://docs.spring.io/spring-boot/reference/packaging/native-image/introducing-graalvm-native-images.html) | Quarkus и его расширения заранее регистрируют многие необходимые типы. Для динамических типов, которые нельзя определить при сборке, разработчик добавляет регистрацию. [Quarkus native tips](https://quarkus.io/guides/writing-native-applications-tips/) |

В JVM-сценарии отличие состоит в способе использования подготовки на этапе сборки: Quarkus применяет ее в стандартном процессе, а Spring Boot предоставляет отдельный режим AOT для JVM. Само по себе это различие не доказывает преимущество в скорости или потреблении ресурсов конкретного сервиса; их нужно измерять. [Spring Boot: AOT на JVM](https://docs.spring.io/spring-boot/reference/packaging/aot.html), [Фазы Quarkus](https://quarkus.io/version/3.40/guides/writing-extensions/)

## Удаление кода: три разных механизма

Здесь важно различать механизмы Quarkus и анализ, который выполняет GraalVM Native Image. ArC удаляет неиспользуемые CDI-бины при сборке Quarkus, а экспериментальная функция JAR tree-shaking может исключать недостижимые классы зависимостей. Анализ достижимости при нативной компиляции действует для приложений как на Quarkus, так и на Spring Boot.

| Механизм | Где действует | Когда нужен контроль |
| --- | --- | --- |
| **ArC удаляет неиспользуемые CDI-бины** | При сборке приложения Quarkus, **включая JVM-режим**. Если бин извлекается программно через `CDI.current()`, ArC может не увидеть использование. Такой бин помечают `@io.quarkus.arc.Unremovable` либо указывают в `quarkus.arc.unremovable-types`. [Quarkus CDI: removing unused beans](https://quarkus.io/version/3.40/guides/cdi-reference/) | Нужны проверки путей, где есть программный поиск CDI-бинов; для обычных статических точек внедрения риск ниже. |
| **Tree-shaking зависимостей JAR** | Отдельная **экспериментальная** функция Quarkus, отключенная по умолчанию. При включении исключает недостижимые классы зависимостей из JAR. К обычной сборке Quarkus приписывать ее нельзя. [Quarkus JAR tree-shaking](https://quarkus.io/guides/jar-tree-shaking/) | Проверять, только если функция включена, особенно при динамической загрузке классов. |
| **Анализ достижимости Native Image** | При нативной компиляции GraalVM может исключить код, который не виден в графе вызовов. Для нужного рефлексивного доступа Quarkus предлагает `@io.quarkus.runtime.annotations.RegisterForReflection` или конфигурацию Native Image; многие типы расширения регистрируют автоматически. Это ограничение native-режима касается и Spring Boot. [Quarkus native tips](https://quarkus.io/guides/writing-native-applications-tips/), [Spring Boot Native Image](https://docs.spring.io/spring-boot/reference/packaging/native-image/introducing-graalvm-native-images.html) | Проверять реальные сценарии нативного исполняемого файла, особенно динамическую сериализацию и сторонние библиотеки. |

`@Unremovable` и `@RegisterForReflection` решают **разные задачи**. Первая сохраняет CDI-бин при анализе ArC и может понадобиться также в JVM-режиме. Вторая сохраняет доступ к классу и его членам через рефлексию в Native Image; она не заменяет сохранение CDI-бина. Если Quarkus выводит тип из сигнатуры REST-метода, ручная регистрация обычно не нужна. При возврате нетипизированного `Response` тип тела может быть неизвестен при сборке, и тогда `@RegisterForReflection` может понадобиться. [Пример Quarkus REST JSON](https://quarkus.io/guides/rest-json/)

## Выбор JVM или Native Image для Quarkus

Для долгоживущего сервиса выбор между JVM и Native Image определяется его нагрузкой и ограничениями на ресурсы. JVM использует JIT-оптимизацию горячего кода и может дать более высокую пиковую пропускную способность; сборка обходится без нативной компиляции. Native Image ускоряет холодный старт и обычно сокращает расход памяти. Он может быть выгоден и для долгоживущего сервиса, если при той же нагрузке укладывается в меньшие лимиты памяти или CPU. [Quarkus: building a native executable](https://quarkus.io/guides/building-native-image/)

Сравнивать режимы стоит на одинаковой функциональной нагрузке: по пропускной способности, задержкам, потреблению памяти и CPU при планируемых лимитах. В расчет также входят время сборки и проверки динамических сценариев, которые требуются для Native Image. Подготовка CDI и других интеграций на этапе сборки сохраняется в JVM-режиме Quarkus.

## Ограничения, которые могут изменить решение

| Ограничение | Вес для команды Jakarta EE |
| --- | --- |
| Приложение использует CDI Full, Portable Extensions или особенности конкретного Jakarta EE-сервера | **Высокий**, если без них нельзя обойтись: ArC реализует CDI Lite с дополнениями, а Portable Extensions не поддерживает в обычном виде. Нужна проверка на прототипе. [Quarkus CDI](https://quarkus.io/version/3.40/guides/cdi-reference/) |
| В организации уже есть большой объем Spring-специфичных библиотек и сервисов | **Высокий** для миграции, ниже для нового проекта. Стоимость переписывания и сопровождения может перевесить пользу от привычных Jakarta API. |
| Используются программный поиск CDI-бинов или динамическая сериализация | **Умеренный**: первый случай требует внимания даже на JVM, второй особенно важен для Native Image. Для обоих есть документированные способы регистрации. [Quarkus CDI](https://quarkus.io/version/3.40/guides/cdi-reference/), [Quarkus REST JSON](https://quarkus.io/guides/rest-json/) |
| Нужна строгая переносимость поведения MicroProfile Fault Tolerance | **Низкий**: отличия поведения невелики, режим совместимости включается настройкой `quarkus.fault-tolerance.mp-compatibility=true`. При переносе проверить используемые асинхронные методы и обработку исключений. [Quarkus Fault Tolerance: Additional resources](https://quarkus.io/version/3.40/guides/smallrye-fault-tolerance/#additional-resources) |

### Альтернативы типовым компонентам Spring

В организации может уже сложиться набор технологий Spring для доступа к данным, безопасности и пакетной обработки. Для этих задач Quarkus предлагает собственные решения; ниже приведены примеры таких альтернатив. При переносе важно сопоставить конкретные используемые возможности и оценить стоимость изменения кода.

| Компонент Spring | Возможности Quarkus | Что проверить при переходе |
| --- | --- | --- |
| Spring Data JPA | Hibernate ORM с Panache для сущностей и репозиториев; расширение совместимости `quarkus-spring-data-jpa` поддерживает часть API Spring Data JPA. [Panache](https://quarkus.io/version/3.40/guides/hibernate-orm-panache/), [Spring Data JPA compatibility](https://quarkus.io/guides/spring-data-jpa/) | Panache использует собственный API; расширение совместимости не реализует все возможности Spring Data JPA. |
| Spring Security | Quarkus Security с расширениями для OIDC и JWT позволяет проверять токены и ограничивать доступ к ресурсам. [Quarkus OIDC](https://quarkus.io/guides/security-oidc-bearer-token-authentication/), [Quarkus JWT](https://quarkus.io/version/3.40/guides/security-jwt/) | Сопоставить используемые правила авторизации, механизмы аутентификации и нестандартные интеграции. |
| Spring Batch | Расширение Quarkiverse JBeret предоставляет Jakarta Batch для пакетной обработки; Quarkus Scheduler или Quartz запускают задачи по расписанию. [JBeret](https://quarkus.io/extensions/io.quarkiverse.jberet/quarkus-jberet/), [Scheduler и Quartz](https://quarkus.io/version/3.40/guides/scheduler-reference/) | Планировщик не заменяет обработку пакетных шагов. API JBeret отличается от Spring Batch; отдельно проверить хранение состояния и повторный запуск заданий. |

**Примечание.** Основной серверный дистрибутив Keycloak [перешел с WildFly на Quarkus](https://www.keycloak.org/docs/26.0.8/upgrading/index.html). Это пример применения Quarkus в крупном продукте безопасности. Сервисы на Quarkus могут использовать Keycloak как провайдера идентификации через [OIDC](https://quarkus.io/guides/security-oidc-bearer-token-authentication/).

Spring Boot может быть предпочтительнее, если сервис зависит от конкретных возможностей Spring Data, Spring Security или Spring Batch, перенос которых потребует существенной переработки. Для нового сервиса стоит оценивать нужные сценарии, а не только названия привычных библиотек.

**Рекомендация:** для нового сервиса команды с опытом Jakarta EE и MicroProfile выбрать Quarkus, если нет существенной зависимости от Spring-экосистемы. Режим JVM или Native Image определить по требованиям к ресурсам и измерениям на целевой нагрузке. При переносе существующего приложения заранее проверить зависимости от CDI Full, Portable Extensions и библиотек конкретного сервера.
