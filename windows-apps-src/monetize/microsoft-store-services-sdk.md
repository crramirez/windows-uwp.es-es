---
author: mcleanbyron
Description: "Microsoft Store Services SDK proporciona bibliotecas y herramientas que puedes usar para agregar características a tus aplicaciones y que te ayudan a obtener más dinero y a ganar clientes."
title: Microsoft Store Services SDK
ms.assetid: 518516DB-70A7-49C4-B3B6-CD8A98320B9C
ms.author: mcleans
ms.date: 02/08/2017
ms.topic: article
ms.prod: windows
ms.technology: uwp
keywords: Windows 10, UWP, Microsoft Store Services SDK
ms.openlocfilehash: 7fa775c319e1d84f8b73e42723d9fb36fdd03b73
ms.sourcegitcommit: d053f28b127e39bf2aee616aa52bb5612194dc53
translationtype: HT
---
# <a name="microsoft-store-services-sdk"></a>Microsoft Store Services SDK

Microsoft Store Services SDK proporciona características que te ayudan a obtener más dinero y a conectar con clientes en tus aplicaciones para Plataforma universal de Windows (UWP), por ejemplo, mostrando anuncios y ejecutando pruebas A/B en tus aplicaciones. Este SDK es una extensión de Visual Studio 2015 y versiones posteriores de Visual Studio.

## <a name="scenarios-supported-by-the-sdk"></a>Escenarios compatibles con el SDK

En este momento, el SDK es compatible con los siguientes escenarios para aplicaciones para UWP. El SDK evolucionará con el tiempo para admitir nuevos escenarios de participación y monetización. Para obtener documentación de referencia sobre las API incluidas en el SDK, consulta [Referencia de las API de Microsoft Store Services SDK](https://msdn.microsoft.com/library/windows/apps/mt691886.aspx).

|  Escenario  |  Descripción   |
|------------|----------------|
|  [Ejecutar experimentos con pruebas A/B en tu aplicación para UWP](run-app-experiments-with-a-b-testing.md)    |  Ejecuta pruebas A/B en la aplicación para la Plataforma universal de Windows (UWP) para medir la eficacia de las características en algunos clientes antes de lanzar las características para todo el mundo. Después de definir un experimento en el panel del Centro de desarrollo, usa la clase [StoreServicesExperimentVariation](https://msdn.microsoft.com/library/windows/apps/microsoft.services.store.engagement.storeservicesexperimentvariation.aspx) para obtener las variaciones del experimento en tu aplicación, usa estos datos para modificar el comportamiento de la característica que estés probando y luego usa el método [LogForVariation](https://msdn.microsoft.com/library/windows/apps/microsoft.services.store.engagement.storeservicescustomeventlogger.logforvariation.aspx) para enviar eventos de visualización y eventos de conversión al Centro de desarrollo. Por último, usa el panel para ver los resultados y administrar el experimento.  |
|  [Iniciar el Centro de opiniones desde la aplicación para UWP](launch-feedback-hub-from-your-app.md)    |  Usa la clase [StoreServicesFeedbackLauncher](https://msdn.microsoft.com/library/windows/apps/microsoft.services.store.engagement.storeservicesfeedbacklauncher.aspx) en tu aplicación para UWP para dirigir a los clientes de Windows 10 al Centro de opiniones, donde pueden enviar sus problemas, sugerencias y votos a favor. A continuación, administra esta información en el [Informe de comentarios](../publish/feedback-report.md) en el panel del Centro de desarrollo. |
|  [Configurar la aplicación para UWP para recibir notificaciones de inserción del Centro de desarrollo](configure-your-app-to-receive-dev-center-notifications.md)    |  Usa la clase [StoreServicesEngagementManager](https://msdn.microsoft.com/library/windows/apps/microsoft.services.store.engagement.storeservicesengagementmanager.aspx) en la aplicación para UWP para registrar la aplicación para la recepción notificaciones push dirigidas que envíes a tus clientes mediante el panel del Centro de desarrollo de Windows.  |
|   [Registrar eventos personalizados en la aplicación para UWP para el informe de uso del Centro de desarrollo](log-custom-events-for-dev-center.md)   |  Usa la clase [StoreServicesCustomEventLogger](https://msdn.microsoft.com/library/windows/apps/microsoft.services.store.engagement.storeservicescustomeventlogger.log.aspx) en la aplicación para UWP para registrar eventos personalizados que estén asociados a tu aplicación en el Centro de desarrollo. A continuación, puedes revisar el total de repeticiones para los eventos personalizados en la sección **Eventos personalizados** del informe de [uso](https://msdn.microsoft.com/windows/uwp/publish/usage-report) en el panel del Centro de desarrollo.  |
|  [Mostrar anuncios en tu aplicación para UWP](display-ads-in-your-app.md)    |  Usa los controles [AdControl](https://msdn.microsoft.com/library/windows/apps/microsoft.advertising.winrt.ui.adcontrol.aspx) o [InterstitialAd](https://msdn.microsoft.com/library/windows/apps/microsoft.advertising.winrt.ui.interstitialad.aspx) de tu aplicación para UWP para aumentar los ingresos mostrando anuncios en banners o anuncios intersticiales.<br/><br/>**Nota:**&nbsp;&nbsp;El Microsoft Store Services SDK solo admite aplicaciones para UWP para Windows 10. Para mostrar anuncios en las aplicaciones de Windows 8.1 y Windows Phone 8.x, usa el [SDK de Microsoft Advertising para Windows y Windows Phone 8.x](http://aka.ms/store-8-sdk).  |

<span id="prerequisites" />
## <a name="prerequisites"></a>Requisitos previos

El Microsoft Store Services SDK requiere lo siguiente:

* Visual Studio 2015 o una versión posterior.
* Visual Studio Tools para aplicaciones universales de Windows que se instala con la versión de Visual Studio.

> [!NOTE]
> Para instalar el SDK con Visual Studio 2015, debes tener instalada la versión 1.1 o posterior de Visual Studio Tools para aplicaciones universales de Windows. Para obtener más información sobre esta actualización a Visual Studio Tools para aplicaciones universales de Windows, consulta las [notas de la versión](http://go.microsoft.com/fwlink/?LinkID=624516).

<span id="install" />
## <a name="install-the-sdk"></a>Instalar el SDK

Hay dos opciones para instalar el Microsoft Store Services SDK para su uso con Visual Studio 2015 (o una versión posterior) en el equipo de desarrollo:

* **Instalador MSI**&nbsp;&nbsp;Puedes instalar el SDK mediante el programa de instalación MSI disponible [aquí](http://aka.ms/store-em-sdk). Con esta opción, las bibliotecas del SDK se instalan en una ubicación compartida en el equipo de desarrollo, de forma que cualquier proyecto UWP de Visual Studio puede hacer referencia a ellas.
* **Paquete de NuGet**&nbsp;&nbsp;Puedes instalar las bibliotecas del SDK para un proyecto UWP específico en Visual Studio con NuGet. Con esta opción, las bibliotecas del SDK solo se instalan para el proyecto en el que hayas instalado el paquete de NuGet.

Microsoft publica periódicamente nuevas versiones de Microsoft Store Services SDK con nuevas características y mejoras de rendimiento. Si tienes proyectos existentes que usan el SDK y quieres usar la versión más reciente, simplemente descarga e instala la versión más reciente del SDK en el equipo de desarrollo.

> [!NOTE]
> Para instalar el SDK con Visual Studio 2015, debes tener instalada la versión 1.1 o posterior de Visual Studio Tools para aplicaciones universales de Windows. Para obtener más información sobre esta actualización a Visual Studio Tools para aplicaciones universales de Windows, consulta las [notas de la versión](http://go.microsoft.com/fwlink/?LinkID=624516).

<span id="install-msi" />
### <a name="install-via-msi"></a>Instalación a través de MSI

Para instalar el Microsoft Store Services SDK mediante el instalador MSI:

1.  Cierra todas las instancias de Visual Studio 2015 (o una versión posterior). Si anteriormente habías instalado una versión anterior del SDK de Microsoft Advertising, el SDK de cliente de anuncios universal, la extensión Ad Mediator o el SDK de Microsoft Store Engagement and Monetization, desinstala ahora esas versiones de SDK.

2.    Abre una ventana de **símbolo del sistema** y ejecuta estos comandos para limpiar todas las versiones de SDK de publicidad anteriores que se hayan instalado con Visual Studio, pero que podrían no aparecer en la lista de programas instalados en el equipo:
  ```
  MsiExec.exe /x{5C87A4DB-31C7-465E-9356-71B485B69EC8}
  MsiExec.exe /x{6AB13C21-C3EC-46E1-8009-6FD5EBEE515B}
  MsiExec.exe /x{6AC81125-8485-463D-9352-3F35A2508C11}
  ```

3.  Descarga e instala el [Microsoft Store Services SDK](http://aka.ms/store-em-sdk). Puede tardar unos minutos en instalarse. Espera a que finalice el proceso.

4.  Reinicia Visual Studio.

5.  Si tienes un proyecto existente que haga referencia a bibliotecas de una versión anterior del Microsoft Store Services SDK, el SDK de Microsoft Advertising, el SDK de cliente de anuncios universal o el SDK de Microsoft Store Engagement and Monetization, te recomendamos abrir el proyecto en Visual Studio y limpiar y recompilar el proyecto (en el **Explorador de soluciones**, haz clic con el botón secundario en el nodo del proyecto, elige **Limpiar**y, a continuación, haz clic de nuevo en el nodo del proyecto y elige **Recompilar**).

  De lo contrario, si es la primera vez que utilizas el SDK en el proyecto, ya estás listo para [agregar las referencias de biblioteca Microsoft Store Services SDK adecuadas a tu proyecto](#references).

<span id="install-nuget" />
### <a name="install-via-nuget"></a>Instalar a través de NuGet

Para instalar las bibliotecas del Microsoft Store Services SDK para un proyecto específico a través de NuGet:

1.  Cierra todas las instancias de Visual Studio 2015 (o una versión posterior). Si anteriormente habías instalado una versión anterior del SDK de Microsoft Advertising, el SDK de cliente de anuncios universal, la extensión Ad Mediator o el SDK de Microsoft Store Engagement and Monetization, desinstala ahora esas versiones de SDK.

2.    Abre una ventana de **símbolo del sistema** y ejecuta estos comandos para limpiar todas las versiones de SDK de publicidad anteriores que se hayan instalado con Visual Studio, pero que podrían no aparecer en la lista de programas instalados en el equipo:
  ```
  MsiExec.exe /x{5C87A4DB-31C7-465E-9356-71B485B69EC8}
  MsiExec.exe /x{6AB13C21-C3EC-46E1-8009-6FD5EBEE515B}
  MsiExec.exe /x{6AC81125-8485-463D-9352-3F35A2508C11}
  ```

3.  Inicia Visual Studio y abre el proyecto en el que quieres usar las bibliotecas de Microsoft Store Services SDK.
    > [!NOTE]
    > Si tu proyecto ya incluye referencias a bibliotecas de una instalación MSI anterior del SDK, elimina esas referencias del proyecto. Esas referencias tendrán iconos de advertencia junto a ellas, porque las bibliotecas a las que hacen referencia se han eliminado en los pasos anteriores.

4. En Visual Studio, haz clic en **Proyecto** y luego en **Administrar paquetes de NuGet**.

5. En el cuadro de búsqueda, escribe **Microsoft.Services.Store.SDK** e instala el paquete Microsoft.Services.Store.SDK.
    > [!NOTE]
    > Si la ventana **Salida** notifica un error de *Install-Package* que indica que la ruta de acceso especificada es demasiado larga, puede que tengas que configurar NuGet para extraer los paquetes en otra ubicación con una ruta más corta que la ubicación predeterminada. Para ello, agrega el valor ```repositoryPath``` a un archivo nuget.config en el equipo y asígnalo a una ruta de carpeta corta donde se puedan extraer paquetes de NuGet. Para obtener más información, consulta [este artículo](http://docs.nuget.org/ndocs/consume-packages/configuring-nuget-behavior) en la documentación de NuGet. Como alternativa, puedes mover tu proyecto de Visual Studio a otra carpeta con una ruta más corta.

6. Cierra el proyecto y, a continuación, vuelve a abrirlo.

7.  Si tu proyecto ya hace referencia a bibliotecas de una versión anterior del Microsoft Store Services SDK que se haya instalado a través de NuGet y has actualizado tu proyecto a una versión más reciente del SDK, te recomendamos limpiar y recompilar el proyecto (en el **Explorador de soluciones**, haz clic con el botón secundario en el nodo del proyecto, elige **Limpiar**y, a continuación, haz clic de nuevo en el nodo del proyecto y elige **Recompilar**).

  De lo contrario, si es la primera vez que utilizas el SDK en el proyecto, ya estás listo para [agregar las referencias de biblioteca Microsoft Store Services SDK adecuadas a tu proyecto](#references).

<span id="references" />
## <a name="add-sdk-library-references-to-your-project"></a>Agregar referencias a bibliotecas de SDK al proyecto

Después de instalar el Microsoft Store Services SDK a través del instalador MSI o de NuGet, sigue estas instrucciones para hacer referencia a las bibliotecas del SDK en el proyecto UWP.

1. Abre el proyecto en Visual Studio.
    > [!NOTE]
    > Si tu proyecto es una aplicación JavaScript que tiene como destino **Cualquier CPU**, actualiza el proyecto para que use una salida de compilación específica de la arquitectura (por ejemplo, **x86**).

2. En el **Explorador de soluciones**, haz clic con el botón secundario en **Referencias** y selecciona **Agregar referencia...**

3. En el **Administrador de referencias**, expande **Universal Windows**, haz clic en **Extensiones**y, a continuación, selecciona la casilla junto a uno de estos elementos.

  * Para usar las API en el espacio de nombres [Microsoft.Services.Store.Engagement](https://msdn.microsoft.com/library/windows/apps/microsoft.services.store.engagement.aspx) para la participación de los clientes, selecciona la casilla junto a **Microsoft Engagement Framework**. Elige esta opción si desea [ejecutar experimentos con pruebas A/ B](run-app-experiments-with-a-b-testing.md), [iniciar el Centro de opiniones](launch-feedback-hub-from-your-app.md), [recibir notificaciones push dirigidas del Centro de desarrollo](configure-your-app-to-receive-dev-center-notifications.md) o [registrar eventos personalizados en el Centro de desarrollo](log-custom-events-for-dev-center.md).

  * Para usar las API para [mostrar anuncios en banners o intersticiales en tu aplicación](display-ads-in-your-app.md), selecciona la casilla junto a **Microsoft Advertising SDK for XAML** o **Microsoft Advertising SDK for JavaScript**, según el tipo de proyecto.

3. Haz clic en **Aceptar**.

> [!NOTE]
> Si instalaste las bibliotecas del SDK a través de NuGet, el proyecto contendrá una referencia **Microsoft.Services.Store.SDK**, además de **SDK de Microsoft Advertising para XAML** o **SDK de Microsoft Advertising para JavaScript**. La referencia **Microsoft.Services.Store.SDK** representa el paquete de NuGet (en lugar de las bibliotecas que contiene) y puedes omitirla.

<span id="framework" />
## <a name="understanding-framework-packages-in-the-sdk"></a>Descripción de los paquetes de marcos en el SDK

Las siguientes bibliotecas incluidas en Microsoft Store Services SDK están configuradas como *paquetes de marcos*:

* Microsoft.Advertising.dll. Esta biblioteca contiene las API de publicidad en los espacios de nombres [Microsoft.Advertising](https://msdn.microsoft.com/library/windows/apps/mt313187.aspx) y [Microsoft.Advertising.WinRT.UI](https://msdn.microsoft.com/library/windows/apps/microsoft.advertising.winrt.ui.aspx).
* Microsoft.Services.Store.Engagement.dll. Esta biblioteca contiene las API en el espacio de nombres [Microsoft.Services.Store.Engagement](https://msdn.microsoft.com/library/windows/apps/microsoft.services.store.engagement.aspx).

De esta manera, después de que un usuario instale una versión de tu aplicación que use estas bibliotecas, las bibliotecas se actualizarán automáticamente en su dispositivo a través de Windows Update cada vez que publiquemos nuevas versiones de las bibliotecas con correcciones y mejoras de rendimiento. Esto ayuda a garantizar que tus clientes tengan siempre la versión más reciente de las bibliotecas instalada en sus dispositivos.

Si publicamos una nueva versión del SDK que incorpora nuevas API o características en estas bibliotecas, tendrás que instalar la versión más reciente del SDK para usar dichas características. En este escenario, también necesitarás publicar la aplicación actualizada en la Tienda.

## <a name="related-topics"></a>Temas relacionados

* [Microsoft Store Services SDK API reference (Referencia de las API de Microsoft Store Services SDK)](https://msdn.microsoft.com/library/windows/apps/mt691886.aspx)
* [Ejecutar experimentos con pruebas A/B](run-app-experiments-with-a-b-testing.md)
* [Iniciar el Centro de opiniones desde la aplicación](launch-feedback-hub-from-your-app.md)
* [Configurar la aplicación para recibir notificaciones de inserción del Centro de desarrollo](configure-your-app-to-receive-dev-center-notifications.md)
* [Registrar eventos personalizados para el Centro de desarrollo](log-custom-events-for-dev-center.md)
* [Mostrar anuncios en tu aplicación](display-ads-in-your-app.md)
