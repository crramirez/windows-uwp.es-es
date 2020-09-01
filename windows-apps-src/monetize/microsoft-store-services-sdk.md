---
Description: Microsoft Store Services SDK proporciona bibliotecas y herramientas que puedes usar para agregar características a tus aplicaciones y que te ayudan a obtener más dinero y a ganar clientes.
title: Conectar con clientes con Microsoft Store Services SDK
ms.assetid: 518516DB-70A7-49C4-B3B6-CD8A98320B9C
ms.date: 08/21/2017
ms.topic: article
keywords: SDK de Windows 10, UWP, Microsoft Store Services
ms.localizationpriority: medium
ms.openlocfilehash: 7b8544f6d4f60b2f4ca91af35ff922fcfe089380
ms.sourcegitcommit: 7b2febddb3e8a17c9ab158abcdd2a59ce126661c
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 08/31/2020
ms.locfileid: "89155469"
---
# <a name="engage-customers-with-the-microsoft-store-services-sdk"></a>Conectar con clientes con Microsoft Store Services SDK

El SDK de Microsoft Store Services proporciona características que le ayudarán a interactuar con los clientes de las aplicaciones Plataforma universal de Windows (UWP), como enviar notificaciones de destino a sus aplicaciones y ejecutar experimentos a/B en sus aplicaciones. Este SDK es una extensión de Visual Studio 2015 y versiones posteriores de Visual Studio.

> [!NOTE]
> Para mostrar anuncios en las aplicaciones para UWP, use el [SDK de Microsoft Advertising](https://marketplace.visualstudio.com/items?itemName=AdMediator.MicrosoftAdvertisingSDK) en lugar del sdk de Microsoft Store Services. Las bibliotecas de publicidad se han migrado del SDK de Microsoft Store Services al SDK de Microsoft Advertising. Para obtener más información, consulta [Mostrar anuncios en tu aplicación](display-ads-in-your-app.md).



## <a name="scenarios-supported-by-the-microsoft-store-services-sdk"></a>Escenarios admitidos por el SDK de Microsoft Store Services

Actualmente, el SDK de Microsoft Store Services admite los siguientes escenarios para las aplicaciones UWP. Para obtener documentación de referencia de API, consulte referencia de la [API del SDK de Microsoft Store Services](/uwp/api/overview/engagement).

|  Escenario  |  Descripción   |
|------------|----------------|
|  [Ejecutar experimentos con pruebas A/B en tu aplicación para UWP](run-app-experiments-with-a-b-testing.md)    |  Ejecuta pruebas A/B en la aplicación para la Plataforma universal de Windows (UWP) para medir la eficacia de las características en algunos clientes antes de lanzar las características para todo el mundo. Después de definir un experimento en el centro de Partners, use la clase [StoreServicesExperimentVariation](/uwp/api/microsoft.services.store.engagement.storeservicesexperimentvariation) para obtener las variaciones del experimento en la aplicación, use estos datos para modificar el comportamiento de la característica que está probando y, a continuación, use el método [LogForVariation](/uwp/api/microsoft.services.store.engagement.storeservicescustomeventlogger.logforvariation) para enviar eventos de visualización y eventos de conversión al centro de Partners. Por último, use el centro de partners para ver los resultados y administrar el experimento.  |
|  [Iniciar el Centro de opiniones desde la aplicación para UWP](launch-feedback-hub-from-your-app.md)    |  Usa la clase [StoreServicesFeedbackLauncher](/uwp/api/microsoft.services.store.engagement.storeservicesfeedbacklauncher) en tu aplicación para UWP para dirigir a los clientes de Windows 10 al Centro de opiniones, donde pueden enviar sus problemas, sugerencias y votos a favor. A continuación, administra estas opiniones en el [Informe de comentarios](../publish/feedback-report.md) del Centro de partners. |
|  [Configuración de la aplicación de UWP para recibir notificaciones de envío de centro de Partners](configure-your-app-to-receive-dev-center-notifications.md)    |  Use la clase [StoreServicesEngagementManager](/uwp/api/microsoft.services.store.engagement.storeservicesengagementmanager) en la aplicación de UWP para registrar la aplicación para recibir notificaciones de envío de destino que envíe a los clientes mediante el centro de Partners.  |
|   [Registro de eventos personalizados en la aplicación para UWP para el informe de uso en el centro de Partners](log-custom-events-for-dev-center.md)   |  Use la clase [StoreServicesCustomEventLogger](/uwp/api/microsoft.services.store.engagement.storeservicescustomeventlogger.log) en la aplicación de UWP para registrar eventos personalizados que están asociados a la aplicación en el centro de Partners. A continuación, revise el total de repeticiones de los eventos personalizados en la sección **eventos personalizados** del [Informe de uso](../publish/usage-report.md) del centro de Partners.  |

<span id="prerequisites" />

## <a name="prerequisites"></a>Requisitos previos

El Microsoft Store Services SDK requiere lo siguiente:

* Visual Studio 2015 o una versión posterior.
* Visual Studio Tools para aplicaciones universales de Windows instalados con tu versión de Visual Studio.

<span id="install" />

## <a name="install-the-sdk"></a>Instalación del SDK

Hay dos opciones para instalar el SDK de Microsoft Store Services en el equipo de desarrollo:

* **Instalador MSI** &nbsp; &nbsp; Puede instalar el SDK mediante el instalador MSI disponible [aquí](https://marketplace.visualstudio.com/items?itemName=AdMediator.MicrosoftStoreServicesSDK).
* **Paquete NuGet** &nbsp; &nbsp; Puede instalar el SDK como un paquete NuGet.

Microsoft publica periódicamente nuevas versiones de Microsoft Store Services SDK con nuevas características y mejoras de rendimiento. Si tienes proyectos existentes que usan el SDK y quieres usar la versión más reciente, simplemente descarga e instala la versión más reciente del SDK en el equipo de desarrollo.

<span id="install-msi" />

### <a name="install-via-msi"></a>Instalación a través de MSI

Para instalar el Microsoft Store Services SDK mediante el instalador MSI:

1.  Cierre todas las instancias de Visual Studio.

2. Si anteriormente instaló el SDK de Microsoft Store Engagement y monetización, el SDK de cliente de ad universal o la extensión de ad mediator, desinstale estos SDK ahora. Opcionalmente, abra una ventana **del símbolo del sistema** y ejecute estos comandos para limpiar cualquier versión anterior del SDK que se haya instalado con Visual Studio, pero que puede que no aparezca en la lista de programas instalados en el equipo:
    ```console
    MsiExec.exe /x{5C87A4DB-31C7-465E-9356-71B485B69EC8}
    MsiExec.exe /x{6AB13C21-C3EC-46E1-8009-6FD5EBEE515B}
    MsiExec.exe /x{6AC81125-8485-463D-9352-3F35A2508C11}
    ```

3.  Descarga e instala el [Microsoft Store Services SDK](https://marketplace.visualstudio.com/items?itemName=AdMediator.MicrosoftStoreServicesSDK). Puede tardar unos minutos en instalarse. Espera a que finalice el proceso.

4.  Reinicie Visual Studio.

5.  Si tiene un proyecto existente que hace referencia a las bibliotecas de cualquier versión anterior del SDK de Microsoft Store Services, Microsoft Advertising SDK, el SDK de cliente de Active Directory o el SDK de Microsoft Store Engagement y monetización, se recomienda que abra el proyecto en Visual Studio y que limpie y recompile el proyecto (en **Explorador de soluciones**, haga clic con el botón derecho en el nodo del proyecto y elija **limpiar**y, a continuación, vuelva a hacer clic con el botón secundario en el nodo del proyecto y elija **reconstruir**).

  De lo contrario, si usa el SDK por primera vez en el proyecto, ahora está listo para [Agregar la referencia de ensamblado al proyecto](#references).

<span id="install-nuget" />

### <a name="install-via-nuget"></a>Instalar a través de NuGet

Para instalar las bibliotecas del SDK de Microsoft Store Services a través de NuGet:

1.  Cierre todas las instancias de Visual Studio.

2. Si anteriormente instaló el SDK de Microsoft Store Engagement y monetización, el SDK de cliente de ad universal o la extensión de ad mediator, desinstale estos SDK ahora. Opcionalmente, abra una ventana **del símbolo del sistema** y ejecute estos comandos para limpiar cualquier versión anterior del SDK que se haya instalado con Visual Studio, pero que puede que no aparezca en la lista de programas instalados en el equipo:
    ```console
    MsiExec.exe /x{5C87A4DB-31C7-465E-9356-71B485B69EC8}
    MsiExec.exe /x{6AB13C21-C3EC-46E1-8009-6FD5EBEE515B}
    MsiExec.exe /x{6AC81125-8485-463D-9352-3F35A2508C11}
    ```

3.  Inicie Visual Studio y abra el proyecto en el que desea usar el SDK de Microsoft Store Services.
    > [!NOTE]
    > Si el proyecto ya incluye referencias de biblioteca de una instalación de MSI anterior del SDK, quite estas referencias del proyecto. Esas referencias tendrán iconos de advertencia junto a ellas, porque las bibliotecas a las que hacen referencia se han eliminado en los pasos anteriores.

4. En Visual Studio, haz clic en **Proyecto** y luego en **Administrar paquetes de NuGet**.

5. En el cuadro de búsqueda, escriba **Microsoft. Services. Store. Engagement** e instale el paquete Microsoft. Services. Store. Engagement. Cuando el paquete termine de instalarse, guarde la solución.
    > [!NOTE]
    > Si la ventana de **salida** informa de un error *de instalación-paquete* que indica que la ruta de acceso especificada es demasiado larga, es posible que deba configurar NuGet para extraer paquetes en una ubicación alternativa con una ruta de acceso más corta que la predeterminada. Para ello, agrega el valor `repositoryPath` a un archivo nuget.config en el equipo y asígnalo a una ruta de carpeta corta donde se puedan extraer paquetes de NuGet. Para obtener más información, consulta [este artículo](/nuget/consume-packages/configuring-nuget-behavior) en la documentación de NuGet. Como alternativa, puedes mover tu proyecto de Visual Studio a otra carpeta con una ruta más corta. El problema también puede deberse a que la ruta de acceso de los paquetes globales es demasiado larga. En este caso, agregue el `globalPackagesFolder` valor al archivo de nuget.config.

6. Cierre la solución de Visual Studio que contiene el proyecto y, a continuación, vuelva a abrir la solución.

7.  Si tu proyecto ya hace referencia a bibliotecas de una versión anterior del Microsoft Store Services SDK que se haya instalado a través de NuGet y has actualizado tu proyecto a una versión más reciente del SDK, te recomendamos limpiar y recompilar el proyecto (en el **Explorador de soluciones**, haz clic con el botón secundario en el nodo del proyecto, elige **Limpiar**y, a continuación, haz clic de nuevo en el nodo del proyecto y elige **Recompilar**).

  De lo contrario, si usa el SDK por primera vez en el proyecto, ahora está listo para [Agregar la referencia de ensamblado al proyecto](#references).

<span id="references" />

## <a name="add-the-assembly-reference-to-your-project"></a>Agregar la referencia de ensamblado al proyecto

Después de instalar el SDK de Microsoft Store Services mediante el instalador de MSI o NuGet, siga estas instrucciones para hacer referencia al ensamblado del SDK en el proyecto de UWP.

1. Abra el proyecto en Visual Studio.
    > [!NOTE]
    > Si el proyecto es una aplicación de JavaScript que tiene como destino **cualquier CPU**, actualice el proyecto para usar una salida de compilación específica de la arquitectura (por ejemplo, **x86**).

2. En el **Explorador de soluciones**, haz clic con el botón secundario en **Referencias** y selecciona **Agregar referencia...**

3. En **Administrador de referencias**, expanda **universal Windows**, haga clic en **extensiones**y, a continuación, active la casilla situada junto a **marco de trabajo de Microsoft Engagement**. Esto le permite usar las API en el espacio de nombres [Microsoft. Services. Store. Engagement](/uwp/api/microsoft.services.store.engagement) .

3. Haga clic en **Aceptar**.

> [!NOTE]
> Si instaló las bibliotecas de SDK a través de NuGet, el proyecto contendrá una referencia de **Microsoft. Services. Store. Engagement** . La referencia **Microsoft. Services. Store. Engagement** representa el paquete NuGet (en lugar de las bibliotecas que hay en él) y puede pasarlo por alto.

<span id="framework" />

## <a name="understanding-framework-packages-in-the-sdk"></a>Descripción de los paquetes de marcos en el SDK

La biblioteca de Microsoft.Services.Store.Engagement.dll del SDK de Microsoft Store Services se configura como un *paquete de Framework*. Esta biblioteca contiene las API en el espacio de nombres [Microsoft.Services.Store.Engagement](/uwp/api/microsoft.services.store.engagement).

Dado que esta biblioteca es un paquete de marco, esto significa que, una vez que un usuario instala una versión de la aplicación que usa esta biblioteca, esta biblioteca se actualiza automáticamente en el dispositivo a través de Windows Update cada vez que se publica una nueva versión de la biblioteca con correcciones y mejoras de rendimiento. Esto ayuda a garantizar que los clientes siempre tengan la última versión disponible de la biblioteca instalada en sus dispositivos.

Si lanzamos una nueva versión del SDK que incorpora nuevas características o API en esta biblioteca, deberá instalar la versión más reciente del SDK para usar esas características. En este escenario, también necesitarás publicar la aplicación actualizada en la Tienda.

## <a name="related-topics"></a>Temas relacionados

* [Microsoft Store Services SDK API reference (Referencia de las API de Microsoft Store Services SDK)](/uwp/api/overview/engagement)
* [Ejecutar experimentos con pruebas A/B](run-app-experiments-with-a-b-testing.md)
* [Iniciar el Centro de opiniones desde la aplicación](launch-feedback-hub-from-your-app.md)
* [Configurar la aplicación para notificaciones de inserción dirigidas](configure-your-app-to-receive-dev-center-notifications.md)
* [Registrar eventos personalizados para el Centro de partners](log-custom-events-for-dev-center.md)