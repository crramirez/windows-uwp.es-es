---
ms.assetid: ''
description: Obtén información sobre cómo administrar la difusión de juegos para una aplicación Plataforma universal de Windows (UWP) mediante la interfaz de usuario del sistema de Windows y las extensiones de escritorio de Windows para UWP.
title: Administrar la difusión de juegos
ms.date: 09/27/2017
ms.topic: article
keywords: Windows 10, juego, difusión
ms.localizationpriority: medium
ms.openlocfilehash: 5dc7042a04ae653434b1742b913b2735b09e8b03
ms.sourcegitcommit: eb725a47c700131f5975d737bd9d8a809e04943b
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 08/27/2020
ms.locfileid: "88970123"
---
# <a name="manage-game-broadcasting"></a>Administrar la difusión de juegos
En este artículo se muestra cómo administrar la difusión de juegos para una aplicación para UWP. Los usuarios deben iniciar la difusión mediante la interfaz de usuario del sistema integrada en Windows, pero a partir de la versión 1709 de Windows 10, las aplicaciones pueden iniciar la interfaz de usuario de difusión del sistema y pueden recibir notificaciones cuando se inicia y se detiene la difusión.

## <a name="add-the-windows-desktop-extensions-for-the-uwp-to-your-app"></a>Agregar las extensiones de escritorio de Windows para UWP a la aplicación
Las API para administrar la difusión de aplicaciones, que se encuentran en el espacio de nombres **[Windows. Media. AppBroadcasting](https://docs.microsoft.com/uwp/api/windows.media.appbroadcasting)** , no se incluyen en el contrato de la API universal. Para acceder a las API, debe agregar una referencia a las extensiones de escritorio de Windows para UWP a la aplicación con los pasos siguientes.

1. En el **Explorador de soluciones**de Visual Studio, expanda el proyecto de UWP y haga clic con el botón derecho en **referencias** y seleccione **Agregar referencia..**.. 
2. Expanda el nodo **universal de Windows** y seleccione **extensiones**.
3. En la lista de extensiones, active la casilla situada junto a **extensiones de escritorio de Windows para la** entrada de UWP que coincida con la compilación de destino del proyecto. En el caso de las características de difusión de aplicaciones, la versión debe ser 1709 o superior.
4. Haga clic en **OK**.

## <a name="launch-the-system-ui-to-allow-the-user-to-initiate-broadcasting"></a>Iniciar la interfaz de usuario del sistema para permitir que el usuario inicie la difusión
Hay varios motivos por los que es posible que la aplicación no pueda difundirse actualmente, incluso si el dispositivo actual no cumple los requisitos de hardware para la difusión o si otra aplicación está difundiendo en este momento. Antes de iniciar la interfaz de usuario del sistema, puede comprobar si la aplicación actualmente puede difundir. En primer lugar, compruebe si las API de difusión están disponibles en el dispositivo actual. Las API no están disponibles en los dispositivos que ejecutan una versión de SO anterior a la versión 1709 de Windows 10. En lugar de buscar una versión específica del sistema operativo, use el método **[ApiInformation. IsApiContractPresent](https://docs.microsoft.com/uwp/api/windows.foundation.metadata.apiinformation.isapicontractpresent)** para consultar la versión 1,0 de *Windows. Media. AppBroadcasting. AppBroadcastingContract* . Si este contrato está presente, las API de difusión están disponibles en el dispositivo.

A continuación, obtenga una instancia de la clase **[AppBroadcastingUI](https://docs.microsoft.com/uwp/api/windows.media.appbroadcasting.appbroadcastingui)** llamando al Factory Method **[GetDefault](https://docs.microsoft.com/uwp/api/windows.media.appbroadcasting.appbroadcastingui.GetDefault)** en PC, donde hay un solo usuario que ha iniciado sesión a la vez. En Xbox, donde varios usuarios pueden iniciar sesión, llame a **[GetForUser](https://docs.microsoft.com/uwp/api/windows.media.appbroadcasting.appbroadcastingui.getforuser)** en su lugar. Después, llame a **[getStatus](https://docs.microsoft.com/uwp/api/windows.media.appbroadcasting.appbroadcastingui.GetStatus)** para obtener el estado de difusión de la aplicación.

La propiedad **[CanStartBroadcast](https://docs.microsoft.com/uwp/api/windows.media.appbroadcasting.appbroadcastingstatus.CanStartBroadcast)** de la clase **AppBroadcastingStatus** indica si la aplicación puede iniciar la difusión actualmente. Si no es así, puede comprobar la propiedad **[detalles](https://docs.microsoft.com/uwp/api/windows.media.appbroadcasting.appbroadcastingstatus.Details)** para determinar la razón por la que la difusión no está disponible. En función de la razón, puede que desee mostrar el estado al usuario o mostrar instrucciones para habilitar la difusión.

[!code-cpp[CanStartBroadcast](./code/AppBroadcast/cpp/AppBroadcastExampleApp/App.cpp#SnippetCanStartBroadcast)]

Solicite que el sistema muestre la interfaz de usuario de difusión de la aplicación mediante una llamada a **[ShowBroadcastUI](https://docs.microsoft.com/uwp/api/windows.media.appbroadcasting.appbroadcastingui.ShowBroadcastUI)**.

> [!NOTE] 
> El método **ShowBroadcastUI** representa una solicitud que puede no realizarse correctamente, dependiendo del estado actual del sistema. La aplicación no debe suponer que la difusión ha comenzado después de llamar a este método. Use el evento **[IsCurrentAppBroadcastingChanged](https://docs.microsoft.com/uwp/api/windows.media.appbroadcasting.appbroadcastingmonitor.IsCurrentAppBroadcastingChanged)** para recibir una notificación cuando se inicie o se detenga la difusión.

[!code-cpp[LaunchBroadcastUI](./code/AppBroadcast/cpp/AppBroadcastExampleApp/App.cpp#SnippetLaunchBroadcastUI)]

## <a name="receive-notifications-when-broadcasting-starts-and-stops"></a>Recibir notificaciones cuando se inicia y se detiene la difusión
Regístrese para recibir notificaciones cuando el usuario Use la interfaz de usuario del sistema para iniciar o detener la difusión de la aplicación mediante la inicialización de una instancia de la clase **[AppBroadcastingMonitor](https://docs.microsoft.com/uwp/api/windows.media.appbroadcasting.appbroadcastingmonitor)** y el registro de un controlador para el evento  **[IsCurrentAppBroadcastingChanged](https://docs.microsoft.com/uwp/api/windows.media.appbroadcasting.appbroadcastingmonitor.IsCurrentAppBroadcastingChanged)** . Como se explicó en la sección anterior, asegúrese de usar **[ApiInformation. IsApiContractPresent](https://docs.microsoft.com/uwp/api/windows.foundation.metadata.apiinformation.isapicontractpresent)** en algún momento para comprobar que las API de difusión están presentes en el dispositivo antes de intentar usarlas. 

[!code-cpp[AppBroadcastingRegisterChangedHandler](./code/AppBroadcast/cpp/AppBroadcastExampleApp/App.cpp#SnippetAppBroadcastingRegisterChangedHandler)]

En el controlador del evento **IsCurrentAppBroadcastingChanged** , puede que desee actualizar la interfaz de usuario de la aplicación para que refleje el estado de difusión actual.

[!code-cpp[AppBroadcastingChangedHandler](./code/AppBroadcast/cpp/AppBroadcastExampleApp/App.cpp#SnippetAppBroadcastingChangedHandler)]

## <a name="related-topics"></a>Temas relacionados

* [Juegos](index.md)

 

 




