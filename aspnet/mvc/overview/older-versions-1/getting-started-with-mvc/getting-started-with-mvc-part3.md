---
uid: mvc/overview/older-versions-1/getting-started-with-mvc/getting-started-with-mvc-part3
title: "Добавление представления | Документы Microsoft"
author: shanselman
description: "Это руководство для новичков, в котором представлены основные сведения по ASP.NET MVC. Вы создадите простое веб-приложение выполняет чтение и запись из базы данных."
ms.author: aspnetcontent
manager: wpickett
ms.date: 08/14/2010
ms.topic: article
ms.assetid: e8f1515c-c277-47ff-a23e-224118f13f02
ms.technology: dotnet-mvc
ms.prod: .net-framework
msc.legacyurl: /mvc/overview/older-versions-1/getting-started-with-mvc/getting-started-with-mvc-part3
msc.type: authoredcontent
ms.openlocfilehash: 509dd301eef7c00431eae194a0df69d70e6d80f8
ms.sourcegitcommit: 9a9483aceb34591c97451997036a9120c3fe2baf
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 11/10/2017
---
<a name="adding-a-view"></a>Добавление представления
====================
по [Скотт Хансельман](https://github.com/shanselman)

> Это руководство для новичков, в котором представлены основные сведения по ASP.NET MVC. Вы создадите простое веб-приложение выполняет чтение и запись из базы данных. Посетите [центр обучения ASP.NET MVC](../../../index.md) для поиска других ASP.NET MVC, учебники и примеры.


В этом разделе мы собираемся просмотрите способом можно иметь нами класса HelloWorldController полностью инкапсулировать генерации HTML ответов обратно клиенту с помощью файла шаблона представления.

Давайте начнем с помощью шаблона представления с нашей метод индекса. Наш метод вызывается индекса, и он находится в HelloWorldController. В настоящее время наши Index() метод возвращает строку с сообщением, жестко задано в класс контроллера.

[!code-csharp[Main](getting-started-with-mvc-part3/samples/sample1.cs)]

Теперь изменим метод индекса вместо этого следующим образом:

[!code-csharp[Main](getting-started-with-mvc-part3/samples/sample2.cs)]

Теперь добавим шаблона представления в свой проект, который можно использовать для наших Index() метода. Для этого щелкните правой кнопкой мыши где-нибудь в середине метода индекс и нажмите кнопку Добавить представление...

![изображение](getting-started-with-mvc-part3/_static/image1.png)

Откроется диалоговое окно «Добавление представления», обеспечивающий нам некоторые параметры как требуется создать шаблон представления, можно использовать наши методом индекса. На данном этапе не внесет необходимые изменения и нажмите кнопку "Добавить".

[![Диалоговое окно добавления представления](getting-started-with-mvc-part3/_static/image3.png)](getting-started-with-mvc-part3/_static/image2.png)

После нажатия кнопки Добавить новую папку и открывается новый файл появится в папке решения, как показано ниже. Теперь у меня есть папку HelloWorld для представления и файл Index.aspx в этой папке.

[![solutionexplorerwithindex](getting-started-with-mvc-part3/_static/image5.png)](getting-started-with-mvc-part3/_static/image4.png)

Новый файл индекса также уже открыта и готова для редактирования. Добавьте текст в первый &lt;h2&gt;индекс&lt;/h2&gt; , например «Hello World».

[!code-html[Main](getting-started-with-mvc-part3/samples/sample3.html)]

Запустите приложение, а также [ `http://localhost:xx/HelloWorld` ](http://localhostxx) снова в браузере. Метод индекса в нашем контроллера в этом примере не выполняет никаких действий, но обращение выполнялось «возвращаемого View()», которая указывает, мы хотим использовать файл шаблона представления для отображения ответа обратно клиенту. Так как не был явно указан имя файла шаблона представления для использования, ASP.NET MVC может использовать файл представления Index.aspx в папке \Views\HelloWorld. Теперь мы видим строку, которую мы жестко запрограммированы в нашем представления.

[![Индекс - Windows Internet Explorer](getting-started-with-mvc-part3/_static/image7.png)](getting-started-with-mvc-part3/_static/image6.png)

Выглядит очень хорошо. Тем не менее Обратите внимание, что заголовок браузера отображается надпись «Index» большой заголовок на странице отображается надпись «Мое приложение MVC.» Изменим их.

### <a name="changing-views-and-master-pages"></a>Изменение представления и главных страниц

Во-первых изменим текст «Мое приложение MVC.» Этот текст является общим и отображается на каждой странице. Он фактически не отображаются только в одном месте в коде, даже если он находится на каждой странице, в нашем приложении. Перейдите к папке /Views/Shared в обозревателе решений и откройте файл Site.Master. Этот файл называется главной страницы и является общей «оболочка», использовать для всех других страницах.

Обратите внимание, текст, указывающий ContentPlaceholder «Тегу» в этом файле.

[!code-aspx[Main](getting-started-with-mvc-part3/samples/sample4.aspx)]

Он располагается где все страницы, создаваемые вами будут отображаться, «оболочкой» на главной странице. Попробуйте изменить заголовок, а затем запустить приложение, а также несколько страниц. Обратите внимание на нескольких страницах отображения одного изменения.

[!code-html[Main](getting-started-with-mvc-part3/samples/sample5.html)]

Теперь каждая страница будет иметь основной заголовок - это H1 - «Фильма приложения MVC.» Которая обрабатывает белый текст вверху существует, общей для всех страниц.

Вот Site.Master целиком с нашей изменен заголовок.

[!code-aspx[Main](getting-started-with-mvc-part3/samples/sample6.aspx)]

Теперь изменим заголовка страницы индекса.

Откройте /HelloWorld/Index.aspx. Есть два места для изменения. Во-первых заголовок, которая отображается в заголовке обозревателя, а затем дополнительный заголовок -, который также является H2. Я сделаю их немного отличается, чтобы было понятно, какая часть кода изменяет какие-либо часть приложения.

[!code-aspx[Main](getting-started-with-mvc-part3/samples/sample7.aspx)]

Запуск приложения и посетите /Movies. Обратите внимание, что заголовок обозревателя, основной заголовок и дополнительные заголовки были изменены. Можно легко вносить существенные изменения в приложения с небольшими изменениями для представления.

[![Список фильмов - Windows Internet Explorer](getting-started-with-mvc-part3/_static/image9.png)](getting-started-with-mvc-part3/_static/image8.png)

Наш небольшую «данных» (в данном случае «Hello World!» сообщение) было сложно закодированных хотя. У нас есть V (представления) и у нас есть C (контроллеров), но не M (модель) еще. Мы рассмотрим вскоре как создать базу данных и получения из него данных модели.

## <a name="passing-a-viewmodel"></a>Передача ViewModel

Прежде чем мы перейдите к базе данных и говорить о модели, менее, сначала поговорим о «ViewModels.» Это объекты, представляющие Просмотр шаблона требуется для отрисовки HTML-ответе клиенту. Они обычно создаются и передает класс контроллера в представление шаблона и должно содержать только данные, требуется Просмотр шаблона - и больше не требуется.

Ранее приведенный образец HelloWorld нашей Welcome() метод действия заняло имя и параметр numTimes и вывода их браузер. Вместо контроллер продолжает отображать этот ответ непосредственно, вместо этого сделаем небольшой класс для хранения этих данных и затем передавать его шаблон представления для отрисовки HTML-ответе, используя его обратно. Таким образом, контроллер отвечает за одно и Просмотр шаблона другой — позволяющих поддерживать чистой «Разделение областей ответственности» в приложении.

Откройте файл HelloWorldController.cs и добавьте новый класс «WelcomeViewModel» и измените метод Добро пожаловать в свой контроллер. Ниже приведен полный HelloWorldController.cs новый класс в тот же файл.

[!code-csharp[Main](getting-started-with-mvc-part3/samples/sample8.cs)]

Несмотря на то, что он находится на несколько строк, нашей приветствия метод действительно является только две инструкции кода. Первая инструкция упаковывает наших двух параметров в объекте ViewModel, а второй передает полученный объект в представление.

Теперь нам требуется шаблон представления Добро пожаловать! Щелкните правой кнопкой мыши в методе приветствия и выберите команду Добавить представление. На этот раз, мы будет проверять «Создать строго типизированное представление» и выберите нашей WelcomeViewModel класс из раскрывающегося списка. Это новое представление будет известно только о WelcomeViewModels и другие типы объектов.

> *Примечание: Необходимо будет выполнена компиляция один раз, после добавления вашего WelcomeViewModel для отображения в раскрывающемся списке.*


Ниже показано, как должны выглядеть ваше диалоговое окно добавления представления. Нажмите кнопку Добавить. ![Добавить представление в кружке](getting-started-with-mvc-part3/_static/image10.png)

Добавьте следующий код под &lt;h2&gt; в ваш новый Welcome.aspx. Мы сделать цикл и поприветствуйте столько раз, сколько пользователя о том, что мы должны!

[!code-aspx[Main](getting-started-with-mvc-part3/samples/sample9.aspx)]

Кроме того, обратите внимание, что при вводе, так как мы сообщили, это представление о WelcomeViewModel (женаты или замужем, помните?), мы получаем полезные Intellisense, каждый раз, мы ссылку нашей модели объекта, представляемого на снимке экрана ниже:

[![NumTime исходного кода](getting-started-with-mvc-part3/_static/image12.png)](getting-started-with-mvc-part3/_static/image11.png)

Запустите приложение и посетите `http://localhost:xx/HelloWorld/Welcome?name=Scott&numtimes=4` еще раз. Теперь данных выполняется переход по URL-адресу, оно передается в нашем контроллер автоматически, нашей контроллера упаковывает данные ViewModel и передает этот объект на нашем представление. Представление не отображает данные в виде HTML для пользователя.

[![Вас приветствует - Windows Internet Explorer](getting-started-with-mvc-part3/_static/image14.png)](getting-started-with-mvc-part3/_static/image13.png)

Ведь это было типа «M» для модели, но не тип базы данных. Посмотрим, что мы узнали и создать базу данных фильмов.

>[!div class="step-by-step"]
[Назад](getting-started-with-mvc-part2.md)
[Вперед](getting-started-with-mvc-part4.md)
