---
uid: web-forms/overview/presenting-and-managing-data/model-binding/updating-deleting-and-creating-data
title: "Обновление, удаление и создание данных с привязкой модели и веб-форм | Документы Microsoft"
author: tfitzmac
description: "Этот учебник ряд демонстрирует основные аспекты использования привязки модели в проекте веб-форм ASP.NET. Привязка модели позволяет взаимодействия с данными дополнительные прямые-..."
ms.author: aspnetcontent
manager: wpickett
ms.date: 02/27/2014
ms.topic: article
ms.assetid: 602baa94-5a4f-46eb-a717-7a9e539c1db4
ms.technology: dotnet-webforms
ms.prod: .net-framework
msc.legacyurl: /web-forms/overview/presenting-and-managing-data/model-binding/updating-deleting-and-creating-data
msc.type: authoredcontent
ms.openlocfilehash: 18c065b44524e7738c048b5908fa50c592188064
ms.sourcegitcommit: 9a9483aceb34591c97451997036a9120c3fe2baf
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 11/10/2017
---
<a name="updating-deleting-and-creating-data-with-model-binding-and-web-forms"></a>Обновление, удаление и создание данных с привязкой модели и веб-форм
====================
по [Tom FitzMacken](https://github.com/tfitzmac)

> Этот учебник ряд демонстрирует основные аспекты использования привязки модели в проекте веб-форм ASP.NET. Привязка модели позволяет взаимодействия с данными более прост, чем работе с данными источника объектов (например, элемент управления ObjectDataSource или SqlDataSource). Эта серия начинается с вводный материал и перемещает на следующих занятиях рассматривается более сложных понятиях.
> 
> Этот учебник демонстрирует создание, обновление и удаление данных с использованием модели привязки. Задает следующие свойства:
> 
> - DeleteMethod
> - InsertMethod
> - UpdateMethod
> 
> Эти свойства получит имя метода, который обрабатывает соответствующей операции. В этом методе предоставляют логику для взаимодействия с данными.
> 
> Этот учебник построен на проект, созданный в первом [часть](retrieving-data.md) ряда.
> 
> Вы можете [загрузки](https://go.microsoft.com/fwlink/?LinkId=286116) весь проект C# или Visual Basic. Загружаемый код работает с Visual Studio 2012 или Visual Studio 2013. Она использует шаблон Visual Studio 2012, который немного отличается от шаблона Visual Studio 2013, показанными в данном руководстве.


## <a name="what-youll-build"></a>Мы создадим

В этом учебнике вы сможете:

1. Добавление шаблонов платформы динамических данных
2. Включить обновление и удаление данных через методы привязки модели
3. Применение правил проверки данных — позволяют создавать новую запись в базе данных

## <a name="add-dynamic-data-templates"></a>Добавление шаблонов платформы динамических данных

Для обеспечения оптимального взаимодействия с пользователем и свести к минимуму повторяющийся код, будет использовать шаблоны динамических данных. Можно легко интегрировать шаблонов готовые платформы динамических данных в существующий сайт путем установки пакета NuGet.

Из **управление пакетами NuGet**, установите **DynamicDataTemplatesCS**.

![шаблоны динамических данных](updating-deleting-and-creating-data/_static/image1.png)

Обратите внимание, что проект теперь содержит папку с именем **DynamicData**. В этой папке будут находиться шаблоны, которые автоматически применяются к динамическим элементам управления в вашей веб-форм.

![Папка динамических данных](updating-deleting-and-creating-data/_static/image2.png)

## <a name="enable-updating-and-deleting"></a>Включение обновления и удаления

Предоставление пользователям возможности обновления и удаления записей в базе данных является очень похожа на процедуру для извлечения данных. В **UpdateMethod** и **DeleteMethod** свойства, укажите имена методов, которые выполняют эти операции. С элементом управления GridView можно задать автоматическое создание, изменение и удаление кнопки. Следующий выделенный код показывает дополнения кода GridView.

[!code-aspx[Main](updating-deleting-and-creating-data/samples/sample1.aspx?highlight=4-5)]

В файле кода добавьте с помощью инструкции для **System.Data.Entity.Infrastructure**.

[!code-csharp[Main](updating-deleting-and-creating-data/samples/sample2.cs)]

Затем добавьте следующие обновления и удаления методов.

[!code-csharp[Main](updating-deleting-and-creating-data/samples/sample3.cs)]

**TryUpdateModel** метод применим совпадающих значений с привязкой к данным из веб-формы, элемент данных. Элемент данных извлекаются на основе значения параметра id.

## <a name="enforce-validation-requirements"></a>Применения проверки требований

Атрибуты проверки, которые были применены к свойствам класса Student FirstName, LastName и год автоматически применяются при обновлении данных. Элементы управления DynamicField добавить клиентские и серверные проверяющие элементы управления на основе атрибутов проверки. Свойства столбцов FirstName и LastName, как требуется. Имя не может превышать 20 символов и LastName не может превышать 40 знаков. Год должен быть допустимым значением для перечисления AcademicYear.

Если пользователь нарушает одно из требований, проверки, продолжение обновления невозможно. Чтобы просмотреть сообщение об ошибке, добавьте элемент управления ValidationSummary над элементом управления GridView. Для отображения ошибок проверки из привязки модели, задайте **ShowModelStateErrors** свойство **true**. 

[!code-aspx[Main](updating-deleting-and-creating-data/samples/sample4.aspx)]

Запуска веб-приложения, обновить и удалить все записи.

![Обновление данных](updating-deleting-and-creating-data/_static/image3.png)

Обратите внимание, что в режиме редактирования значение свойства год автоматически отображается в виде раскрывающегося списка. Свойство год — это значение перечисления, и шаблон динамических данных для значения перечисления указывает раскрывающегося списка для редактирования. Этот шаблон можно найти, открыв **перечисления\_Edit.ascx платформы** файла в **DynamicData**/**FieldTemplates** папки.

Обновление завершается успешно, если указать допустимые значения. Если Вы нарушили одно из требований, проверки, обновление не запускается, и сообщение об ошибке отображается над сеткой.

![Сообщение об ошибке](updating-deleting-and-creating-data/_static/image4.png)

## <a name="add-new-records"></a>Добавление новых записей

Элемент управления GridView не поддерживает **InsertMethod** свойство и поэтому не может использоваться для добавления новой записи с использованием привязки модели. Можно найти в свойство InsertMethod **FormView**, **DetailsView**, или **ListView** элементов управления. В этом учебнике используется элемент управления FormView для добавления новой записи.

Во-первых необходимо добавьте ссылку на новую страницу, созданные для добавления новой записи. Над ValidationSummary добавьте следующую команду:

[!code-aspx[Main](updating-deleting-and-creating-data/samples/sample5.aspx)]

Новая ссылка появится в верхней части содержимого для учащихся страницы.

![Новая ссылка](updating-deleting-and-creating-data/_static/image5.png)

Затем добавьте новую веб-форму с помощью главной страницы и назовите его **AddStudent**. Выберите главную страницу Site.Master.

Отрисовка полей для добавления нового студента с помощью **DynamicEntity** управления. Элемент управления DynamicControl подготавливает к просмотру, редактируемых свойств в класс, указанный в свойстве ItemType. Свойство с идентификатором StudentID был помечен атрибутом **[ScaffoldColumn(false)]** атрибута, поэтому он не отображается. Заполнитель тегу AddStudent страницы добавьте следующий код.

[!code-aspx[Main](updating-deleting-and-creating-data/samples/sample6.aspx)]

Добавьте в файл кода (AddStudent.aspx.cs), **с помощью** инструкции для **ContosoUniversityModelBinding.Models** пространства имен.

[!code-csharp[Main](updating-deleting-and-creating-data/samples/sample7.cs)]

Затем добавьте следующие методы для указания способа вставки новой записи и обработчик событий для кнопки "Отмена".

[!code-csharp[Main](updating-deleting-and-creating-data/samples/sample8.cs)]

Сохраните все изменения.

Создание нового студента и веб-приложения.

![Добавление нового студента](updating-deleting-and-creating-data/_static/image6.png)

Нажмите кнопку **вставить** и обратите внимание на то, будет создана нового студента.

![Отображение нового студента](updating-deleting-and-creating-data/_static/image7.png)

## <a name="conclusion"></a>Заключение

В этом учебнике включено обновление, удаление и создание данных. Гарантируется, что условия проверяются при взаимодействии с данными.

В следующем [учебника](sorting-paging-and-filtering-data.md) в этой серии будет включена сортировка, разбиение по страницам и фильтрация данных.

>[!div class="step-by-step"]
[Назад](retrieving-data.md)
[Вперед](sorting-paging-and-filtering-data.md)
