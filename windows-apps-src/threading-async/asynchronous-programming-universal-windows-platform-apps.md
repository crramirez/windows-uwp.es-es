---
ms.assetid: 23FE28F1-89C5-4A17-A732-A722648F9C5E
title: Programación asincrónica
description: En este tema se describe la programación asincrónica en el Plataforma universal de Windows (UWP) y su representación en C#, Microsoft Visual Basic .NET, C++ y JavaScript.
ms.date: 05/14/2018
ms.topic: article
keywords: Windows 10, UWP, asincrónico
ms.localizationpriority: medium
ms.openlocfilehash: 8e7cdffd484c426faa9b877240f45f122ccc5ec4
ms.sourcegitcommit: 7b2febddb3e8a17c9ab158abcdd2a59ce126661c
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 08/31/2020
ms.locfileid: "89161759"
---
# <a name="asynchronous-programming"></a>Programación asincrónica
En este tema se describe la programación asincrónica en el Plataforma universal de Windows (UWP) y su representación en C#, Microsoft Visual Basic .NET, C++ y JavaScript.

Con la programación asincrónica puedes conseguir que tu aplicación responda con rapidez cuando realice tareas que pueden durar un tiempo prolongado. Por ejemplo, puede que una aplicación que descarga contenido de Internet deba esperar varios segundos a que llegue el contenido. Si usaste un método sincrónico en el subproceso de IU para recuperar el contenido, la aplicación se bloquea hasta que el método regresa. La aplicación no responderá a la interacción del usuario, y como parece que tarda en responder, es posible que esto provoque cierta frustración en el usuario. Un método mucho mejor es usar la programación asincrónica, donde la aplicación continúa ejecutándose y respondiendo a la interfaz de usuario mientras espera que una operación se complete.

Para aquellos métodos que probablemente demanden mucho tiempo en completarse, debe usarse la programación asincrónica y no la excepción en Windows en UWP. JavaScript, C#, Visual Basic y C++ proporcionan compatibilidad con lenguajes para los métodos asincrónicos.

## <a name="asynchronous-programming-in-the-uwp"></a>Programación asincrónica en UWP
Muchas características de UWP, como las API de [**MediaCapture**](/uwp/api/Windows.Media.Capture.MediaCapture) y las API de [**StorageFile**](/uwp/api/Windows.Storage.StorageFile) , se exponen como API asincrónicas. Por Convención, los nombres de las API asincrónicas terminan en "Async" para indicar que parte de su ejecución es probable que se produzca después de que el control se devuelva al autor de la llamada.

Cuando uses API asincrónicas en tu aplicación de Plataforma universal de Windows (UWP), el código realizará llamadas de desbloqueo de manera coherente. Cuando implementes estos modelos asincrónicos en tus propias definiciones de API, los autores de las llamadas podrán comprender y usar tu código de manera predecible.

Estas son algunas tareas comunes que requieren la llamada a las API de Windows Runtime asincrónicas.

-   Mostrar un cuadro de diálogo de mensaje

-   Trabajar con el sistema de archivos, mostrando un selector de archivos

-   Enviar y recibir datos en Internet

-   Uso de sockets, secuencias y conectividad

-   Trabajar con citas, contactos y calendario

-   Trabajar con tipos de archivo, como abrir archivos PDF (Portable Document Format) o decodificar formatos de imagen y multimedia

-   Interactuar con un dispositivo o un servicio

Con el patrón asincrónico de UWP, es posible que puedas evitar explícitamente administrar cualquier subproceso. Todos los lenguajes de programación son compatibles con el patrón asincrónico para UWP a su manera:

| Lenguaje de programación | Representación asincrónica           |
|----------------------|---------------------------------------|
| C#                   | palabra clave **async**, operador **await** |
| Visual Basic         | **Async** (palabra clave), **Await** (operador) |
| C++/WinRT            | corrutina y **co_await** (operador)  |
| C++/CX               | clase **task**, método **.then**      |
| JavaScript           | 
objeto de promesa, función **then**     |

## <a name="asynchronous-patterns-in-uwp-using-c-and-visual-basic"></a>Modelos asincrónicos en UWP con C# y Visual Basic
Un segmento típico de código escrito en C# o Visual Basic se ejecuta sincrónicamente, lo que significa que cuando se ejecuta una línea, finaliza antes de que se ejecute la siguiente. Ha habido modelos de programación de Microsoft .NET anteriores para la ejecución asincrónica, pero el código resultante tiende a enfatizar la mecánica de la ejecución del código asincrónico en lugar de centrarse en la tarea que el código está tratando de realizar. UWP, .NET Framework y los compiladores de C# y Visual Basic han agregado características que extraen los mecanismos asincrónicos del código. Puedes escribir código asincrónico para .NET y UWP que se centre en qué es lo que hace el código, en lugar de en cómo y cuándo hacerlo. El código asincrónico tendrá un aspecto muy similar al código sincrónico. Para obtener más información, consulta [Llamar a API asincrónicas en C# o Visual Basic](call-asynchronous-apis-in-csharp-or-visual-basic.md).

## <a name="asynchronous-patterns-in-uwp-with-cwinrt"></a>Modelos asincrónicos en UWP con C++/WinRT
Con C++/WinRT, se usan corrutinas y el operador **co_await** . Para obtener más información y ejemplos de código, vea [programación asincrónica en C++/WinRT](../cpp-and-winrt-apis/concurrency.md).

## <a name="asynchronous-patterns-in-uwp-with-ccx"></a>Patrones asincrónicos en UWP con C++/CX
En C++ / CX, la programación asincrónica se basa en la [**clase task**](/cpp/parallel/concrt/reference/task-class) y su [**método then**](/cpp/parallel/concrt/reference/task-class?view=vs-2017). La sintaxis es similar a la de las promesas de JavaScript. La **clase task** y sus tipos relacionados también proporcionan la funcionalidad para cancelación y administración del contexto del subproceso. Para obtener más información, vea [programación asincrónica en C++/CX](asynchronous-programming-in-cpp-universal-windows-platform-apps.md).

La [** \_ función Create Async**](/cpp/parallel/concrt/reference/concurrency-namespace-functions?view=vs-2017) proporciona compatibilidad para generar API asincrónicas que se pueden usar desde JavaScript o cualquier otro lenguaje que admita UWP. Para obtener más información, consulte [crear operaciones asincrónicas en C++/CX](/cpp/parallel/concrt/creating-asynchronous-operations-in-cpp-for-windows-store-apps).

## <a name="asynchronous-patterns-in-uwp-using-javascript"></a>Modelos asincrónicos en UWP con JavaScript
En JavaScript, la programación asincrónica sigue el estándar propuesto de [Common JS Promises/A](https://wiki.commonjs.org/wiki/Promises/A) haciendo que los métodos asincrónicos devuelvan objetos de promesa. Las promesas se usan tanto en UWP como en la biblioteca de Windows para JavaScript.

Un objeto de promesa representa un valor que se entregará en el futuro. En UWP, obtienes un objeto de promesa de una función de fábrica que, convencionalmente, tiene un nombre que termina en "Async".

En muchos casos, llamar a una función asincrónica es casi tan sencillo como llamar a una función convencional. La diferencia es que se usa el método [**then**](/previous-versions/windows/apps/br229728(v=win.10)) o [**done**](/previous-versions/windows/apps/hh701079(v=win.10)) para asignar los controladores para los resultados o los errores y para iniciar la operación.

## <a name="related-topics"></a>Temas relacionados
* [Llamar a API asincrónicas en C# o Visual Basic](call-asynchronous-apis-in-csharp-or-visual-basic.md)
* [Programación asincrónica con Async y Await (C# y Visual Basic)](/previous-versions/visualstudio/visual-studio-2012/hh191443(v=vs.110))
* [Escenarios de la característica de muestra Reversi: código asincrónico](/previous-versions/windows/apps/jj712233(v=win.10))