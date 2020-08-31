---
ms.assetid: 3aeddb83-5314-447b-b294-9fc28273cd39
description: Obtenga información sobre cómo instalar el SDK de Microsoft Advertising para mostrar anuncios en aplicaciones Plataforma universal de Windows (UWP) para Windows 10.
title: Instala el SDK de Microsoft Advertising
ms.date: 02/18/2020
ms.topic: article
keywords: Windows 10, UWP, anuncios, publicidad, instalación, SDK, biblioteca de publicidad
ms.localizationpriority: medium
ms.openlocfilehash: d5c5c18c41996c5d46c261f351a900fea2532a93
ms.sourcegitcommit: 45dec3dc0f14934b8ecf1ee276070b553f48074d
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 08/29/2020
ms.locfileid: "89094681"
---
# <a name="install-the-microsoft-advertising-sdk"></a>Instala el SDK de Microsoft Advertising

>[!WARNING]
> A partir del 1 de junio de 2020, se cerrará la plataforma de monetización de Microsoft ad para aplicaciones UWP de Windows. [Más información](https://social.msdn.microsoft.com/Forums/windowsapps/en-US/db8d44cb-1381-47f7-94d3-c6ded3fea36f/microsoft-ad-monetization-platform-shutting-down-june-1st?forum=aiamgr)

Para mostrar anuncios en las aplicaciones para UWP para Windows 10, instale el [SDK de Microsoft Advertising](https://marketplace.visualstudio.com/items?itemName=AdMediator.MicrosoftAdvertisingSDK). Este SDK es una extensión de Visual Studio 2015 y versiones posteriores.

> [!NOTE]
> Si está desarrollando una aplicación de UWP JavaScript/HTML y ha instalado el SDK de Windows 10 versión 10.0.14393 (actualización de aniversario) o posterior, también debe instalar la biblioteca [WinJS](https://github.com/winjs/winjs) . Esta biblioteca solía incluirse en versiones anteriores del SDK de Windows 10, pero a partir de la versión 10.0.14393 del SDK de Windows 10 (actualización de aniversario) esta biblioteca debe instalarse por separado.

<span id="install-msi" />

## <a name="install-via-msi"></a>Instalación a través de MSI

Para instalar el SDK de Microsoft Advertising mediante el instalador de MSI:

1.  Cierre todas las instancias de Visual Studio.

2. Si anteriormente habías instalado una versión anterior del SDK de Microsoft Advertising, el SDK de cliente de anuncios universal, la extensión Ad Mediator o el SDK de Microsoft Store Engagement and Monetization, desinstala ahora esas versiones de SDK. Opcionalmente, abra una ventana **del símbolo del sistema** y ejecute estos comandos para limpiar cualquier versión anterior del SDK de publicidad que pueda haberse instalado con Visual Studio, pero que puede que no aparezca en la lista de programas instalados en el equipo:
    ```console
    MsiExec.exe /x{5C87A4DB-31C7-465E-9356-71B485B69EC8}
    MsiExec.exe /x{6AB13C21-C3EC-46E1-8009-6FD5EBEE515B}
    MsiExec.exe /x{6AC81125-8485-463D-9352-3F35A2508C11}
    ```

3.  Descargue e instale el [SDK de Microsoft Advertising](https://marketplace.visualstudio.com/items?itemName=AdMediator.MicrosoftAdvertisingSDK). Puede tardar unos minutos en instalarse. Espera a que finalice el proceso.

4.  Reinicie Visual Studio.

5.  Si tiene un proyecto existente que hace referencia a las bibliotecas de publicidad de cualquier versión anterior del SDK de Microsoft Advertising, del SDK de cliente de universal ad o del SDK de Microsoft Store Engagement y monetización, se recomienda que abra el proyecto en Visual Studio y que limpie y recompile el proyecto (en **Explorador de soluciones**, haga clic con el botón secundario en el nodo del proyecto **y elija** **limpiar**

  De lo contrario, si usa el SDK de Microsoft Advertising por primera vez en el proyecto, ahora está listo para [Agregar una referencia al SDK de Microsoft Advertising](#reference).

<span id="install-nuget" />

## <a name="install-via-nuget"></a>Instalar a través de NuGet

Para instalar el SDK de Microsoft Advertising en un proyecto de UWP específico a través de NuGet:

1.  Cierre todas las instancias de Visual Studio.

2.  Si anteriormente habías instalado una versión anterior del SDK de Microsoft Advertising, el SDK de cliente de anuncios universal, la extensión Ad Mediator o el SDK de Microsoft Store Engagement and Monetization, desinstala ahora esas versiones de SDK. Opcionalmente, abra una ventana **del símbolo del sistema** y ejecute estos comandos para limpiar cualquier versión anterior del SDK de publicidad que pueda haberse instalado con Visual Studio, pero que puede que no aparezca en la lista de programas instalados en el equipo:
    ```console
    MsiExec.exe /x{5C87A4DB-31C7-465E-9356-71B485B69EC8}
    MsiExec.exe /x{6AB13C21-C3EC-46E1-8009-6FD5EBEE515B}
    MsiExec.exe /x{6AC81125-8485-463D-9352-3F35A2508C11}
    ```

3.  Inicie Visual Studio y abra el proyecto en el que desea usar el SDK de Microsoft Advertising.
    > [!NOTE]
    > Si el proyecto ya incluye referencias de biblioteca de una instalación de MSI anterior del SDK, quite estas referencias del proyecto. Esas referencias tendrán iconos de advertencia junto a ellas, porque las bibliotecas a las que hacen referencia se han eliminado en los pasos anteriores.

4. En Visual Studio, haz clic en **Proyecto** y luego en **Administrar paquetes de NuGet**.

5. En el cuadro de búsqueda, escriba **Microsoft. Advertising. Xaml** (para un proyecto XAML) o **Microsoft.Advertising.JS** (para un proyecto de JavaScript/HTML) e instale el paquete correspondiente. Cuando el paquete termine de instalarse, guarde la solución.
    > [!NOTE]
    > Si la ventana de **salida** informa de un error *de instalación-paquete* que indica que la ruta de acceso especificada es demasiado larga, es posible que deba configurar NuGet para extraer paquetes en una ubicación alternativa con una ruta de acceso más corta que la predeterminada. Para ello, agrega el valor `repositoryPath` a un archivo nuget.config en el equipo y asígnalo a una ruta de carpeta corta donde se puedan extraer paquetes de NuGet. Para obtener más información, consulta [este artículo](https://docs.microsoft.com/nuget/consume-packages/configuring-nuget-behavior) en la documentación de NuGet. Como alternativa, puedes mover tu proyecto de Visual Studio a otra carpeta con una ruta más corta.

6. Cierre la solución y vuelva a abrirla.

7.  Si el proyecto ya hace referencia a bibliotecas de una versión anterior del SDK de Microsoft Advertising que se instaló a través de NuGet y ha actualizado el proyecto a una versión más reciente del SDK, se recomienda que limpie y recompile el proyecto (en **Explorador de soluciones**, haga clic con el botón derecho en el nodo del proyecto **y elija** **limpiar**y, a continuación, vuelva a hacer clic con el botón secundario en el nodo del

  De lo contrario, si usa el SDK por primera vez en el proyecto, ahora está listo para [Agregar una referencia al SDK de Microsoft Advertising](#reference).

<span id="reference" />

## <a name="add-a-reference-to-the-microsoft-advertising-sdk"></a>Agregar una referencia al SDK de Microsoft Advertising

Después de instalar el SDK de Microsoft Advertising, siga estas instrucciones para hacer referencia al SDK en el proyecto para poder usar las API de publicidad.

1. Abra el proyecto en Visual Studio.
    > [!NOTE]
    > Si el destino del proyecto es **Cualquier CPU**, actualiza el proyecto para que use una salida de compilación específica por arquitectura (por ejemplo, **x86**). Si el proyecto tiene como destino una **CPU**, no podrá agregar una referencia al SDK de Microsoft Advertising correctamente en los pasos siguientes. Para obtener más información, consulta [Errores de referencia derivados de orientar el proyecto a Cualquier CPU](known-issues-for-the-advertising-libraries.md#reference_errors).

2. En el **Explorador de soluciones**, haz clic con el botón secundario en **Referencias** y selecciona **Agregar referencia...**

3. En **Administrador de referencias**, expanda **universal Windows**, haga clic en **extensiones**y, a continuación, active la casilla situada junto a **Microsoft Advertising SDK para xaml** (para aplicaciones XAML) o **Microsoft Advertising SDK para JavaScript** (para aplicaciones compiladas con JavaScript y HTML).

4.  En el **Administrador de referencias**, haz clic en Aceptar.

Para ver tutoriales que muestran cómo empezar a usar las API de publicidad, consulte los siguientes artículos:

* [Anuncios intersticiales](interstitial-ads.md)
* [Anuncios nativos](native-ads.md)
* [AdControl en XAML y .NET](adcontrol-in-xaml-and--net.md)
* [AdControl en HTML 5 y JavaScript](adcontrol-in-html-5-and-javascript.md)

<span id="framework" />

## <a name="understanding-framework-packages-in-the-microsoft-advertising-sdk"></a>Descripción de los paquetes de .NET Framework en el SDK de Microsoft Advertising

La biblioteca de Microsoft.Advertising.dll del [SDK de Microsoft Advertising](https://marketplace.visualstudio.com/items?itemName=AdMediator.MicrosoftAdvertisingSDK) (para aplicaciones UWP) se configura como un *paquete de marco*. Esta biblioteca contiene las API de publicidad en los espacios de nombres [Microsoft.Advertising](https://docs.microsoft.com/uwp/api/microsoft.advertising) y [Microsoft.Advertising.WinRT.UI](https://docs.microsoft.com/uwp/api/microsoft.advertising.winrt.ui).

Dado que esta biblioteca es un paquete de marco, esto significa que, una vez que un usuario instala una versión de la aplicación que usa esta biblioteca, esta biblioteca se actualiza automáticamente en el dispositivo a través de Windows Update cada vez que se publica una nueva versión de la biblioteca con correcciones y mejoras de rendimiento. Esto ayuda a garantizar que los clientes siempre tengan la última versión disponible de la biblioteca instalada en sus dispositivos.

Si lanzamos una nueva versión del SDK que incorpora nuevas características o API en esta biblioteca, deberá instalar la versión más reciente del SDK para usar esas características. En este escenario, también necesitarás publicar la aplicación actualizada en la Tienda.
