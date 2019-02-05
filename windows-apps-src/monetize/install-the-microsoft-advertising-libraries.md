---
ms.assetid: 3aeddb83-5314-447b-b294-9fc28273cd39
description: Aprende a instalar el SDK de Microsoft Advertising.
title: Instalar el SDK de Microsoft Advertising
ms.date: 08/23/2017
ms.topic: article
keywords: Windows 10, uwp, anuncios, publicidad, instalar, SDK, biblioteca de publicidad
ms.localizationpriority: medium
ms.openlocfilehash: 121accdfc8996c609c616838f645f19e2377c7c5
ms.sourcegitcommit: bf600a1fb5f7799961914f638061986d55f6ab12
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 02/05/2019
ms.locfileid: "9047756"
---
# <a name="install-the-microsoft-advertising-sdk"></a>Instalar el SDK de Microsoft Advertising

Para mostrar anuncios en las aplicaciones para UWP en Windows10, instala el [SDK de Microsoft Advertising](https://aka.ms/ads-sdk-uwp). Este SDK es una extensión de Visual Studio 2015 y versiones posteriores.

> [!NOTE]
> Si estás desarrollando una aplicación para UWP HTML o JavaScript e instalaste el SDK de Windows 10 versión 10.0.14393 (actualización de aniversario) o una versión posterior, debes instalar también la biblioteca [WinJS](https://github.com/winjs/winjs) . Antes, esta biblioteca estaba incluida en versiones anteriores del SDK de Windows 10 pero desde la versión 10.0.14393 del SDK de Windows 10 (Actualización de aniversario), debe instalarse por separado.

<span id="install-msi" />

## <a name="install-via-msi"></a>Instalación a través de MSI

Para instalar el SDK de Microsoft Advertising mediante el instalador MSI:

1.  Cierra todas las instancias de Visual Studio.

2. Si anteriormente habías instalado una versión anterior del SDK de Microsoft Advertising, el SDK de cliente de anuncios universal, la extensión Ad Mediator o el SDK de Microsoft Store Engagement and Monetization, desinstala ahora esas versiones de SDK. Opcionalmente, abre una ventana de **símbolo del sistema** y ejecuta estos comandos para limpiar todas las versiones de SDK de publicidad anteriores que se hayan instalado con Visual Studio, pero que podrían no aparecer en la lista de programas instalados en el equipo:
    ```
    MsiExec.exe /x{5C87A4DB-31C7-465E-9356-71B485B69EC8}
    MsiExec.exe /x{6AB13C21-C3EC-46E1-8009-6FD5EBEE515B}
    MsiExec.exe /x{6AC81125-8485-463D-9352-3F35A2508C11}
    ```

3.  Descarga e instala el [SDK de Microsoft Advertising](https://aka.ms/ads-sdk-uwp). Puede tardar unos minutos en instalarse. Espera a que finalice el proceso.

4.  Reinicia Visual Studio.

5.  Si tienes un proyecto existente que haga referencia a bibliotecas de publicidad de una versión anterior del SDK de Microsoft Advertising, el SDK de cliente de anuncios universal o el SDK de Microsoft Store Engagement and Monetization, te recomendamos abrir el proyecto en Visual Studio y limpiar y recompilar el proyecto (en el **Explorador de soluciones**, haz clic con el botón secundario en el nodo del proyecto, elige **Limpiar**y, a continuación, haz clic de nuevo en el nodo del proyecto y elige **Recompilar**).

  De lo contrario, si es la primera vez que utilizas el SDK de Microsoft Advertising en el proyecto, ya estás listo para [agregar una referencia al SDK de Microsoft Advertising](#reference).

<span id="install-nuget" />

## <a name="install-via-nuget"></a>Instalar a través de NuGet

Para instalar el SDK de Microsoft Advertising en un proyecto de UWP específico a través de NuGet:

1.  Cierra todas las instancias de Visual Studio.

2.  Si anteriormente habías instalado una versión anterior del SDK de Microsoft Advertising, el SDK de cliente de anuncios universal, la extensión Ad Mediator o el SDK de Microsoft Store Engagement and Monetization, desinstala ahora esas versiones de SDK. Opcionalmente, abre una ventana de **símbolo del sistema** y ejecuta estos comandos para limpiar todas las versiones de SDK de publicidad anteriores que se hayan instalado con Visual Studio, pero que podrían no aparecer en la lista de programas instalados en el equipo:
    ```
    MsiExec.exe /x{5C87A4DB-31C7-465E-9356-71B485B69EC8}
    MsiExec.exe /x{6AB13C21-C3EC-46E1-8009-6FD5EBEE515B}
    MsiExec.exe /x{6AC81125-8485-463D-9352-3F35A2508C11}
    ```

3.  Inicia Visual Studio y abre el proyecto en el que quieres usar el SDK de Microsoft Advertising.
    > [!NOTE]
    > Si tu proyecto ya incluye referencias a bibliotecas de una instalación MSI anterior del SDK, elimina esas referencias del proyecto. Esas referencias tendrán iconos de advertencia junto a ellas, porque las bibliotecas a las que hacen referencia se han eliminado en los pasos anteriores.

4. En VisualStudio, haz clic en **Proyecto** y en **Administrar paquetes de NuGet**.

5. En el cuadro de búsqueda, escribe **Microsoft.Advertising.XAML** (para un proyecto de XAML) o **Microsoft.Advertising.JS** (para un proyecto de JavaScript o HTML) e instala el paquete correspondiente. Cuando el paquete se haya instalado, guarda la solución.
    > [!NOTE]
    > Si la ventana **Salida** notifica un error de *Install-Package* que indica que la ruta de acceso especificada es demasiado larga, puede que tengas que configurar NuGet para extraer los paquetes en otra ubicación con una ruta más corta que la ubicación predeterminada. Para ello, agrega el valor ```repositoryPath``` a un archivo nuget.config en el equipo y asígnalo a una ruta de carpeta corta donde se puedan extraer paquetes de NuGet. Para obtener más información, consulta [este artículo](https://docs.nuget.org/ndocs/consume-packages/configuring-nuget-behavior) en la documentación de NuGet. Como alternativa, puedes mover tu proyecto de Visual Studio a otra carpeta con una ruta más corta.

6. Cierra la solución y, a continuación, vuelve a abrirla.

7.  Si tu proyecto ya hace referencia a bibliotecas de una versión anterior del SDK de Microsoft Advertising que se haya instalado a través de NuGet y has actualizado tu proyecto a una versión más reciente del SDK, te recomendamos limpiar y recompilar el proyecto (en el **Explorador de soluciones**, haz clic con el botón secundario en el nodo del proyecto, elige **Limpiar**y, a continuación, haz clic de nuevo en el nodo del proyecto y elige **Recompilar**).

  De lo contrario, si es la primera vez que utilizas el SDK en el proyecto, ya estás listo para [agregar una referencia al SDK de Microsoft Advertising](#reference).

<span id="reference" />

## <a name="add-a-reference-to-the-microsoft-advertising-sdk"></a>Agregar una referencia al SDK de Microsoft Advertising

Después de instalar el SDK de Microsoft Advertising, sigue estas instrucciones para hacer referencia al SDK en tu proyecto para usar las API de publicidad.

1. Abre el proyecto en Visual Studio.
    > [!NOTE]
    > Si el destino del proyecto es **Cualquier CPU**, actualiza el proyecto para que use una salida de compilación específica por arquitectura (por ejemplo, **x86**). Si el destino del proyecto es **Cualquier CPU**, no podrás agregar una referencia al SDK de Microsoft Advertising correctamente a través de los siguientes pasos. Para obtener más información, consulta [Errores de referencia derivados de orientar el proyecto a Cualquier CPU](known-issues-for-the-advertising-libraries.md#reference_errors).

2. En el **Explorador de soluciones**, haz clic con el botón derecho en **Referencias** y selecciona **Agregar referencia…**

3. En **Administrador de referencias**, amplía **Universal de Windows**, haz clic en **Extensiones** y luego selecciona la casilla de verificación junto a **SDK de Microsoft Advertising para XAML** (para aplicaciones XAML) o **SDK de Microsoft Advertising para JavaScript** (para aplicaciones compiladas con JavaScript y HTML).

4.  En el **Administrador de referencias**, haz clic en Aceptar.

Para ver tutoriales que muestran cómo comenzar a usar las API de publicidad, consulta los siguientes artículos:

* [Anuncios intersticiales](interstitial-ads.md)
* [Anuncios nativos](native-ads.md)
* [AdControl en XAML y .NET](adcontrol-in-xaml-and--net.md)
* [AdControl en HTML 5 y JavaScript](adcontrol-in-html-5-and-javascript.md)

<span id="framework" />

## <a name="understanding-framework-packages-in-the-microsoft-advertising-sdk"></a>Descripción de los paquetes de marcos en el SDK de Microsoft Advertising

La biblioteca Microsoft.Advertising.dll en el [SDK de Microsoft Advertising](https://aka.ms/ads-sdk-uwp) (para aplicaciones para UWP) está configurada como un *paquete de marcos*. Esta biblioteca contiene las API de publicidad en los espacios de nombres [Microsoft.Advertising](https://docs.microsoft.com/uwp/api/microsoft.advertising) y [Microsoft.Advertising.WinRT.UI](https://docs.microsoft.com/uwp/api/microsoft.advertising.winrt.ui).

Dado que esta biblioteca es un paquete de marcos, después de que un usuario instale una versión de tu aplicación que use esta biblioteca, esta se actualizará automáticamente en su dispositivo a través de Windows Update cada vez que publiquemos una nueva versión de la biblioteca con correcciones y mejoras de rendimiento. Esto ayuda a garantizar que tus clientes tengan siempre instalada en sus dispositivos la versión más reciente de la biblioteca.

Si publicamos una nueva versión del SDK que incorpore nuevas API o características en esta biblioteca, tendrás que instalar la versión más reciente del SDK para usar dichas características. En este escenario, también necesitarás publicar la aplicación actualizada en la Store.
