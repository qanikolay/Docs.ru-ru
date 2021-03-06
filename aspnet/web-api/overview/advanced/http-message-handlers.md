---
uid: web-api/overview/advanced/http-message-handlers
title: "Обработчики сообщений HTTP в ASP.NET Web API | Документы Microsoft"
author: MikeWasson
description: 
ms.author: aspnetcontent
manager: wpickett
ms.date: 02/13/2012
ms.topic: article
ms.assetid: 9002018b-3aa3-4358-bb1c-fbb5bc751d01
ms.technology: dotnet-webapi
ms.prod: .net-framework
msc.legacyurl: /web-api/overview/advanced/http-message-handlers
msc.type: authoredcontent
ms.openlocfilehash: 931ad3330d5e4dc2424ab37b8a6e2a3d123a8684
ms.sourcegitcommit: 9a9483aceb34591c97451997036a9120c3fe2baf
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 11/10/2017
---
<a name="http-message-handlers-in-aspnet-web-api"></a>Обработчики HTTP-сообщения в веб-API ASP.NET
====================
по [Mike Wasson](https://github.com/MikeWasson)

Объект *обработчик сообщений* является классом, который получает HTTP-запрос и возвращает ответ HTTP. Обработчики сообщений являются производными от абстрактного **HttpMessageHandler** класса.

Как правило ряд обработчики сообщений соединяются друг с другом. Первый обработчик получает запрос HTTP, выполняет некоторую обработку и дает в обработчик следующий запрос. На некотором этапе ответ создается и возвращается в цепочке. Эта модель называется *делегирование* обработчика.

![](http-message-handlers/_static/image1.png)

## <a name="server-side-message-handlers"></a>Обработчики сообщений на стороне сервера

На стороне сервера в конвейер веб-API использует обработчики для некоторых встроенных сообщений:

- **Сервер HTTP** получает запрос от узла.
- **HttpRoutingDispatcher** отправляет запрос на основе маршрута.
- **HttpControllerDispatcher** отправляет запрос на контроллер веб-API.

Можно добавить пользовательские обработчики в конвейере. Обработчики сообщений хорошо подходят для решении общих задач, которые работают на уровне HTTP сообщения (а не действия контроллера). Например обработчик сообщений могут:

- Чтение и изменение заголовков запроса.
- Добавьте заголовок ответа для ответов.
- Проверяет запросы, прежде чем они достигнут контроллера.

На этой диаграмме показаны две пользовательские обработчики, вставляемых в конвейере:

![](http-message-handlers/_static/image2.png)

> [!NOTE]
> На стороне клиента HttpClient также использует обработчики сообщений. Дополнительные сведения см. в разделе [обработчики сообщений HttpClient](httpclient-message-handlers.md).


## <a name="custom-message-handlers"></a>Обработчики пользовательских сообщений

Чтобы написать обработчик пользовательского сообщения, являются производными от **System.Net.Http.DelegatingHandler** и Переопределите **SendAsync** метод. Этот метод имеет следующую сигнатуру:

[!code-csharp[Main](http-message-handlers/samples/sample1.cs)]

Этот метод принимает **HttpRequestMessage** как входные данные и асинхронно возвращает **HttpResponseMessage**. Типичная реализация выполняет следующие задачи.

1. Обрабатывает сообщение запроса.
2. Вызовите `base.SendAsync` отправлять запрос внутреннему обработчику.
3. Внутренний обработчик возвращает ответное сообщение. (Этот шаг является асинхронной.)
4. Обработать ответ и возвращается вызывающему объекту.

Вот упрощенного примера:

[!code-csharp[Main](http-message-handlers/samples/sample2.cs)]

> [!NOTE]
> Вызов `base.SendAsync` является асинхронным. Если обработчик не выполняет никаких действий после этого вызова, используйте **await** ключевое слово, как показано.


Делегирование обработчик можно также пропустить внутренний обработчик и непосредственного создания ответа:

[!code-csharp[Main](http-message-handlers/samples/sample3.cs)]

Если делегирование обработчик создает ответ без вызова `base.SendAsync`, запрос пропускает оставшуюся часть конвейера. Это может быть полезно для обработчик, который проверяет запрос (Создание ответ с ошибкой).

![](http-message-handlers/_static/image3.png)

## <a name="adding-a-handler-to-the-pipeline"></a>Добавление обработчика в конвейере

Чтобы добавить обработчик сообщений на стороне сервера, добавьте обработчик **HttpConfiguration.MessageHandlers** коллекции. Если шаблон «ASP.NET MVC 4 веб-приложения» используется для создания проекта, это можно сделать это внутри **WebApiConfig** класса:

[!code-csharp[Main](http-message-handlers/samples/sample4.cs)]

Обработчики сообщений вызываются в том же порядке, в котором они появляются в **MessageHandlers** коллекции. Поскольку они вкладываются ответное сообщение перемещается в обратном направлении. Последним обработчиком является первым получает ответное сообщение.

Обратите внимание, что не требуется задать внутренних обработчиков; Платформа веб-API автоматически подключается обработчиков сообщений.

Если вы являетесь [резидентным](../older-versions/self-host-a-web-api.md), создайте экземпляр класса **HttpSelfHostConfiguration** и добавьте обработчики для **MessageHandlers** коллекции.

[!code-csharp[Main](http-message-handlers/samples/sample5.cs)]

Теперь давайте рассмотрим некоторые примеры обработчики пользовательских сообщений.

## <a name="example-x-http-method-override"></a>Пример: X-HTTP-Method-Override

X-HTTP-Method-Override является стандартным заголовком HTTP. Она предназначена для клиентов, которые не отправляют определенные типы HTTP-запросов, такой как PUT или DELETE. Вместо этого клиент отправляет запрос POST и задает заголовок X-HTTP-Method-Override для нужного метода. Пример:

[!code-console[Main](http-message-handlers/samples/sample6.cmd)]

Ниже приведен обработчик сообщений, который добавляет поддержку для X-HTTP-Method-Override.

[!code-csharp[Main](http-message-handlers/samples/sample7.cs)]

В **SendAsync** метод, обработчик проверяет, находится ли сообщение-запрос POST-запрос и содержит ли заголовок X-HTTP-Method-Override. В этом случае он проверяет значение заголовка и затем изменяет метод запроса. Наконец, обработчик вызывает `base.SendAsync` для передачи сообщения обработчику Далее.

Когда запрос достигает **HttpControllerDispatcher** класса **HttpControllerDispatcher** направит запрос на основе метода обновленный запрос.

## <a name="example-adding-a-custom-response-header"></a>Пример: Добавление настраиваемого заголовка ответа

Ниже приведен обработчик сообщений, который добавляет пользовательский заголовок в каждое сообщение ответа.

[!code-csharp[Main](http-message-handlers/samples/sample8.cs)]

Во-первых, обработчик вызывает `base.SendAsync` для передачи запроса внутренний обработчик сообщений. Внутренний обработчик возвращает ответное сообщение, но не асинхронно с помощью **задачи&lt;T&gt;**  объекта. Ответное сообщение недоступно до `base.SendAsync` завершения асинхронно.

В этом примере используется **await** ключевое слово для выполнения работы асинхронно после `SendAsync` завершения. Если вы используете .NET Framework 4.0, используйте **задачи**&lt;T&gt;**. ContinueWith** метод:

[!code-csharp[Main](http-message-handlers/samples/sample9.cs)]

## <a name="example-checking-for-an-api-key"></a>Пример: Поиск ключа API

Некоторые веб-службы требуется клиентов включить ключ API в свой запрос. В следующем примере показано, как обработчик сообщений можно проверить запросы действительного ключа API:

[!code-csharp[Main](http-message-handlers/samples/sample10.cs)]

Этот обработчик ищет ключ API в строке запроса URI. (В этом примере предполагается, что ключ является статическая строка. Фактической реализации, скорее всего, использовать более сложные проверки.) Если строка запроса содержит ключ, обработчик передает запрос внутреннему обработчику.

Если запрос не имеет допустимый ключ, обработчик создает ответное сообщение с состояния 403, запрещено. В этом случае обработчик вызывает `base.SendAsync`, поэтому внутренний обработчик не получает запрос, а также контроллер. Таким образом контроллер может предполагается, что все входящие запросы допустимый ключ API.

> [!NOTE]
> Если ключ API применяется только к определенным действиям контроллера, можно использовать фильтр действий вместо обработчик сообщений. Фильтры действий выполняются после выполняется маршрутизация URI.


## <a name="per-route-message-handlers"></a>Обработчики сообщений на маршрут

Обработчики в **HttpConfiguration.MessageHandlers** коллекции применяются глобально.

Кроме того можно добавить обработчик сообщений для конкретного маршрута при определении маршрута:

[!code-csharp[Main](http-message-handlers/samples/sample11.cs?highlight=16)]

В этом примере, если URI запроса совпадает с «Route2», запрос отправляется `MessageHandler2`. В примере ниже показан конвейер для этих маршрутов:

![](http-message-handlers/_static/image4.png)

Обратите внимание, что `MessageHandler2` заменяет значение по умолчанию **HttpControllerDispatcher**. В этом примере `MessageHandler2` создает ответ и запросы, которые соответствуют «Route2» никогда не переходить к контроллеру. Это позволяет заменить весь механизм контроллера веб-API с пользовательской конечной точкой.

Кроме того, можно делегировать обработчик сообщений-маршрута **HttpControllerDispatcher**, который затем перенаправляется в контроллере.

![](http-message-handlers/_static/image5.png)

Ниже показано, как настройка этого маршрута:

[!code-csharp[Main](http-message-handlers/samples/sample12.cs)]
