---
title: Iniciar una aplicación en un dispositivo remoto
description: Obtén más información sobre cómo iniciar una aplicación en un dispositivo remoto con Project Rome.
ms.date: 02/12/2018
ms.topic: article
keywords: dispositivos Windows 10, uwp, conectados, sistemas remotos, Roma, proyecto rome
ms.assetid: 54f6a33d-a3b5-4169-8664-653dbab09175
ms.localizationpriority: medium
ms.openlocfilehash: 26a67816195105572d9f690599b9a880ece90c98
ms.sourcegitcommit: b4c502d69a13340f6e3c887aa3c26ef2aeee9cee
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 12/03/2018
ms.locfileid: "8469783"
---
# <a name="launch-an-app-on-a-remote-device"></a>Iniciar una aplicación en un dispositivo remoto

Este artículo explica cómo iniciar una aplicación de Windows en un dispositivo remoto.

A partir de Windows10, versión 1607, una aplicación para UWP puede iniciar una aplicación para UWP o una aplicación de escritorio de Windows remotamente en otro dispositivo que también esté ejecutando Windows 10, versión 1607 o posterior, siempre que ambos dispositivos hayan iniciado sesión con la misma cuenta de Microsoft (MSA). Este es el caso de uso más sencillo del proyecto Rome.

La característica de inicio remoto ofrece experiencias de usuario orientadas a tareas, es decir, un usuario puede iniciar una tarea en un dispositivo y terminarla en otro. Por ejemplo, si el usuario está escuchando música en su teléfono en el automóvil, luego podría trasladar la funcionalidad de reproducción a su Xbox One al llegar a casa. El inicio remoto permite a las aplicaciones pasar datos contextuales a la aplicación remota que se inicia para continuar la tarea donde se haya dejado.

## <a name="preliminary-setup"></a>Configuración preliminar

### <a name="add-the-remotesystem-capability"></a>Agregar la funcionalidad remoteSystem

Para que la aplicación pueda iniciar una aplicación en un dispositivo remoto, debes agregar la funcionalidad `remoteSystem` al manifiesto del paquete de la aplicación. Si quieres usar el diseñador de manifiestos del paquete para agregarla, selecciona **Sistema remoto** en la pestaña **Funcionalidades** o bien puedes agregar manualmente la siguiente línea al archivo _Package.appxmanifest_ del proyecto.

``` xml
<Capabilities>
   <uap3:Capability Name="remoteSystem"/>
</Capabilities>
```

### <a name="enable-cross-device-sharing"></a>Habilitar el uso compartido en todos los dispositivos

Además, debe establecerse el dispositivo cliente para permitir el uso compartido entre dispositivos. Esta configuración, a la que se obtiene acceso en **Configuración**: **Sistema** > **Experiencias compartidas** > **Compartir entre dispositivos**, está habilitada de manera predeterminada. 

![página de configuración de experiencias compartidas](images/shared-experiences-settings.png)

## <a name="find-a-remote-device"></a>Buscar un dispositivo remoto

En primer lugar, debes buscar el dispositivo al que quieres conectarte. [Discover remote devices](discover-remote-devices.md) describe cómo hacerlo de manera detallada. Usaremos un enfoque sencillo que renuncia al filtro por tipo de dispositivo o de conectividad. Crearemos un monitor de sistema remoto que busque dispositivos remotos y escribiremos controladores para los eventos que se generen cuando se detecten o se quiten dispositivos. Esto nos proporcionará una colección de dispositivos remotos.

El código de estos ejemplos requiere que tengas una instrucción `using Windows.System.RemoteSystems` en el archivo de clase.

[!code-cs[Main](./code/RemoteLaunchScenario/MainPage.xaml.cs#SnippetBuildDeviceList)]

Lo primero que debes hacer antes de realizar un inicio remoto es llamar a `RemoteSystem.RequestAccessAsync()`. Comprueba el valor devuelto para asegurarte de que tu aplicación puede acceder a dispositivos remotos. Es posible que no se pueda realizar esta comprobación si no has agregado la funcionalidad `remoteSystem` a tu aplicación.

La llamada a los controladores de eventos del monitor de sistema tiene lugar cuando un dispositivo con el que nos podemos conectar se detecta o ya no está disponible. Usaremos estos controladores de eventos para mantener una lista actualizada de dispositivos con los que nos podemos conectar.

[!code-cs[Main](./code/RemoteLaunchScenario/MainPage.xaml.cs#SnippetEventHandlers)]


Realizaremos un seguimiento de los dispositivos mediante el identificador del sistema remoto mediante un objeto **Dictionary**. Se usa una clase **ObservableCollection** para contener la lista de dispositivos que podemos enumerar. Una clase **ObservableCollection** también facilita el enlace de la lista de dispositivos a la interfaz de usuario, aunque no lo haremos en este ejemplo.

[!code-cs[Main](./code/RemoteLaunchScenario/MainPage.xaml.cs#SnippetMembers)]

Agrega una llamada a `BuildDeviceList()` en el código de inicio de la aplicación antes de intentar iniciar una aplicación remota.

## <a name="launch-an-app-on-a-remote-device"></a>Iniciar una aplicación en un dispositivo remoto

Para iniciar una aplicación de forma remota, pasa el dispositivo al que quieres conectarte con la API [**RemoteLauncher.LaunchUriAsync**](https://msdn.microsoft.com/library/windows/apps/windows.system.remotelauncher.launchuriasync.aspx). Hay tres sobrecargas para este método. La más simple, que se muestra en este ejemplo, especifica el URI que activará la aplicación en el dispositivo remoto. En este ejemplo, el URI abre la aplicación Mapas en el equipo remoto con una vista 3D de la torre Space Needle.

Otras sobrecargas de **RemoteLauncher.LaunchUriAsync** te permiten especificar opciones, como el URI del sitio web, para ver si en el dispositivo remoto no se puede iniciar ninguna aplicación que pueda controlar el URI, y una lista opcional de nombres de familia de paquete, que se podría usar para iniciar el URI en el dispositivo remoto. También puedes proporcionar datos en forma de pares clave-valor. Podrías pasar datos a la aplicación que estás activando para proporcionar contexto a la aplicación remota, como el nombre de la canción que se va a reproducir y la ubicación de reproducción actual al pasar la reproducción de un dispositivo a otro.

En casos prácticos, podrías proporcionar la interfaz de usuario para seleccionar el dispositivo al que quieres dirigirte. No obstante, para simplificar este ejemplo, solo usaremos el primer dispositivo remoto de la lista.

[!code-cs[Main](./code/RemoteLaunchScenario/MainPage.xaml.cs#SnippetRemoteUriLaunch)]

El objeto [**RemoteLaunchUriStatus**](https://msdn.microsoft.com/library/windows/apps/windows.system.remotelaunchuristatus.aspx) que se devuelve de **RemoteLauncher.LaunchUriAsync()** indica si el inicio remoto se ha realizado correctamente y, si no es así, la causa.

## <a name="related-topics"></a>Temas relacionados

[Referencia de API de sistemas remotos](https://msdn.microsoft.com/library/windows/apps/Windows.System.RemoteSystems)  
[Introducción a aplicaciones y dispositivos conectados (Project Rome)](connected-apps-and-devices.md)  
[Detectar dispositivos remotos](discover-remote-devices.md)  
La [muestra de sistemas remotos](https://github.com/Microsoft/Windows-universal-samples/tree/dev/Samples/RemoteSystems) indica cómo detectar un sistema remoto, iniciar una aplicación en un sistema remoto y usar los servicios de aplicaciones para enviar mensajes entre aplicaciones que se ejecuten en dos sistemas.
