---
title: Iniciar una aplicación en un dispositivo remoto
description: Obtenga información sobre cómo iniciar una aplicación para UWP o una aplicación de escritorio de Windows de forma remota en otro dispositivo, lo que permite al usuario iniciar una tarea en un dispositivo y finalizarla en otro.
ms.date: 02/12/2018
ms.topic: article
keywords: Windows 10, UWP, dispositivos conectados, sistemas remotos, Roma, proyecto Roma
ms.assetid: 54f6a33d-a3b5-4169-8664-653dbab09175
ms.localizationpriority: medium
ms.openlocfilehash: 4163106c5439ec8881c1b5042f63fb7abf4fd668
ms.sourcegitcommit: c3ca68e87eb06971826087af59adb33e490ce7da
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 09/02/2020
ms.locfileid: "89362508"
---
# <a name="launch-an-app-on-a-remote-device"></a>Iniciar una aplicación en un dispositivo remoto

Este artículo explica cómo iniciar una aplicación de Windows en un dispositivo remoto.

A partir de Windows 10, versión 1607, una aplicación para UWP puede iniciar una aplicación para UWP o una aplicación de escritorio de Windows remotamente en otro dispositivo que también esté ejecutando Windows 10, versión 1607 o posterior, siempre que ambos dispositivos hayan iniciado sesión con la misma cuenta de Microsoft (MSA). Este es el caso de uso más sencillo del proyecto Roma.

La característica de inicio remoto habilita experiencias de usuario orientadas a tareas; un usuario puede iniciar una tarea en un dispositivo y finalizarla en otro. Por ejemplo, si el usuario está escuchando música en su teléfono en su coche, podría controlar la funcionalidad de reproducción a su Xbox One cuando lleguen a casa. El inicio remoto permite que las aplicaciones pasen datos contextuales a la aplicación remota que se está iniciando, con el fin de recoger dónde se dejó la tarea.

## <a name="preliminary-setup"></a>Configuración preliminar

### <a name="add-the-remotesystem-capability"></a>Agregar la funcionalidad remoteSystem

Para que la aplicación pueda iniciar una aplicación en un dispositivo remoto, debes agregar la funcionalidad `remoteSystem` al manifiesto del paquete de la aplicación. Puede usar el diseñador de manifiestos de paquete para agregarlo seleccionando **sistema remoto** en la pestaña **capacidades** , o puede Agregar manualmente la siguiente línea al archivo _Package. appxmanifest_ del proyecto.

``` xml
<Capabilities>
   <uap3:Capability Name="remoteSystem"/>
</Capabilities>
```

### <a name="enable-cross-device-sharing"></a>Habilitar el uso compartido entre dispositivos

Además, el dispositivo cliente debe estar configurado para permitir el uso compartido entre dispositivos. Esta configuración, a la que se tiene acceso en **configuración**: el uso compartido de **System**  >  **experiencias compartidas**del sistema  >  **entre dispositivos**, está habilitado de forma predeterminada. 

![Página de configuración de experiencias compartidas](images/shared-experiences-settings.png)

## <a name="find-a-remote-device"></a>Buscar un dispositivo remoto

En primer lugar, debes buscar el dispositivo al que quieres conectarte. [Discover remote devices](discover-remote-devices.md) describe cómo hacerlo de manera detallada. Usaremos un enfoque sencillo que renuncia al filtro por tipo de dispositivo o de conectividad. Crearemos un monitor de sistema remoto que busque dispositivos remotos y escribiremos controladores para los eventos que se generen cuando se detecten o se quiten dispositivos. Esto nos proporcionará una colección de dispositivos remotos.

El código de estos ejemplos requiere que tenga una `using Windows.System.RemoteSystems` instrucción en los archivos de clase.

:::code language="csharp" source="~/../snippets-windows/windows-uwp/launch-resume/RemoteLaunchScenario/cs/MainPage.xaml.cs" id="SnippetBuildDeviceList":::

Lo primero que debes hacer antes de realizar un inicio remoto es llamar a `RemoteSystem.RequestAccessAsync()`. Comprueba el valor devuelto para asegurarte de que tu aplicación puede acceder a dispositivos remotos. Es posible que no se pueda realizar esta comprobación si no has agregado la funcionalidad `remoteSystem` a tu aplicación.

La llamada a los controladores de eventos del monitor de sistema tiene lugar cuando un dispositivo con el que nos podemos conectar se detecta o ya no está disponible. Usaremos estos controladores de eventos para mantener una lista actualizada de dispositivos con los que nos podemos conectar.

:::code language="csharp" source="~/../snippets-windows/windows-uwp/launch-resume/RemoteLaunchScenario/cs/MainPage.xaml.cs" id="SnippetEventHandlers":::


Realizaremos un seguimiento de los dispositivos mediante el identificador del sistema remoto mediante un objeto **Dictionary**. Un **ObservableCollection** se usa para contener la lista de dispositivos que se pueden enumerar. Un **ObservableCollection** también facilita el enlace de la lista de dispositivos a la interfaz de usuario, aunque no lo haremos en este ejemplo.

:::code language="csharp" source="~/../snippets-windows/windows-uwp/launch-resume/RemoteLaunchScenario/cs/MainPage.xaml.cs" id="SnippetMembers":::

Agrega una llamada a `BuildDeviceList()` en el código de inicio de la aplicación antes de intentar iniciar una aplicación remota.

## <a name="launch-an-app-on-a-remote-device"></a>Iniciar una aplicación en un dispositivo remoto

Para iniciar una aplicación de forma remota, pasa el dispositivo al que quieres conectarte con la API [**RemoteLauncher.LaunchUriAsync**](/uwp/api/windows.system.remotelauncher.launchuriasync). Hay tres sobrecargas para este método. La más simple, que se muestra en este ejemplo, especifica el URI que activará la aplicación en el dispositivo remoto. En este ejemplo, el URI abre la aplicación Mapas en el equipo remoto con una vista 3D de la torre Space Needle.

Otras sobrecargas de **RemoteLauncher.LaunchUriAsync** te permiten especificar opciones, como el URI del sitio web, para ver si en el dispositivo remoto no se puede iniciar ninguna aplicación que pueda controlar el URI, y una lista opcional de nombres de familia de paquete, que se podría usar para iniciar el URI en el dispositivo remoto. También puedes proporcionar datos en forma de pares clave-valor. Podrías pasar datos a la aplicación que estás activando para proporcionar contexto a la aplicación remota, como el nombre de la canción que se va a reproducir y la ubicación de reproducción actual al pasar la reproducción de un dispositivo a otro.

En casos prácticos, podrías proporcionar la interfaz de usuario para seleccionar el dispositivo al que quieres dirigirte. No obstante, para simplificar este ejemplo, solo usaremos el primer dispositivo remoto de la lista.

:::code language="csharp" source="~/../snippets-windows/windows-uwp/launch-resume/RemoteLaunchScenario/cs/MainPage.xaml.cs" id="SnippetRemoteUriLaunch":::

El objeto [**RemoteLaunchUriStatus**](/uwp/api/windows.system.remotelaunchuristatus) que se devuelve de **RemoteLauncher.LaunchUriAsync()** indica si el inicio remoto se ha realizado correctamente y, si no es así, la causa.

## <a name="related-topics"></a>Temas relacionados

[Referencia de API de sistemas remotos](/uwp/api/Windows.System.RemoteSystems)  
[Información general sobre dispositivos y aplicaciones conectados (proyecto Roma)](connected-apps-and-devices.md)  
[Detectar dispositivos remotos](discover-remote-devices.md)  
La [muestra de sistemas remotos](https://github.com/Microsoft/Windows-universal-samples/tree/dev/Samples/RemoteSystems) indica cómo detectar un sistema remoto, iniciar una aplicación en un sistema remoto y usar los servicios de aplicaciones para enviar mensajes entre aplicaciones que se ejecuten en dos sistemas.
