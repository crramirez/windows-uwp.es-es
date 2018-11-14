---
author: drewbatgit
ms.assetid: ''
description: Muestra cómo administrar la difusión de juegos para una aplicación para UWP.
title: Administrar la difusión de juegos
ms.author: drewbat
ms.date: 09/27/2017
ms.topic: article
keywords: windows 10, juego, difusión
ms.localizationpriority: medium
ms.openlocfilehash: ae70c29927925abcf948435ed768871ba2427fd9
ms.sourcegitcommit: 4d88adfaf544a3dab05f4660e2f59bbe60311c00
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 11/13/2018
ms.locfileid: "6455600"
---
# <a name="manage-game-broadcasting"></a>Administrar la difusión de juegos
Este artículo muestra cómo administrar la difusión de juegos para una aplicación para UWP. Los usuarios deben iniciar la difusión usando la interfaz de usuario del sistema integrada en Windows pero, a partir de Windows 10, versión 1709, las aplicaciones pueden iniciar la interfaz de usuario de difusión del sistema y pueden recibir notificaciones cuando se inicia y se detiene la difusión.

## <a name="add-the-windows-desktop-extensions-for-the-uwp-to-your-app"></a>Agregar las extensiones de escritorio de Windows para UWP a tu aplicación
Las API para administrar la emisión de la aplicación, que se encuentran en el espacio de nombres **[Windows.Media.AppBroadcasting](https://docs.microsoft.com/uwp/api/windows.media.appbroadcasting)** no se incluyen en el contrato de API Universal. Para acceder a las API, debes agregar una referencia a las extensiones de escritorio de Windows para UWP a tu aplicación, con los siguientes pasos.

1. En Visual Studio, en **Explorador de soluciones**, expande el proyecto UWP, haz clic con el botón derecho en **Referencias** y entonces selecciona **Agregar referencia...**. 
2. Expande el nodo **Windows Universal** y selecciona **Extensiones**.
3. En la lista de extensiones, marca la casilla de verificación junto a la entrada de **Extensiones de escritorio de Windows para UWP** que coincida con la compilación de destino del proyecto. Para las funciones de difusión de aplicaciones, la versión debe ser 1709 o superior.
4. Haz clic en **Aceptar**.

## <a name="launch-the-system-ui-to-allow-the-user-to-initiate-broadcasting"></a>Iniciar la interfaz de usuario del sistema para permitir que el usuario inicie la difusión
Existen varios motivos por los que la aplicación tal vez no pueda difundir, incluyendo si el dispositivo actual no cumple los requisitos de hardware para la difusión o si otra aplicación está actualmente difundiendo. Antes de iniciar la interfaz de usuario del sistema, puedes comprobar si la aplicación es capaz de difundir actualmente. En primer lugar, comprueba si las API de difusión están disponibles en el dispositivo actual. Las API no están disponibles en dispositivos que ejecutan una versión de sistema operativo anterior a Windows 10, versión 1709. En lugar de comprobar si hay una versión específica del sistema operativo, usa el método **[ApiInformation.IsApiContractPresent](https://docs.microsoft.com/uwp/api/windows.foundation.metadata.apiinformation.isapicontractpresent)** para preguntar por la versión 1.0 de *Windows.Media.AppBroadcasting.AppBroadcastingContract*. Si este contrato está presente, las API de difusión están disponibles en el dispositivo.

A continuación, obtén una instancia de la clase **[AppBroadcastingUI](https://docs.microsoft.com/uwp/api/windows.media.appbroadcasting.appbroadcastingui)** llamando al método de fábrica **[GetDefault](https://docs.microsoft.com/uwp/api/windows.media.appbroadcasting.appbroadcastingui.GetDefault)** en el PC, donde hay un solo usuario con sesión iniciada en cada momento. En XBox, donde varios usuarios pueden haber iniciado sesión, llama en su lugar a **[GetForUser](https://docs.microsoft.com/uwp/api/windows.media.appbroadcasting.appbroadcastingui.getforuser)**. A continuación, llama a **[GetStatus](https://docs.microsoft.com/uwp/api/windows.media.appbroadcasting.appbroadcastingui.GetStatus)** para obtener el estado de emisión de la aplicación.

La propiedad **[CanStartBroadcast](https://docs.microsoft.com/uwp/api/windows.media.appbroadcasting.appbroadcastingstatus.CanStartBroadcast)** de la clase **AppBroadcastingStatus** te indica si la aplicación puede comenzar a difundir actualmente. Si no es así, puedes comprobar la propiedad **[Detalles](https://docs.microsoft.com/uwp/api/windows.media.appbroadcasting.appbroadcastingstatus.Details)** para determinar la razón por la que la difusión no está disponible. Según el motivo, puede que te interese mostrar el estado al usuario o mostrar instrucciones para habilitar la difusión.

[!code-cpp[CanStartBroadcast](./code/AppBroadcast/cpp/AppBroadcastExampleApp/App.cpp#SnippetCanStartBroadcast)]

Solicita que el sistema muestre la interfaz de usuario de difusión llamando a **[ShowBroadcastUI](https://docs.microsoft.com/uwp/api/windows.media.appbroadcasting.appbroadcastingui.ShowBroadcastUI)**.

> [!NOTE] 
> El método **ShowBroadcastUI** representa una solicitud que puede que no prospere, dependiendo del estado actual del sistema. La aplicación no debería suponer que la difusión ha comenzado después de llamar a este método. Usa el evento **[IsCurrentAppBroadcastingChanged](https://docs.microsoft.com/uwp/api/windows.media.appbroadcasting.appbroadcastingmonitor.IsCurrentAppBroadcastingChanged)** para que se te notifique cuando la difusión se inicie o detenga.

[!code-cpp[LaunchBroadcastUI](./code/AppBroadcast/cpp/AppBroadcastExampleApp/App.cpp#SnippetLaunchBroadcastUI)]

## <a name="receive-notifications-when-broadcasting-starts-and-stops"></a>Recibir notificaciones cuando la difusión se inicia y se detiene
Regístrate para recibir notificaciones cuando el usuario use la interfaz de usuario del sistema para iniciar o detener la transmisión de tu aplicación mediante la iniciación de una instancia de clase **[AppBroadcastingMonitor](https://docs.microsoft.com/uwp/api/windows.media.appbroadcasting.appbroadcastingmonitor)** y el registro de un controlador para el evento **[IsCurrentAppBroadcastingChanged](https://docs.microsoft.com/uwp/api/windows.media.appbroadcasting.appbroadcastingmonitor.IsCurrentAppBroadcastingChanged)**. Como se explicó en la sección anterior, asegúrate de usar **[ApiInformation.IsApiContractPresent](https://docs.microsoft.com/uwp/api/windows.foundation.metadata.apiinformation.isapicontractpresent)** en algún momento para comprobar que las API de difusión estén presentes en el dispositivo antes de intentar usarlas. 

[!code-cpp[AppBroadcastingRegisterChangedHandler](./code/AppBroadcast/cpp/AppBroadcastExampleApp/App.cpp#SnippetAppBroadcastingRegisterChangedHandler)]

En el controlador del evento **IsCurrentAppBroadcastingChanged**, puede convenirte actualizar la interfaz de usuario de la aplicación para reflejar el estado actual de difusión.

[!code-cpp[AppBroadcastingChangedHandler](./code/AppBroadcast/cpp/AppBroadcastExampleApp/App.cpp#SnippetAppBroadcastingChangedHandler)]

## <a name="related-topics"></a>Temas relacionados

* [Juegos](index.md)

 

 




