---
uid: web-pages/overview/ui-layouts-and-themes/10-working-with-video
title: "Отображение видео в ASP.NET Web Pages (Razor) сайта | Документы Microsoft"
author: tfitzmac
description: "В этой главе описывается отображение видео в веб-страниц ASP.NET со страницей синтаксиса Razor."
ms.author: aspnetcontent
manager: wpickett
ms.date: 02/20/2014
ms.topic: article
ms.assetid: 332fb3da-e2a5-460d-bb90-dd911e1e2c95
ms.technology: dotnet-webpages
ms.prod: .net-framework
msc.legacyurl: /web-pages/overview/ui-layouts-and-themes/10-working-with-video
msc.type: authoredcontent
ms.openlocfilehash: 0e1849fb780908b55520d8108e2227d046759987
ms.sourcegitcommit: 9a9483aceb34591c97451997036a9120c3fe2baf
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 11/10/2017
---
<a name="displaying-video-in-an-aspnet-web-pages-razor-site"></a>Отображение видео в веб-страницы (Razor) узла ASP.NET
====================
по [Tom FitzMacken](https://github.com/tfitzmac)

> В этой статье описывается использование видеопроигрывателя (носителей) в веб-сайта ASP.NET Web Pages (Razor) для просмотра видео, которые хранятся на сайте. Веб-страниц ASP.NET с синтаксисом Razor позволяет воспроизводить Flash (*.swf*), Media Player (*.wmv*) и Silverlight (*.xap*) видео.
> 
> Что вы узнаете следующее.
> 
> - Как выбрать видеопроигрыватель.
> - Инструкции по добавлению видео на веб-страницу.
> - Как задать атрибуты видеопроигрыватель.
> 
> Это ASP.NET Razor pages функции, представленные в статье:
> 
> - `Video` Вспомогательные.
>   
> 
> ## <a name="software-versions-used-in-the-tutorial"></a>Версии программного обеспечения, используемая в этом учебнике
> 
> 
> - Веб-страниц ASP.NET (Razor) 2
> - WebMatrix 2
>   
> 
> Этот учебник также работает с WebMatrix 3.


## <a name="introduction"></a>Вступление

Может потребоваться отобразить видео на сайте. Один из способов сделать это — для связи с узлом, уже имеет видео, например YouTube. Если вы хотите внедрить видео из этих сайтов непосредственно в свои собственные страницы, можно обычно получить разметки HTML с сайта и скопировать его на страницу. Например приведенный ниже показано, как внедрить YouTube видео:

[!code-html[Main](10-working-with-video/samples/sample1.html?highlight=10-14)]

Если вы хотите воспроизвести видео на собственных веб-сайта (не на общедоступном сайте общий доступ к видео), нельзя связать непосредственно к ней с помощью внедренных разметки следующим образом. Тем не менее, воспроизведение видео с веб-узла с помощью `Video` вспомогательного приложения, который отображает проигрыватель непосредственно в страницу.

<a id="Choosing_a_Video_Player"></a>
## <a name="choosing-a-video-player"></a>Выбрав видеопроигрывателем

Существует множество форматов видеофайлов, и каждый формат обычно требует разных проигрывателя и другой способ настройки проигрывателя. На страницах ASP.NET Razor можно воспроизводить видео в веб-страницы с помощью `Video` поддержки. `Video` Вспомогательный упрощает процесс внедрения видео на веб-странице, так как Visual Studio автоматически создает `object` и `embed` HTML-элементов, которые обычно используются для добавления видео на страницу.

`Video` Вспомогательные поддерживает следующие универсальные проигрыватели:

- Adobe Flash
- Проигрывателя Windows Media
- Microsoft Silverlight

### <a name="the-flash-player"></a>Проигрыватель Flash Player

`Flash` Игрока `Video` вспомогательный позволяют воспроизводить видео флэш-памяти (*.swf* файлы) на веб-странице. Как минимум необходимо указать путь к файлу изображения. Если указать только путь проигрыватель использует значения по умолчанию, установленные с текущей версией Flash. Типичные значения приведены ниже.

- Видео отображается с использованием высоту и ширину по умолчанию и без цвета фона.
- Видео воспроизводится автоматически при загрузке страницы.
- Видео циклов непрерывно, пока не был явно остановлен.
- Видео масштабируется для отображения всех видео, а не обрезки видео в соответствии с указанными размерами.
- Видео воспроизводится в окне.

### <a name="the-mediaplayer-player"></a>Проигрыватель MediaPlayer

`MediaPlayer` Игрока `Video` вспомогательный позволяет воспроизводить видео Windows Media (*.wmv* файлы), Windows Media audio (*.wma* файлы) и MP3 (*.mp3* файлы) на веб-странице. Необходимо включить путь к файлу мультимедиа для воспроизведения; все остальные параметры являются необязательными. Если указать только путь проигрыватель использует параметры по умолчанию, задать с помощью текущей версии MediaPlayer, например:

- Видео отображается с использованием высоту и ширину по умолчанию.
- Видео воспроизводится автоматически при загрузке страницы.
- Оно воспроизводится один раз (он не цикла).
- Проигрыватель отображает полный набор элементов управления в пользовательском интерфейсе.
- Оно воспроизводится в окне.

### <a name="the-silverlight-player"></a>Проигрыватель Silverlight

`Silverlight` Игрока `Video` вспомогательный позволяет воспроизводить Windows Media Video (*.wmv* файлы), Windows Media Audio (*.wma* файлы) и MP3 (*.mp3* файлы). Необходимо задать параметр path для указания пакета приложения на основе Silverlight (*.xap* файла). Также необходимо задать параметры width и height. Все остальные параметры являются необязательными. При использовании проигрывателя Silverlight для видео, если устанавливается только для обязательных параметров проигрывателя Silverlight отображает видео без цвета фона.

> [!NOTE]
> В случае, если вы еще не знаете Silverlight: *.xap* файл представляет собой сжатый файл, содержащий инструкции макета в *.xaml* файла управляемого кода в сборках и дополнительные ресурсы. Можно создать *.xap* файл в Visual Studio, как проект приложения Silverlight.


`Silverlight` Видеопроигрыватель использует как параметры, указываемые для проигрывателя и параметры, которые содержатся в *.xap* файла.

> [!TIP] 
> 
> <a id="SB_MimeTypes"></a>
> ### <a name="mime-types"></a>Типы MIME
> 
> Когда браузер загружает файл, браузер обеспечивает соответствие MIME-тип, указанный для документа, который готовится к просмотру тип файлов. MIME-тип является типом или носителя тип содержимого файла. `Video` Вспомогательные используются следующие типы MIME:
> 
> - `application/x-shockwave-flash`
> - `application/x-mplayer2`
> - `application/x-silverlight-2`


<a id="Playing_Flash"></a>
## <a name="playing-flash-swf-videos"></a>Воспроизведение видео Flash (.swf)

Эта процедура показано, как для воспроизведения видео флэш-памяти с именем *sample.swf*. В процедуре предполагается, что у вас есть папка с именем *мультимедиа* на веб-узла и что *.swf* файл находится в этой папке.

1. Добавить библиотека вспомогательных методов ASP.NET Web веб-сайта, как описано в [Установка помощников в узел веб-страниц ASP.NET](https://go.microsoft.com/fwlink/?LinkId=252372), если не было добавлено.
2. Веб-сайта, добавьте страницы и назовите его *FlashVideo.cshtml*.
3. Добавьте следующую разметку страницы: 

    [!code-cshtml[Main](10-working-with-video/samples/sample2.cshtml)]
4. Запустите страницу в браузере. (Убедитесь, что выбран страницы **файлы** рабочей области, прежде чем запускать его.) Эта страница отображается, видео воспроизводится автоматически. 

    ![[image] ] (10-working-with-video/_static/image1.jpg "ch08_video 1.jpg")

Можно задать `quality` для Flash-видео, чтобы параметр `low`, `autolow`, `autohigh`, `medium`, `high`, и `best`:

[!code-cshtml[Main](10-working-with-video/samples/sample3.cshtml)]

Вы можете изменить Flash-видео для воспроизведения на определенного размера с помощью `scale` параметр, который можно задать следующее:

- `showall`. Это делает видимыми видео целиком сохранением первоначальных пропорций. Тем не менее вы может оказаться границы для каждой стороны.
- `noorder`. При этом размер видео, сохраняя исходные пропорции, но может обрезаться.
- `exactfit`. Это делает все видео видимыми не сохраняя исходные пропорции, но может происходить искажение.

Если не указать `scale` параметр, все видео будут отображаться и будет поддерживать исходные пропорции без всякой обрезки. В следующем примере показано, как использовать `scale` параметр:

[!code-cshtml[Main](10-working-with-video/samples/sample4.cshtml)]

Проигрыватель Flash player поддерживает видео режим параметра `windowMode`. Вы можете задать `window`, `opaque`, и `transparent`. По умолчанию `windowMode` равно `window`, отображающий видео в отдельном окне на веб-странице. `opaque` Параметр скрывает все за видео на веб-странице. `transparent` Параметр позволяет фона веб-страницы видны сквозь видео, при условии, что какой-либо части видео является прозрачным.

<a id="Playing_MediaPlayer"></a>
## <a name="playing-mediaplayer-wmv-videos"></a>Воспроизведение MediaPlayer (*.wmv*) видео

Ниже показано, как для воспроизведения видео окна носителя с именем *sample.wmv* , находящийся в *Media* папки.

1. Добавить библиотека вспомогательных методов ASP.NET Web веб-сайта, как описано в [Установка помощников в узел веб-страниц ASP.NET](https://go.microsoft.com/fwlink/?LinkId=252372), если это еще не сделано.
2. Создание новой страницы с именем *MediaPlayerVideo.cshtml*.
3. Добавьте следующую разметку страницы: 

    [!code-cshtml[Main](10-working-with-video/samples/sample5.cshtml)]
4. Запустите страницу в браузере. Видео загружает и воспроизводит автоматически. 

    ![[image] ] (10-working-with-video/_static/image2.jpg "ch08_video 2.jpg")

Можно задать `playCount` в целое число, указывающее, сколько раз для автоматического воспроизведения видео:

[!code-cshtml[Main](10-working-with-video/samples/sample6.cshtml)]

`uiMode` Позволяет указать, какие элементы управления отображаются в пользовательском интерфейсе. Можно задать `uiMode` для `invisible`, `none`, `mini`, или `full`. Если не указать `uiMode` параметра, будет видео отображается в окне состояния, поиска панели управления кнопок и громкости Помимо окна видео. Эти элементы управления будет отображаться также в том случае, если использовать проигрыватель для воспроизведения звукового файла. Ниже приведен пример использования `uiMode` параметр:

[!code-cshtml[Main](10-working-with-video/samples/sample7.cshtml)]

По умолчанию аудио включено при воспроизведении видео. Отключение звука аудио, задав `mute` параметр в значение true:

[!code-cshtml[Main](10-working-with-video/samples/sample8.cshtml)]

Можно задать уровень аудио, видео MediaPlayer установив `volume` параметр значение в диапазоне от 0 до 100. Значение по умолчанию — 50. Ниже приведен пример:

[!code-cshtml[Main](10-working-with-video/samples/sample9.cshtml)]

<a id="Playing_Silverlight"></a>
## <a name="playing-silverlight-videos"></a>Воспроизведение видео Silverlight

Эта процедура показано, как воспроизвести видео, содержащихся в Silverlight *.xap* страницу, которая находится в папке с именем *носителя*.

1. Добавить библиотека вспомогательных методов ASP.NET Web веб-сайта, как описано в [Установка помощников в узел веб-страниц ASP.NET](https://go.microsoft.com/fwlink/?LinkId=252372), если это еще не сделано.
2. Создание новой страницы с именем *SilverlightVideo.cshtml*.
3. Добавьте следующую разметку страницы: 

    [!code-cshtml[Main](10-working-with-video/samples/sample10.cshtml)]
4. Запустите страницу в браузере. 

    ![[image] ] (10-working-with-video/_static/image3.jpg "ch08_video 3.jpg")

<a id="Additional_Resources"></a>
## <a name="additional-resources"></a>Дополнительные ресурсы


[Общие сведения об Silverlight](https://msdn.microsoft.com/en-us/library/bb404700(VS.95).aspx)

[Атрибуты тега флэш-памяти ОБЪЕКТА и ВНЕДРЕНИЯ](http://kb2.adobe.com/cps/127/tn_12701.html)

[Windows Media Player 11 SDK PARAM теги](https://msdn.microsoft.com/en-us/library/aa392321(VS.85).aspx)
