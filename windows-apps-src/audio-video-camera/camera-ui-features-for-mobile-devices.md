---
ms.assetid: c43d4af3-9a1a-4eae-a137-1267c293c1b5
description: Este artículo muestra cómo sacar provecho de las características especiales de la interfaz de usuario de la cámara que solo están presentes en los dispositivos móviles.
title: Características de la interfaz de usuario de la cámara para dispositivos móviles
ms.date: 02/08/2017
ms.topic: article
keywords: windows 10, uwp
ms.localizationpriority: medium
ms.openlocfilehash: 4e1076c0632299ff79a8ca2fc226865d0ff3ebef
ms.sourcegitcommit: c3ca68e87eb06971826087af59adb33e490ce7da
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 09/02/2020
ms.locfileid: "89362898"
---
# <a name="camera-ui-features-for-mobile-devices"></a>Características de la interfaz de usuario de la cámara para dispositivos móviles

Este artículo muestra cómo sacar provecho de las características especiales de la interfaz de usuario de la cámara que solo están presentes en los dispositivos móviles. 

## <a name="add-the-mobile-extension-to-your-project"></a>Agregar la extensión móvil al proyecto 

Para usar estas características, debes agregar una referencia al SDK de extensión móvil de Microsoft para la plataforma de aplicación universal al proyecto.

**Para agregar una referencia al SDK de extensión móvil para admitir el soporte del botón de cámara hardware**

1.  En **Explorador de soluciones**, haga clic con el botón derecho en **referencias** y seleccione **Agregar referencia**.

2.  Expanda el nodo **universal de Windows** y seleccione **extensiones**.

3.  Selecciona la casilla **SDK de extensión móvil de Microsoft para la plataforma de aplicación universal**.

## <a name="hide-the-status-bar"></a>Ocultar la barra de estado

Los dispositivos móviles tienen un control [**StatusBar**](/uwp/api/Windows.UI.ViewManagement.StatusBar) que proporciona al usuario información del estado del dispositivo. Este control ocupa espacio en la pantalla que puede interferir con la interfaz de usuario de captura de multimedia. Puedes ocultar la barra de estado llamando a [**HideAsync**](/uwp/api/windows.ui.viewmanagement.statusbar.hideasync), pero debes hacer esta llamada desde dentro de un bloque condicional cuando usas el método [**ApiInformation.IsTypePresent**](/uwp/api/windows.foundation.metadata.apiinformation.istypepresent) para determinar si la API está disponible. Este método solo devolverá true en dispositivos móviles que admitan la barra de estado. Debes ocultar la barra de estado cuando se inicie la aplicación o al empezar a obtener una vista previa de la cámara.

:::code language="csharp" source="~/../snippets-windows/windows-uwp/audio-video-camera/BasicMediaCaptureWin10/cs/MainPage.xaml.cs" id="SnippetHideStatusBar":::

Cuando se cierra la aplicación o cuando el usuario abandona la página de captura multimedia de la aplicación, puedes hacer que el control vuelva a ser visible.

:::code language="csharp" source="~/../snippets-windows/windows-uwp/audio-video-camera/BasicMediaCaptureWin10/cs/MainPage.xaml.cs" id="SnippetShowStatusBar":::

## <a name="use-the-hardware-camera-button"></a>Usar el botón de cámara de hardware

Algunos dispositivos móviles tienen un botón dedicado a la cámara de hardware, que algunos usuarios prefieren a un control en pantalla. Para recibir notificaciones cuando se presiona el botón de hardware de cámara, registra un controlador para el evento [**HardwareButtons.CameraPressed**](/uwp/api/windows.phone.ui.input.hardwarebuttons.camerapressed). Como esta API está disponible solo en dispositivos móviles, debes volver a usar el **IsTypePresent** para asegurarte de que la API es compatible con el dispositivo actual antes de intentar acceder a este.

:::code language="csharp" source="~/../snippets-windows/windows-uwp/audio-video-camera/BasicMediaCaptureWin10/cs/MainPage.xaml.cs" id="SnippetPhoneUsing":::

:::code language="csharp" source="~/../snippets-windows/windows-uwp/audio-video-camera/BasicMediaCaptureWin10/cs/MainPage.xaml.cs" id="SnippetRegisterCameraButtonHandler":::

En el controlador para el evento **CameraPressed**, puedes iniciar una captura de fotos.

:::code language="csharp" source="~/../snippets-windows/windows-uwp/audio-video-camera/BasicMediaCaptureWin10/cs/MainPage.xaml.cs" id="SnippetCameraPressed":::

Cuando se cierra la aplicación o el usuario abandona la página de captura multimedia de la aplicación, anula el registro del controlador de botón de hardware.

:::code language="csharp" source="~/../snippets-windows/windows-uwp/audio-video-camera/BasicMediaCaptureWin10/cs/MainPage.xaml.cs" id="SnippetUnregisterCameraButtonHandler":::

## <a name="related-topics"></a>Temas relacionados

* [Cámara](camera.md)
* [Captura básica de fotos, audio y vídeo con MediaCapture](basic-photo-video-and-audio-capture-with-MediaCapture.md)
