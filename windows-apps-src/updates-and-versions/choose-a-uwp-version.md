---
author: QuinnRadich
title: Elegir una versión de UWP
description: Al escribir una aplicación para UWP en Microsoft Visual Studio, puedes elegir la versión de destino. Obtén información sobre la diferencia entre las diferentes versiones de UWP y sobre cómo configurar las opciones en proyectos nuevos y existentes.
ms.author: quradic
ms.date: 4/10/2018
ms.topic: article
ms.prod: windows
ms.technology: uwp
keywords: windows 10, uwp, versión, compilación, versiones, windows, elegir, actualizar, actualizaciones
ms.assetid: a8b7830f-4929-44c6-90be-91f38be5f364
ms.localizationpriority: medium
ms.openlocfilehash: c7951098e576047b5c82da72b7c4e9118ffb7569
ms.sourcegitcommit: 72710baeee8c898b5ab77ceb66d884eaa9db4cb8
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 09/11/2018
ms.locfileid: "3848709"
---
# <a name="choose-a-uwp-version"></a>Elegir una versión de UWP

Cada versión de Windows10 ha aportado características nuevas y mejoradas a la plataforma UWP. Al crear una aplicación para UWP en Microsoft Visual Studio, puedes elegir la versión de destino. Los proyectos que usen [.NET 2.0 estándar](https://docs.microsoft.com/dotnet/standard/net-standard) debe tener un **versión mínima** de la compilación 16299 o posterior.

> [!WARNING]
> Proyectos UWP creados en las versiones actuales de Visual Studio no se puede abrir en Visual Studio 2015.

En la tabla siguiente se describen las versiones disponibles de Windows10. Ten en cuenta que esta tabla solo se aplica a la creación de aplicaciones para UWP, que solo son compatibles con Windows10. No puedes desarrollar aplicaciones para UWP para versiones anteriores de Windows, y tienes que haber [instalado la versión adecuada del SDK](http://go.microsoft.com/fwlink/?LinkId=821431) con el fin de seleccionar esa versión como destino. 

| Versión | Descripción |
| --- | --- |
| Compilación 17134 (versión 1803) | Esta es la última versión de Windows10, publicada en abril de 2018. **Ten en cuenta que _debes_ usar Visual Studio2017 para seleccionar esta versión de Windows como destino.** Algunas características destacadas de esta versión incluyen: </br> \* **Aprendizaje automático de Windows:** el aprendizaje automático de Windows te permite crear aplicaciones que evalúen modelos de aprendizaje automático previamente entrenados de forma local en tus dispositivos con Windows 10. Para obtener más información sobre la plataforma, consulta [Aprendizaje automático de Windows](https://docs.microsoft.com/windows/ai/). </br> \* **Fluent Design:** se han agregado características nuevas a Windows 10, como la vista de árbol, la función de extraer para actualizar y la vista de navegación. Consulta las más recientes en la [introducción a Fluent design](../design/fluent-design-system/index.md). </br> \* **Aplicaciones para UWP de consola:** ahora ya puedes escribir aplicaciones de consola C++ /WinRT o /CX UWP que se ejecutan en una ventana de consola como una ventana de consola DOS o PowerShell. </br> Para obtener información sobre estas y muchas otras funciones agregadas en esta versión de Windows, visita [el Centro de desarrollo](https://developer.microsoft.com/windows/windows-10-for-developers) o la página de información detallada en [Novedades para desarrolladores en Windows 10](../whats-new/windows-10-build-17134.md).
| Compilación 16299, (Fall Creators Update, versión 1709) | Esta versión de Windows10 se publicó en octubre de 2017. **Ten en cuenta que _debes_ usar Visual Studio2017 para seleccionar esta versión de Windows como destino.** Algunas características destacadas de esta versión incluyen: </br> \* **.NET Standard 2.0:** Disfruta de un gran aumento en el número de las API de .NET e incorpora tus paquetes NuGet favoritos y bibliotecas de terceros en .NET Standard. Puedes ver más detalles y explorar la documentación [aquí](https://docs.microsoft.com/dotnet/standard/net-standard). Ten en cuenta que debes establecer tu **versión mínima** en la compilación 16299 para poder acceder a estas nuevas API. </br> \* **Fluent Design:** Usa la luz, la profundidad, la perspectiva y el movimiento para mejorar tu aplicación y que los usuarios puedan centrarse en los elementos de la interfaz de usuario más importantes. </br> \* **XAML condicional:** Establece fácilmente propiedades y crea instancias de objetos en función de la presencia de una API en tiempo de ejecución, permitiendo así a tus aplicaciones ejecutarse sin problemas entre dispositivos y versiones. </br> Para obtener información sobre estas y muchas otras funciones agregadas en esta versión de Windows, visita [el Centro de desarrollo](https://developer.microsoft.com/windows/windows-10-for-developers) o la página de información detallada en [Novedades para desarrolladores en Windows 10](../whats-new/windows-10-build-16299.md).
| Compilación 15063, (Creators Update, versión 1703) | Esta versión de Windows10 se publicó en marzo de 2017. **Ten en cuenta que _debes_ usar Visual Studio2017 para seleccionar esta versión de Windows como destino**. Algunas características destacadas de esta versión incluyen:  </br> \* **Análisis de la entrada de lápiz:** Windows Ink ahora puede clasificar los trazos de lápiz en escribir o dibujar trazos, así como reconocer texto, formas y estructuras de diseño básico. </br> \* **API de Windows.Ui.Composition:** Combina y aplica animaciones de manera sencilla en tu aplicación. </br> \* **Edición dinámica:** Edita en XAML mientras se ejecuta la aplicación y ve cómo los cambios se aplican en tiempo real. </br> Para obtener información sobre estas y muchas otras funciones agregadas en esta versión de Windows, visita [el Centro de desarrollo](https://developer.microsoft.com/windows/windows-10-for-developers) o la página de información detallada en [Novedades para desarrolladores en Windows 10](../whats-new/windows-10-build-15063.md).  |
| Compilación 14393 (Actualización de aniversario, versión 1607) | Esta versión de Windows10 se publicó en julio de 2016. Algunas características destacadas de esta versión incluyen: </br> \* **Windows Ink:** Nuevos controles InkCanvas e InkToolbar. </br> \* **API de Cortana:** Usa nuevas acciones de Cortana para integrar la compatibilidad de Cortana con funciones específicas de tu aplicación. </br> \* **Windows Hello:** Microsoft Edge ahora admite Windows Hello, lo que proporciona a los desarrolladores web acceso a la autenticación biométrica. </br> Para obtener información sobre estas y muchas otras funciones agregadas en esta versión de Windows, visita [el Centro de desarrollo](https://developer.microsoft.com/windows/windows-10-for-developers) o la página de información detallada en [Novedades para desarrolladores en Windows 10](../whats-new/windows-10-build-14393.md).  |
| Compilación 10586 (actualización de noviembre, versión 1511) | Esta versión de Windows10 se publicó en noviembre de 2015. Las características destacadas incluyen la introducción de las API ORTC (Comunicaciones en tiempo real mediante objetos) para la comunicación de vídeo en Microsoft Edge y las API de proveedores para permitir a las aplicaciones usar la autenticación de rostro de Windows Hello. [Más información sobre las funciones introducidas en esta compilación.](../whats-new/windows-10-build-10586.md) |
| Compilación 10240 (Windows 10, versión 1507) | Esta es la versión inicial de Windows 10, publicada en julio de 2015. [Más información sobre las características introducidas en esta compilación.](../whats-new/windows-10-build-10240.md) |

Es muy recomendable que los nuevos desarrolladores y los desarrolladores que escriban código para un público general siempre usen la compilación más reciente de Windows (17134). Los desarrolladores que escriban aplicaciones de empresa deberían pensar seriamente en ofrecer compatibilidad para una **versión mínima** más antigua.

## <a name="whats-different-in-each-uwp-version"></a>¿Qué es diferente en cada versión de UWP?

En cada versión sucesiva de Windows10 están disponibles API nuevas y modificadas para UWP. Para obtener información específica sobre qué funciones se han agregado en qué versión, consulta [Novedades para desarrolladores de Windows10](../whats-new/windows-10-version-latest.md).

Para ver los temas de consulta que enumeran todas las familias de dispositivos y sus versiones, así como todos los contratos de API y sus versiones, consulta [Familias de dispositivos](https://msdn.microsoft.com/library/windows/apps/dn706137.aspx) y [Contratos de API](https://msdn.microsoft.com/library/windows/apps/dn706135.aspx).

## <a name="net-api-availability-in-uwp-versions"></a>Disponibilidad de la API de .NET en las versiones UWP

UWP admite un subconjunto limitado de las API de .NET que están disponibles, independientemente de la **Versión de destino** o la **Versión mínima** del proyecto. [Esta página proporciona más información sobre los tipos disponibles](https://msdn.microsoft.com/library/windows/apps/xaml/mt185501(d=robot).aspx).

Si quieres crear bibliotecas multiplataforma reutilizables, .NET Standard es compatible con UWP. La [documentación de .NET Standard](https://docs.microsoft.com/dotnet/standard/net-standard) ofrece información en el que .NET Standard es compatible con las versiones UWP.

Si vas a desarrollar una aplicación de escritorio, consulte en su lugar, [las dependencias y las versiones de .NET Framework](https://docs.microsoft.com/dotnet/framework/migration-guide/versions-and-dependencies) para obtener información detallada sobre la disponibilidad de .NET framework.

## <a name="choose-which-version-to-use-for-your-app"></a>Elegir la versión que usarás para la aplicación

En el diálogo **Nuevo proyecto de Windows universal** de Visual Studio, puedes elegir una versión para la **Versión de destino** y otra para la **Versión mínima**. Además, puedes cambiar la **Versión de destino** y la **Versión mínima** de tu aplicación para UWP en la sección *aplicación* de las **Propiedades** de la aplicación.

* **Versión de destino**. Esto establece el ajuste *TargetPlatformVersion* en el archivo del proyecto. También determina el valor del atributo *TargetDeviceFamily@MaxVersionTested* en el manifiesto del paquete de la aplicación. El valor que elijas especificará la versión de la plataforma UWP a la que está destinada tu proyecto (y, por lo tanto, el conjunto de API disponibles para tu aplicación), por lo que recomendamos que elijas la versión más reciente que sea posible. Para obtener más información sobre el manifiesto del paquete de la aplicación y algunas directrices sobre cómo configurar TargetDeviceFamily manualmente, consulta [TargetDeviceFamily](https://msdn.microsoft.com/library/windows/apps/dn986903).
* **Versión mínima**. Esto establece el ajuste *TargetPlatformMinVersion* en el archivo del proyecto. También determina el valor del atributo *TargetDeviceFamily@MinVersion* en el manifiesto del paquete de la aplicación. El valor que elijas especificará la versión mínima de la plataforma UWP con la que puede funcionar tu proyecto.

Ten en cuenta que vas a declarar que tu aplicación funciona en cualquier versión de Windows en el rango desde la **Versión mínima** a la **Versión de destino**. Si las dos son la misma versión, no necesitas hacer nada especial. Si son diferentes, estas son algunas cosas que debes tener en cuenta.

* En el código, puedes llamar libremente (es decir, sin comprobaciones condicionales) a las API que existen en la versión especificada por la **Versión mínima**.
* Asegúrate de probar tu código en un dispositivo en el que se ejecute la **Versión mínima**, para asegurarte de que funciona sin necesidad de API que solo están presentes en la **Versión de destino**.
* El valor de **Versión de destino** se usa para identificar todas las referencias (winmds del contrato) para compilar el proyecto. Pero estas referencias te permitirán compilar el código con llamadas a API que no tienen por qué existir en los dispositivos que hayas declarado que admites (a través de **Versión mínima**). Por lo tanto, cualquier API que se haya introducido después de la **Versión mínima** necesitará que se la llame a través de código adaptativo. Para obtener más información sobre el código adaptativo, consulta [Código adaptativo para versiones](https://docs.microsoft.com/windows/uwp/debug-test-perf/version-adaptive-code).