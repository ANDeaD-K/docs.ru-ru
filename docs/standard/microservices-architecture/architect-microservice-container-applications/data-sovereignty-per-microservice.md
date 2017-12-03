---
title: "Независимости данных на микрослужбу"
description: "Архитектура Микрослужбами .NET для приложений .NET в контейнерах | Независимости данных на микрослужбу"
keywords: "Docker, микрослужбы, ASP.NET, контейнер"
author: CESARDELATORRE
ms.author: wiwagn
ms.date: 05/26/2017
ms.prod: .net-core
ms.technology: dotnet-docker
ms.topic: article
ms.openlocfilehash: c51daae04215cfa6f5b5b8d2158a8ed1a8949652
ms.sourcegitcommit: c2e216692ef7576a213ae16af2377cd98d1a67fa
ms.translationtype: HT
ms.contentlocale: ru-RU
ms.lasthandoff: 10/22/2017
---
# <a name="data-sovereignty-per-microservice"></a>Независимости данных на микрослужбу

Правило для архитектуры микрослужбами является то, что каждого микрослужбу должен быть владельцем домена данных и логику. Так же, как завершенное приложение, которому принадлежит его логику и данные, поэтому необходимо каждого микрослужбу владельцем его логику и данные в группе автономных жизненного цикла, с помощью развертывания на микрослужбу независимо.

Это означает, что концептуальной модели домена будет отличаться от подсистем или микрослужбами. Рассмотрим корпоративных приложений, где клиента приложения управления взаимоотношениями (CRM), транзакций приобрести подсистемы и подсистемы поддержки клиентов в атрибуты сущностей уникального клиента и данные каждого вызова, а каждый использует другой Связывает контекст (BC).

Этот принцип аналогичен [разработки на основе домена (DDD)](https://en.wikipedia.org/wiki/Domain-driven_design), где каждый [ограниченного контекста](https://martinfowler.com/bliki/BoundedContext.html) или автономной подсистемы или службы должен быть владельцем его модели домена (данные плюс логику и поведение). В каждом контексте DDD ограниченного сопоставляется с одной бизнес микрослужбу (одну или несколько служб). (Мы расширяются на эту точку о шаблон ограниченного контекста в следующем разделе).

С другой стороны этот подход традиционных (монолитные данных), используется во многих приложениях — централизованной базе данных или несколько баз данных. Часто это нормализованной базы данных SQL, используемый для всего приложения и всех внутренних подсистем, как показано на рисунке 4-7.

![](./media/image7.png)

**Рис. 4-7**. Сравнение данных независимости: монолитные базы данных ее микрослужбами

Подход централизованную базу данных изначально выглядит более простой кажется, обеспечивая повторное использование сущностей в разные подсистемы, чтобы все согласованное. Но в действительности, вы получаете большой таблицы, которые обслуживают множество разные подсистемы и включать атрибуты и столбцы, которые не требуются в большинстве случаев. Он аналогичен пытается использовать то же сопоставление физических Туризм короткий следа, занимает trip car длительностью в день и обучения geography.

Монолитные приложения с обычно одну реляционную базу данных имеет два важных преимущества: [ACID-транзакции](https://en.wikipedia.org/wiki/ACID) и языка SQL, оба работы для всех таблиц и данных, связанных с приложением. Такой подход позволяет легко создавать запросы, объединяющий данные из нескольких таблиц.

Тем не менее при перемещении к архитектуре микрослужбами доступа к данным становится гораздо более сложной. Но даже в том случае, когда ACID-транзакции может или должен использоваться внутри микрослужбу или ограниченного контекста, данных, владельцем каждого микрослужбу является закрытым для этого микрослужбу и может осуществляться только через API-интерфейса микрослужбы. Инкапсуляция данных гарантирует, что микрослужбами слабо связаны и могут изменяться независимо друг от друга. Если несколько служб доступ к тем же данным, обновления схемы потребует скоординированного обновлений ко всем службам. Это нарушит автономность микрослужбу жизненного цикла. Но структуры распределенных данных означает через микрослужбами вносить одной транзакции ACID. В свою очередь означает, что необходимо использовать окончательной согласованности, если бизнес-процесс охватывает несколько микрослужбами. Это намного труднее реализации, чем простой соединения SQL; Аналогично многие другие возможности реляционной базы данных недоступны через несколько микрослужбами.

Даже продолжив, другой микрослужбами часто используют разные *типов* баз данных. Современных приложений для магазина и процесс различных типов данных и реляционной базы данных не всегда лучший выбор. Для некоторых вариантов использования, баз данных NoSQL, таких как Azure DocumentDB или MongoDB может быть более удобным модель данных и обеспечивают более высокую производительность и масштабируемость, чем в базе данных SQL как SQL Server или база данных SQL Azure. В других случаях реляционной базы данных по-прежнему является лучшим подходом. Таким образом, приложения на основе микрослужбами часто используют сочетание баз данных SQL и NoSQL, который иногда называют [polyglot сохраняемости](http://martinfowler.com/bliki/PolyglotPersistence.html) подход.

Секционированные постоянные полиглот архитектуры для хранения данных имеет множество преимуществ. К ним относятся слабо связанных служб и более высокую производительность, масштабируемость, затраты и управляемость. Тем не менее, может вызвать некоторые проблемы управления распределенными данными, как будет описано в «[определение границ модели домена](#identifying-domain-model-boundaries-for-each-microservice)» далее в этой главе.

## <a name="the-relationship-between-microservices-and-the-bounded-context-pattern"></a>Связь между микрослужбами и шаблон ограниченного контекста

Понятие микрослужбу является производным от [шаблона, ограниченное контекста (BC)](http://martinfowler.com/bliki/BoundedContext.html) в [разработки на основе домена (DDD)](https://en.wikipedia.org/wiki/Domain-driven_design). DDD имеет дело с большими моделями, разбивая их на несколько BCs и выполняется явно их границы. Каждый BC должен иметь собственную модель и база данных; Аналогичным образом каждый микрослужбу владеет соответствующими данными. Кроме того, каждый BC обычно имеет свой собственный [единый язык](http://martinfowler.com/bliki/UbiquitousLanguage.html) для взаимодействия между разработчиками и специалистами в предметной области.

Эти термины (главным образом сущности домена), в единый язык может иметь разные имена в различных контекстах ограниченных, даже в том случае, когда разные домена сущности используют одно удостоверение (то есть уникальный идентификатор, используемый для чтения объекта из хранилища). Для экземпляра в контексте ограниченных профиля пользователя, сущности пользователя домена может используют удостоверение с сущностью домена покупателей в контексте упорядочивания ограниченного.

Микрослужбу таким образом подобно ограниченного контекста, но также указывает, что это распределенная служба. Создается как отдельный процесс для каждого контекста ограниченных и он должен использовать распределенные протоколы, уже отмечалось ранее, например HTTP или HTTPS, WebSockets, или [AMQP](https://en.wikipedia.org/wiki/Advanced_Message_Queuing_Protocol). Шаблон ограниченного контекста, однако указывает ли контекст ограниченных — это распределенная служба, или это просто логическими границами (например, универсальный подсистемы) в рамках монолитные развертывания приложения.

Это важно обратить внимание, что определения службы для каждого контекста ограниченных является удобным инструментом для начала. Однако нет необходимости ограничивать его структуру. Иногда необходимо создать контекст ограниченных или микрослужбу бизнеса, состоящие из нескольких физических служб. Но в конечном счете, для обоих шаблонов — связывает контекст и микрослужбу — тесно связаны.

Преимущества микрослужбами путем получения реальных границы в виде распределенных микрослужбами ддд. Но идеи, такими как не совместное использование модели между микрослужбами также выберите в контексте ограниченных.

### <a name="additional-resources"></a>Дополнительные ресурсы

-   **Крис Ричардсон. Шаблон: Базы данных каждой службы**
    [*http://microservices.io/patterns/data/database-per-service.html*](http://microservices.io/patterns/data/database-per-service.html)

-   **Мартин Фоулер (Martin Fowler). BoundedContext**
    [*http://martinfowler.com/bliki/BoundedContext.html*](http://martinfowler.com/bliki/BoundedContext.html)

-   **Мартин Фоулер (Martin Fowler). PolyglotPersistence**
    [*http://martinfowler.com/bliki/PolyglotPersistence.html*](http://martinfowler.com/bliki/PolyglotPersistence.html)

-   **Alberto Brandolini. Стратегическое домена Driven Design с сопоставлением контекста**
    [*https://www.infoq.com/articles/ddd-contextmapping*](https://www.infoq.com/articles/ddd-contextmapping)


>[!div class="step-by-step"]
[Предыдущие] микрослужбами (architecture.md) [Далее] (логического и физического architecture.md)