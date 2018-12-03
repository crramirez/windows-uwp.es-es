---
ms.assetid: 066711E0-D5C4-467E-8683-3CC64EDBCC83
title: Llamar a API asincrónicas en C# o Visual Basic
description: La Plataforma universal de Windows (UWP) incluye muchas API asincrónicas para que tu aplicación tenga capacidad de respuesta mientras realiza trabajos que pudieran llevar algún tiempo.
ms.date: 02/08/2017
ms.topic: article
keywords: Windows 10, UWP, C#, Visual Basic, asincrónica
ms.localizationpriority: medium
ms.openlocfilehash: 899af2ffd26419d4c8906d703d6708d202f8c150
ms.sourcegitcommit: b4c502d69a13340f6e3c887aa3c26ef2aeee9cee
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 12/03/2018
ms.locfileid: "8472129"
---
# <a name="call-asynchronous-apis-in-c-or-visual-basic"></a>Llamar a API asincrónicas en C# o Visual Basic



La Plataforma universal de Windows (UWP) incluye muchas API asincrónicas para que tu aplicación tenga capacidad de respuesta mientras realiza trabajos que pudieran llevar algún tiempo. En este tema se describe cómo usar métodos asincrónicos desde la UWP en C# o Microsoft Visual Basic.

Las API asincrónicas evitan que la aplicación tenga que esperar a que finalicen las grandes operaciones antes de continuar con la ejecución. Por ejemplo, una aplicación que descarga información de Internet probablemente destine varios segundos a esperar que la información llegue. Si usas un método sincrónico para recuperar la información, la aplicación se bloquea hasta que el método vuelve. La aplicación no responderá a la interacción del usuario, y como parece que tarda en responder, es posible que esto provoque cierta frustración en el usuario. Gracias a las API asincrónicas, UWP ayuda a garantizar que tu aplicación continúe respondiendo al usuario mientras realiza operaciones largas.

La mayoría de las API asincrónicas en UWP no tiene su contrapartida sincrónica, por lo que debes asegurarte de comprender cómo se usan las API asincrónicas con C# o Visual Basic en tu aplicación de Plataforma universal de Windows (UWP). Aquí te mostraremos como llamar a las API asincrónicas de UWP.

## <a name="using-asynchronous-apis"></a>Usar API asincrónicas


Convencionalmente, a los métodos asincrónicos se les asignan nombres terminados en "Async". Normalmente llamas a las API asincrónicas en respuesta a una acción del usuario, como cuando haces clic en un botón. Llamar un método asincrónico en un controlador de eventos es una de las formas más simples de usar API asincrónicas. Aquí usamos el operador **await** a modo de ejemplo.

Supongamos que tienes una aplicación que enumera títulos de entradas de blog desde cierta ubicación. La aplicación tiene un botón [**Button**](https://msdn.microsoft.com/library/windows/apps/BR209265) en el que el usuario hace clic para obtener los títulos. Los títulos se muestran en un [**TextBlock**](https://msdn.microsoft.com/library/windows/apps/BR209652). Cuando el usuario hace clic en el botón, es importante que la aplicación continúe respondiendo mientras espera la información del sitio web del blog. Para garantizar esta capacidad de respuesta, UWP ofrece un método asincrónico, [**SyndicationClient.RetrieveFeedAsync**](https://msdn.microsoft.com/library/windows/apps/BR243460), para descargar la fuente.

En este ejemplo se obtienen las listas de entradas de un blog llamando al método asincrónico, [**SyndicationClient.RetrieveFeedAsync**](https://msdn.microsoft.com/library/windows/apps/BR243460), y esperando el resultado.

> [!div class="tabbedCodeSnippets" data-resources="OutlookServices.Calendar"]
[!code-csharp[Main](./AsyncSnippets/csharp/MainPage.xaml.cs#SnippetDownloadRSS)]
[!code-vb[Main](./AsyncSnippets/vbnet/MainPage.xaml.vb#SnippetDownloadRSS)]

Hay un par de aspectos importantes acerca de este ejemplo. En primer lugar, la línea `SyndicationFeed feed = await client.RetrieveFeedAsync(feedUri)` usa el operador **await** con la llamada al método asincrónico [**RetrieveFeedAsync**](https://msdn.microsoft.com/library/windows/apps/BR243460). Puedes considerar que el operador **await** indica al compilador que estás llamando a un método asincrónico, que hace que el compilador haga cierto trabajo extra para que no tengas que hacerlo. A continuación, la declaración del controlador de eventos incluye la palabra clave **async**. Debes incluir esta palabra clave en la declaración de cualquier método en el que uses el operador **await**.

En este tema no entraremos en muchos detalles sobre lo que hace el compilador con el operador **await**, pero examinemos qué hace la aplicación para ser asincrónica y tener capacidad de respuesta. Ten en cuenta lo que sucede cuando usas un código sincrónico. Por ejemplo, supongamos que existe un método llamado `SyndicationClient.RetrieveFeed` que es sincrónico. (No hay ningún método de este tipo, pero imagina que lo hay). Si tu aplicación incluyó la línea `SyndicationFeed feed = client.RetrieveFeed(feedUri)`, en lugar de `SyndicationFeed feed = await client.RetrieveFeedAsync(feedUri)`, la ejecución de la aplicación se detendrá hasta que el valor devuelto de `RetrieveFeed` esté disponible. Mientras la aplicación espera a que el método se complete, no puede responder a ningún otro evento como, por ejemplo, otro evento [**Click**](https://msdn.microsoft.com/library/windows/apps/BR227737). Es decir, tu aplicación estaría bloqueada hasta que se devuelva `RetrieveFeed`.

Pero si llamas a `client.RetrieveFeedAsync`, el método inicia la recuperación y vuelve de inmediato. Si usas **await** con [**RetrieveFeedAsync**](https://msdn.microsoft.com/library/windows/apps/BR243460), la aplicación sale temporalmente del controlador de eventos. Después puede procesar otros eventos mientras **RetrieveFeedAsync** se ejecuta asincrónicamente. Esto hace que la aplicación responda al usuario. Cuando **RetrieveFeedAsync** se completa y [**SyndicationFeed**](https://msdn.microsoft.com/library/windows/apps/BR243485) se encuentra disponible, la aplicación vuelve a entrar en el controlador de eventos de donde salió, después de `SyndicationFeed feed = await client.RetrieveFeedAsync(feedUri)`, y finaliza el resto del método.

Lo bueno de usar el operador **await** es que el código no es muy distinto del código que se utiliza en el método `RetrieveFeed` imaginario. El código asincrónico de C# o Visual Basic se puede escribir de distintas maneras sin el operador **await**, pero el código resultante tiende a enfatizar la mecánica de ejecución asincrónica. Esto hace que el código asincrónico sea difícil escribir, comprender y mantener. Con el operador **await**, obtienes las ventajas de una aplicación asincrónica sin que el código sea complejo.

## <a name="return-types-and-results-of-asynchronous-apis"></a>Tipo y resultados devueltos de las API asincrónicas


Si seguiste el vínculo a [**RetrieveFeedAsync**](https://msdn.microsoft.com/library/windows/apps/BR243460), posiblemente hayas notado que el tipo devuelto de **RetrieveFeedAsync** no es [**SyndicationFeed**](https://msdn.microsoft.com/library/windows/apps/BR243485). En cambio, el tipo devuelto es `IAsyncOperationWithProgress<SyndicationFeed, RetrievalProgress>`. Visto desde la sintaxis sin formato, las API asincrónicas devuelven un objeto que contiene el resultado. Si bien es frecuente, y a veces útil, considerar que se puede esperar al método asincrónico, el operador **await** realmente opera sobre el valor devuelto del método y no sobre el método. Al aplicar el operador **await**, lo que obtienes es el resultado de llamar a **GetResult** en el objeto devuelto por el método. En el ejemplo, el **SyndicationFeed** es el resultado de **RetrieveFeedAsync.GetResult()**.

Al usar un método asincrónico, puedes examinar la firma para ver qué obtendrás después de esperar el valor devuelto del método. Todas las API asincrónicas en la UWP devuelven uno de los siguientes tipos:

-   [**IAsyncOperation&lt;TResult&gt;**](https://msdn.microsoft.com/library/windows/apps/BR206598)
-   [**IAsyncOperationWithProgress&lt;TResult, TProgress&gt;**](https://msdn.microsoft.com/library/windows/apps/BR206594)
-   [**IAsyncAction**](https://msdn.microsoft.com/library/windows/apps/windows.foundation.iasyncaction.aspx)
-   [**IAsyncActionWithProgress&lt;TProgress&gt;**](https://msdn.microsoft.com/library/windows/apps/br206581.aspx)

El tipo de resultado de un método asincrónico es el mismo que el parámetro del tipo `      TResult`. Los tipos sin `TResult` no tienen un resultado. Puedes considerar que el resultado es **void**. En Visual Basic, un procedimiento [Sub](https://msdn.microsoft.com/library/windows/apps/xaml/831f9wka.aspx) equivale a un método con un tipo devuelto **void**.

En esta tabla se proporcionan ejemplos de métodos asincrónicos y se indican el tipo devuelto y el tipo de resultado de cada uno.

| Método asincrónico                                                                           | Tipo devuelto                                                                                                                                         | Tipo de resultado                                       |
|-----------------------------------------------------------------------------------------------|----------------------------------------------------------------------------------------------------------------------------------------------------|---------------------------------------------------|
| [**SyndicationClient.RetrieveFeedAsync**](https://msdn.microsoft.com/library/windows/apps/BR243460)     | [**IAsyncOperationWithProgress&lt;SyndicationFeed, RetrievalProgress&gt;**](https://msdn.microsoft.com/library/windows/apps/BR206594)                                 | [**SyndicationFeed**](https://msdn.microsoft.com/library/windows/apps/BR243485) |
| [**FileOpenPicker.PickSingleFileAsync**](https://msdn.microsoft.com/library/windows/apps/JJ635275) | [**IAsyncOperation&lt;StorageFile&gt;**](https://msdn.microsoft.com/library/windows/apps/BR206598)                                                                                | [**StorageFile**](https://msdn.microsoft.com/library/windows/apps/BR227171)          |
| [**XmlDocument.SaveToFileAsync**](https://msdn.microsoft.com/library/windows/apps/BR206284)                 | [**IAsyncAction**](https://msdn.microsoft.com/library/windows/apps/windows.foundation.iasyncaction.aspx)                                                                                                           | **void**                                          |
| [**InkStrokeContainer.LoadAsync**](https://msdn.microsoft.com/library/windows/apps/Hh701757)               | [**IAsyncActionWithProgress&lt;UInt64&gt;**](https://msdn.microsoft.com/library/windows/apps/br206581.aspx)                                                                   | **void**                                          |
| [**DataReader.LoadAsync**](https://msdn.microsoft.com/library/windows/apps/BR208135)                            | [**DataReaderLoadOperation**](https://msdn.microsoft.com/library/windows/apps/BR208120), una clase de resultados personalizada que implementa **IAsyncOperation&lt;UInt32&gt;** | [**UInt32**](https://msdn.microsoft.com/library/windows/apps/br206598.aspx)                     |

 

Los métodos asincrónicos que se definen en [**.NET para aplicaciones para UWP**](https://msdn.microsoft.com/library/windows/apps/xaml/br230232.aspx) tienen un tipo devuelto [**Task**](https://msdn.microsoft.com/library/windows/apps/xaml/system.threading.tasks.task.aspx) o [**Task&lt;TResult&gt;**](https://msdn.microsoft.com/library/windows/apps/xaml/dd321424.aspx). Los métodos que devuelven **Task** son similares a los métodos asincrónicos de UWP que devuelven [**IAsyncAction**](https://msdn.microsoft.com/library/windows/apps/windows.foundation.iasyncaction.aspx). En cada caso, el resultado del método asincrónico es **void**. El tipo devuelto **Task&lt;TResult&gt;** es similar a [**IAsyncOperation&lt;TResult&gt;**](https://msdn.microsoft.com/library/windows/apps/BR206598) en que el resultado del método asincrónico al ejecutar la tarea es el mismo tipo que el parámetro de tipo `TResult`. Para obtener más información sobre el uso de **.NET para aplicaciones para UWP** y las tareas, consulta [Introducción a .NET para aplicaciones de Windows en tiempo de ejecución](https://msdn.microsoft.com/library/windows/apps/xaml/br230302.aspx).

## <a name="handling-errors"></a>Control de errores


Cuando uses el operador **await** para recuperar tus resultados de un método asincrónico, puedes usar un bloque **try/catch** para controlar los errores que se producen en los métodos asincrónicos, tal y como lo harías para los métodos sincrónicos. El ejemplo anterior encapsula el método **RetrieveFeedAsync** y la operación **await** en un bloque **try/catch** para controlar errores cuando se inicia una excepción.

Cuando los métodos asincrónicos llaman a otros métodos asincrónicos, cualquier método asincrónico que dé origen a una excepción se propagará a los métodos externos. Esto significa que puedes poner un bloque **try/catch** en el método más externo para capturar los errores de los métodos asincrónicos anidados. Una vez más, esto es similar al modo en que capturas excepciones para los métodos sincrónicos. Sin embargo, no puedes usar **await** en el bloque **catch**.

**Sugerencia**partir de C# en Microsoft Visual Studio2005, puedes usar **await** en el bloque **catch** .

## <a name="summary-and-next-steps"></a>Resumen y pasos siguientes

El patrón de llamada a un método asincrónico que mostramos aquí es el más sencillo de usar cuando se llama a API asincrónicas en un controlador de eventos. También puedes usar este patrón cuando llames a un método asincrónico en un método anulado que devuelve **void** o un **Sub** en Visual Basic.

A medida que encuentras métodos asincrónicos en UWP, conviene recordar:

-   Convencionalmente, a los métodos asincrónicos se les asignan nombres terminados en "Async".
-   Todo método que use el operador **await** debe tener la declaración marcada con la palabra clave **async**.
-   Cuando la aplicación encuentra el operador **await**, continúa respondiendo a la interacción del usuario mientras se ejecuta el método asincrónico.
-   Esperar el valor devuelto por un método asincrónico devuelve un objeto que contiene el resultado. En la mayoría de los casos, lo útil es el resultado que se encuentra en el valor devuelto, no el valor devuelto en sí mismo. Puedes encontrar el tipo de valor que se encuentra en el resultado examinando el tipo devuelto del método asincrónico.
-   El uso de API asincrónicas y de patrones **async** suele ser una forma de mejorar la capacidad de respuesta de la aplicación.

El ejemplo de este tema produce texto con la siguiente apariencia.

``` syntax
Windows Experience Blog
PC Snapshot: Sony VAIO Y, 8/9/2011 10:26:56 AM -07:00
Tech Tuesday Live Twitter #Chat: Too Much Tech #win7tech, 8/8/2011 12:48:26 PM -07:00
Windows 7 themes: what’s new and what’s popular!, 8/4/2011 11:56:28 AM -07:00
PC Snapshot: Toshiba Satellite A665 3D, 8/2/2011 8:59:15 AM -07:00
Time for new school supplies? Find back-to-school deals on Windows 7 PCs and Office 2010, 8/1/2011 2:14:40 PM -07:00
Best PCs for blogging (or working) on the go, 8/1/2011 10:08:14 AM -07:00
Tech Tuesday – Blogging Tips and Tricks–#win7tech, 8/1/2011 9:35:54 AM -07:00
PC Snapshot: Lenovo IdeaPad U460, 7/29/2011 9:23:05 AM -07:00
GIVEAWAY: Survive BlogHer with a Sony VAIO SA and a Samsung Focus, 7/28/2011 7:27:14 AM -07:00
3 Ways to Stay Cool This Summer, 7/26/2011 4:58:23 PM -07:00
Getting RAW support in Photo Gallery & Windows 7 (…and a contest!), 7/26/2011 10:40:51 AM -07:00
Tech Tuesdays Live Twitter Chats: Photography Tips, Tricks and Essentials, 7/25/2011 12:33:06 PM -07:00
3 Tips to Go Green With Your PC, 7/22/2011 9:19:43 AM -07:00
How to: Buy a Green PC, 7/22/2011 9:13:22 AM -07:00
Windows 7 themes: the distinctive artwork of Cheng Ling, 7/20/2011 9:53:07 AM -07:00
```
