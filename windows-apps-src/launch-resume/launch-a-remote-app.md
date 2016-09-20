---
author: TylerMSFT
title: "Iniciar una aplicación en un dispositivo remoto"
description: "Aprende a iniciar una aplicación en un dispositivo remoto mediante el proyecto &quot;Roma&quot;."
translationtype: Human Translation
ms.sourcegitcommit: ff8e16d0e376d502157ae42b9cdae11875008554
ms.openlocfilehash: d8c3783d68a1b3b216058790d84255a7fb4b612c

---

# Iniciar una aplicación en un dispositivo remoto

En este artículo se explica cómo iniciar una aplicación para la Plataforma universal de Windows (UWP) o una aplicación de escritorio de Windows en un dispositivo remoto.

A partir de Windows 10, versión 1607, una aplicación para UWP puede iniciar una aplicación para UWP o una aplicación de escritorio de Windows de forma remota en otro dispositivo que también ejecute Windows 10, versión 1607 o posterior.

Un escenario para iniciar una aplicación en un dispositivo remoto es permitir que un usuario inicie una tarea en un dispositivo y la finalice en otro. Por ejemplo, si estás escuchando música en el teléfono de camino a casa, podrías usar el inicio remoto para pasar la reproducción a tu Xbox al llegar a casa. Puedes pasar datos a la aplicación remota para proporcionar el contexto de la aplicación remota con el propósito de continuar con la tarea.

## Agregar la funcionalidad RemoteSystem

Para que la aplicación pueda iniciar una aplicación en un dispositivo remoto, debes agregar la funcionalidad <uap3:Capability Name="remoteSystem"/> al manifiesto del paquete de la aplicación. Para usar el diseñador de manifiestos del paquete para agregarla, puedes seleccionar **Sistema remoto** en la pestaña **Funcionalidades**, o bien puedes hacer manualmente lo que haría el diseñador de manifiestos del paquete y agregar lo siguiente al archivo Package.appxmanifest.

``` xml
<Capabilities>
   <uap3:Capability Name="remoteSystem"/>
 </Capabilities>
```
## Buscar un dispositivo remoto

En primer lugar, debes buscar el dispositivo al que quieres conectarte. [Discover remote devices](discover-remote-devices.md) describe cómo hacerlo de manera detallada. Usaremos un enfoque sencillo que renuncia al filtro por tipo de dispositivo o de conectividad. Crearemos un monitor de sistema remoto que busque dispositivos remotos y escriba controladores de eventos, que se notificarán cuando se detecten o se quiten dispositivos que usen la misma cuenta de Microsoft. Esto nos proporcionará una colección de dispositivos remotos.

El código de estos ejemplos supone que tienes una instrucción `using Windows.System.RemoteSystems` en el archivo.

[!code-cs[Principal](./code/RemoteLaunchScenario/MainPage.xaml.cs#SnippetBuildDeviceList)]

Lo primero que debes hacer antes de realizar un inicio remoto es llamar a `RemoteSystem.RequestAccessAsync()`. Comprueba el valor devuelto para asegurarte de que tu aplicación puede acceder a dispositivos remotos. Es posible que no se pueda realizar esta comprobación si no has agregado la funcionalidad `remoteSystem` a tu aplicación.

La llamada a los controladores de eventos del monitor de sistema tiene lugar cuando un dispositivo con el que nos podemos conectar se detecta o ya no está disponible. Usaremos estos controladores de eventos para mantener una lista actualizada de dispositivos con los que nos podemos conectar.

[!code-cs[Principal](./code/RemoteLaunchScenario/MainPage.xaml.cs#SnippetEventHandlers)]

Se realizará un seguimiento de los dispositivos por el identificador de sistema remoto mediante un objeto Dictionary. Se usa una clase ObservableCollection para contener la lista de dispositivos que podemos enumerar. Una clase ObservableCollection también facilita el enlace de la lista de dispositivos a la interfaz de usuario, aunque no lo haremos en este ejemplo.

[!code-cs[Principal](./code/RemoteLaunchScenario/MainPage.xaml.cs#SnippetMembers)]

Agrega una llamada a `BuildDeviceList()` en el código de inicio de la aplicación antes de intentar iniciar una aplicación remota.

## Iniciar una aplicación en un dispositivo remoto

Para iniciar una aplicación de forma remota, pasa el dispositivo al que quieres conectarte con la API [RemoteLauncher.LaunchUri](https://msdn.microsoft.com/en-us/library/windows/apps/windows.system.remotelauncher.launchuriasync.aspx). Hay tres sobrecargas para esta función. La más simple, que se muestra en este ejemplo, especifica el URI que activará la aplicación en el dispositivo remoto. En este ejemplo, el URI abre la aplicación Mapas en el equipo remoto con una vista 3D de la torre Space Needle.

Otras sobrecargas de **RemoteLauncher.LaunchUriAsync** te permiten especificar opciones, como el URI del sitio web, para ver si una aplicación que puede controlar el URI no se puede iniciar en el dispositivo remoto, y una lista opcional de nombres de familia de paquete, que se puede usar para iniciar el URI en el dispositivo remoto. También puedes proporcionar datos en forma de pares clave-valor. Podrías pasar datos a la aplicación que estás activando en el dispositivo remoto para proporcionar contexto a la aplicación remota, como el nombre de la canción que se va a reproducir y la ubicación de reproducción actual al pasar la reproducción de un dispositivo a otro.

En un uso real, podrías proporcionar la interfaz de usuario para seleccionar el dispositivo que quieres usar. No obstante, para simplificar este ejemplo, solo usaremos el primero de la lista para realizar la llamada remota.

[!code-cs[Principal](./code/RemoteLaunchScenario/MainPage.xaml.cs#SnippetRemoteUriLaunch)]

La enumeración [RemoteLaunchUriStatus](https://msdn.microsoft.com/en-us/library/windows/apps/windows.system.remotelaunchuristatus.aspx) que devuelve **RemoteLauncher.LaunchUriAsync()** indica si la ejecución remota se realizó correctamente y, si no es así, la causa.

## Temas relacionados

[Referencia de API de sistemas remotos](https://msdn.microsoft.com/en-us/library/windows/apps/Windows.System.RemoteSystems)  
[Introducción a las aplicaciones y dispositivos conectados (proyecto "Roma")](connected-apps-and-devices.md)  
La [muestra de sistemas remotos](https://github.com/Microsoft/Windows-universal-samples/tree/dev/Samples/RemoteSystems ) indica cómo detectar un sistema remoto, iniciar una aplicación en un sistema remoto y usar los servicios de aplicaciones para enviar mensajes entre aplicaciones que se ejecuten en dos sistemas.



<!--HONumber=Aug16_HO5-->


