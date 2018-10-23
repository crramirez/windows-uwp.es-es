---
author: Xansky
Description: The Microsoft Store Services SDK provides libraries and tools that you can use to add features to your apps that help you make more money and gain customers.
title: Conectar con clientes con Microsoft Store Services SDK
ms.assetid: 518516DB-70A7-49C4-B3B6-CD8A98320B9C
ms.author: mhopkins
ms.date: 08/21/2017
ms.topic: article
ms.prod: windows
ms.technology: uwp
keywords: Windows 10, UWP, Microsoft Store Services SDK
ms.localizationpriority: medium
ms.openlocfilehash: 070e1e5bcccee27a19f3afb8a1c9d8ae356a9d73
ms.sourcegitcommit: c4d3115348c8b54fcc92aae8e18fdabc3deb301d
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 10/22/2018
ms.locfileid: "5405479"
---
# <a name="engage-customers-with-the-microsoft-store-services-sdk"></a>Conectar con clientes con Microsoft Store Services SDK

Microsoft Store Services SDK proporciona características que te ayudan a conectar con clientes en tus aplicaciones para la Plataforma universal de Windows (UWP), por ejemplo, enviar notificaciones dirigidas del Centro de desarrollo a tus aplicaciones y ejecutar pruebas A/B en tus aplicaciones. Este SDK es una extensión de Visual Studio 2015 y versiones posteriores de Visual Studio.

> [!NOTE]
> Para mostrar anuncios en tus aplicaciones para UWP, usa el [SDK de Microsoft Advertising](http://aka.ms/ads-sdk-uwp) en lugar del Microsoft Store Services SDK. Se han movido las bibliotecas de publicidad desde el Microsoft Store Services SDK al SDK de Microsoft Advertising. Para obtener más información, consulta [Mostrar anuncios en tu aplicación](display-ads-in-your-app.md).



## <a name="scenarios-supported-by-the-microsoft-store-services-sdk"></a>Escenarios admitidos por el Microsoft Store Services SDK

En este momento, Microsoft Store Services SDK es compatible con los siguientes escenarios para aplicaciones para UWP. Para obtener documentación de referencia de API, consulta [Referencia de las API de Microsoft Store Services SDK](https://docs.microsoft.com/uwp/api/overview/engagement).

|  Escenario  |  Descripción   |
|------------|----------------|
|  [Ejecutar experimentos con pruebas A/B en tu aplicación para UWP](run-app-experiments-with-a-b-testing.md)    |  Ejecuta pruebas A/B en la aplicación para la Plataforma universal de Windows (UWP) para medir la eficacia de las características en algunos clientes antes de lanzar las características para todo el mundo. Después de definir un experimento en el panel del Centro de desarrollo, usa la clase [StoreServicesExperimentVariation](https://docs.microsoft.com/uwp/api/microsoft.services.store.engagement.storeservicesexperimentvariation) para obtener las variaciones del experimento en tu aplicación, usa estos datos para modificar el comportamiento de la característica que estés probando y luego usa el método [LogForVariation](https://docs.microsoft.com/uwp/api/microsoft.services.store.engagement.storeservicescustomeventlogger.logforvariation) para enviar eventos de visualización y eventos de conversión al Centro de desarrollo. Por último, usa el panel para ver los resultados y administrar el experimento.  |
|  [Iniciar el Centro de opiniones desde la aplicación para UWP](launch-feedback-hub-from-your-app.md)    |  Usa la clase [StoreServicesFeedbackLauncher](https://docs.microsoft.com/uwp/api/microsoft.services.store.engagement.storeservicesfeedbacklauncher) en tu aplicación para UWP para dirigir a los clientes de Windows 10 al Centro de opiniones, donde pueden enviar sus problemas, sugerencias y votos a favor. A continuación, administra esta información en el [Informe de comentarios](../publish/feedback-report.md) en el panel del Centro de desarrollo. |
|  [Configurar la aplicación para UWP para recibir notificaciones de inserción del Centro de desarrollo](configure-your-app-to-receive-dev-center-notifications.md)    |  Usa la clase [StoreServicesEngagementManager](https://docs.microsoft.com/uwp/api/microsoft.services.store.engagement.storeservicesengagementmanager) en la aplicación para UWP para registrar la aplicación para la recepción notificaciones push dirigidas que envíes a tus clientes mediante el panel del Centro de desarrollo de Windows.  |
|   [Registrar eventos personalizados en la aplicación para UWP para el informe de uso del Centro de desarrollo](log-custom-events-for-dev-center.md)   |  Usa la clase [StoreServicesCustomEventLogger](https://docs.microsoft.com/uwp/api/microsoft.services.store.engagement.storeservicescustomeventlogger.log) en la aplicación para UWP para registrar eventos personalizados que estén asociados a tu aplicación en el Centro de desarrollo. A continuación, puedes revisar el total de repeticiones para los eventos personalizados en la sección **Eventos personalizados** del informe de [uso](https://msdn.microsoft.com/windows/uwp/publish/usage-report) en el panel del Centro de desarrollo.  |

<span id="prerequisites" />

## <a name="prerequisites"></a>Requisitos previos

El Microsoft Store Services SDK requiere lo siguiente:

* Visual Studio 2015 o una versión posterior.
* Visual Studio Tools para aplicaciones universales de Windows que se instalan con la versión de Visual Studio.

<span id="install" />

## <a name="install-the-sdk"></a>Instalar el SDK

Hay dos opciones para instalar el Microsoft Store Services SDK en el equipo de desarrollo:

* **Instalador MSI**&nbsp;&nbsp;Puedes instalar el SDK mediante el programa de instalación MSI disponible [aquí](http://aka.ms/store-em-sdk).
* **Paquete NuGet**&nbsp;&nbsp;Puedes instalar el SDK como un paquete NuGet.

Microsoft publica periódicamente nuevas versiones de Microsoft Store Services SDK con nuevas características y mejoras de rendimiento. Si tienes proyectos existentes que usan el SDK y quieres usar la versión más reciente, simplemente descarga e instala la versión más reciente del SDK en el equipo de desarrollo.

<span id="install-msi" />

### <a name="install-via-msi"></a>Instalación a través de MSI

Para instalar el Microsoft Store Services SDK mediante el instalador MSI:

1.  Cierra todas las instancias de Visual Studio.

2. Si anteriormente habías instalado el SDK de Microsoft Store Engagement and Monetization, el SDK de cliente de anuncios universal o la extensión Ad Mediator , desinstala ahora esos SDK. Opcionalmente, abre una ventana de **símbolo del sistema** y ejecuta estos comandos para limpiar todas las versiones de SDK anteriores que se hayan instalado con Visual Studio, pero que podrían no aparecer en la lista de programas instalados en el equipo:
    ```
    MsiExec.exe /x{5C87A4DB-31C7-465E-9356-71B485B69EC8}
    MsiExec.exe /x{6AB13C21-C3EC-46E1-8009-6FD5EBEE515B}
    MsiExec.exe /x{6AC81125-8485-463D-9352-3F35A2508C11}
    ```

3.  Descarga e instala el [Microsoft Store Services SDK](http://aka.ms/store-em-sdk). Puede tardar unos minutos en instalarse. Espera a que finalice el proceso.

4.  Reinicia Visual Studio.

5.  Si tienes un proyecto existente que haga referencia a bibliotecas de una versión anterior del Microsoft Store Services SDK, el SDK de Microsoft Advertising, el SDK de cliente de anuncios universal o el SDK de Microsoft Store Engagement and Monetization, te recomendamos abrir el proyecto en Visual Studio y limpiar y recompilar el proyecto (en el **Explorador de soluciones**, haz clic con el botón secundario en el nodo del proyecto, elige **Limpiar**y, a continuación, haz clic de nuevo en el nodo del proyecto y elige **Recompilar**).

  De lo contrario, si es la primera vez que utilizas el SDK en el proyecto, ya estás listo para [agregar la referencia de ensamblado a tu proyecto](#references).

<span id="install-nuget" />

### <a name="install-via-nuget"></a>Instalar a través de NuGet

Para instalar las bibliotecas de Microsoft Store Services SDK mediante NuGet:

1.  Cierra todas las instancias de Visual Studio.

2. Si anteriormente habías instalado el SDK de Microsoft Store Engagement and Monetization, el SDK de cliente de anuncios universal o la extensión Ad Mediator , desinstala ahora esos SDK. Opcionalmente, abre una ventana de **símbolo del sistema** y ejecuta estos comandos para limpiar todas las versiones de SDK anteriores que se hayan instalado con Visual Studio, pero que podrían no aparecer en la lista de programas instalados en el equipo:
    ```
    MsiExec.exe /x{5C87A4DB-31C7-465E-9356-71B485B69EC8}
    MsiExec.exe /x{6AB13C21-C3EC-46E1-8009-6FD5EBEE515B}
    MsiExec.exe /x{6AC81125-8485-463D-9352-3F35A2508C11}
    ```

3.  Inicia Visual Studio y abre el proyecto en el que quieres usar el Microsoft Store Services SDK.
    > [!NOTE]
    > Si tu proyecto ya incluye referencias a bibliotecas de una instalación MSI anterior del SDK, elimina esas referencias del proyecto. Esas referencias tendrán iconos de advertencia junto a ellas, porque las bibliotecas a las que hacen referencia se han eliminado en los pasos anteriores.

4. En VisualStudio, haz clic en **Proyecto** y en **Administrar paquetes de NuGet**.

5. En el cuadro de búsqueda, escribe **Microsoft.Services.Store.Engagement** e instala el paquete Microsoft.Services.Store.Engagement. Cuando el paquete se haya instalado, guarda la solución.
    > [!NOTE]
    > Si la ventana **Salida** notifica un error de *Install-Package* que indica que la ruta de acceso especificada es demasiado larga, puede que tengas que configurar NuGet para extraer los paquetes en otra ubicación con una ruta más corta que la ubicación predeterminada. Para ello, agrega el valor ```repositoryPath``` a un archivo nuget.config en el equipo y asígnalo a una ruta de carpeta corta donde se puedan extraer paquetes de NuGet. Para obtener más información, consulta [este artículo](http://docs.nuget.org/ndocs/consume-packages/configuring-nuget-behavior) en la documentación de NuGet. Como alternativa, puedes mover tu proyecto de Visual Studio a otra carpeta con una ruta más corta.

6. Cierra la solución de Visual Studio que contiene el proyecto y, a continuación, volver a abrir la solución.

7.  Si tu proyecto ya hace referencia a bibliotecas de una versión anterior del Microsoft Store Services SDK que se haya instalado a través de NuGet y has actualizado tu proyecto a una versión más reciente del SDK, te recomendamos limpiar y recompilar el proyecto (en el **Explorador de soluciones**, haz clic con el botón secundario en el nodo del proyecto, elige **Limpiar**y, a continuación, haz clic de nuevo en el nodo del proyecto y elige **Recompilar**).

  De lo contrario, si es la primera vez que utilizas el SDK en el proyecto, ya estás listo para [agregar la referencia de ensamblado a tu proyecto](#references).

<span id="references" />

## <a name="add-the-assembly-reference-to-your-project"></a>Agregar la referencia de ensamblado al proyecto

Después de instalar Microsoft Store Services SDK a través del instalador MSI o de NuGet, sigue estas instrucciones para hacer referencia al ensamblado del SDK en el proyecto UWP.

1. Abre el proyecto en Visual Studio.
    > [!NOTE]
    > Si tu proyecto es una aplicación JavaScript que tiene como destino **Cualquier CPU**, actualiza el proyecto para que use una salida de compilación específica de la arquitectura (por ejemplo, **x86**).

2. En el **Explorador de soluciones**, haz clic con el botón secundario en **Referencias** y selecciona **Agregar referencia...**

3. En el **Administrador de referencias**, expande **Windows Universal**, haz clic en **Extensiones** y luego selecciona la casilla junto a **Microsoft Engagement Framework**. Esto te permite usar las API en el espacio de nombres [Microsoft.Services.Store.Engagement](https://docs.microsoft.com/uwp/api/microsoft.services.store.engagement).

3. Haz clic en **Aceptar**.

> [!NOTE]
> Si instalaste las bibliotecas del SDK a través de NuGet, el proyecto contendrá una referencia **Microsoft.Services.Store.Engagement**. La referencia **Microsoft.Services.Store.Engagement** representa el paquete de NuGet (en lugar de las bibliotecas que contiene) y puedes omitirla.

<span id="framework" />

## <a name="understanding-framework-packages-in-the-sdk"></a>Descripción de los paquetes de marcos en el SDK

La biblioteca Microsoft.Services.Store.Engagement.dll en Microsoft Store Services SDK está configurada como un *paquete de marcos*. Esta biblioteca contiene las API en el espacio de nombres [Microsoft.Services.Store.Engagement](https://docs.microsoft.com/uwp/api/microsoft.services.store.engagement).

Dado que esta biblioteca es un paquete de marcos, después de que un usuario instale una versión de tu aplicación que use esta biblioteca, esta se actualizará automáticamente en su dispositivo a través de Windows Update cada vez que publiquemos una nueva versión de la biblioteca con correcciones y mejoras de rendimiento. Esto ayuda a garantizar que tus clientes tengan siempre instalada en sus dispositivos la versión más reciente de la biblioteca.

Si publicamos una nueva versión del SDK que incorpore nuevas API o características en esta biblioteca, tendrás que instalar la versión más reciente del SDK para usar dichas características. En este escenario, también necesitarás publicar la aplicación actualizada en la Store.

## <a name="related-topics"></a>Temas relacionados

* [Microsoft Store Services SDK API reference (Referencia de las API de Microsoft Store Services SDK)](https://docs.microsoft.com/uwp/api/overview/engagement)
* [Ejecutar experimentos con pruebas A/B](run-app-experiments-with-a-b-testing.md)
* [Iniciar el Centro de opiniones desde la aplicación](launch-feedback-hub-from-your-app.md)
* [Configurar la aplicación para recibir notificaciones de inserción del Centro de desarrollo](configure-your-app-to-receive-dev-center-notifications.md)
* [Registrar eventos personalizados para el Centro de desarrollo](log-custom-events-for-dev-center.md)
